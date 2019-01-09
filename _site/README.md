# 毛毛虫_Wendy

我的个人博客：<http://mmcwendy.info>，欢迎 Star 和 Fork。

## 概览

<!-- vim-markdown-toc GFM -->

* [效果预览](#效果预览)
* [Fork 指南](#fork-指南)
* [经验与思考](#经验与思考)
* [致谢](#致谢)

<!-- vim-markdown-toc -->

## 效果预览

**[在线预览 &rarr;](http://mmcwendy.info)**

![screenshot home](http://mmcwendy.info/assets/images/screenshots/home.png)

## Fork 指南

Fork 本项目之后，还需要做一些事情才能让你的页面「正确」跑起来。

1. 正确设置项目名称与分支。

   按照 GitHub Pages 的规定，名称为 `username.github.io` 的项目的 master 分支，或者其它名称的项目的 gh-pages 分支可以自动生成 GitHub Pages 页面。

2. 修改域名。

   如果你需要绑定自己的域名，那么修改 CNAME 文件的内容；如果不需要绑定自己的域名，那么删掉 CNAME 文件。本人在 https://sg.godaddy.com 上购买的域名，第一次用，仅仅在CNAME添加了自己购买的域名，但是实际上还需要解析域名。详见[域名解析][5]

3. 修改配置。

   网站的配置基本都集中在 \_config.yml 文件中，将其中与个人信息相关的部分替换成你自己的，比如网站的 url、title、subtitle 和第三方评论模块的配置等。

   **评论模块：** 目前支持 disqus、gitment 和 gitalk，选用其中一种就可以了，推荐使用 gitment。它们各自的配置指南链接在 \_config.yml 文件的 Comments 一节里都贴出来了，搭建过程
[参见资料][6]

   **注意：** 如果使用 disqus，因为 disqus 处理用户名与域名白名单的策略存在缺陷，请一定将 disqus.username 修改成你自己的，否则请将该字段留空。我对该缺陷的记录见 [Issues#2][3]。

4. 删除我的文章与图片。

   如下文件夹中除了 template.md 文件外，都可以全部删除，然后添加你自己的内容。

   * \_posts 文件夹中是我已发布的博客文章。
   * \_drafts 文件夹中是我尚未发布的博客文章。
   * \_wiki 文件夹中是我已发布的 wiki 页面。
   * images 文件夹中是我的文章和页面里使用的图片。

5. 修改「关于」页面。

   pages/about.md 文件内容对应网站的「关于」页面，里面的内容多为个人相关，将它们替换成你自己的信息，包括 \_data 目录下的 skills.yml 和 social.yml 文件里的数据。

6. 增加博客阅读统计功能。
   使用的是leancloud，参照的[博客来源][7]
## 经验与思考

* 因为偶然的机会知道github page可以自己建立博客，也很幸运的找到了很多 [模板][4]，github资源真是太丰富了。
* 虽然有很好的模板，但是很多东西还是需要自己的理解，了解最基本的原理，避免走弯路。
* 好好利用这些资源，记录知识，必定受益无穷。

## 致谢

本博客外观
基于 [DONGChuan](http://dongchuan.github.io) 
基于 [Zhuang Ma](http://mazhuang.org/)
修改，非常感谢！

[1]: https://github.com/mzlogin/chinese-copywriting-guidelines
[2]: https://help.github.com/articles/setting-up-your-pages-site-locally-with-jekyll/
[3]: https://github.com/mzlogin/mzlogin.github.io/issues/2
[4]: http://jekyllthemes.org/
[5]: https://www.zhihu.com/question/31377141
[6]: https://imsun.net/posts/gitment-introduction/
[7]: http://blog.csdn.net/u013553529/article/details/63357382
