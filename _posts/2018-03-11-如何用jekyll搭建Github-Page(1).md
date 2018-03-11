---
layout: post
title:  "如何用jekyll搭建Github Page(1)"
date:   2018-03-11 16:12:51 +0800
categories: Web
tags: Jekyll
description: 面向小白的Github Page搭建方法.
---

## 前言

其实关于用jekyll搭建Github Page的方法，在各个博客园，知乎上都有人对此编写一些实用教程，但奈何部分教程时间久远，或是信息不全，使得我们在按照这些教程进行博客搭建时，总会遇到这样或那样的问题（我也其中的受害者之一），于是我把自己进行搭建博客的经历做了一个总结，帮助后面的小伙伴们避开这些大坑。好了，话不多说，我们开始！

***

## 步骤

总结下来，搭建这个博客可以分为以下几步：

### 1. Github网站上的相关准备工作

* 在github上创建一个名为 *username.github.com* 的远程仓库，其中 **username** 请用你自己的用户名替换。注意，是用户名！即登陆后点击右上角所显示的名字，如下图：

<div align="center">  
  <img src="{{ site.baseurl }}/assets/images/ref01.PNG"/>
</div>
​        在创建后，你会发现，仓库名一定是全为小写的，如：
<div align="center">  
  <img src="{{ site.baseurl }}/assets/images/ref02.PNG"/>
</div>

* 在本地的任意位置上创立一个名为 *username.github.com* 的文件夹，同样的，其中 **username** 请用你自己的用户名替换, 且注意将用户名中的字母 *统一小写* ，与github上的远程仓库名保持一致，这个文件夹日后就是你的本地仓库。
* 在github上找到一个心仪的博客模板，使用*Git* 工具将它拉入你的本地仓库。
* 将本地库与远程仓库连接起来，将本地内容上传到远程仓库中。
* 在浏览器中输入 *username.github.io* , 现在你就可以看到自己的博客的初稿了 (注：有的资料可能会说输入 *username.github.com* ，但其实这已经是过时的写法了。)

  如果看了上面这些过程后，你还是一脸懵逼，那说明可能你还缺少关于**Git**工具的了解，但        也没关系，请看下面这个链接，你会得到很大的帮助：
  > 关于上述部分，我推荐大家去看一下csdn上的一篇博文 [手把手教你用github pages搭建博客](http://blog.csdn.net/superjimmy/article/details/51626842)。可以说，这篇博文的作者确实是面向小白所写的教程，每一步都图文并茂，十分详细，但遗憾的是，对于这篇文章，我们只能看到[博客搭建](http://blog.csdn.net/superjimmy/article/details/51626842#博客搭建)这部分, 后面对于jekyll的安装部分，作者的描述是有错误的。

---

### 2. 安装jekyll

* 安装ruby和gem

  关于**下载** 而言，没什么好说的。对于 Windows 用户而言，非常简单，从 [rubyinstaller.org](http://rubyinstaller.org/downloads/) 下载即可。而 Mac 用户，最新版的系统已经自带，无需处理。

  > 但我需要说明的是，由于官方已经发布了*Ruby2.4.3* ，可能进入改网站后，会推荐你首选该版本，但对此，我建议你还是选择**Ruby2.3.3** 版本 (至于是x86还是 x64，那要取决于你的操作系统是32位还是64位了）, 因为在安装Ruby2.4.3后，在进一步安装jekyll时，会有一些奇怪的报错，但由于Ruby2.4.3暂时是比较新的版本，目前搜不到对此的解答，所以建议大家有坑慎入。  

  ##### 另外 ，注意在安装时勾选将Ruby添加到Path中。

  判断安装是否成功的标准是, 在命令行(cmd/PowerShell)中输入如下指令：

    ```
  ruby -v
    ```

  如果出现了版本号，则恭喜你，说明下载成功。同时，Ruby安装时会附带一起安装gem, 我们可以通过 

  ```
  gem -v
  ```

  查看版本号，判断其是否安装成功。若无gem，请通过[rubygems.org](https://rubygems.org/pages/download)自行选择版本下载，下载完成后，进入它的解压目录，在命令行中输入如下指令进行安装：

  ```
  ruby setup.rb
  ```

* 安装Development Kit(仅对于Ruby 2.4以下)

  该下载链接位于[rubyinstaller.org](http://rubyinstaller.org/downloads/)左下角，选择   合适的版本，下载即可。

  完成下载后，进入它的解压后所在的文件目录，打开命令行，依次输入下面两条指令, 即完成安   装

  ```
  ruby dk.rb init
  ruby dk.rb install
  ```

* 安装jekyll

  是不是觉得安装过程过于繁琐，但没事，再坚持一下，我们马上就要成功了！

  由于一些原因，我们可能无法访问到国外的gem下载源，所以我们需要手动地将下载源设为国内   的下载地址：

  ```
  gem sources --remove https://rubygems.org/  #注：windows下可能要将该条指令的https改为http
  gem sources -a https://gems.ruby-china.org/
  ```

  接下来，我们正式开始安装：

  ```
  gem install bundler
  gem install jekyll
  ```

  现在，我们就成功安装了jekyll。进入我们之前创建的本地仓库，调出命令行输入

  ```
  jekyll server
  ```

  我们会惊喜地发现，报了错！你可能看到如下错误：

  > Dependency Error: Yikes! It looks like you don’t have jekyll-paginate   or one of its dependencies installed. In order to use Jekyll as           currently configured, you’ll need to install this gem. The full error     message from Ruby is: ‘cannot load such file – jekyll-paginate’ If you   run into trouble, you can find helpful resources at Getting Help

  > jekyll 3.1.2 | Error: jekyll-paginate

  这个时候不要着急，不就是缺个 jekyll-paginate吗，我们补上就行了：

  ```
  gem install jekyll-paginate
  ```

  至此，再次输入jekyll server指令，然后在浏览器中打开[lhttp://ocalhost:4000](http://localhost:4000), 你就能在本地欣赏到自己的博客界面了。

  ---

### 3. 发布你自己的博文

​      这部分因为模板众多，很难说出一个具体的操作，但一些大体的框架可以参看这篇:  [github上利用jekyll搭建自己的blog的操作顺序](https://www.zhihu.com/question/30018945/answer/50507749)



