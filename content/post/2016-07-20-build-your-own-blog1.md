---
slug: build-your-own-blog1
title: "GitHub + Hugo 搭建个人博客"
date: 2016-07-20T02:16:04+08:00
draft: false
categories:
- 10000 Hours
- 环境搭建
tags:
- Hugo
- GitHub
- Git
---

更新：

- 2018-05-26：主题换成 Even，补充多处内容。
- 2018-02-11：主题换成 Kiera。

---

## 自己搭博客的N个理由

- 你的笔记非常值钱，丢了就等于丢了N年工作经验。
- 大多数博客网站都满足不了你：
    - 倒闭/停止服务/被删库时，你多半没备份。
    - 格式互不兼容，导入导出困难，复制粘贴会乱。
    - 富文本编辑器极其难用。
    - 可配置的地方不够，你讨厌的功能一堆。
- 可以使用 Markdown 写博文：
    - 任何一个文本编辑器都能写，随时随地。
    - 足够沉浸，让你忘记格式，专注内容。
    - 备份容易：
        - GitHub, GitBook, 有道云笔记，CSDN，博客园，简书等都支持，到处复制粘贴也能得到大致相同的格式。
        - 纯文本本身也有足够可读性，网站/软件不支持也无所谓。
- GitHub 提供免费空间和 CDN，站点放 GitHub 仓库也方便做版本控制。
    - 可以当有道云笔记的图片空间用，弥补 Markdown 笔记无法贴图的缺点。
- 可以作为学习新技术的试验田。

---

## 准备工作

### 注册 GitHub

先注册一个~~全球最大的同性交友网站~~ [GitHub](https://github.com/) 账号。  

- 用户名会成为网址的一部分，最好全英文小写，没空格，不太长。  
    - 例：用户名 `keithmork`，对应网址 `keithmork.github.io`

---

### 安装Git

#### Mac

```sh
brew install git
```

#### PC

下载安装 [Git](https://git-scm.com/) ，全用默认设置，然后打开 Git 终端。

- 默认工作目录是 Git 安装目录下的 mingw64 文件夹。
- 路径分隔符使用 `/` 或 `\\`

---

### 设置 Git 默认用户信息

```sh
git config --global user.name "keithmork"
git config --global user.email "keith.mork@gmail.com"

# 使用 GitHub 注册的用户名和邮箱
```

这台机器上的每个项目提交时默认都会用这些信息。

---

### 创建 GitHub Pages

在 GitHub 上新建仓库(repository)：  

- 名字**必须**为 *username*.github.io ，如：`keithmork.github.io`

只要往里面提交一个 `index.html` 文件，就能通过  https://keithmork.github.io 访问了。

不过除了前端，没几个人愿意碰 HTML 和 CSS，这么建站实在太2000年代了，生成器用起来。

---

## 使用静态网页生成器搭建站点

个人网站/博客通常只需要简单的静态页面，[这类工具](https://www.staticgen.com/) 正好可以把 Markdown 文档转换成网页，非常方便。

这里选择 [Hugo](http://gohugo.io/)，特点是生成速度非常快（[构建5000篇文章不用10秒](https://www.youtube.com/watch?v=CdiDYZ51a2o)），安装配置也不算复杂，适合不想折腾的人。

它最实用的就是实时预览功能，不需要发布到 Github 就能看到改动。

### 安装

#### Mac

```sh
brew install hugo

# 查看版本
hugo version

# 查看帮助
man hugo
hugo help
```

#### PC

下载后解压，把 `hugo.exe` 的路径加进环境变量 `Path` 就行。

---

### 新建站点

假设站点文件想放在当前目录的 `hugo-blog/` 目录下：

```sh
hugo new site -f yaml hugo-blog

# -f, --format {yaml|json|toml}    指定 config 和 front matter 的格式，默认 toml。
# 推荐 yaml 格式。
```

生成的目录结构如下：

```sh
$ cd hugo-blog
$ tree
.
├── archetypes  # 博文模板
│   └── default.md  # 默认模板文件
├── config.yaml  # 主配置文件
├── content  # 存放博文，里面的子目录叫做 section。
├── data  # 生成网站时的配置
├── layouts  # 网页框架，放在这里的文件会比主题里的同名文件优先，可以在不动主题源码的情况下覆盖部分设置。
├── static  # 静态资源，这目录下的文件会原封不动的拷到站点根目录下。
└── themes  # 主题
```

> https://gohugo.io/commands/hugo_new_site/  
> https://gohugo.io/getting-started/directory-structure/  

---

### 下载主题

默认不带主题，从 [这里](https://github.com/spf13/hugoThemes) 挑喜欢的下载。

推荐 [CR](http://www.b12.cn/article/NDI2MGIxMg.html)（黑白极简）风格的主题，我用的是 ~~[Steam](https://themes.gohugo.io/steam/)~~ ~~[Kiera](https://themes.gohugo.io/hugo-kiera/)~~ [Even](https://themes.gohugo.io/hugo-theme-even/) ：

```sh
git clone https://github.com/olOwOlo/hugo-theme-even themes/even
```

---

### 修改配置文件

编辑 config.yaml，默认值如下：

```yaml
baseURL: http://example.org/  # 替换为你的网址
languageCode: en-us  # 中文博客建议改为 zh-cn。如果文章内容不是这里指定的编码，Chrome 会弹出要不要翻译页面的提示。
title: My New Hugo Site  # 替换为你的博客标题
```

【重要】建议在配置文件里指定主题，否则每次构建都要通过命令行传參。

如果配置文件和命令行都不指定主题，打开网站只能看到一片空白。

```yaml
theme: even
```

常用设置：

```yaml
googleAnalytics: XXX  # Google Analytics ID
# 多数国外主题只支持 Google 的站点统计，Even 作为国人开发的主题，还支持不蒜子和百度统计。

defaultContentLanguage: zh-cn  # 默认 en，文章内容的默认编码，必须用 "-" 和小写。有些主题支持多语言，这里的设置会改变菜单和按钮的显示语言。
#defaultContentLanguageInSubdir: false  # 如果站点支持多语言，这选项为 true 时，/ 会重定向到对应的语言版本，如 /en/、/zh-cn/ 等。
hasCJKLanguage: true  # 文章内容是否有中日韩文，会影响字数统计。

enableEmoji: true  # 支持表情
enableGitInfo: true  # 开启后可以用 Git 提交时间作为文章最后修改时间。
enableRobotsTXT: true  # 自动生成给搜索引擎爬虫用的 robots.txt。如果你打算手写，可以不启用。
# https://gohugo.io/templates/robots/

metaDataFormat: yaml  # 文章开头的元数据（Hugo 称为 front matter）的格式
preserveTaxonomyNames: true  # 标签名是西欧字母时保留原文，不自动转为对应的英文字母

paginate: 10  # 分页长度，每页显示多少篇博文

# 元数据设置，各种日期的匹配规则。从数组第1个元素开始，匹配不上才到下一个。
frontmatter:
  date: ['date', ':filename', ':default']  # 名为 date 的变量、文件名开头 yyyy-mm-dd- 的前缀、默认
  lastmod: [':git', ':fileModTime', ':default']  # Git 提交时间、文件属性里的修改时间、默认


# https://gohugo.io/content-management/urls/#canonicalization
disablePathToLower: true  # URL 路径区分大小写
permalinks:
  post: /:sections/:year/:month/:day/:slug

# URL 格式，每个 key 对应 content/ 下的一级子目录。
# :sections 代表 content/ 下的目录结构，原样映射成 URL 路径。
# :year、:month、:day 都会从文件前缀 yyyy-mm-dd 里提取。
# :slug 在文章的 front matter 里定义，如果没定义会用文件名。
```

其他主题常用，但 Even 不需要的设置：

```yaml
disqusShortname: XXX  # Disqus 用户名
# 静态网站只能靠第三方插件实现评论功能，很多国外主题使用 Disqus，但国内访问有点慢，有时加载不出来。
# Even 支持 gitment，用 GitHub issue 当评论，不需要这个。


# 很多主题都使用默认的 Pygments 做语法高亮，设置如下。
# 颜色比较丑而且行号从来对不齐，这也是我放弃 Kiera 转向 Even 的原因之一。
# Even 用 highlight.js 做语法高亮，效果跟 Pygments 完全没得比。

# https://gohugo.io/content-management/syntax-highlighting/
pygmentsCodeFences: true
pygmentsCodeFencesGuessSyntax: true
# https://help.farbox.com/pygments.html
pygmentsStyle: friendly  # {monokai | friendly | vs | autumn | ...}
#pygmentsOptions: "linenos=inline"  # {inline | table}
```

除了 Hugo 本身的参数，每个主题都有自己特有的参数，请参考相应主题的官方文档。

Even 可以看看官方的 [配置文件示例](https://github.com/olOwOlo/hugo-theme-even/blob/master/exampleSite/config.toml) 和我的 [笔记](http://keithmo.me/post/2018/05/27/custom-hugo-even-theme/)。

> https://gohugo.io/getting-started/configuration/

---

### 编辑默认模板

在 `archetype/` 下新建 .md 文件，如果文件名跟新建文章时的 SECTIONNAME 一致，则该 section 下的文章都会套用这模板。

找不到匹配的 section 才会套用 `default.md`。

`default.md` 的默认 front matter：

```yaml
---
title: '{{ replace .Name "-" " " | title }}'
date: {{ .Date }}
draft: true
---
```

添加常用 front matter（加在分隔符之间）：

```yaml
slug: {{ .TranslationBaseName }}  # URL 路径的最后一部分
# .TranslationBaseName 为不带语言标识符的文件名。比如文件名为 foo.en.md，得到 foo
tags: []
categories: []
```

另外 Even 主题还配置了一些特有的参数，参见它的 [默认模板](https://github.com/olOwOlo/hugo-theme-even/blob/master/archetypes/default.md)。

> https://gohugo.io/content-management/archetypes/  
> https://gohugo.io/variables/files/   

---

### 调整样式

如果对主题某些地方不满意，又不想直接改主题源文件，可以去 `themes/<主题名>/layouts/` 看看都有什么文件，把要改的拷到 `layouts/` 下。

文件查找顺序大致如下，同名文件在 `layouts/` 下的比主题里的优先，匹配到 section 名的比 _default 优先。

```sh
layouts/<section>/...
themes/<主题名>/layouts/<section>/...
layouts/_default/...
themes/<主题名>/layouts/_default/...
```

> https://gohugo.io/templates/lookup-order/#examples-layout-lookup-for-regular-pages

---

## 写博客

### 新建博文

```sh
hugo new post/2016-07-19-first.md

# 格式：<SECTIONNAME>/<FILENAME>.<FORMAT>
```

可以看到在 `content/` 下生成了 `post/` 目录，`post/` 下有 `2016-07-19-first.md` 文件。

推荐文件名加上日期前缀，因为通常所有博文都放在同一个目录下（这样配置最简单），带日期一眼就能区分新旧文章。

**这操作经常用，建议写成脚本**：（例如叫做 `new`）

- 自动加上当天日期做前缀
- section 名默认叫 `post`
- 因为上面在配置文件里已经设置了把日期前缀提取到 URL 路径，front matter 里的 slug 去掉前缀。

```sh
#!/bin/bash

[[ -z "$1" ]] && echo "Usage: $(basename "$0") FILENAME(without suffix) [SECTIONNAME]" && exit 1
section_name=${2:-post}

date_prefix=$(date +%Y-%m-%d-)
file_path="${section_name}/${date_prefix}$1.md"
hugo new "${file_path}"

sed -i '.bak' "s/slug: ${date_prefix}/slug: /" "content/${file_path}"
rm -f "content/${file_path}.bak"

# Mac 的 sed -i 的第1个参数必须为备份文件后缀名
```

### 编辑博文

用文本编辑器打开刚才生成的文件，可以看见类似如下内容（YAML格式）：

```yaml
---
title: "2016 07 19 First"
date: 2018-02-10T23:16:02+08:00
draft: true
---
```

（或默认的 TOML 格式）：

```toml
+++
date = "2016-07-19T00:12:34+08:00"
draft = true
title = "2016 07 19 first"

+++
```

像这样的出现在每篇文章前的元数据叫 **front matter**，`---` 之间包着的内容会解析为 YAML，`+++` 之间包着的内容会解析为 TOML。

在下面的空白处用 [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) 格式写正文，如：

```md
## Hello Hugo

坚持写博客的好处：

- 记录心得
- 整理思路
- 分享交流
- 求职展示
```

完整文件见[这里](https://raw.githubusercontent.com/keithmork/hugo-blog/master/content/post/2016-07-19-first.md)，渲染成 HTML 的效果见 [这里](http://keithmo.me/post/2016/07/19/first/)。

> https://gohugo.io/content-management/front-matter/  

---

### 资源文件

放到 `static/` 下，这目录下的所有文件和目录都会原封不动的拷到站点根目录下。

我习惯用的目录结构：

```sh
.
├── CNAME
├── css  # CSS 文件目录
├── googleXXX.html  # Google Analytics 的验证网页
├── img  # 图片目录
│   ├── reward  # 特定的主题专门建目录
│   │   ├── alipay.jpg  # 支付宝收款二维码
│   │   └── wechat.png  # 微信赞赏二维码
│   └── post  # 一般的博文配图按年月日建目录
│       └── 2018
│           └── 02
│              └── 28
├── js  # JS 脚本目录
└── robots.txt
```

【注意】

- 博文里引用图片时要写完整 URL，前面是实际的域名，后面是相对 `static/` 目录的路径。
    - 例：`![微信打赏](http://keithmo.me/img/wechat_reward_qrcode.png)`
- Hugo 不会帮你压缩图片，也不提供缩略图，建议提交前用工具处理图片。
- 不重要的图可以压成 70% jpg，宽度缩到 600px。颜色少可以试试 8色 png。
- 命令行工具有 `imagemagick` 用来裁剪和压缩图片，转格式等，`exiftool` 用来去掉 EXIF 信息。

---

### 本地预览

运行 `hugo server`，就会启动一个很简单的 HTTP 服务器。浏览器打开 `localhost:1313` 就能看到生成的网页，样式跟发布到线上完全相同。

之后文件有任何改动都会自动重新构建和刷新浏览器页面。

【注意】

- 运行 `hugo server` 时，当前工作目录必须为站点根目录（包含配置文件），否则提示 `Error: Unable to locate Config file.`
- Hugo 每次生成站点时不会删旧文件，因此推荐把预览和发布目录分开，并且每次生成前把旧目录删除。

**这操作经常用，建议写成脚本**：（例如叫做 `dev_preview`）

```sh
#!/bin/bash

[[ -d dev ]] && rm -rf dev
hugo server --buildDrafts --destination dev --disableFastRender

# -D, --buildDrafts[=false]    文章的默认状态是草稿，草稿默认不会构建，必须加上这参数才会生成页面。
# -w, --watch    文件有改动时自动重新构建并刷新浏览器页面。（默认就带这个，不传也行）
# -d, --destination DIR    输出目录。不传这参数的话构建出来的文件只会放在内存里。
# --disableFastRender    有改动时触发完整构建。反正 Hugo 非常快，几百毫秒根本感觉不到。
```

`ctrl + c` 结束 hugo server 。

---

### 生成发布页面

首先把要发布的文章的 `draft` 属性改为 `false`，运行 `hugo`。

执行后会生成 `public/` 目录，可以看到之前的 Markdown 文件转换成了文件夹 + HTML 文件。

**这操作经常用，建议写成脚本**：（例如叫做 `release`）

```sh
#!/bin/bash

push_to_github=${1:-false}

if [[ -d public ]]; then
    GLOBIGNORE=*.git
    rm -rf -v public/*
fi

hugo

if [[ "${push_to_github}" == "true" ]]; then
    cd public
    git add -A
    git commit -m 'new publish'
    git push
fi
```

再用 `hugo server` 检查一下有没有 draft 忘了改，确定文章能看到就可以发布了。

这步也可以写成脚本：（例如叫做 `preview`）

```sh
#!/bin/bash

hugo server --disableFastRender
```

本地图片由于还没上传，肯定全是叉。介意的话可以先把图传上去，验证过路径都写对了才发布网页。

---

## 发布到 GitHub

可以装 GitHub Desktop 或 SourceTree 等客户端，或者直接命令行：

```sh
cd public
git init
git remote add origin "git@github.com:keithmork/keithmork.github.io.git"  # 替换成你的 GitHub Pages 仓库地址
git add -A
git commit -m "first commit"
git pull
git push -u origin master

# 注意：要用 SSH 的仓库地址才能用公钥，如果用了 HTTPS 的仓库地址，必须每次输用户名密码。
```

之后会提示输入在 GitHub 注册的邮箱和密码。

提交成功后，浏览器打开`https://keithmork.github.io`，就能看到刚才的页面了。

第一次麻烦点，之后每次提交都很简单：（已经写在上面的 release 脚本里，参数传 true 就会提交和发布）

```sh
git add -A
git commit -m "XXX"
git push
```

---

## 使用 SSH 密钥登录 GitHub

每次发博文都输用户名密码太麻烦，用密钥代替密码就方便多了。

前提是机器只有你一个人用。

### 生成SSH密钥对

```sh
mkdir ~/.ssh
cd ~/.ssh
ssh-keygen -t rsa -C "keith.mork@gmail.com"
```

- 提示 `Enter file in which to save the key` 时直接回车，使用默认设置。
    - 文件名不用改，GitHub 连接时只认 `id_rsa`
- 提示 `Enter passphrase` 和 `Enter same passphrase again` 时，可以直接回车，不使用口令（否则每次提交时会要求输入口令）。

> https://help.github.com/articles/generating-an-ssh-key/

---

### 添加 SSH 公钥到 GitHub

1. 复制公钥文件（默认 `~/.ssh/id_rsa.pub`）的内容。
2. 登上 GitHub，在个人设置里找到 `SSH and GPG keys`，新建 SSH key，粘贴进去。
3. 取个容易识别的名字，如 `Mac-Home` `PC-Work` 等，保存。
4. 测试是否成功：

```sh
ssh -T git@github.com  # 用户名就是git，不用改。
```

```sh
# 如果报错，这样看详细信息：
ssh -vT git@github.com

# 看到以下讯息就是成功了：（虽然命令返回 1）
# You've successfully authenticated, but GitHub does not provide shell access.
```

看到 `Are you sure you want to continue connecting` 时输 `yes` 回车，然后就可以了。

如果依然每次都问用户名密码，可能是当初加仓库地址时用了 HTTPS 格式，改为 SSH 格式的地址就好了：

```sh
git remote set-url origin git@github.com:keithmork/keithmork.github.io.git
```


---

## 发布 Hugo 工程源文件到 GitHub

源文件比生成的发布文件重要得多，必须备份到 GitHub。发布文件丢了随时重新生成，源文件丢了就没了。

- 在 GitHub 新建仓库，例如叫 `hugo-blog`，不要勾选创建 README.md 。
- 在工程根目录下创建 `.gitignore` 文件，写上不需要备份的目录和文件，例：

```sh
public
themes
dev
.git
.DS_Store
*.bak
*_bak
*.old
*.log
```

- 如果想写项目简介，创建 README.md 文件。
- 和之前类似的操作：

```sh
git init
git remote add origin "git@github.com:keithmork/hugo-blog.git"  # 替换成你的 GitHub 仓库地址
git add -A
git commit -m "first commit"
git pull
git push -u origin master
```

如果忘了写 `.gitignore`，把 public 和 themes 也提交了上去，会发现它们被认为是 submodule （因为里面有 .git 目录）。即使之后设置忽略它们，每次里面的文件有更新时都会出现烦人的子模块状态变更的记录。

解决方法：先把那2个目录复制到别的地方，把它们删掉，写好 `.gitignore`，在 `.git/config` 里删掉 `[submodule]` 相关内容，提交，再把它们搬回来。

---

## 结束语

到这里，一个属于自己的静态博客基本成型了。

可能样式、功能或别的细节不能完全令人满意，但那些基本不影响写文章，可以先专注于输出内容，其他留到以后慢慢优化。

---

## 参考

> [基于GitHub Pages + Hugo构建个人博客](https://zhuzhangliang.github.io/post/hugo/), 2018-03   
> [GitHub Pages](https://pages.github.com/) (`User or organization site`下面有手把手教程)  
> http://gohugo.io/overview/usage/  
> [使用hugo搭建个人博客站点](http://blog.coderzh.com/2015/08/29/hugo/), 2015-08    
> [Hugo 对比 Jekyll ：两大领先的静态页面生成器之间的比较](https://linux.cn/article-8633-1.html), 2017-06
