## HardCandy-Jekyll



### Preview





### 主题特性

- 主题基于 `jekyll 3.8.1` 开发
- 响应式布局
- 文章标签索引
- 文章时间线索引
- 博主个人信息展示
- 支持9种代码高亮主题色
- 支持 `dispus` 、 `来必力` 、 `Gitment` 三种评论系统
- 支持 `百度统计` 、`谷歌分析` 两种网站追踪系统
- 支持13款不同社交平台图标及链接地址指向
- 支持11个不同平台的文章分享路口


### 配置文档

- 开始
  - [关于博客](#关于博客)
  - [写文章](#写文章)
- 组件
  - [博主个人信息](#博主个人信息)
  - [社交媒体](#社交媒体)
  - [首页显示信息](#首页显示信息)
  - [导航栏](#导航栏)
  - [分页](#分页)
  - [代码高亮主题](#代码高亮主题)
  - [友情链接](#友情链接)
  - [页脚](#页脚)
- 第三方服务
  - [评论系统的切换](#评论系统的切换)
  - [文章分享的路口](#文章分享的路口)
  - [网站流量追综的配置](#网站流量追综的配置)



> ​	通用修改 `_config.yml` 文件，你便可以轻松搭建属于你自己的个人博客。
>
> ​	一部分配置，默认已经是配置好的，你只需要修改下面列出的内容即可完成搭建。



#### 关于博客

```yaml
---
# Site settings 配置站点
title: '安全'
description: '博客站会时不时放出一些菜鸡博主自己的网络安全博客~。~'
keywords: 'Security，analysis'
url: 'https://abc.github.io' # your host
---
```




#### 写文章

​	博客通过解析 `markdown` 文件来部署文章页面的，所以用户写文章只需要写一篇markdown，并放置在站点根目录下的 `_post` 文件夹即可。具体的markdown语法自行上网搜索学习，或使用markdown编辑器进行写作。推荐一款 markdown编辑器：[typora](https://www.typora.io) 。支持 windows 、mac OSX 、Linux 。

关于文章 YAML头信息：

```yaml
layout: post
title:  "post title"
subtitle: 'post subtitle'
date:   2018-05-29 08:44:13
tags: html js css
description: ''
color: 'rgb(154,133,255)'
cover: ''
```

关于color：

​	此处的color用于post页面的顶部位置的背景色。如上面展示图所示为 `rgb(154,133,255)` 色。

​	对于color的书写，如果颜色代码为 `rgb` 或 `rgba` 又或是 `英文单词` 的话，可以不用引号包裹，但如果颜色代码为 `#123456` 这种16进制码的话，就必须使用引号包裹。所以，在使用中，推荐一致都使用引号，以免错误使用。

​	当然，如果你在书写文章时，忘记写color的值的话，主题默认会为你填写 `rgb(154,133,255)` 色。就是上图显示的颜色。虽然不影响页面的显示，但如果想要更多彩的页面效果的话，建议在每一篇的头信息里写上 color 值。

关于cover：

​	此处需填写某一张图片的 `url` ，`url` 值可以是线上的某张图片，也可以是博客目录下的图片。关键是要书写正确。这张图片用于在首页下博客列表里显示，如下图。

![5](/screenshot/5.png)



#### 博主个人信息

```yaml
# 博主
author: true
name: 'your awesome name'
NickName: 'your awesome nickname'
webtitle: 'your awesome webtitle'
bio: 'your awesome bio'
about: true
aboutyou: 'your introduction'
portraits: '/assets/profile.jpg' # your portraits image file path
```

​	该部分显示在 `关于博主` 页面，与 `社交媒体` 一同在下图显示。

![6](/screenshot/6.png)

关于author：

​	使用 `true` 或者 `false` 来打开或关闭博主信息卡片，默认 true ，最佳体验也是 true 。

关于about：

​	使用 `true` 或者 `false` 来打开或关闭博主关于信息，即是否显示 aboutyou 部分的信息。默认 true ，该部分需要在 aboutyou 中输入相关信息，支持在此填写html代码。



#### 社交媒体

```yaml
# SNS
SNS: true
SNS-icon: #['Facebook', 'weibo', 'qq', 'github', 'Dribbble', 'Twitter', 'instagram', 'weixin', 'Codepen']
  mail: 'mailto:boypeng101@gmail.com'
  github: ''

```

​	~~主题一共配置了 13种 社交媒体的图标，只要在需要开启的社交账号的名字后填写你的个人主页链接即可，不需要开启的就在那一行的头部用 `#` 注释这一行即可。同样的，如果需要更换每个图标的排列位置，只需要改变他们的每一行排列的顺序即可。~~

​	在 `SNS` 后填写  `true` 或者 `false` 来打开或者关闭这一部分。

2018/09/28 更新：

![7](/screenshot/sns-icon.png)

- 更新社交图标为 线上地址 ，便于管理与修改。
- 添加 **Codepen** 图标
- 修改原来的圆形图标为不规则图标



#### 首页显示信息

```yaml
---
layout: default
title: your awesome title
page-title: awesome page-title.
home-title: awesome home-title.
description: description
---
```

​	该部分位于 `index.html` 页面，修改 `title` 、`page-title` 、`home-title`  、`description`为个人想要的信息，默认配置的显示效果如下图。

![7](/screenshot/7.png)



#### 导航栏

```yaml
# nav 中文字符空格：&emsp;
nav: # 最佳体验 六个标签 且最好每个标签不超过4中文字
  首页: '/'
  标签: '/tags.html'
  时间线: '/timeline.html'
  关于博主: '/about.html'
  友情链接: '/friendLink.html'
```

​	默认全部开启他们，当然如果想要自己添加，按照格式填在下方即可，当然页面显示顺序与每一行的位置有关。



#### 分页

```yaml
# 分页
paginate: 2
paginatepath: ['page:num']
```

​	随个人爱好在，在上面填写你需要的在首页一页最多显示多少篇博客的数字。

​	本地部署的需要使用 `gem install jekyll-paginate` 或 `sudo gem install jekyll-paginate` 安装Jekyll的分页插件。



#### 代码高亮主题

```yaml
# 代码高亮 使用rouge
highlighter: rouge
# 代码高亮主题使用pygments主题: autumn\ default\ emacs\ friendly\ manni\ murphy\ pastie\ perldoc\ tango 任选一个你喜欢的主题名称填在下面的单引号中
pygmentsTheme: 'default'
```

​	代码高亮使用 jekyll3.0 之后的默认高亮引擎 `rouge` 。关于主题，只需要在 `pygmentsTheme` 后填写喜欢的主题名称即可。共有9款主题可选，主题名见上文。

​	代码高亮的写法：

~~~markdown
``` css
*{
 margin:0;
 padding:0;
}
```
~~~

2018/09/28 更新：

![7](/screenshot/博客代码高亮例子.png)

上图为 **代码高亮试例图** ，仅以 html 作为参考例子，其他代码参考 上图，或自行切换测试选择自己喜欢的代码高亮主题



#### 友情链接

```yaml
# 友情链接
friends:
  jekyll: 'https://www.jekyll.com.cn/'
```

​	按格式填写即可，排序与配置文件里的排序有关。



#### 页脚

```yaml
# since
footer:
  since: 2018
```

​	用于页脚显示时间。



#### 评论系统的切换

```yaml
# 评论 最佳体验 在disqus、livere和Gitment之间三选一
# disqus 评论
disqus: false
disqus_url: '' # https://abc.disqus.com/embed.js
# 来必力评论
livere: true
livere_uid: 'MTAyMC8zNDI2OS8xMDgwNg==' # MTAyMC8zNDI2OS8xMDgwNg==
# Gitment评论 OAuth Application
Gitment: false
Gitment_owner: ''  # github用户名
Gitment_repo: ''  # github博客存放的仓库名
client_id: ''  # 注册 OAuth Application 后获得的 client_id
client_secret: ''  # 注册 OAuth Application 后获得的 client_secret
```

​	按申请第三方评论是获取的相关信息在配置文件中进行填写即可。

​	共有三款评论可供选择，使用 `true` 或者 `false` 开启或关闭某个评论系统。可开启多个甚至全开。当然，最佳体验，开一个即可。

​	三款评论的样式如下图：

dispus：

![8](/screenshot/8.png)

来必力：

![9](/screenshot/9.png)

Gitment评论：

![10](/screenshot/10.png)

​	三款评论各有各的优势与坏处。出于显示样式与中国大陆网络环境考虑，主题默认开启 `来必力` 评论为最佳体验。当然需要填写好相关的 `livere_uid` 代码。



#### 文章分享的路口

```yaml
# Share : weibo, qq, wechat, tencent, douban, qzone, linkedin, diandian, facebook, twitter, google
social-share: true
social-share-items: ['qq', 'wechat', 'weibo', 'twitter', 'facebook']
```

​	为了让文章更方便地分享，使用了第三方分享插件[Share.js](http://overtrue.github.io/share.js/)，支持一键分享到微博、QQ空间、QQ好友、微信、腾讯微博、豆瓣、Facebook、Twitter、Linkedin、Google+、点点等社交网站。

​	只需要填写相关的名称在 `social-share-items` 后即可，显示顺序与书写顺序有关。



#### 网站流量追综的配置

```yaml
# 百度统计 在baidu-url里填写自己相关的url代码
baidu: true
baidu-url: ''
# 谷歌分析 在google-ID里填写自己在谷歌分析获得的追踪ID
google: false
google-ID: ''
```

​	在 `baidu-url` 和 `google-ID` 分别填上注册获取的相关信息。使用 `true` 或者 `false` 开启或关闭他们。出于中国大陆网络环境，默认开启 百度统计 ，当然可以多开。



### License 许可证

HardCandy-Jekyll is licensed under [MIT](https://github.com/xukimseven/HardCandy-Jekyll/blob/master/LICENSE).



### 求Star 求关注

看到这里，如果你喜欢我的小项目，欢迎下载使用他，也请你为我点个小星星 😜 ，多谢。