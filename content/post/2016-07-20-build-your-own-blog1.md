---
categories: ["10000 hours"]
date: 2016-07-20T02:16:04+08:00
draft: false
series: ["用 GitHub 搭建个人博客"]
tags: ["Hugo", "GitHub", "Git"]
title: "GitHub + Hugo 搭建个人博客(1) - 发布第一篇博文"
---

## 自己搭博客的N个理由

- 你的笔记非常值钱，丢了就等于丢了N年工作经验
- 大多数博客网站都满足不了你
    - 倒闭/停止服务/被删库时，你多半没备份
    - 格式互不兼容，导入导出困难，复制粘贴会乱
    - 编辑器极其难用
    - 可配置的地方不够，你讨厌的功能一堆
- 可以使用Markdown写博文
    - 任何一个文本编辑器都能写，随时随地
    - 足够沉浸，让你忘记格式，专注内容
    - 备份容易
        - CSDN、博客园、简书、有道云笔记等都支持，到处复制粘贴也能得到大致相同的格式
        - 纯文本本身也有足够可读性，网站/软件不支持也无所谓
- GitHub提供免费空间和CDN，站点放GitHub仓库也方便做版本控制
- 可以作为学习新技术的试验田


---

## 准备工作

### 注册GitHub

行内没几个人还没有 [GitHub](https://github.com/) 账号了吧  
~~很惭愧，我所有的提交记录全是博文……~~

- 用户名会成为网址的一部分，最好全英文小写，没空格，不太长  
  例：用户名`keithmork`，对应网址`keithmork.github.io`

---

### 安装Git

#### Mac

#### PC
下载安装 [Git](https://git-scm.com/) ，全用默认设置，然后打开Git终端

- 默认工作目录是Git安装目录下的`mingw64`文件夹
- 路径分隔符请使用`/`或`\\`

---

### 设置Git用户信息

```sh
git config --global user.name "keithmork"
git config --global user.email "keith.mork@gmail.com"
```

- 使用GitHub注册的邮箱和用户名  
  这台机器上的每个项目提交时都会用这些信息

---

### 创建GitHub个人主页

在GitHub上新建仓库(repository)  

- 名字**必须**为 *username*.github.io ，如：`keithmork.github.io`

只要往里面提交一个`index.html`文件，就能通过 https://keithmork.github.io 访问了  
不过先不急，继续往下看


---
## 使用静态网站生成器搭建站点

个人网站/博客通常只需要简单的静态页面，[这类工具](https://www.staticgen.com/)正好可以把Markdown文档转换成网页，非常方便

这里选择 [Hugo](http://gohugo.io/)，特点是生成速度非常快，安装配置简单，还带实时预览

### 安装

下载后解压，把`hugo`（Windows下是`hugo.exe`）的路径加进环境变量`PATH`就行  
命令行运行`hugo version`不报错就是成功了

---
### 新建站点

假设想放在用户家目录的`site/blog/`下

```
mkdir -p ~/site/blog
hugo new site ~/site/blog
```
会在指定目录下生成如下内容

> archetypes/  
> content/  
> data/  
> layouts/  
> static/  
> themes/  
> config.toml  

---
### 创建页面

```
hugo new post/2016-07-19-first.md
```
可以看到在`content/`下生成了指定的目录和文件   

- `content/`下的文件夹默认代表类型
- 博文按上面那样取名比较容易管理，推荐

用文本编辑器打开可以看见如下模板：

```md
+++
date = "2016-07-19T00:12:34+08:00"
draft = true
title = "2016 07 19 first"

+++
```
在模板下面的空白处写正文，如

```md
+++
date = "2016-07-19T00:12:34+08:00"
draft = true
title = "我的第一篇博文"

+++

## Hello Hugo

坚持写博客的好处：

- 记录心得
- 整理思路
- 分享交流
- 求职展示
```

---
### 下载主题

- 默认不带主题，要从 [这里](https://github.com/spf13/hugoThemes) 挑喜欢的下载

```
cd themes
git clone https://github.com/digitalcraftsman/hugo-steam-theme.git
```

[Steam](http://themes.gohugo.io/steam/)这主题用了2016年国外流行的[CR](http://www.b12.cn/article/NDI2MGIxMg.html)风格，看着还不错

---
### 本地预览

```
cd ..
hugo server -Dw -t hugo-steam-theme
```

- `-D` 或 `--buildDraft`，加上这参数才会生成页面
- `-w` 或 `--watch`，文件有改动时自动重新生成并刷新浏览器页面
- `-t` 或 `--theme`，指定主题
  
之后对文章的改动都能在浏览器实时看到，样式跟发布到线上完全相同  
默认网址为`localhost:1313`

- 当前工作目录必须为站点根目录，否则提示`Error: Unable to locate Config file.`
- 必须指定theme，如果没下载或没指定，只能看到一片空白
- `ctrl + c`结束hugo server

---
### 生成发布页面

```
hugo -D -b "https://keithmork.github.io" -t hugo-steam-theme
```
  - `-b` 也作 `--baseURL` ，要发布到的网站目录地址

执行后会生成`public/`目录，可以看到之前的.md文件转换成了文件夹 + HTML

---
### 部署到GitHub

```
cd public
git init
git remote add origin "https://github.com/keithmork/keithmork.github.io"
git add -A
git commit -m "first commit"
git pull
git push origin master
```

之后会提示输入在GitHub注册的邮箱和密码  
提交成功后，浏览器打开`https://keithmork.github.io`，就能看到刚才的页面了

**大功告成！**  


---
## （可选）使用SSH密钥登录GitHub

每次发博文都输用户名密码太麻烦，用密钥代替密码就方便多了  
前提是机器只有你一个人用

### 生成SSH密钥对

```
mkdir ~/.ssh
cd ~/.ssh
ssh-keygen -t rsa -C "keith.mork@gmail.com"
```
- 提示`Enter file in which to save the key`时直接回车，使用默认设置
- 提示`Enter passphrase`和`Enter same passphrase again`时，可以直接回车，不使用加密串（否则每次提交时会验证加密串）

> [详细教程](https://help.github.com/articles/generating-an-ssh-key/)

---
### 添加SSH公钥到GitHub

1. 用Vim编辑公钥文件 

```
vim id_rsa.pub
```

2. 输入`yy`复制整行，`:q`退出

3. 登上GitHub，在个人设置里找到`SSH and GPG keys`，新建SSH key，粘贴进去  
取个容易识别的名字，如`Mac-Home` `PC-Work`等，保存

4. 测试是否成功

```
ssh -T git@github.com    # 就这样不用改
```
- 看到`Are you sure you want to continue connecting`时输`yes`回车，然后就可以了


---
## 参考

> [Hugo官网例子](http://gohugo.io/overview/usage/)  
> [GitHub Pages](https://pages.github.com/) 的`User or organization site`下面有手把手教程  
> [使用hugo搭建个人博客站点](http://blog.coderzh.com/2015/08/29/hugo/)
