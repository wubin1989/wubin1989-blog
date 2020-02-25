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
![hugo](/img/hugo-logo-wide.svg "Logo Title Text 1")  
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
```
[taxonomies]
    tag = "tags"
    category = "categories"
    archive = "archives"
```
如果想把内容归类，需要在内容文件的元信息中分别声明在上述分类体系中具体的类型，如：
```
tags:        ["backend", "hugo"]
categories:  ["Tech"]
archives:    "2020"
```

&nbsp; 
&nbsp; 
## 创建博客工程
打开命令行，执行如下命令：
```
hugo new site my-hugo-blog
```
生成的项目结构如下：
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
大家看到项目根路径下有一个`themes`文件夹，里面还是空的，需要执行如下命令，生成一套主题脚手架：
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

  [params.social]
  rss            = true 
  email          = "328454505@qq.com"
  github         = "https://github.com/wubin1989"
  wechat         = "img/wechat_qrcode.jpg"
  
[taxonomies]
    tag = "tags"
    category = "categories"
    archive = "archives"
```
&nbsp; 
### archetypes
hugo Cli工具支持`hugo new`命令生成markdown文件，例如：
```
hugo --verbose new post/2020-02-24-how-to-create-your-own-hugo-theme.md
```
这个命令会在项目根目录下的`content`文件夹下生成`post`文件夹，然后生成`2020-02-24-how-to-create-your-own-hugo-theme.md`文件。  
而`hugo-cxy-theme`文件夹里的`archetypes`的作用就是，开发者可以在里面放入各种类型的文章的生成模板，这样在执行上述命令的时候，会自动生成一些定义好的元信息。  
我们可以看到现在`hugo-cxy-theme`文件夹里的`archetypes`文件夹下只有一个`default.md`文件，里面只有
```
+++
+++
```
就是什么都没有。还记得上面在讲[模板](#archetypes)的时候说过hugo查找对应模板首是先从项目根路径下的`archetypes`文件夹里找。这个文件夹里也是只有一个`default.md`文件，里面的内容是：
```
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
---
```
再来看生成的`2020-02-24-how-to-create-your-own-hugo-theme.md`文件，里面不出所料的有如下内容：
```
---
title: "2020 02 24 How to Create Your Own Hugo Theme"
date: 2020-02-25T23:16:58+08:00
draft: true
---
```
下面，我们想自定义自己的模板，怎么做？  
我们在`hugo-cxy-theme`文件夹里的`archetypes`里创建`post.md`
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
然后我们删掉之前创建的`2020-02-24-how-to-create-your-own-hugo-theme.md`，重新生成，可以看到里面的内容变成了：
```

```
