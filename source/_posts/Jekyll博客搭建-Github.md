---
layout: post
title:    "Jekyll博客搭建-Github"
index_img: https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Jekyll博客搭建-Github/下载.png
date:   2019-12-01  10:7 
updated: 2019-12-01  10:7 
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 03-瞎折腾
---


介绍本地搭建 **Jekyll** 博客的详细过程，实现博客文档修改和本地实时预览；并进一步介绍如何将本地博客部署到 Github 仓库中，实现线上访问。所有内容都是基于2019年11月最新版 Jekyll 讲解。

<!--more-->

**官方网站：**

1.[Jekyll中文官网](https://www.jekyll.com.cn/)  
2.[Jekyll英文官网](https://jekyllrb.com/)

# 安装Jekyll

## 1、安装Ruby

1、从[RubyInstaller](https://rubyinstaller.org/downloads/)官网下载并安装 **Ruby + Devkit** 版本，选择第一个最新版本就行。  
使用默认选项进行安装，**不要更改安装路径**（我这里改了，尝试过一次，会在后面无法安装Jekyll），注意下图两个选项一定要勾选（默认已勾选）

![install_ruby](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Jekyll博客搭建-Github/install_ruby.png)

安装完成后，会自动弹出 **cmd.exe** 提示安装 MSYS2，它是用来编译 Ruby 本地包的，我们需要同时键入 `1,2,3`，一次完成所有的安装（建议一个个依次安装）。这里因为众所周知的网络原因，速度奇慢无比，如果网络状况良好，能够一次装成功或者以失败告终。 失败了就重新尝试（或翻墙），这一步是没法跳过的。

这里如果没有弹出命令行 MSYS2 安装界面或者把它关掉了，那么可以重新打开**cmd命令行**，输入 `ridk install` 来再次进入MSYS2安装界面。 

所有的安装完成后，建议再次重新来一遍，直到没有明显的安装过程，只提示`已经未最新`等则表示安装完成。

**参考链接：**

- [What do these RubyInstaller 2.4 components do?](https://stackoverflow.com/questions/44229081/what-do-these-rubyinstaller-2-4-components-do)
- [快速在 Windows 上搭建 Jekyll 开发环境](https://blog.walterlv.com/post/setup-jekyll-in-windows.html)
- [Windows安装Jekll](https://segmentfault.com/a/1190000010195733)
- [Jekyll、MSYS2、Vibora 及在 Windows 下用 Linux](https://kaffa.im/jekyll-msys2-vibora-and-use-linux-on-windows.html)

![enter description here](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Jekyll博客搭建-Github/1575169649155.png)

安装完成后，关闭上面的cmd命令行再重新打开或在`...press ENTER []`后敲回车，分别输入 `ruby -v`和 `gem -v` 查看版本，确认安装完成

![check_ruby](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Jekyll博客搭建-Github/check_ruby.png)


## 2、安装Jekyll

接下来就是正式安装Jekyll了，在cmd中输入 `gem install jekyll bundler`,这里实际还安装了一个 **bundler**，[官方建议](https://www.jekyll.com.cn/docs/ruby-101/#bundler)安装，咱就安装吧。如果失败你可能需要输入 `sudo gem install jekyll bundler` 重新安装。安装完成输入`jekyll -v` 检查版本，确认安装完成。

> 前面如果在安装 Ruby 时更改了安装路径，这里的Jekyll 很可能会安装失败，具体解决办法也没找到，最后还是保持了默认安装路径才解决，卸载Ruby时一定要删除干净，不然再安装会自动安装到上一次安装时的目录，可以使用 **everything** 软件，搜索 "ruby" 彻底删除相关文件。

**参考链接：**  
以下为解决`gem install jekyll bundler`安装错误的参考信息，都没啥帮助，还是自己重新安装ruby解决，留作记录。
- [安装jekyll及初步使用](https://pashanhu.github.io/pages/jekyll/)
- [windows10安装jekyll和ruby环境](https://mojotv.cn/2019/07/13/install-jekyll-in-windows10)
- [ruby - 安装jekyll时发生错误：错误：无法构建gem本机扩展](http://www.ojit.com/article/1114873)
- [gem インストール時に発生したエラーとその解決方法まとめ](https://kzy52.com/entry/2014/11/09/000511)
- [\[Install Windows\] ERROR: Failed to build gem native extension.](https://github.com/jekyll/jekyll/issues/7000)
- [Installing Jekyll: ERROR: Failed to build gem native extension](https://github.com/jekyll/jekyll-help/issues/209)
- [Error installing jekyll: ERROR: Failed to build gem native extension](https://stackoverflow.com/questions/51699761/error-installing-jekyll-error-failed-to-build-gem-native-extension)
- [windows系统下安装jekyll报错：Error installing jekyll](https://segmentfault.com/q/1010000013418668)

## 3、新建博客

接着使用 `jekyll new myblog` 命令在 ./myblog 目录下创建一个全新的 Jekyll 网站，这里文件位置可以自己指定，我是安装在D盘(先进入D盘，再执行指令，涉及[cmd命令](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/cmd))。等待几分钟安装完成。

![add_myblog](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Jekyll博客搭建-Github/add_myblog.png)

`cd myblog`进入新创建的目录，再输入`bundle exec jekyll serve` 构建网站并启动一个本地 web服务，在浏览器中打开 `http://127.0.0.1:4000` 网址访问网站。

![add_site](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Jekyll博客搭建-Github/1575180879786.png)

`bundle exec jekyll serve`这个命令后面会经常用到，每次我们更改了博客的**配置文件（.yml）**，都需要执行这个命令，本地网站才会刷新显示，但如果已经部署到github上，那么你编辑后直接推送上去就行，GitHub会自动构建，但是要想本地网站刷新还是需要运行该命令。

## 目录结构

**官方文档**：[目录结构](https://www.jekyll.com.cn/docs/structure/)

我们可以打开我们的博客文件夹，其格式如下：

```
.
├── _posts
│   └── 2018-03-31-welcome-to-jekyll.markdown 
├── _config.yml  
├── 404.html  
├── about.md
├── Gemfile
├── Gemfile.lock 
└── index.html
```

|    _posts             | 	 存放博客文章  |
| --------------------- | --- |
|    _config.yml     |  网站的一些配置信息    |
|    404.html         | 无法访问404界页面  |
|    about.md       |  关于页面  |
|    Gemfile          |  跟踪构建Jekyll站点所需的gems和gem版本。最新版本才有|
|    Gemfile.lock  |  跟踪构建Jekyll站点所需的gems和gem版本。最新版本才有   |
|    index.html     |  网站主页面   |

我们以后写的博客文章都会放在 **\_posts** 文件夹下，而修改一些关于网站的基本信息和配置则是 **\_config.yml** 文件。**about.md**是**关于我**的页面内容。其他的可以不用管。

这里为止，我们已经成功在本地搭建了 Jekyll 博客系统，后面我们将会将其部署到GitHub上，使得可以通过互联网访问。这样才算完整搭建博客。

# Github部署

现在我们将本地博客网站部署到 GitHub 上，使得我们的博客可以线上访问。

**常用命令：**

- gem install jekyll  #使用gem安装Jekyll
- jekyll new blog    #使用Jekyll创建你的博客站点
- cd myblog      #进入myblog目录,记得一定要进入创建的目录，否则服务无法开启   
- jekyll serve  #开启Jekyll服务 
- bundle exec jekyll serve  #在Jekyll自带的本地服务器上预览你的项目，
bundle exec 表示在当前项目依赖的上下文环境中执行命令 jekyll serve

## 1、注册 Github

首先你需要注册 **Github** 账号，并新建一个仓库（请自行搜索），仓库里面不需要任何东西。仓库名要和你的用户名一致。如：    
**我的用户名为：wifimake，则仓库名为：wifimake.github.io**

## 2、安装 Github Desktop

我们需要 [Github Desktop](https://desktop.github.com/) 软件帮助我们将文件推送到自己的GitHub账户仓库中完成部署。

安装和使用方法见：[GitHub Desktop使用指南](https://lonlypan.com/archivers/GitHub-Desktop使用指南)，**注意克隆仓库时一定要克隆和用户名相同的仓库。**

## 3、部署

找到我们克隆仓库的本地文件夹，将博客文件（myblog下的所有文档）复制到本地仓库下（wifimake.github.io文件夹下），随后使用 Github Desktop 推送到线上，如此便完成线上部署，这时可以打开链接 `https://wifimake.github.io/` 即可访问我们的博客了。

## 4、自定义域名

虽然在Internet上可以访问我们的网站，但是用的 Github 地址，而我们想使用我们自己的个性化域名，这就需要绑定我们自己的域名。登录到阿里云，进入管理控制台的域名列表（你需要先买一个域名），找到你的域名，进入解析。  按下图输入解析信息。

 - @：可以使用  xxx.com解析
 - www：使用www.xxx.com解析
- 记录值：仓库地址
至于别的教程说的要添加自己 GitHub 的 **IP** 地址，经过实测，并不需要。

![解析](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Hexo博客搭建-Github/解析.png)

登录 GitHub，进入之前创建的仓库，点击 **settings**，设置 **Custom domain**，输入你的域名，点击 **save** 保存。

![github_site](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Hexo博客搭建-Github/github_site.png)

进入本地博客文件夹 **wifimake.github.io** 目录下，创建一个记事本文件，输入你的域名，如果带有www，那么以后访问的时候必须带有www完整的域名才可以访问，如果不带有www，以后访问的时候带不带www都可以访问。所以建议，不要带有www。  
保存时，命名为CNAME （后缀**.txt** 不要），注意保存成**所有文件**而不是txt文件。

![CNAME](https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/Hexo博客搭建-Github/CNAME.png)

使用 Github Desktop 推送到线上，稍等片刻，就可使用域名访问了。

# 后期维护

## 新建博客

\_posts目录是专门存放博客文件的，你可以使用markdown或者html格式的文件来写博客，我个人是用markdown格式写的。但是不管是哪种格式的文件都需要包含 YAML 头信息， Jekyll 才会把它当做一个特殊的文件来处理。

在_posts目录下新建一个markdown文件，头信息必须在文件的开始部分，并且需要按照 YAML 的格式写在两行三虚线之间。如下所示：

> 博客文章需要 Markdown 语法，相关资料有很多，请自行搜索。  
文章头信息需要 YAML 语法，相关资料有很多，请自行搜索。

```
---
layout: post
title:  "Welcome to Jekyll!"
date:   2019-12-07 11:27:05 +0800
categories: jekyll update
---

...
这里就可以使用markdown格式或其他格式写博客内容
```

|    变量名称   |  描述  |
| ---------------- | --- |
|      layout      |  指定使用该模板文件。指定模板文件时候不需要文件扩展名。模板文件必须放在\_layouts 目录下。  |
|        title        |   文章名  |
|      date        |   这里的日期会覆盖文章名字中的日期。这样就可以用来保障文章排序的正确。日期的具体格式为YYYY-MM-DD HH:MM:SS +/-TTTT；时，分，秒和时区都是可选的。  |
|  categories   |  指定博客的一个或者多个分类属性   |

文章写完后，使用 `bundle exec jekyll serve` 可以进行本地预览，或使用Github Desktop推送到Github上，发布文章。

> 建议先使用本地预览，速度是实时的；而推送线上是需要等待几分钟才会看到更新的。

 
## 网站配置

Jekyll 有这非常灵活和强大的配置功能，既可以在网站根目录下的 \_config.yml 文件中进行配置，也可以作为命令行参数来配置。默认配置大致如下：

yml文件使用了YAML语法，如果想更好的理解Jekyll就需要了解一下YAML语法的内容。
> 配置修改需要 YAML 语法，相关资料有很多，请自行搜索。

一般主题中也会自动帮你写好这些自定义属性，搭建你自己的博客时你只需要将这些自定义属性改为你想要的值即可。如下是官方默认主题的配置文件，冒号 ':' 后的内容都可以自定义修改，多多尝试。

```
title: Your awesome title #你的网站名
email: your-email@example.com #有的邮箱地址
description: >- #你的网站描述，下面一大段都是。
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # 网站副链接，填在引号中 the subpath of your site, e.g. /blog
url: "" # 网站主链接，填在引号中 the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jekyllrb  #账户名
github_username:  jekyll  #账户名
```

## 主题修改

这个问题就比较复杂了，由于 Jekyll 官方更新频繁，且属于不向下兼容，导致此类教程随时不可用。这次 Jekyll 更新4.0版本，文件夹格式都变了，主题文件夹直接和博客文件夹分离，所以另开一文专门介绍。

## 注意事项

文档格式一定要是 UTF-8 格式，GB2312 格式实际测试上传后并不会显示。

**参考链接**

- [码志](https://mazhuang.org/)
- [Jekyll中文官网](https://www.jekyll.com.cn/)
- [如何快速给自己构建一个温馨的"家"——用Jekyll搭建静态博客](https://www.jianshu.com/p/9a6bc31d329d)
- [Jekyll博客系统初探(持续更新中...)](https://www.jianshu.com/p/558a5d50e077)
- [Jekyll + Github Pages 博客搭建入门](https://www.jianshu.com/p/9f198d5779e6)
- [Github+Jekyll 搭建个人网站详细教程](https://www.jianshu.com/p/9f71e260925d)
- [jekyll+github搭建个人博客](https://www.cnblogs.com/yehui-mmd/p/6286271.html)
- [\[Jekyll\] permalink -- 修改文章的链接地址](https://my.oschina.net/u/3729927/blog/1931051)
- [jekyll-theme-EasyBook主题](https://github.com/laobubu/jekyll-theme-EasyBook)
- [Jekyll 博客系列-视频教程](https://www.youtube.com/watch?v=Zt_QzSbyDcw&list=PLK2w-tGRdrj7vzX7Y-GqKPb2QPrHCYZY1&index=1)

