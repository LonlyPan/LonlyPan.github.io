---
layout: post
title:    "Hexo博客搭建-Github"
index_img: https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/Hexo博客搭建-Github/662949ef-1d9a-4b56-bc56-5e26c9936cf1.png
date:   2022-05-03  10:27 
categories: 网页
---

如何使用github和hexo搭建个人博客
- Hexo 博客框架。使用 Markdown解析文章，利用主题生成静态网页。
- Github 一个托管网站
- 小书匠拥有 文章试试在线编写

## 安装Hexo

这是 Hexo 的官方中文教程网站：[Hexo官方教程中文](https://hexo.io/zh-cn/docs/)  
教程内容是根据自己个人情况书写的安装教程，包含一些官方未提及到的错误。

- 安装环境：Win10
- 第一次安装Hexo

<!--more-->

### 1、Git

Windows：下载并安装 [git](https://git-scm.com/download/win)。  
中国大陆地区用户，可以前往 [淘宝 Git for Windows 镜像](https://npm.taobao.org/mirrors/git-for-windows/) 下载 git 安装包。

安装过程很简单，一路默认即可，你可以自己更改安装路径。安装完成后，在笔记本左下角搜索 ~~PowerShell~~，（PowerShell 和 hexo 有兼容性问题，建议使用 **cmd命令操作符** 操作，两者不同软件，一样效果；且建议右键以**管理员身份运行**）。  
输入`git version`回车，就会显示你已经安装的git版本，证明安装成功。

![powershell_87](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/powershell_87.png)

### 2、安装Node.js

Windows：官方的 [安装程序](https://nodejs.org/en/download/)。  
对于中国大陆地区用户，可以前往 [淘宝 Node.js 镜像](https://npm.taobao.org/mirrors/node) 下载。

安装过程同样保持默认即可，可自行更改安装路径。完成后，需要重新打开 `PowerShell`（关闭之前的重开），输入 `node -v`，若显示安装的版本信息，表示安装成功。

![node_-v](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/node_-v.png)

### 3、安装Hexo

Hexo 就是我们的个人博客网站的框架， 这里需要自己在电脑常里创建一个文件夹，可以命名为 hexo blog，Hexo 框架与以后你自己发布的网页都在这个文件夹中。创建好后，进入文件夹中，按住**shift键，右击鼠标**，点击命令行

![Hexo_blog](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/Hexo_blog.png)

在 PowerShell 输入 `npm install -g hexo-cli `,安装完成后，命令行会出现下面两个警告：

![hexo_warringpng](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/hexo_warringpng.png)

这是因为 mac 下需要 fsevents，在 windows 或 linux 环境下，请忽略这个错误。由于使用的 Win10，所以会出现，忽略它。  
这里也提醒大家，安装过程中若出现明显的警告、错误、不一致，请多多百度、谷歌解决，每个人都有不同的错误，教程也无法面面俱到。

**参考链接：**
- [解决npm WARN notsup SKIPPING OPTIONAL DEPENDENCY](https://www.jianshu.com/p/395edd93fd6f)    
- [npm WARN notsup SKIPPING OPTIONAL DEPENDENCY](https://stackoverflow.com/questions/40226745/npm-warn-notsup-skipping-optional-dependency-unsupported-platform-for-fsevents)


### 4、新建博客

上述安装完成后，再输入 `hexo init my_blog` ,这里 **my_blog** 是你博客网站的保存文件夹，可自行命名。这个安装过程比较长，耐心等待，安装完成后如下：

![fsevent_warring](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/fsevent_warring.png)

这里我们依然会看到几个警告，但仔细查看每条警告都是关于  fsevents 的，所以忽略它。

接着输入命令 `cd my_blog` 进入博客文件夹下，再输入`npm install`，同样会显示警告，忽略。
> 注意这一步很多老的教程没有，现在已经改了，官方教程中视频也是有这一步的，少了这一步，经测试，是无法进行后面的命令操作的。

![npm_fsevent_warring](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/npm_fsevent_warring.png)

这里我在实际操作时，记得可能无法新建博客，会提示`nodemon运行 提示错误：无法加载文件 C:\Users\gxf\AppData\Roaming\npm\nodemon.ps1，因为在此系统上禁止运行脚本。` 错误。这是你笔记本禁止运行脚本。  
**解决办法：**  
1. 管理员身份打开powerShell  
2. 输入`set-ExecutionPolicy RemoteSigned`  
![set-execution](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/set-execution.png)
3. 选择 Y 或者 A ，就好了  

接下来就是创建博客页面了，输入命令`hexo g` 创建页面，我们会发现显示信息不对劲，似乎少了什么东西，如下图左半部分。这是 Hexo 和 windows powershell 兼容行问题，实际已经创建完成了。

不放心的读者可以打开**cmd命令提示符**工具，这里默认是在C盘下，我们需要重新定位到 **my_blog** 文件夹下，按照下图右半部分红框内提示进入该文件夹，并重新输入命令 `hexo g`，会发现似乎没啥变化，那是因为刚在 PowerShall 中已经创建完成了。这里我们将 **my_blog** 文件夹下的 **public** 文件夹删除（该文件夹就是 hexo g命令新建的），再次输入 `hexo g`，就会看到完整的安装信息了。

![hexo_g](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/hexo_g.png)

接着输入`hexo s`,会显示本地的网站地址，复制到浏览器，就可以看到我们建立好的博客网站。

![local_site](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/local_site.png)

到这里为止，博客已经建立完成了，但这一切都只存在**本地-自己电脑**上，别人是无法通过互联网看到的，这时我们就需要借助 Github，将我们的本地博客部署-搬运上去，才能实现联网访问。

## 写作部署

### hexo常用命令简写

 - hexo n "新的博文" == hexo new "新的博文" #新建文章
 - hexo g == hexo generate #生成
 - hexo s == hexo server #启动服务预览
 - hexo d == hexo deploy #部署
 - hexo server  #Hexo会监视文件变动并自动更新，无须重启服务器
 - hexo server -s #静态模式
 - hexo server -p 5000 #更改端口
 - hexo server -i 192.168.1.1 #自定义 IP
 - hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令

- hexo new draft "draft title"  #新建草稿，保存在博客中但不显示
- hexo new page "page title"  

**那么我们新建一篇博客文章的顺序就是：**
1. hexo n "new post" 
2. hexo g
3. hexo s（至此可以实现本地访问）
4. hexo d（只有先部署到Github上才行）

### 一键部署

首先你需要注册 **Github** 账号，并新建一个仓库（请自行百度），仓库里面不需要任何东西。仓库名要和你的用户名一致。如：    
**我的用户名为：wifimake，则仓库名为：wifimake.github.io**

输入`npm install hexo-deployer-git --save`  指令安装 git，同样忽略警告。  
使用指令 `npm list hexo-deployer-git`可以查看 git 版本检查是否安装成功。

![add_git](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/add_git.png)

修改_config.yml文件。更改后如下：

```
deploy:
  type: git
  repo: https://github.com/wifimake/wifimake.github.io.git
```

- repo	库（Repository）地址，不要忘了加上 **.git**
- branch	分支名称。如果不指定，则默认值为 master,可不填
-  `https://github.com/wifimake/wifimake.github.io`就是你的仓库地址

保存好文件后，依次输入指令：`hexo g` ，`hexo d`，就会开始自动上传部署，完成后，打开浏览器，在地址栏输入你的个人网站的仓库路径，即 http://xxxx.github.io ，就可以在线访问网站了。

这里在输入`hexo d` 指令部署时，可能会弹出 Github 的登陆界面，直接登陆就好了，这之后可能让你在指令界面再次输入用户名和密码，直接输入回车即可，然后继续等待部署结束。

另一个问题是，这里使用 PowerShell 操作时，在弹出的 Github 登陆界面中无法输入密码或者登陆不成功，建议切换到 **cmd命令操作符** 操作。

### 绑定域名

虽然在Internet上可以访问我们的网站，但是用的 Github 地址，而我们想使用我们自己的个性化域名，这就需要绑定我们自己的域名。登录到阿里云，进入管理控制台的域名列表（你需要先买一个域名），找到你的域名，进入解析。  按下图输入解析信息。

 - @：可以使用  xxx.com解析
 - www：使用www.xxx.com解析

至于别的教程说的要添加自己 GitHub 的 **IP** 地址，经过实测，并不需要。

![解析](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/Hexo博客搭建-Github/解析.png)

登录 GitHub，进入之前创建的仓库，点击 **settings**，设置 **Custom domain**，输入你的域名，点击 **save** 保存。

![github_site](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@master/hexo_images/Hexo博客搭建-Github/github_site.png)

进入本地博客文件夹 ，进入**my_blog/source** 目录下，创建一个记事本文件，输入你的域名，如果带有www，那么以后访问的时候必须带有www完整的域名才可以访问，如果不带有www，以后访问的时候带不带www都可以访问。所以建议，不要带有www。  
保存时，命名为CNAME （后缀**.txt** 不要），注意保存成**所有文件**而不是txt文件。

![CNAME](https://cdn.jsdelivr.net/gh/LonlyPan/LonlyPan.github.io@hexo_source/hexo_images/Hexo博客搭建-Github/CNAME.png)
进入blog目录中，按住shift键右击打开命令行，依次输入
```
hexo g
hexo d
```
稍等几分钟，打开浏览器在地址栏输入你的域名将会直接进入你自己搭建的网站。

## 参考链接

- [hexo官网](https://hexo.io/zh-cn/)
- [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)
- [hexo-Anisina主题](https://github.com/Haojen/hexo-theme-Anisina)
- [Jekyll和Hexo详细对比](https://zuoridangnian.com/archives/3043)
- [我的个人博客之旅：从jekyll到hexo](https://blog.csdn.net/u011475210/article/details/79023429)
- [nodemon运行 提示错误：无法加载文件 C:\Users\gxf\AppData\Roaming\npm\nodemon.ps1](https://www.cnblogs.com/xiaofenguo/p/11511789.html)
- [使用 GitHub Actions 自动部署 Hexo 博客到 GitHub Pages](https://zhuanlan.zhihu.com/p/161969042)
- [Hexo使用GitHub Action进行自动部署](https://www.igerm.ee/cn/tech/Hexo%E4%BD%BF%E7%94%A8GitHub%20Action%E8%BF%9B%E8%A1%8C%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2/)
- [利用 Github Actions 自动部署 Hexo 博客](https://sanonz.github.io/2020/deploy-a-hexo-blog-from-github-actions/)
- [Github Actions 初体验之自动化部署 Hexo 博客](https://razeen.me/posts/use-github-action-to-deploy-your-hexo-blog/)
- [使用 GitHub Actions 自动部署 Hexo 博客](https://printempw.github.io/use-github-actions-to-deploy-hexo-blog/)
- [hexo配合github action 自动构建（多种形式）](https://segmentfault.com/a/1190000040767893)
- [如何使用 GitHub Actions 自动部署 Hexo 博客](https://juejin.cn/post/6943895271751286821)
- [GitHub Action 实现 Hexo 博客持续集成](https://blog.linioi.com/posts/9/)
- [Hexo 排坑日记（持续更新）](https://www.erenship.com/posts/7487.html#) 


