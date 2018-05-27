---
slug: jmeter-grafana-dashboard
title: 'JMeter 实时监控仪表板配置 (Grafana + InfluxDB)'
date: 2017-12-16T20:04:19+08:00
draft: false
tags:
- Grafana
- InfluxDB
- JMeter
categories:
- 10000 Hours
- 性能测试工具
---


在服务器上跑 JMeter 做压测的话，给工具本身也配上实时监控是必须的，命令行输出能提供的信息太少。

JMeter的 Backend Listener 支持 Graphite 和 InfluxDB，这里选择 InfluxDB 做时序数据库，支持类似 SQL 的查询语法是最大的优点。另外在 JMeter 3.2+ 里配置起来也比 Graphite 方便太多。（缺点是直到写这篇文章时官网文档都没更新，要自己查存储的字段，猜它有什么用）

Grafana 能配出非常漂亮的监控仪表板，就是配的过程非常痛苦，不做非常详细的笔记的话过几天又忘光了，于是有了这篇东西。

---

【前提】

- 采集器：JMeter 3.2+，Backend Listener 里选择 `InfluxdbBackendListenerClient`
- 数据源：InfluxDB 1.4+
- 面板：Grafana 4.6+ 
    - 已添加好数据源
    - 新建面板，添加 3 行

【注意】

吞吐率和响应时间图表只计算成功的请求（失败的通常没意义，超时失败的能在表格里看到数量），结果可能会跟JMeter里看到的有出入。

【效果】

![总体](http://keithmo.me/img/screenshot/dashboard/20171216_grafana_jmeter_dashboard1.png)

![错误数](http://keithmo.me/img/screenshot/dashboard/20171216_grafana_jmeter_dashboard2.png)

![单个接口](http://keithmo.me/img/screenshot/dashboard/20171216_grafana_jmeter_dashboard3.png)

已经上传到 Grafana 官网，可以从以下地址下载JSON文件，或通过ID `4026` 直接导入：

https://grafana.com/dashboards/4026

---

JMeter Backend Listener 参考配置：

![JMeter设置](http://keithmo.me/img/screenshot/dashboard/20171216_jmeter_backend_listener_config.png)

---

### Settings

- General
    - Name: `JMeter Dashboard`
    - Description: `Monitor your JMeter load test in real time with InfluxDB and Grafana.`
    - Tags: `load_test`
- Rows
    - `Summary`, `Errors`, `Individual Transaction - $transaction`

---

### Templating

`$data_source`

- Name: `data_source`
- Type: Datasource
- Type: InfluxDB

`$application`

- Name: `application`
- Type: Query
- Data source: `$data_source`
- Refresh: On Dashboard Load
- Query: `SHOW TAG VALUES FROM "$measurement_name" WITH KEY = "application"`

`$transaction`

- Name: `transaction`
- Type: Query
- Data source: `$data_source`
- Refresh: On Dashboard Load
- Query: `SHOW TAG VALUES FROM "$measurement_name" WITH KEY = "transaction" WHERE "application" =~ /^$application$/ AND "transaction" != 'internal' AND "transaction" != 'all'`

可惜 templating 里不支持 `$timeFilter`（由于 InfluxDB `show tag values` 语法的限制），时间久了之后各种接口名看着会比较乱。

`$measurement_name`

- Name: `measurement_name`
- Label: `Measurement name`
- Type: Constant
- Hide: Variable
- Value: `jmeter` （JMeter Backend Listener 默认）

`$send_interval`

- Name: `send_interval`
- Label: `Backend send interval`
- Type: Constant
- Hide: Variable
- Value: `5` （JMeter InfluxdbBackendListenerClient 默认）

---

### Annotations

编辑 `Annotations & Alerts(Built-in)`

- Name: `Start/stop marker`
- Data source: `$data_source`
- Query: `select text from events where $timeFilter`

---

## 第1行

### 第1排

Singlestat - **`Total Requests`**, Span `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("count")  FROM "$measurement_name" WHERE ("application" =~ /^$application$/ AND "transaction" = 'all') AND $timeFilter GROUP BY time($__interval) fill(null)`
- Options
    - Value
        - Stat: Total
        - Postfix: ` Requests`
        - Decimals: `0`
    - Coloring: Value，把中间的颜色换成浅一点的黄色
- Value Mappings: null -> `0`

Singlestat - **`Failed Requests`**, Span `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("countError") FROM "$measurement_name" WHERE ("transaction" = 'all' AND "application" =~ /^$application$/) AND $timeFilter GROUP BY time($__interval) fill(null)`
- Options
    - Value
        - Stat: Total
        - Postfix: ` Failed`
        - Decimals: `0`
    - Coloring: Value，把中间的颜色换成红色
- Value Mappings: null -> `0`

Singlestat - **`Error Rate %`**, Span `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("error") / sum("all") FROM (SELECT sum("count") AS "all" FROM "$measurement_name" WHERE "transaction" = 'all' AND "application" =~ /^$application$/ AND $timeFilter GROUP BY time($__interval) fill(null)), (SELECT sum("countError") AS "error" FROM "$measurement_name" WHERE "transaction" = 'all' AND "application" =~ /^$application$/ AND $timeFilter GROUP BY time($__interval) fill(null))`
- Options
    - Value
        - Stat: Total
        - Unit: percent(0.0-1.0)
        - Decimals: `2`
    - Coloring: Value，Thresholds: `0,0.01`
    - Gauge: Show，Max: `1`
- Value Mappings: null -> `0`

### 第2排

Graph - **`Total Throughput`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT mean("count") / $send_interval FROM "$measurement_name" WHERE ("transaction" = 'all' AND "application" =~ /^$application$/) AND $timeFilter GROUP BY time($__interval) fill(null)`
    - alias: `Req / sec`
- Legend
    - As Table, Min, Max, Avg，Decimals: `2`
- Display
    - Lines，Fill: `7`, Null value: `null`

Graph - **`Total Errors`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("countError") FROM "$measurement_name" WHERE ("transaction" = 'all' AND "application" =~ /^$application$/) AND $timeFilter GROUP BY time($__interval) fill(null)`
    - alias: `Num of Errors`
- Axes
    - Decimals: `0`
- Legend
    - As Table, Total，Decimals: `0`
- Display
    - Lines，Fill: `7`, Null value: `null`

Graph - **`Active Threads`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT last("maxAT") FROM "$measurement_name" WHERE ("transaction" = 'internal' AND "application" =~ /^$application$/) AND $timeFilter GROUP BY time($__interval) fill(null)`
    - alias: `Threads`
- Axes
    - Decimals: `0`
- Legend
    - As Table, Current，Decimals: `0`
- Display
    - Lines，Fill: `7`, Null value: `null`

### 第3排

Graph - **`Transactions Response Times (95th pct)`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT mean("pct95.0") FROM "$measurement_name" WHERE ("statut" = 'ok' AND "application" =~ /^$application$/) AND $timeFilter GROUP BY "transaction", time($__interval) fill(null)`
    - alias: `$tag_transaction`
- Axes
    - Units: `milliseconds(ms)`
- Legend
    - As Table, To the right, Max, Avg，Decimals: `2`
- Display
    - Lines，Null value: `null`
    - Thresholds
        - T1: lt `500`, ok, Fill, Line
        - T2: gt `1500`, warning, Line
        - T3: gt `5000`, critical, Fill, Line

---

## 第2行

Table - **`Errors per Transaction`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("count") FROM "$measurement_name" WHERE ("application" =~ /^$application$/ AND "statut" = 'ko') AND $timeFilter GROUP BY "transaction"`
    - format: Table
- Column Styles
    - Time - Type: Hidden
    - /.*/ - Decimals: `0`

Table - **`Error Info`**, Span: `8`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("count") FROM "$measurement_name" WHERE ("application" =~ /^$application$/ AND "responseCode" !~ /^$/) AND $timeFilter GROUP BY "responseCode","responseMessage"`
    - format: Table
- Column Styles
    - Time: Type - Hidden
    - /.*/ : Decimals `0`

---

## 第3行

复制第1行的图表（除了线程图），改一下SQL和一些细节就行。

### 第1排

Singlestat - **`Total Requests - $transaction`**, Span `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("count") FROM "$measurement_name" WHERE ("application" =~ /^$application$/ AND "transaction" =~ /^$transaction$/ AND "statut" = 'all') AND $timeFilter GROUP BY time($__interval) fill(null)`
- Options
    - Value
        - Stat: Total
        - Postfix: ` Requests`
        - Decimals: `0`
    - Coloring: Value，把中间的颜色换成浅一点的黄色
- Value Mappings: null -> `0`

Singlestat - **`Failed Requests - $transaction`**, Span `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("count") FROM "$measurement_name" WHERE ("application" =~ /^$application$/ AND "transaction" =~ /^$transaction$/ AND "statut" = 'ko') AND $timeFilter GROUP BY time($__interval) fill(null)`
- Options
    - Value
        - Stat: Total
        - Postfix: ` Failed`
        - Decimals: `0`
    - Coloring: Value，把中间的颜色换成红色
- Value Mappings: null -> `0`

Singlestat - **`Error Rate % - $transaction`**, Span `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("error") / sum("all") FROM (SELECT sum("count") AS "all" FROM "$measurement_name" WHERE "transaction" =~ /^$transaction$/ AND "statut" = 'all' AND "application" =~ /^$application$/ AND $timeFilter GROUP BY time($__interval) fill(null)), (SELECT sum("count") AS "error" FROM "$measurement_name" WHERE "transaction" =~ /^$transaction$/ AND "statut" = 'ko' AND "application" =~ /^$application$/ AND $timeFilter GROUP BY time($__interval) fill(null))`
- Options
    - Value
        - Stat: Total
        - Unit: percent(0.0-1.0)
        - Decimals: `2`
    - Coloring: Value，Thresholds: `0,0.01`
    - Gauge: Show，Max: `1`
- Value Mappings: null -> `0`

### 第2排

Graph - **`Throughput - $transaction`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT last("count") / $send_interval FROM "$measurement_name" WHERE ("transaction" =~ /^$transaction$/ AND "statut" = 'ok') AND $timeFilter GROUP BY time($__interval)`
    - alias: `Req / sec`
- Legend
    - As Table, Min, Max, Avg，Decimals: `2`
- Display
    - Lines，Fill: `7`, Null value: `null`

Graph - **`Errors - $transaction`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT sum("count") FROM "$measurement_name" WHERE "application" =~ /^$application$/ AND "transaction" =~ /^$transaction$/ AND "statut" = 'ko' AND $timeFilter GROUP BY time($__interval) fill(null)`
    - alias: `Num of Errors`
- Axes
    - Decimals: `0`
- Legend
    - As Table, Total，Decimals: `0`
- Display
    - Lines，Fill: `7`
    - Points, Point Radius: `1`
    - Null value: `null`

### 第3排

Graph - **`Response Times - $transaction`**, Span: `4`

- Metric
    - Data Source: `$data_source`
        - Options - Min time interval: `[[send_interval]]s`
    - `SELECT last("avg") FROM "$measurement_name" WHERE ("transaction" =~ /^$transaction$/ AND "statut" = 'ok') AND $timeFilter GROUP BY time($__interval)`
        - alias: `Average`
    - `SELECT last("pct50.0") FROM "$measurement_name" WHERE ("transaction" =~ /^$transaction$/ AND "statut" = 'ok') AND $timeFilter GROUP BY time($__interval)`
        - alias: `Median`
    - `SELECT last("pct90.0") FROM "$measurement_name" WHERE ("transaction" =~ /^$transaction$/ AND "statut" = 'ok') AND $timeFilter GROUP BY time($__interval) fill(null)`
        - alias: `90th Percentile`
    - `SELECT last("pct95.0") FROM "$measurement_name" WHERE ("transaction" =~ /^$transaction$/ AND "statut" = 'ok') AND $timeFilter GROUP BY time($__interval) fill(null)`
        - alias: `95th Percentile`
    - `SELECT last("pct99.0") FROM "$measurement_name" WHERE ("transaction" =~ /^$transaction$/ AND "statut" = 'ok') AND $timeFilter GROUP BY time($__interval) fill(null)`
        - alias: `99th Percentile`
    - `SELECT last("max") FROM "$measurement_name" WHERE ("transaction" =~ /^$transaction$/ AND "statut" = 'ok') AND $timeFilter GROUP BY time($__interval) fill(null)`
        - alias: `Max`
- Axes
    - Units: `milliseconds(ms)`
- Legend
    - As Table, To the right
    - Max, Avg，Decimals: `2`
    - Hide Series: With only nulls
- Display
    - Lines，Null value: `null`
    - Thresholds
        - T1: lt `500`, ok, Fill, Line
        - T2: gt `1500`, warning, Line
        - T3: gt `5000`, critical, Fill, Line

---

导出的 JSON 文件没有 data source，无法直接导入，需要手动编辑文件，在 `"__inputs": []` 里加入以下：

```js
{
  "name": "JMETER_DASHBOARD",
  "label": "DB name",
  "description": "",
  "type": "datasource",
  "pluginId": "influxdb",
  "pluginName": "InfluxDB"
},
```

如果想上传到官网，为了能正确分类，`"__requires": []` 里还要加入以下：

```js
{
  "type": "datasource",
  "id": "influxdb",
  "name": "InfluxDB",
  "version": "1.4.0"
},
```

---

参考：

> https://grafana.com/dashboards/3351
