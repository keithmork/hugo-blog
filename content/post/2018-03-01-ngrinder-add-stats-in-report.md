---
slug: ngrinder-add-stats-in-report
title: 'nGrinder 改造 - 在详细报告里增加更多统计项'
date: 2018-03-01T02:25:25+08:00
draft: false
tags:
- nGrinder
categories:
- 10000 Hours
- 性能测试工具
---


### 背景

nGrinder 以轻量、容易上手、容易扩展的特点在众多性能测试工具中脱颖而出，受到不少公司追捧。其实它的缺点也相当明显，更适合作为交给开发自测的工具。

其中有一项几乎一票否决的重大缺陷就是报告里只统计了平均响应时间——凡是不按百分位数统计的工具都是耍流氓，也就能拿去忽悠人，对发现和定位问题帮助不大。

幸好网上已经有人给出了详细的改造方法：

> [nGrinder 项目剖析及二次开发](https://testerhome.com/topics/4225), neven7 (胡刚), 2016-02

下面的代码基本照搬自原帖，删了一些开发通常不感兴趣的字段，改了部分名字。在 nGrinder 3.4.1 测试通过。

如果懒得自己动手，打好的包在这里：https://github.com/keithmork/oh-my-ngrinder

---

首先，到官方repo https://github.com/naver/ngrinder 克隆源码到本地：

```sh
git clone git@github.com:naver/ngrinder.git
```

### 0. 添加依赖包

在 `ngrinder-core` 下的 `pom.xml` 中添加 commons-math3：

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-math3</artifactId>
    <version>3.4</version>
</dependency>
```

### 1. 修改 Controller 层

在 `ngrinder-core/src/main/java/net/grinder/SingleConsole.java` 中搜 `updateStatistics` 方法：

```java
// 添加成员变量：

private List<Double> responseTimeList = new CopyOnWriteArrayList<Double>();

// 修改这方法：
protected void updateStatistics(StatisticsSet intervalStatistics, StatisticsSet accumulatedStatistics) {
    // ...
    // 找到这里：
    for (Entry<String, StatisticExpression> each : getExpressionEntrySet()) {
        // ...
        // 添加以下内容，收集所有响应时间记录：
        
        if ("Mean_Test_Time_(ms)".equals(each.getKey())) {
            responseTimeList.add((Double) getRealDoubleValue(each.getValue()
                .getDoubleValue(intervalStatistics)));
        }
    }
    
    // ...
}
```

添加以下方法，统计各个百分位数的响应时间：

```java
private void getAdditionalStats() {
    if (LOGGER.isDebugEnabled()) {
        LOGGER.debug("getAdditionalStats() responseTimeList is {}", responseTimeList.toString());
    }

    // list to array
    int i = 0;
    double[] rtArray = new double[responseTimeList.size()];
    for (double responseTime : responseTimeList) {
        rtArray[i++] = responseTime;
    }

    Arrays.sort(rtArray);

    Percentile percentile = new Percentile();
    Map<String, Object> additionalStats = newHashMap();
    additionalStats.put("minRT", rtArray[0]);
    additionalStats.put("pct25RT", percentile.evaluate(rtArray, 25));
    additionalStats.put("pct50RT", percentile.evaluate(rtArray, 50));
    additionalStats.put("pct75RT", percentile.evaluate(rtArray, 75));
    additionalStats.put("pct90RT", percentile.evaluate(rtArray, 90));
    additionalStats.put("pct95RT", percentile.evaluate(rtArray, 95));
    additionalStats.put("pct99RT", percentile.evaluate(rtArray, 99));
    additionalStats.put("maxRT", rtArray[rtArray.length - 1]);

    LOGGER.debug("SingleConsole getAdditionalStats additionalStats {}", additionalStats);

    this.statisticData.put("additionalStats", additionalStats);
}
```

修改以下方法：

```java
public void unregisterSampling() {
    // ...
    // 在最后调用上面的方法，采样结束后处理数据：

    getAdditionalStats();
}
```

---

### 2. 修改 Model 层

编辑 `ngrinder-core/src/main/java/org/ngrinder/model/PerfTest.java`，添加以下字段：

```java
// followings are test result members
// ...

@Expose
@Column(name = "minRT")
private Double minRT;

@Expose
@Column(name = "pct25RT")
private Double pct25RT;

@Expose
@Column(name = "pct50RT")
private Double pct50RT;

@Expose
@Column(name = "pct75RT")
private Double pct75RT;

@Expose
@Column(name = "pct90RT")
private Double pct90RT;

@Expose
@Column(name = "pct95RT")
private Double pct95RT;

@Expose
@Column(name = "pct99RT")
private Double pct99RT;

@Expose
@Column(name = "maxRT")
private Double maxRT;

// 再加上对应的 getter 和 setter （IDEA 按 cmd + N，或右键 -> Generate...）
```

#### 修改 DB 保存字段

编辑 `ngrinder-controller/src/main/resources/ngrinder_datachange_logfile/db.changelog_schema_H2.xml`，在对应位置添加上面的字段：

```java
create table PERF_TEST (
    /* ... 
    peak_tps double, */
    errorRate double,
    minRT double,
    pct25RT double,
    pct50RT double,
    pct75RT double,
    pct90RT double,
    pct95RT double,
    pct99RT double,
    maxRT double,
    /*port integer,
    ... */
)
```

---

### 3. 修改 Service 层

在 `ngrinder-controller/src/main/java/org/ngrinder/perftest/service/PerfTestService.java` 中搜 `updatePerfTestAfterTestFinish` 方法，这里会从 SingleConsole 类中获取刚才的 statisticData。

在对应位置添加以下内容：

```java
public void updatePerfTestAfterTestFinish(PerfTest perfTest) {
    // ...
    //Map<String, Object> totalStatistics = MapUtils.getMap( ...
    
    @SuppressWarnings("unchecked")
    Map<String, Object> additionalStats = MapUtils.getMap(result, "additionalStats", MapUtils.EMPTY_MAP);
    
    //LOGGER.info("Total Statistics ...
    LOGGER.info("Additional Statistics for test {}  is {}", perfTest.getId(), additionalStats);
    
    // ...
    
    perfTest.setMinRT(parseDoubleWithSafety(additionalStats, "minRT", 0D));
    perfTest.setPct25RT(parseDoubleWithSafety(additionalStats, "pct25RT", 0D));
    perfTest.setPct50RT(parseDoubleWithSafety(additionalStats, "pct50RT", 0D));
    perfTest.setPct75RT(parseDoubleWithSafety(additionalStats, "pct75RT", 0D));
    perfTest.setPct90RT(parseDoubleWithSafety(additionalStats, "pct90RT", 0D));
    perfTest.setPct95RT(parseDoubleWithSafety(additionalStats, "pct95RT", 0D));
    perfTest.setPct99RT(parseDoubleWithSafety(additionalStats, "pct99RT", 0D));
    perfTest.setMaxRT(parseDoubleWithSafety(additionalStats, "maxRT", 0D));
}
```

---

### 4. 修改 View 层

编辑 `ngrinder-controller/src/main/webapp/WEB-INF/ftl/perftest/detail_report.ftl`，在对应位置添加：

```xml
<table class="table table-bordered compactpadding">
    <!-- ... -->
    <tr>
        <th><@spring.message "perfTest.report.errorRate"/></th>
        <td>${(test.errors /(test.tests + test.errors))!""}</td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.minRT"/></th>
        <td>${test.minRT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.pct25RT"/></th>
        <td>${test.pct25RT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.pct50RT"/></th>
        <td>${test.pct50RT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.pct75RT"/></th>
        <td>${test.pct75RT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.pct90RT"/></th>
        <td>${test.pct90RT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.pct95RT"/></th>
        <td>${test.pct95RT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.pct99RT"/></th>
        <td>${test.pct99RT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
    <tr>
        <th><@spring.message "perfTest.report.maxRT"/></th>
        <td>${test.maxRT!""}&nbsp;&nbsp; <code>ms</code></td>
    </tr>
</table>
```

---

### 5. 修改不同语言的UI显示文字

在 `ngrinder-controller/src/main/resources` 下找到以下文件，在对应位置添加如下内容：

messages_cn.properties

```java
// ...
//perfTest.report.errors=Errors
perfTest.report.errorRate=\u9519\u8bef\u7387
perfTest.report.minRT=\u6700\u5c0f RT
perfTest.report.pct25RT=25 \u767e\u5206\u4f4d\u6570 RT
perfTest.report.pct50RT=50 \u767e\u5206\u4f4d\u6570 RT
perfTest.report.pct75RT=75 \u767e\u5206\u4f4d\u6570 RT
perfTest.report.pct90RT=90 \u767e\u5206\u4f4d\u6570 RT
perfTest.report.pct95RT=95 \u767e\u5206\u4f4d\u6570 RT
perfTest.report.pct99RT=99 \u767e\u5206\u4f4d\u6570 RT
perfTest.report.maxRT=\u6700\u5927 RT
// ...
```

messages_en.properties

```java
// ...
//perfTest.report.errors=Errors
perfTest.report.errorRate=Error Rate
perfTest.report.minRT=Min RT
perfTest.report.pct25RT=25th Pct RT
perfTest.report.pct50RT=50th Pct RT
perfTest.report.pct75RT=75th Pct RT
perfTest.report.pct90RT=90th Pct RT
perfTest.report.pct95RT=95th Pct RT
perfTest.report.pct99RT=99th Pct RT
perfTest.report.maxRT=Max RT
// ...
```

---

### 6. 打包

有几个依赖的 jar 包在 Maven 仓库找不到，到这里下：

- https://github.com/naver/nhnopensource.maven.repo/tree/master/releases/grinder/grinder-patch/3.9.1-patch
- https://github.com/naver/nhnopensource.maven.repo/tree/master/releases/org/ngrinder/universal-analytics-java/1.0
- https://github.com/naver/nhnopensource.maven.repo/tree/master/releases/sigar/sigar-native/1.0

放到 `~/.m2/repository` 下对应的目录，如 `grinder/grinder-patch/3.9.1-patch`。

检查依赖：

```sh
mvn -Dmaven.test.skip=true dependency:analyze
mvn -Dmaven.test.skip=true dependency:resolve
```

如果包已经放到指定地方，IDEA 的项目设置（F4）里没报错，但 Maven 面板依然显示有依赖错误，重启 IDEA 试试。

没报错就打包：

```sh
mvn -Dmaven.test.skip=true clean package
```

找到 `ngrinder-controller/ngrinder-controller-<version>-SNAPSHOT.war`，拿去运行。

#### 注意

如果之前已经运行过原版 nGrinder，会在 `~/.ngrinder/db` 里保存了数据。由于表加了字段，跟原来的数据不兼容，启动会报错。

把 `~/.ngrinder/db` 目录删掉，重新运行程序就好。

---

### 效果

中文界面

![详细测试结果截图-中文](http://keithmo.me/img/post/2018/02/28/ngrinder-report-cn.jpg)

英文界面

![详细测试结果截图-英文](http://keithmo.me/img/post/2018/02/28/ngrinder-report-en.jpg)

