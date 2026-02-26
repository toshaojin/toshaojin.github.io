# 再一次尝试博客

<!-- more -->
# 历史
很早之前就尝试过博客，但是自己是个钢铁工科男，写不了生活。

模糊记得曾经玩过WordPress、Google Blogger，还利用免费的VPS搭建过WordPress，反正那时有的是时间、各种折腾着玩。后来，这些都是荒废的，因为不习惯于码字长文，于是经常在玩的也就是Twitter、Instagram、Foursquare等社交类app，以及可以串联这些服务的Tumblr。

近期觉得Tumblr此前被廉价出售，会不会服务停摆。于是，就想着把Tumblr的文章导出来。


# Gridea 和 GitHub Page

很早看到利用GitHub Page来创建个人博客的各类技术类文章，但每每看到命令行操作，就望而却步，所以一直没尝试。

最近发现一个软件，很适合我这种小白──[Gridea](https://gridea.dev/)。官网有快速上手指引文档，非常方便。当然还有其他人写的一些指引文章：

- [Gridea 让你更方便地管理 Github Pages](https://sspai.com/post/54212)
- [Gridea 上手教程——小白也可以用的 GitHub Pages 搭建工具](https://zhuanlan.zhihu.com/p/71681116)

配置完成后，就可以利用Gridea来发布文章了。

![](https://i.loli.net/2020/03/12/X5Eb9qTxsCMo8tf.png)

#  迁移

因为之前折腾过好几个博客，搭建还算顺利。每次折腾都会涉及到文章迁移问题，虽然有实质内容的文章其实并不多，毕竟文字功底较差，没东西写得出来。

目前大部分记录日常生活的内容都留在Tumblr，包括一些通过IFTTT、foursquare、Twitter、Instagram等app同步的内容。怎么将这些内容方便的迁移到GitHub Page是一个问题。

通过谷歌搜索，发现能从Tumblr直接迁移GitHub Page的工具几乎没有，难道要一个个手动导入？考虑到Gridea通过将MarkDown文档直接复制到posts文件夹可以直接导入文章。那么Tumblr是否能将文章导出为MarkDown文档呢？然而不可以，Tumblr导出的是xml格式的备份。如果Tumblr不行，Tumblr──Wordpress──MarkDown路径是否可行呢？可行，但是Wordpress需要安装专用插件，而安装插件需要收费。最后，兜兜转转，找到一个工具，xml转MarkDown的工具[wpXml2Jekyll](https://github.com/theaob/wpXml2Jekyll/releases)。

将转换完成后的MarkDown文档复制至Gridea的posts文件夹后，重新启动Gridea发现首页会一片空白、原有设置会全部消失。琢磨了很久应该是导入文档不匹配的问题，最终还是需要手动一个个修改MarkDown文档的信息头来解决这个问题，必须按照Gridea的MarkDown文档来。

```
title: '再一次尝试博客' //文章标的
date: 2020-03-11 20:40:00 //文章日期
tags: [博客,Github page,Gridea] //Gridea只支持tag
published: true //是否发布
hideInList: false //是否在列表中隐藏
feature: 
isTop: false  //是否置顶
```

所以，一个个手动用Typora修改文档，然后顺便回顾了以下以前的记录，也是好玩的，顺便把一些无关的Twitter推文和Instagram图片删除了。修改完成后，再将文档复制到posts文件夹后，重启Gridea就正常了。

# 域名

是否要买一个域名？这是个问题。虽然目前自带域名*.github.io这个域名已经够短，但是看起来总有点别扭。一直想要个自己的域名，可如果发挥不了它的作用也有点浪费吧？之前有用过免费的tk域名，貌似也不错。

在[namesilo](https://www.namesilo.com/)找了几个域名，com太贵，me不错。

有句话，免费的往往是最贵的。之前用过一个tk域名，后来发现被别人注册了，域名商能随时收回免费域名。所以在写这篇文章的过程中，最终在[namesilo](https://www.namesilo.com/)入手了**shaojin.me**域名。中间，设置域名搞了很久，很久没玩一下子搞不清怎么设置后台了。

# 能坚持多久？

鬼知道能坚持多久!说不定搞定这些东西，发现学到了什么，而写文章从来不是什么重点，第二天就束之高阁。












