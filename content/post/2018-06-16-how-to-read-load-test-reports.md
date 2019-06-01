---
slug: how-to-make-reasonable-load-test-reports
title: '性能测试报告打脸指南'
date: 2018-06-16T18:08:12+08:00
draft: true
tags: ['JMeter', 'Gatling', 'nGrinder']
categories:
- 10000 Hours
- 性能测试基础
#author: ''

hiddenFromHomePage: false
hideHeaderAndFooter: false
#enableOutdatedInfoWarning: false
#reward: false
#comment: false
---

我们平时

下面举的例子主要是后端接口性能测试和中间件的 benchmark，不涉及前端或硬件的性能测试，虽然一些统计

## 1. 

![MySQL benchmark](http://keithmo.me/static/img/post/2018/06/mysql-official-benchmark.png)

https://www.mysql.com/why-mysql/benchmarks/

---

> Gil Tene 的 LatencyTipOfTheDay 系列, 2014-06：  
[You can't average percentiles. Period.](http://latencytipoftheday.blogspot.com/2014/06/latencytipoftheday-you-cant-average.html)  
[Average (def): a random number that falls somewhere between the maximum and 1/2 the median. Most often used to ignore reality.](http://latencytipoftheday.blogspot.com/2014/06/latencytipoftheday-average-random.html)  
[Measure what you need to monitor. Don't just monitor what you happen to be able to easily measure.](http://latencytipoftheday.blogspot.com/2014/06/latencytipoftheday-measure-what-you.html) （无内容，标题说明一切）  
[If you are not measuring and/or plotting Max, what are you hiding (from)?](http://latencytipoftheday.blogspot.com/2014/06/latencytipoftheday-if-you-are-not.html)  
[Q: What's wrong with this picture? 
A: Everything!](http://latencytipoftheday.blogspot.com/2014/06/latencytipoftheday-q-whats-wrong-with_21.html)  
[MOST page loads will experience the 99%'lie server response](http://latencytipoftheday.blogspot.com/2014/06/latencytipoftheday-most-page-loads.html)  

---

## 2. 

---

## 3. Coordinated Omission (CO)

> Youtube - ["How NOT to Measure Latency" by Gil Tene](https://www.youtube.com/watch?v=lJ8ydIuPFeU), 2015-09  
[Gil Tene — Understanding Latency and Application Responsiveness](https://www.youtube.com/watch?v=YpBwt-rO07o), 2016-10  



[Coordinated Omission (CO) - possible strategies](http://www.jmeter-archive.org/Coordinated-Omission-CO-possible-strategies-td5718456.html), 2013-10  



[这里](http://keithmo.me/static/test-results/benchmark/load-test-tools/jmeter/coordinatedOmission/dashboard)

