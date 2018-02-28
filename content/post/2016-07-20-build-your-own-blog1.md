---
slug: build-your-own-blog1
title: "GitHub + Hugo 搭建个人博客"
date: 2016-07-20T02:16:04+08:00
draft: false
categories:
- 10000 Hours
tags:
- Hugo
- GitHub
- Git
---

最后更新：

- 2018-02-11

---

## 自己搭博客的N个理由

- 你的笔记非常值钱，丢了就等于丢了N年工作经验。
- 大多数博客网站都满足不了你：
    - 倒闭/停止服务/被删库时，你多半没备份。
    - 格式互不兼容，导入导出困难，复制粘贴会乱。
    - 编辑器极其难用。
    - 可配置的地方不够，你讨厌的功能一堆。
- 可以使用 Markdown 写博文：
    - 任何一个文本编辑器都能写，随时随地。
    - 足够沉浸，让你忘记格式，专注内容。
    - 备份容易：
        - CSDN、博客园、简书、有道云笔记等都支持，到处复制粘贴也能得到大致相同的格式。
        - 纯文本本身也有足够可读性，网站/软件不支持也无所谓。
- GitHub 提供免费空间和 CDN，站点放 GitHub 仓库也方便做版本控制。
    - 可以当有道云笔记的图片空间用，弥补 Markdown 笔记无法贴图的缺点。
- 可以作为学习新技术的试验田。

---

## 0. 准备工作

### 0.1. 注册 GitHub

先注册一个~~全球最大的同性交友网站~~ [GitHub](https://github.com/) 账号。  

- 用户名会成为网址的一部分，最好全英文小写，没空格，不太长。  
    - 例：用户名 `keithmork`，对应网址 `keithmork.github.io`

---

### 0.2. 安装Git

#### Mac

```sh
brew install git
```

#### PC

下载安装 [Git](https://git-scm.com/) ，全用默认设置，然后打开 Git 终端。

- 默认工作目录是 Git 安装目录下的 mingw64 文件夹。
- 路径分隔符请使用 `/` 或 `\\`

---

### 0.3. 设置 Git 默认用户信息

```sh
git config --global user.name "keithmork"
git config --global user.email "keith.mork@gmail.com"

# 替换成你在 GitHub 注册的邮箱和用户名
```

这台机器上的每个项目提交时默认都会用这些信息。

---

### 0.4. 创建 GitHub 个人主页

在 GitHub 上新建仓库(repository)  

- 名字**必须**为 *username*.github.io ，如：`keithmork.github.io`

只要往里面提交一个 `index.html` 文件，就能通过  https://keithmork.github.io 访问了。  

不过先不急，继续往下看。


---

## 1. 使用静态网站生成器搭建站点

个人网站/博客通常只需要简单的静态页面，[这类工具](https://www.staticgen.com/) 正好可以把 Markdown 文档转换成网页，非常方便。

这里选择 [Hugo](http://gohugo.io/)，特点是生成速度非常快，安装配置简单，还带实时预览。

### 1.1. 安装

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

### 1.2. 新建站点

假设想放在当前目录的 `hugo-blog/` 下：

```sh
hugo new site hugo-blog -f yaml

# -f, --format {yaml | json | toml}    指定 config 和 front matter 的格式，默认 toml。
# 推荐 yaml 格式。

cd hugo-blog
```

会在目录下生成如下内容：

- `archetypes/`：文章模板
- `content/`：网站内容
- `data/`：生成网站时的配置
- `layouts/`：网页框架
- `static/`：静态资源
- `themes/`：主题
- `config.yaml`：配置文件

> https://gohugo.io/commands/hugo_new_site/  
> https://gohugo.io/getting-started/directory-structure/  

---

### 1.3. 下载主题

默认不带主题，从 [这里](https://github.com/spf13/hugoThemes) 挑喜欢的下载。

推荐 [CR](http://www.b12.cn/article/NDI2MGIxMg.html)风格（黑白极简）的主题，我用的是 [Kiera](https://themes.gohugo.io/hugo-kiera/) ：

```sh
cd themes
git clone https://github.com/avianto/hugo-kiera.git kiera

cd ..
```

---

### 1.4 修改配置文件

以 YAML 格式为例：

```yaml
# 默认会有这3项：
baseURL: http://example.org/  # 替换为你的网址
languageCode: zh-cn  # 默认 en-us，如果文章内容不是这里指定的编码，Chrome 会提示要不要翻译页面。
title: My New Hugo Site  # 替换为你的博客标题

# 推荐在配置文件里指定主题，否则每次都要通过命令行传參。
# 如果配置文件和命令行都不指定主题，打开网站只能看到一片空白。
theme: kiera

# 代码语法高亮设置：
pygmentsCodeFences: true
pygmentsCodeFencesGuessSyntax: true
pygmentsStyle: friendly

# 下面是 kiera 的配置。每款主题的配置都不一样，照着主题的文档的指示做就行。

params:
  tagline: 这里替换成你的博客的副标题

# 设置社交账号，显示为链接按钮。
author:
  name: Keith Mo       # Author name
  github: keithmork     # Github username
  #gitlab:     # Gitlab username
  #linkedin:   # LinkedIn username
  #facebook:   # Facebook username
  #twitter:    # Twitter username
  #instagram:  # Instagram username

# 有对应的账号才能用相应功能：

# 评论（Disqus）
disqusShortname: XXX  # Disqus shortname
# 站点统计（Google Analytics）
googleAnalytics: XXX  # Google Analytics ID

```

参考[我的完整配置](https://github.com/keithmork/hugo-blog/blob/master/config.yaml)。

> https://gohugo.io/getting-started/configuration/

---

### 1.5. 创建页面文件

```sh
hugo new post/2016-07-19-first.md

# 格式：<SECTIONNAME>/<FILENAME>.<FORMAT>
```

可以看到在 `content/` 下生成了 `post/` 目录，`post/` 下有 `2016-07-19-first.md` 文件。   

**这操作经常用，建议写成脚本**：（例如叫做 `new`）

```
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

用文本编辑器打开刚才生成的文件，可以看见类似如下内容（YAML格式）：

```yaml
---
title: "2016 07 19 First"
date: 2018-02-10T23:16:02+08:00
draft: true
---
```

（默认的 TOML 格式）：

```toml
+++
date = "2016-07-19T00:12:34+08:00"
draft = true
title = "2016 07 19 first"

+++
```

像这样的出现在每篇文章前的前置配置叫 **front matter**，`---` 包着的内容会解析为 YAML，`+++` 包着的内容会解析为 TOML。

在下面的空白处用 Markdown 写正文，如：

```md
## Hello Hugo

坚持写博客的好处：

- 记录心得
- 整理思路
- 分享交流
- 求职展示
```

完整文件见[这里](https://raw.githubusercontent.com/keithmork/hugo-blog/master/content/post/2016-07-19-first.md)。

> https://gohugo.io/content-management/front-matter/  

---

### 1.6. 编辑模板

在 `archetype/` 下新建 .md 文件，如果文件名跟新建文章时的 SECTIONNAME 一致，则该 section 下的文章都套用这模板。

找不到匹配的 section 才会套用 `default.md`。

`default.md` 的默认 front matter：

```yaml
---
title: '{{ replace .Name "-" " " | title }}'
date: {{ .Date }}
draft: true
---
```

如果希望每篇新建的文章都带上特定的 front matter，写在2个 `---` 之间，如：

```yaml
slug: {{ .Name }}
tags: []
categories: []
```

如果想带上特定内容，写在 front matter 下面，如：

```md
![微信收钱二维码](/img/wechat-receive-money-qrcode-0.01.jpg)
```

资源文件放在 `static/` 下，==注意==：

- 建议用 imagemagick 压缩图片，Hugo 不会帮你压缩。
- kiera 主题要求图片宽度至少 600px，不够的会自动拉伸。如果图片确实很小，建议编辑一下加上透明底。

参考[我的配置](https://github.com/keithmork/hugo-blog/tree/master/archetypes)。

> https://gohugo.io/content-management/archetypes/  

---

### 1.7. 本地预览

Hugo 每次生成站点时不会删旧文件，因此推荐把预览和发布目录分开，并且每次生成前把旧目录删除。

```sh
hugo server -Dw -d dev 

# -D, --buildDrafts[=false]    文章的默认状态是草稿，草稿默认不会构建，必须加上这参数才会生成页面。
# -w, --watch    文件有改动时自动重新构建并刷新浏览器页面。
# -d, --destination DIR    输出目录，默认 public/
```

==注意==：运行 `hugo server` 时，当前工作目录必须为站点根目录（包含配置文件），否则提示 `Error: Unable to locate Config file.`

**这操作经常用，建议写成脚本**：（例如叫做 `preview`）

```sh
#!/bin/bash

rm -rf dev
hugo server --buildDrafts --destination dev --disableFastRender
```

浏览器打开 `localhost:1313`，之后任何改动都会实时刷新，样式跟发布到线上完全相同。  

`ctrl + c` 结束 hugo server 。

---

### 1.8. 生成发布页面

首先把要发布的文章的 `draft` 属性改为 `false`，运行：

```sh
hugo
```

执行后会生成 `public/` 目录，可以看到之前的 Markdown 文件转换成了文件夹 + HTML 文件。

**这操作经常用，建议写成脚本**：（例如叫做 `release`）

```sh
#!/bin/bash

push_to_github=${1:-false}

if [[ -d public ]]; then
    GLOBIGNORE=*.git:*CNAME:*googleXXX.html:*robots.txt
    rm -rf -v public/*
fi

hugo

if [[ "${push_to_github}" == "true" ]]; then
    cd public
    git add -A
    git commit -m 'new publish'
    git push
fi


# googleXXX.html 是 Google Analytics 要求你放在站点根目录用来验证你对站点的所有权的文件。
```

---

### 1.9. 发布到 GitHub

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

以后每次提交都这样：（已经写在上面的发布脚本里）

```sh
git add -A
git commit -m "XXX"
git push
```

或者装 SourceTree、GitHub Desktop 等客户端。

---

## 2. 使用 SSH 密钥登录 GitHub

每次发博文都输用户名密码太麻烦，用密钥代替密码就方便多了。

前提是机器只有你一个人用。

### 2.1. 生成SSH密钥对

```sh
mkdir ~/.ssh
cd ~/.ssh
ssh-keygen -t rsa -C "keith.mork@gmail.com"
```

- 提示 `Enter file in which to save the key` 时直接回车，使用默认设置。
    - 文件名不用改，GitHub 连接时似乎只认 `id_rsa`
- 提示 `Enter passphrase` 和 `Enter same passphrase again` 时，可以直接回车，不使用口令（否则每次提交时会要求输入口令）。

> https://help.github.com/articles/generating-an-ssh-key/

---

### 2.2. 添加 SSH 公钥到 GitHub

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

# 看到以下就是成功了：（虽然命令返回 1）
# You've successfully authenticated, but GitHub does not provide shell access.
```

- 看到 `Are you sure you want to continue connecting` 时输 `yes` 回车，然后就可以了。

如果依然每次都问用户名密码，可能是当初加仓库地址时用了 HTTPS 格式，改为 SSH 格式的地址就好了：

```sh
git remote set-url origin git@github.com:keithmork/keithmork.github.io.git
```


---

## 3. 发布 Hugo 工程源文件到 GitHub

源文件比生成的发布文件重要得多，必须备份到 GitHub。发布文件丢了随时重新生成，源文件丢了就没了。

1. 在 GitHub 新建仓库，例如叫 `hugo-blog`，不要勾选创建 README.md 。

2. 在工程根目录下创建 `.gitignore` 文件，写上不需要备份的目录和文件，例：

```sh
public
themes
dev
.git
.DS_Store
*.bak
*.old
```

3. 如果想写项目简介，创建 README.md 文件。

4. 和之前类似的操作：

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

> [GitHub Pages](https://pages.github.com/) 的`User or organization site`下面有手把手教程  
> http://gohugo.io/overview/usage/  
> [使用hugo搭建个人博客站点](http://blog.coderzh.com/2015/08/29/hugo/), 2015-08    
> [Hugo 对比 Jekyll ：两大领先的静态页面生成器之间的比较](https://linux.cn/article-8633-1.html), 2017-06
> 
> ---

![微信打赏二维码](http://keithmo.me/img/wechat_reward_qrcode.png)

