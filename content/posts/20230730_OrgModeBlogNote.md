+++
title = "借助Hugo和GithubPages实现用OrgMode写博客"
author = ["Cheng Xia"]
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [安装Hugo](#安装hugo)
- [新建site](#新建site)
- [安装ox-hugo](#安装ox-hugo)
- [写一篇博客](#写一篇博客)
- [将site目录上传到github并发布](#将site目录上传到github并发布)
    - [本地site提交到远端github](#本地site提交到远端github)
    - [配置github actions](#配置github-actions)
- [问题记录](#问题记录)
    - [图片显示](#图片显示)
    - [文章内容宽度](#文章内容宽度)
    - [增加基于utterances的comment支持](#增加基于utterances的comment支持)
- [总结](#总结)

</div>
<!--endtoc-->

这里介绍的操作，都是基于MacsOS。 <br/>


## 安装Hugo {#安装hugo}

参考[^fn:1] <br/>

```bash
brew install hugo
```


## 新建site {#新建site}

使用如下git的submodule命令[^fn:2]安装ananke主题[^fn:3]： <br/>

```bash
hugo new site blog
cd blog
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
hugo server
```

访问，效果如下： <br/>

{{< figure src="/ox-hugo/01_hugoStartPage.png" width="900" >}} <br/>


## 安装ox-hugo {#安装ox-hugo}

在.emacs中添加配置，安装ox-hugo： <br/>

```emacs-lisp
;; 安装ox-huge
(use-package ox-hugo
  :ensure t   ;Auto-install the package from Melpa
  :pin melpa  ;`package-archives' should already have ("melpa" . "https://melpa.org/packages/")
  :after ox)
```


## 写一篇博客 {#写一篇博客}

新建一个org文件，在前面加入 `#+HUGO_BASE_DIR: /xxx/blog` 指定site目录，这样，执行 `M-x org-hugo-auto-export` 命令(这个命令实际上是启动了一个minor mode，每当文件保存的时候，会同步转为对应目录下的md文件)后，org文件转换的md文件将会放在 `/xxx/blog/content/posts/` 目录下。 <br/>

这时候，通过在site目录下执行 `hugo server` 命令，就可以启动site。在本地访问 `http://localhost:1313/` ，就可以正常看到刚写的博客，图片也可以正常展现(图片存在site根目录的static目录下)。如下： <br/>

{{< figure src="/ox-hugo/02_hugoFirstBlogPage.png" width="900" >}} <br/>


## 将site目录上传到github并发布 {#将site目录上传到github并发布}


### 本地site提交到远端github {#本地site提交到远端github}

首先，将前面的site目录，初始化为git仓库，并执行本地提交。然后，在github创建一个新的git仓库，然后通过在site目录执行如下命令将前面的site目录提交到远端： <br/>

```bash
git remote add origin git@github.com:xia7139/yoyo.git
git branch -M main
git push -u origin main
```


### 配置github actions {#配置github-actions}

在"Build and deployment"设置中，Source选”Github Actions“，然后选中hugo，对这个配置文件不用做任何改动，直接页面commit即可。 <br/>

step1： <br/>

{{< figure src="/ox-hugo/03_githubActionsConfPageA.png" width="900" >}} <br/>

step2： <br/>

{{< figure src="/ox-hugo/03_githubActionsConfPageB.png" width="900" >}} <br/>

step3： <br/>

{{< figure src="/ox-hugo/03_githubActionsConfPageC.png" width="900" >}} <br/>

step4： <br/>

{{< figure src="/ox-hugo/03_githubActionsConfPageD.png" width="900" >}} <br/>

注意，本地需要及时拉取更新，不然后续的commit无法push到远端。 <br/>

这样，就可以实现每次push之后，自动部署到pages博客。每次推送之后，可以在actions之后看到部署的过程。如下图： <br/>

{{< figure src="/ox-hugo/04_githubActionsDeploying.png" width="900" >}} <br/>


## 问题记录 {#问题记录}


### 图片显示 {#图片显示}

如果需要图片无法显示的问题，需要在site目录的hugo.toml配置文件中，添加 `canonifyURLs = true` ，比较并推到远端后，自动部署完成后，图片可以正常显示[^fn:4]。 <br/>


### 文章内容宽度 {#文章内容宽度}

这里采用的是ananke主题，文章内容宽度只能占屏幕宽度的三分之二。查了下，需要修改主题目录下 `layouts/_default/single.html` 文件，在该文件中搜索"two-thirds"，然后在对应的dom元素上删除这个class即可[^fn:5]。 <br/>

以上这种方式在本地可以生效，但是，推送到github之后，无法正常部署，原因应该是这个改动没有推送到子模块的远端。这里采用如下方式[^fn:6]。 <br/>

首先，在site目录下新建一个自定义的css文件，内容如下(应该主要是最后一行起作用)： <br/>

```text
$ cat assets/ananke/css/custom.css 
.container {
  margin: 0 auto;
  // Adjusting the max-width to increase page width. 
  // The original file path - `/<website-root>/themes/<theme-name>/assets/scss/..`
  // max-width: 90rem;
  max-width: 100rem;
  width: 100%;
  padding-left: 2rem;
  padding-right: 2rem;
}
.w-two-thirds-l{
  width:100%;
}
$
```

然后，在site目录的配置文件中添加自定义css配置。如下： <br/>

```text
$ cat hugo.toml 
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'Nono Site'
theme = 'ananke'
canonifyURLs = true
disqusShortname = "Nono"
[params]
  commentoEnable = true
  custom_css = ["custom.css"]
$
```

执行hugo server命令，重启site，即可生效。 <br/>


### 增加基于utterances的comment支持 {#增加基于utterances的comment支持}

前面如果配置了disqusShortname或者commento，可以支持评论，但是，需要额外的登录。这里该用utterances，一个轻量的基于github issue的评论系统[^fn:7]。 <br/>

首先，需要新建一个public的github仓库，然后，在其中安装utterances。如下： <br/>

{{< figure src="/ox-hugo/05_install_comment1.png" >}} <br/>

{{< figure src="/ox-hugo/05_install_comment2.png" >}} <br/>

这里用的主题是ananke，需要将site目录中的文件 `themes/ananke/layouts/partials/commento.html` ，拷贝到 `layouts/partials/commento.html` 目录(如果目录不存在则新建)。并修改其内容如下： <br/>

```html
<script src="https://utteranc.es/client.js"
	  repo="xia7139/yoyo_comment" 
	  issue-term="title"
	  theme="github-light"
	  crossorigin="anonymous"
	  async>
  </script>
```

最后，修改site目录hugo.toml配置如下： <br/>

```text
$ cat hugo.toml 
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'YoYo Site'
theme = 'ananke'
canonifyURLs = true
[params]
  commentoEnable = true
  custom_css = ["custom.css"]
$ 
```

执行 `hugo server` 命令，可以在本地看到效果，推送到github部署完成之后，即可生效。 <br/>


## 总结 {#总结}

这样，写一篇博客的工作流如下： <br/>

-   编辑org文件，可以通过单独的git库来对这个org文件进行版本管理； <br/>
-   运行 `M-x org-hugo-auto-export` 命令，将org文件转换为site目录下的md文件； <br/>
-   将site目录的改动commit，并推送到远端，就会自动发布到pages博客。 <br/>

[^fn:1]: [使用 Hugo 搭建个人网站（博客、个人主页）并发布到 Github 上](https://kinredon.github.io/post/how-to-publish-personal-website-on-github/)  <br/>
[^fn:2]: [Git中submodule的使用](https://zhuanlan.zhihu.com/p/87053283)  <br/>
[^fn:3]: [Hugo Quick start](https://gohugo.io/getting-started/quick-start/)  <br/>
[^fn:4]: [Images from Hugo not displayed on GitHub pages](https://stackoverflow.com/questions/61574702/images-from-hugo-not-displayed-on-github-pages)  <br/>
[^fn:5]: [body width #91](https://github.com/theNewDynamic/gohugo-theme-ananke/issues/91)  <br/>
[^fn:6]: [Change page-width and fonts in your Hugo website](https://www.sharank.com/posts/websites/change-pg-width-and-font-size/)  <br/>
[^fn:7]: [utterances](https://utteranc.es/?installation_id=40781403&setup_action=install)  <br/>