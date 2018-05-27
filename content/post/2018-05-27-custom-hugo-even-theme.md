---
slug: custom-hugo-even-theme
title: 'Hugo Even 主题的配置和改造'
date: 2018-05-27T18:20:33+08:00
draft: true
tags:
- blog
- Hugo
categories:
- 10000 Hours
- 环境搭建
#author: ''

hiddenFromHomePage: false
hideHeaderAndFooter: false
#oldInfoWarning: false
#reward: false
#comment: false
---

```yaml

defaultContentLanguage: zh-cn  # Even主题的i18n下的文件是 zh-CN，这里必须写成 zh-cn，否则hugo不认
```

Even 主题特有：

```yaml


timeago.js

要写成 zh_CN 才有中文,写死了

https://github.com/hustcc/timeago.js/blob/master/src/locales.js
```

https://cnodejs.org/topic/576d3463d3baaf401780bb48

---


### 修改样式

layouts 下创建 _default ，从 themes/even/layouts/_复制 default taxonomy.html 和 section.html 过来

taxonomy.html 第4行改成：（去掉 where 和后面的参数，所有section的页面都传给 .Paginate）

```sh
{{ $paginator := .Paginate (.Data.Pages.ByDate.Reverse) .Site.Params.archivePaginate }}
```

> https://glennmccomb.com/articles/how-to-build-custom-hugo-pagination/  
> https://discourse.gohugo.io/t/whats-the-difference-between-site-pages-and-data-pages/2252

static/img/reward 里有作者的收款码，你不在 static 里放同路径同名文件，构建时会把作者的码放进去
