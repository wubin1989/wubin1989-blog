---
title:       "如何创建自己的hugo主题"
subtitle:    ""
description: ""
date:        2020-02-24
author:      ""
image:       ""
tags:        ["backend", "hugo"]
categories:  ["Tech"]
archives:    "2020"
---

辞旧迎新，想改版自己的博客，一番google之后，选择了[hugo](https://gohugo.io/)。  
&nbsp;  
&nbsp;    
![hugo](/img/hugo-logo-wide.svg "Hugo Logo")  
&nbsp;  
&nbsp; 

根据hugo官网的介绍，hugo是世界上最受欢迎的静态网站生成器之一，基于golang开发，已经发布到0.65.0版本。hugo采用开源的[goldmark](https://github.com/yuin/goldmark/)作为markdown的解析器，兼容GitHub-Flavored Markdown标准规范。个人认为，hugo适合用来做博客、做静态展示型的企业网站等。  
hugo提供了通过主题构建网站的机制。hugo生态已经提供了300+的主题可以用。主题类似一种前端框架，可以帮助我们快速建站。我调研了一些主题，各有优点，也有令我不满意的地方，所以决定还是自己开发一套主题来用。  
开发主题需要结合实际的项目来做，一边看效果一边做调整。下面以开发个人博客为例，介绍如何DIY自己的主题。
&nbsp;  
&nbsp; 
## 安装hugo
hugo提供了cli工具。下面是常见的安装方法。
&nbsp; 
### Homebrew
```
brew install hugo
```
&nbsp; 
### Chocolatey (Windows)
```
choco install hugo -confirm
```
&nbsp; 
### Scoop (Windows) 
```
scoop install hugo
```
&nbsp;  
&nbsp; 
## 本文涉及的主要概念
### 内容格式（Content Formats）
HTML和Markdown两种格式都支持。用户可以往`/content`文件夹里放置任何类型的文件，但是hugo首先会从内容文件的元信息里找markup属性，如果没有找到，hugo会根据文件后缀名选择合适的解析器来解析内容。
&nbsp; 
### 元信息（Front Matter）
英文叫`[Front Matter](https://gohugo.io/content-management/front-matter/)`，我依据个人的理解，把这个概念翻译成了元信息。hugo支持在内容文件开头以四种格式来声明这篇文章的元信息。四种格式分别是：
* TOML: 以`+++`开头和结尾
* YAML: 以`---`开头和结尾，这是本文介绍的主题采用的格式
* JSON: 本人觉得不常用，略过
* ORG: 本人觉得不常用，略过
&nbsp; 
### 内容类型（Content Types）
粗浅的理解[Content Types](https://gohugo.io/content-management/types/)就是hugo用来组织网站内容的一种方式。hugo首先会从内容文件的元信息里找`type`属性，如果没有找到，hugo会认定`content`的文件夹下第一级文件夹的名称作为其中包含的所有内容文件的内容类型，例如`content/blog/my-first-event.md`路径下的`my-first-event.md`文件的内容类型是`blog`。
&nbsp; 
### 模板（Archetypes）
模板（[Archetypes](https://gohugo.io/content-management/archetypes/)）是指放在`archetypes`文件夹里的文件，里面可以预定义一些元信息，也可以提前写好一些内容生成逻辑，或其他什么内容。当执行`hugo new`命令来生成内容文件的时候，就会调用对应内容类型的模板文件来帮你自动生成一些内容。  
假如以`posts`作为内容类型，生成`posts`文件时模板的查找路径依次是：  
1. `archetypes/posts.md`
2. `archetypes/default.md`
3. `themes/my-theme/archetypes/posts.md`
4. `themes/my-theme/archetypes/default.md`
&nbsp; 
### 分类体系（Taxonomies）
分类体系（[Taxonomies](https://gohugo.io/content-management/taxonomies/)）表示作者对内容的一套或多套分类。比如标签（`tags`）、类目（`categories`）、归档（`archives`）等。分类体系需在项目根路径下配置文件中定义，例如在`config.toml`文件中加入：
&nbsp;  
&nbsp;  
```
[taxonomies]
    tag = "tags"
    category = "categories"
    archive = "archives"
```
&nbsp;  
如果想把内容归类，需要在内容文件的元信息中分别声明在上述分类体系中具体的类型，如：
&nbsp;  
&nbsp;  
```
tags:        ["backend", "hugo"]
categories:  ["Tech"]
archives:    "2020"
```

&nbsp; 
&nbsp; 
## 创建博客工程
打开命令行，执行如下命令：
&nbsp;  
&nbsp;  
```
hugo new site my-hugo-blog
```
&nbsp;  
生成的项目结构如下：
&nbsp;  
&nbsp;  
```
➜  my-hugo-blog ll
total 8
drwxr-xr-x  3 wubin1989  staff    96B  2 25 20:32 archetypes
-rw-r--r--  1 wubin1989  staff    82B  2 25 20:32 config.toml
drwxr-xr-x  2 wubin1989  staff    64B  2 25 20:32 content
drwxr-xr-x  2 wubin1989  staff    64B  2 25 20:32 data
drwxr-xr-x  2 wubin1989  staff    64B  2 25 20:32 layouts
drwxr-xr-x  2 wubin1989  staff    64B  2 25 20:32 static
drwxr-xr-x  2 wubin1989  staff    64B  2 25 20:32 themes
```
&nbsp;  
大家看到项目根路径下有一个`themes`文件夹，里面还是空的，需要执行如下命令，生成一套主题脚手架：
&nbsp;  
&nbsp;  
```
➜  my-hugo-blog hugo new theme hugo-cxy-theme 
Creating theme at /Users/wubin1989/workspace/go/src/my-hugo-blog/themes/hugo-cxy-theme
➜  my-hugo-blog cd themes 
➜  themes ll
total 0
drwxr-xr-x  7 wubin1989  staff   224B  2 25 20:43 hugo-cxy-theme
➜  themes cd hugo-cxy-theme 
➜  hugo-cxy-theme ll
total 16
-rw-r--r--  1 wubin1989  staff   1.1K  2 25 20:43 LICENSE
drwxr-xr-x  3 wubin1989  staff    96B  2 25 20:43 archetypes
drwxr-xr-x  6 wubin1989  staff   192B  2 25 20:43 layouts
drwxr-xr-x  4 wubin1989  staff   128B  2 25 20:43 static
-rw-r--r--  1 wubin1989  staff   440B  2 25 20:43 theme.toml
```
&nbsp; 
### config.toml
```
baseURL = "/"
languageCode = "zh-cn"
title = "武斌的博客"
theme = "hugo-cxy-theme"
preserveTaxonomyNames = true
paginate = 10 #frontpage pagination
hasCJKLanguage = true

[outputs]
home = ["HTML", "RSS"]

[params]
hero_bg = "img/home-bg-road.jpg"
SEOTitle = "wubin1989的博客 | wubin1989 Blog"
description = "wubin1989，程序员, 摄影爱好者, 背包客 | 这里是 wubin1989 的博客，边走边看，边读边写。"
keyword = "wubin1989, wubin1989, wubin1989的网络日志, wubin1989的博客, wubin1989 Blog, 博客, 个人网站, 互联网, Web, Nodejs, Reactjs, SaaS, Golang, 微服务, Microservice"
slogan = "跨过高山，走过四季，不忘初心，永不言弃"
brief_info = "全栈工程师/背包客/摄影爱好者"
info = "常年写reactjs、vuejs、java和golang，专注微服务架构和devops相关，喜欢旅游、爬山、外语"
avatar = "img/avatar-wubin1989.jpg"      # use absolute URL, seeing it's used in both `/` and `/about/`

image_404 = "img/404-bg.jpg"
title_404 = "你来到了没有知识的荒原 :("

[taxonomies]
tag = "tags"
category = "categories"
archive = "archives"
```
&nbsp; 
### archetypes
hugo Cli工具支持`hugo new`命令生成markdown文件，例如：
&nbsp;  
&nbsp;  
```
hugo --verbose new post/2020-02-24-how-to-create-your-own-hugo-theme.md
```
&nbsp;  
这个命令会在项目根目录下的`content`文件夹下生成`post`文件夹，然后生成`2020-02-24-how-to-create-your-own-hugo-theme.md`文件。  
而`hugo-cxy-theme`文件夹里的`archetypes`的作用就是，开发者可以在里面放入各种类型的文章的生成模板，这样在执行上述命令的时候，会自动生成一些定义好的元信息。  
我们可以看到现在`hugo-cxy-theme`文件夹里的`archetypes`文件夹下只有一个`default.md`文件，里面只有
&nbsp;  
&nbsp;  
```
+++
+++
```
&nbsp;  
就是什么都没有。还记得上面在讲[模板](#archetypes)的时候说过hugo查找对应模板首是先从项目根路径下的`archetypes`文件夹里找。这个文件夹里也是只有一个`default.md`文件，里面的内容是：
&nbsp;  
&nbsp;  
```
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
---
```
&nbsp;  
再来看生成的`2020-02-24-how-to-create-your-own-hugo-theme.md`文件，里面不出所料的有如下内容：
&nbsp;  
&nbsp;  
```
---
title: "2020 02 24 How to Create Your Own Hugo Theme"
date: 2020-02-25T23:16:58+08:00
draft: true
---
```
&nbsp;  
下面，我们想自定义自己的模板，怎么做？  
我们在`hugo-cxy-theme`文件夹里的`archetypes`里创建`post.md`
&nbsp;  
&nbsp;  
```
---
title:       "{{ with slicestr .Name 10 }}{{replace . "-" " "  | strings.TrimLeft " " | title }}{{end}}"
subtitle:    ""
description: ""
date:        {{ slicestr .Name 0 10 }}
author:      ""
image:       ""
tags:        ["tag1", "tag2"]
categories:  ["Tech"]
archives:    "{{ slicestr .Name 0 4 }}"
---
```
&nbsp;  
然后我们删掉之前创建的`2020-02-24-how-to-create-your-own-hugo-theme.md`，重新生成，可以看到里面的内容变成了：
&nbsp;  
&nbsp;  
```
---
title:       "How to Create Your Own Hugo Theme"
subtitle:    ""
description: ""
date:        2020-02-24
author:      ""
image:       ""
tags:        ["tag1", "tag2"]
categories:  ["Tech"]
archives:    "2020"
---
```
&nbsp; 
### layouts
跟网站整体框架布局相关的文件都放在`layouts`里面。首先要修改`_default`文件夹里的`baseof.html`文件。这个文件里配置了网站的`header`、`main`和`footer`等。
&nbsp;  
&nbsp;  
```
<!DOCTYPE html>
<html>
    {{- partial "head.html" . -}}
    <body>
        {{- partial "header.html" . -}}
        <div class="section">
            <div class="container">
                <div class="columns cxy-gap">
                    {{- partial "side.html" . -}}
                    <div class="column" id="content">
                    {{- block "main" . }}{{- end }}
                    </div>
                </div>
            </div>
        </div>
        {{- partial "footer.html" . -}}
    </body>
</html>
```
&nbsp;  
代码里涉及的文件`head.html`、`header.html`、`side.html`和`footer.html`文件都放在`partial`文件夹里。
&nbsp;  
&nbsp;  
```
<!-- head.html -->
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="keyword"  content="{{ .Site.Params.keyword }}">
    <link rel="shortcut icon" href="{{ "img/favicon.ico" | relURL }}">

    <title>{{ if .Title }}{{ .Title }}-{{ .Site.Params.SEOTitle }}{{ else }}{{ .Site.Params.SEOTitle }}{{ end }}</title>

    <link rel="canonical" href="{{ .URL | relURL }}">
	
    <!-- bulma CSS -->
    <link rel="stylesheet" href="{{ "css/bulma.min.css" | relURL }}">

    <!-- zangshang CSS -->
    <link rel="stylesheet" href="{{ "css/zanshang.css" | relURL }}">
    
    <!--  Custom CSS  -->
    <link rel="stylesheet" href="{{ "css/custom.css" | relURL }}">
    {{ range .Site.Params.custom_css -}}
    <link rel="stylesheet" href="{{ . | absURL }}">
    {{- end }}

    <!-- jQuery -->
    <script src="{{ "js/jquery.min.js" | relURL }}"></script>

    <script src="{{ "js/all.js" | relURL }}"></script>

    <!-- Custom JS -->
    {{ range .Site.Params.custom_js }}s
    <script src="{{ . | absURL }}"></script>
    {{ end }}
</head>
```
&nbsp;  
解释一下这段代码中涉及的hugo变量和函数：
* 变量
  * `.Site.Params.keywords`
    * `.Site`: 全局对象
    * `.Params`: `config.toml`文件里的`[params]`的配置
  * `.Title`: 文章标题
* 函数
  * `relURL`: 将输入值转换为相对路径的url
  * `absURL`: 将输入值转换为绝对路径的url
&nbsp;  
&nbsp;  
```
<!-- header.html -->
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    {{ if ( ne .Site.Title "" ) }}
    <a class="navbar-item" href="{{ .Site.BaseURL | relLangURL }}"> {{ .Site.Title }} </a>
    {{ end }}

    <a role="button" class="navbar-burger burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>

  <div class="navbar-menu">
    <div class="navbar-start">
      <a class="navbar-item"href="{{ "/" | relLangURL }}">Home</a>

      {{ range $name, $taxonomy := .Site.Taxonomies.categories }}
          <a class="navbar-item" href="{{ "categories/" | relLangURL }}{{ $name | urlize }}">{{ $name | title }}</a>
      {{ end }}
    </div>
  </div>
</nav>
<section class="hero is-medium cxy-hero" style="background-image: url('{{ .Site.Params.hero_bg | relURL }}')">
  <div class="hero-body">
    <div class="container"> 
      <div class="has-text-centered is-size-4 has-text-white">
        {{ if not .IsHome }}
        <h1 class="title has-text-white">
          {{ .Title }}
        </h1>
        {{ if and (eq .Type "post") .IsPage }}
        <h2 class="subtitle has-text-white-bis">
          {{ .Date.Format "2006-01-02"}}
        </h2>
        {{ end }}
        {{ else }}
        <h1 class="title has-text-white is-size-4">
          {{ .Site.Params.slogan }}
        </h1>
        {{ end }}
      </div>
    </div>
  </div>
</section>
```
&nbsp;  
解释一下这段代码中涉及的hugo变量和函数：
* 变量
  * `.Site.Taxonomies.categories`
    * `.Site`: 全局对象
    * `.Taxonomies`: `config.toml`文件里的`[taxonomies]`的配置
  * `.Site.Title`: `config.toml`文件里的`title`的配置
  * `.Site.BaseURL`: `config.toml`文件里的`baseURL`的配置
  * `.IsHome`: 是否是网站首页
  * `.Type`: 内容的类型
  * `.IsPage`: 是否是"page"类型
* 函数
  * `relLangURL`: 将输入值转换为以正确的语言变量值为前缀的相对路径的url，多语言网站才会用到
  * `urlize`: 将输入值编码成url路径，同时把空格改成中横线"-"
  * `title`: 将输入值转换为首字母大写的标题
  * `.Date.Format`: 日期时间格式化
&nbsp;  
&nbsp;  
```
<!-- side.html -->
<div class="column is-one-quarter sidebar">
    <div class="card">
        <div class="card-content">
            <div class="">
                <figure class="image is-128x128 is-inline-block">
                <img class="" src="{{ .Site.Params.avatar | relURL }}">
                </figure>
                <div class="title is-6" style="margin-top: 10px;">
                    {{ .Site.Params.brief_info }}
                </div>
            </div>
            <div class="subtitle is-6" style="margin-top: 5px;">
                {{.Site.Params.info}}
            </div>
        </div>
    </div>

    <div class="card">
        <div class="card-content">
            <h1 class="title is-5">Tags</h1>
            <div class="tags">
            {{ range $name, $taxonomy := .Site.Taxonomies.tags }}
                <span class="tag"><a href="{{ "tags" | absURL }}/{{ $name | urlize }}">{{ $name }}</a></span>
            {{ end }}
            </div>          
        </div>
    </div>
      
    <div class="card">
        <div class="card-content">
            <h1 class="title is-5">Archives</h1>
            {{ range (where .Site.RegularPages "Section" "post").GroupByDate "2006" }}
                <a href="{{ "archives" | absURL }}/{{ .Key }}">{{ .Key }}</a> ({{ len .Pages }})<br>
            {{ end }}
        </div>
    </div>
</div>
```
&nbsp;  
解释一下这段代码中涉及的hugo变量和函数：
* 变量
  * `.Site.RegularPages`: 表示所有"Kind"属性是"page"的内容页面
* 函数
  * `where`: 从一组集合中查询出符合条件的元素，非常有用，详细用法请参考hugo文档的[这一页](https://gohugo.io/functions/where/)。一般情况，`where`函数是用来从`.Site.Pages`集合或者是`.Pages`集合里做查询。关于`.Site.Pages`和`.Pages`的区别请参考hugo文档的[这一页](https://gohugo.io/variables/site/#site-pages)
  * `.GroupByDate`: 按内容文件的元信息里的`date`属性来对内容页面做分组，参数是日期时间格式化字符串，返回值是一个字典类型的值，包含`.Key`和`.Pages`属性
&nbsp;  
&nbsp;  
```
<!-- footer.html -->
<footer>
    <div class="container">
        <div class="row">
            <div class="has-text-centered">
                <ul>
                    {{ with .Site.Params.social.wechat }}
                    <li class="is-inline">
                        <a target="_blank" href="{{ . | relURL }}">
                            <span class="icon is-medium">
                            <span class="fa-stack">
                                <i class="fa fa-circle fa-stack-2x"></i>
                                <i class="fab fa-weixin fa-stack-1x fa-inverse"></i>
                            </span>
                            </span>
                        </a>
                    </li>
		    {{ end }}
                    {{ with .Site.Params.social.github }}
                    <li class="is-inline">
                        <a target="_blank" href="{{ . }}">
                            <span class="icon is-medium">
                                <span class="fa-stack">
                                    <i class="fa fa-circle fa-stack-2x"></i>
                                    <i class="fab fa-github fa-stack-1x fa-inverse"></i>
                                </span>
                            </span>
                        </a>
                    </li>
		    {{ end }}
                </ul>
                <p class="copyright text-muted">
                    Copyright &copy; {{ .Site.Title }} {{ now.Year }}
                </p>
            </div>
        </div>
    </div>
</footer>
```
&nbsp;  
这段代码中涉及的hugo函数只有`now`，表示返回当前的本地时间  
首页的内容放在`index.html`文件里。
&nbsp;  
&nbsp;  
```
{{ define "main" }}

{{ $paginator := .Paginate (where (where .Site.Pages "Type" "post") "IsPage" true) }}
{{ range $paginator.Pages }}
<div class="columns">
    <div class="column is-four-fifths">
        <a href="{{ .Permalink }}">
            <div class="title is-size-4">
                {{ .Title }}
            </div>
        </a>
    </div>
    <div class="column is-right is-vertical-center subtitle">
        {{ .Date.Format "2006-01-02" }}
    </div>
</div>
<hr>
{{ end }}

{{ $.Scratch.Set "paginator" $paginator }}
{{- partial "pagination.html" . -}}

{{ end }}
```
&nbsp;  
解释一下这段代码中涉及的hugo变量和函数： 
* 变量
  * `.Scratch`: 类似字典，可以设置键值对，作用域就是当前页面，可以在页面的其他地方通过`.Scratch.Get`获取值。 推荐参考这篇[文章](https://regisphilibert.com/blog/2017/04/hugo-scratch-explained-variable/)更加深入的了解`.Scratch`。
* 函数
  * `.Paginate`: 传入通过where函数返回的页面集合，生成`Paginator`分页器，可以用来构建分页组件
&nbsp;  
`_default`文件夹里的`single.html`里配置markdown文件内容如何渲染到html文件中输出。
&nbsp;  
&nbsp;  
```
<!DOCTYPE html>
<html>
    {{- partial "head.html" . -}}
    <body>
        {{- partial "header.html" . -}}
        <div class="section">
            <div class="container">
                <div class="card" id="content">
                    <div class="card-content">
                        <div class="cxy-post-content">
                            {{ if not (eq (.Param "showtoc") false) }}
                            <header>
                                <h2>TOC</h2>
                            </header>
                            {{ .TableOfContents}}
                            {{ end }}
                            {{ .Content }}
                        </div>
                        <hr>
                        <ul>
                            {{ if .PrevInSection }}
                            <li class="is-inline">
                                <a href="{{ .PrevInSection.URL }}" class="pagination-previous">&larr;
                                    {{ .PrevInSection.Title}}</a>
                            </li>
                            {{ end }}
                            {{ if .NextInSection }}
                            <li class="is-inline is-pulled-right">
                                <a href="{{ .NextInSection.URL }}" class="pagination-previous">
                                    {{ .NextInSection.Title}} &rarr;</a>
                            </li>
                            {{ end }}
                        </ul>
                    </div>
                </div>
            </div>
        </div>
        {{- partial "footer.html" . -}}
    </body>
</html>
```
&nbsp;  
解释一下这段代码中涉及的hugo变量和函数： 
* 变量
  * `.TableOfContents`: hugo可以自动从markdown文件中解析出目录渲染到页面里
  * `.Content`: hugo从markdown中解析出的文章内容
  * `.PrevInSection`: 前一篇文章
  * `.NextInSection`: 下一篇文章
* 函数
  * `.Param`: 从内容文件的元信息（`front matter`）里取出参数值为属性名的属性值，如果找不到，再从项目配置文件（`config.toml`）里找。
&nbsp;  
&nbsp;  
## 总结
以上是我在创建自己的hugo博客和主题时总结的部分概念和编写的源码。还有很多概念或者代码逻辑比较复杂，没有在本文提及，打算以后再仔细分析。希望上面的内容能帮到需要的朋友。
&nbsp;  
&nbsp;  
我的hugo主题参考了：
* [hugo-theme-cleanwhite](https://github.com/zhaohuabing/hugo-theme-cleanwhite)，感谢[zhaohuabing](https://github.com/zhaohuabing)  
* [hugo-blog-jeffprod](https://github.com/Tazeg/hugo-blog-jeffprod)，感谢[JeffProd](https://github.com/Tazeg)    

最后的效果大概是这样的。
![demo](/img/theme-demo.png "Theme Demo")  