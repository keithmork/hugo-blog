---
slug: custom-hugo-even-theme
title: 'Hugo Even 主题的配置和改造'
date: 2018-05-27T18:20:33+08:00
draft: true
tags:
- blog
- Hugo
- hugo-even-theme
categories:
- 10000 Hours
- 环境搭建
series: 
- 如何搭建技术博客

hiddenFromHomePage: false
hideHeaderAndFooter: false
#reward: false
#comment: false
---

这周末又折腾起了很久没碰的博客，

https://imsun.net/posts/gitment-introduction/

https://www.codestudies.net/hugo/recipes/relative-age-hugo/

https://gohugo.io/categories/functions

https://golang.org/pkg/text/template/
---

:git  用 .GitInfo.AuthorDate

https://gohugo.io/variables/git/

git有个问题是反映的是生成的html文件的更新时间，换了主题或改了设置都会让生成的html不一样，但文章内容没变

page：
lastmod

.Lastmod

https://gohugo.io/variables/page/

:fileModTime

Fetches the date from the content file’s last modification timestamp.

http://gohugo.io/getting-started/configuration/

文件修改时间有个问题是换电脑或从这里拷到那里，修改时间全变了


`.Lastmod` 和 `.GitInfo.AuthorDate` 二者取小，作为文章的更新时间

---

go template

变量定义域只在 if with range 范围内

---

https://www.google.com/webmasters/verification/details

Search Console -> 添加网站 -> 点进去右上角齿轮 -> Verification Details

建议选 Meta tag

如果主题不支持，选 HTML file，把文件放到 static 下

---

https://gohugo.io/templates/taxonomy-templates/
https://gohugo.io/templates/lists/

首页 
    {{ $paginator := .Paginate (where (where .Data.Pages "Type" "in" "post") ".Params.hiddenfromhomepage" "!=" true) }}

不要用 "Section"，直接放在content下的md文件也会生成空分页，只是没有summary.html看不见。把分页设成1就能发现空页面。Type没这问题。

---

当初看到老帖子在右手边有点不适应，以为作者喜欢这样，一看 layouts/post/single.html，`{{ with .NextInSection }}` 和 `{{ with .PrevInSection }}` 的位置写反了，跟里面的文字和class名都对不上。

```go
      <!-- Post Pagination -->
      <nav class="post-nav">
        {{ with .NextInSection }}
          <a class="prev" href="{{ .URL }}">
            <i class="iconfont icon-left"></i>
            <span class="prev-text nav-default">{{ .Title }}</span>
            <span class="prev-text nav-mobile">{{ T "prevPost" }}</span>
          </a>
        {{- end }}
        {{ with .PrevInSection }}
          <a class="next" href="{{ .URL }}">
            <span class="next-text nav-default">{{ .Title }}</span>
            <span class="prev-text nav-mobile">{{ T "nextPost" }}</span>
            <i class="iconfont icon-right"></i>
          </a>
        {{- end }}
      </nav>
```

改为

```go
      <!-- Post Pagination -->
      <nav class="post-nav">
        {{ with .PrevInSection }}
          <a class="prev" href="{{ .URL }}">
            <i class="iconfont icon-left"></i>
            <span class="prev-text nav-default">{{ .Title }}</span>
            <span class="prev-text nav-mobile">{{ T "prevPost" }}</span>
          </a>
        {{- end }}
        {{ with .NextInSection }}
          <a class="next" href="{{ .URL }}">
            <span class="next-text nav-default">{{ .Title }}</span>
            <span class="next-text nav-mobile">{{ T "nextPost" }}</span>
            <i class="iconfont icon-right"></i>
          </a>
        {{- end }}
      </nav>
```

src/css/_partial/_post/_footer.scss 目前无论 pre-text 还是 next-text 都没定义

---

## 增加内容过时提醒

> [How to show the relative age of Hugo content in real time?](https://www.codestudies.net/hugo/recipes/relative-age-js/), 2018-01  
> 

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
{{ $paginator := .Paginate .Data.Pages.ByDate.Reverse .Site.Params.archivePaginate }}
```

> https://glennmccomb.com/articles/how-to-build-custom-hugo-pagination/  
> https://discourse.gohugo.io/t/whats-the-difference-between-site-pages-and-data-pages/2252

static/img/reward 里有作者的收款码，你不在 static 里放同路径同名文件，构建时会把作者的码放进去

gitment的回调地址要写对，包括https和http
登录授权后每篇文章下都要初始化才能评论


http://rs.luminousspice.com/hugo-site-search/
