---
/_posts/image/art_pig.jpgtitle: 【记】Github和Gitee上搭建博客
date: 2020-03-27 14:32:20
category: 程序员的优雅之路
tags:
- GitHub / Gitee
- Blog
- Next主题
description: 那些年我们走过的那些坑
typora-root-url: ..
---

折腾了可能有3、4天，才用  [Jekyll](http://jekyllcn.com/) + [Next](https://github.com/Simpleyyt/jekyll-theme-next) 搭建好个人博客（`Windows` 配置 `Jekyll` 挺不友好的），其中参考了很多教程和博客，最后正常运行，但是其中的原理还是没有太明白。



梳理下大概的流程，以后有时间在新系统上重新配置一遍：



# Windows下 Jekyll的Next主题搭建



### 安装Ruby

 - 下载[Ruby官网的Windows版本exe](https://rubyinstaller.org/)（菜菜的我爱着傻瓜式的exe）
   - [官方教程](http://jekyllcn.com/docs/windows/#installation)上是用[chocolatey](https://chocolatey.org/install)作为平台再安装Ruby的，但是后面的步骤遇到一些版本不匹配问题，所以我后来舍弃了。不同人的电脑配置不同，可以尝试。
 - 运行exe到最后一步，先不启动Windows PowerShell（不打勾），换下源（不然速度太慢，参考 [this blog](https://blog.csdn.net/mscf/article/details/82627951)）
 - 管理员身份运行cmd，输入“ridk install”并回车，安装1和3
   - 看网上的说法2非必要，然后我自己装了2出现了点问题，卸载了单独装1+3才好的，我也解释不清楚。





### 利用 gem 安装 nokogiri （不确定是不是必要项）

 - 先换源：（完事第一步）

   ```
   gem sources -a http://gems.ruby-china.com/
   ```

 - [官方教程](http://jekyllcn.com/docs/windows/#installation)的说法是很多参数的命令行：

   ```
   gem install nokogiri --^
      --with-xml2-include=C:\Chocolatey\lib\libxml2.2.7.8.7\build\native\include^
      --with-xml2-lib=C:\Chocolatey\lib\libxml2.redist.2.7.8.7\build\native\bin\v110\x64\Release\dynamic\cdecl^
      --with-iconv-include=C:\Chocolatey\lib\libiconv.1.14.0.11\build\native\include^
      --with-iconv-lib=C:\Chocolatey\lib\libiconv.redist.1.14.0.11\build\native\bin\v110\x64\Release\dynamic\cdecl^
      --with-xslt-include=C:\Chocolatey\lib\libxslt.1.1.28.0\build\native\include^
      --with-xslt-lib=C:\Chocolatey\lib\libxslt.redist.1.1.28.0\build\native\bin\v110\x64\Release\dynamic
   ```

   - 这部分因为我反复卸载用了很多方法，不确定哪个奏效，留个坑





### 利用gem安装Github-pages

 - 安装bundle，输入如下命令行：

   ```
   gem install bundler
   ```

   - 关于[bundle的作用，和Gem的关系](https://www.jianshu.com/p/bcbb278e9208)

 - 本地博客的根目录下，手动建立名为“Gemfile”的无后缀文件，在该文件中写入

   ```
   source 'http://rubygems.org'
   gem 'github-pages'
   ```

   - 如果是用别人的模板（比如在下，git了[Next主题](https://github.com/Simpleyyt/jekyll-theme-next)的模板，然后在上面改），那么就下载好文件到你的博客目录下，里面自带“Gemfile”文件的。

 - 命令行切换到本地博客的根目录，输入： 

   ```
   bundle install
   ```

- 按照道理，之后就可以正常运行了。启动：

  ```
  jekyll server
  ```

  - 如果出现“exec”啥的报错（具体不大记得了~~），就换下面的命令

    ```
    bundle exec jekyll server
    ```

    





# 修改配置 + 写博客



### 修改配置

这一部分就是在根目录下的_config.yml文件中进行修改，我列一些我用到的配置项（用文本编辑器打开，搜一个改一个 ~）



#### 网站的基本信息：

```yaml
# Site
title: 沙沙响
subtitle: Rustling
description: 佛曰：God bless you~ 
author: 沙哥
# Support language: de, en, fr-FR, id, ja, ko, pt-BR, pt, ru, zh-Hans, zh-hk, zh-tw
language: zh-Hans
date_format: '%Y-%m-%d'

```

注意：zh-Hans 即简体中文，名称的选取是源自 /_data/languages/ 目录下的文件名



#### 域名：

```yaml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com' and baseurl as '/child'
url:
baseurl:
permalink: pretty

```

如果之后挂载到 Gitee 上，那么需要配置（具体见 3.2 Gitee篇），Github实验下来貌似不需要。



#### 浏览器标签页的小标

```yaml
# Put your favicon.ico into `assets/` directory.
favicon: /assets/art_pig.jpg

```

即浏览器标签页的小图标，就像这样：

![favicon](/_posts/image/favicon.jpg)

![favicon](https://github.com/oldsandyoungman/oldsandyoungman.github.io/_posts/image/favicon.jpg)



#### 菜单的内容和相应图标

```yaml
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------

# When running the site in a subdirectory (e.g. domain.tld/blog), remove the leading slash (/archives -> archives)
menu:
  home: /
  categories: /categories/
  about: /about/
  archives: /archives/
  tags: /tags/
  #sitemap: /sitemap.xml
  #commonweal: /404.html


# Enable/Disable menu icons.
# Icon Mapping:
#   Map a menu item to a specific FontAwesome icon name.
#   Key is the name of menu item and value is the name of FontAwesome icon. Key is case-senstive.
#   When an question mask icon presenting up means that the item has no mapping icon.
menu_icons:
  enable: true
  #KeyMapsToMenuItemKey: NameOfTheIconFromFontAwesome
  home: home
  about: user
  categories: th
  schedule: calendar
  tags: tags
  archives: archive
  sitemap: sitemap
  commonweal: heartbeat

```



#### Next主题的几大样式选择

```yaml

# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces

```

Pisces 是你当前看到的样式，分为左右两栏



#### 菜单栏下方的联系方式及相应图标

```yaml
# ---------------------------------------------------------------
# Sidebar Settings
# ---------------------------------------------------------------


# Social Links
# Key is the link label showing to end users.
# Value is the target link (E.g. GitHub: https://github.com/iissnan)
social:
  #LinkLabel: Link
  GitHub: https://github.com/oldsandyoungman
  School: http://www.seu.edu.cn


# Social Links Icons
# Icon Mapping:
#   Map a menu item to a specific FontAwesome icon name.
#   Key is the name of the item and value is the name of FontAwesome icon. Key is case-senstive.
#   When an globe mask icon presenting up means that the item has no mapping icon.
social_icons:
  enable: true
  # Icon Mappings.
  # KeyMapsToSocialItemKey: NameOfTheIconFromFontAwesome
  GitHub: github
  Twitter: twitter
  Weibo: weibo
  School: university

```

这里我借鉴别人的，也是设置了 Github 和我大学的官网，也可以添加其他链接



#### 你的头像

```yaml
# Sidebar Avatar
# in directory: /assets/images/avatar.gif
#avatar:
in directory: 
avatar: assets/art_pig.jpg

```

可以用动图gif，我没试验hiahia





#### 侧栏属性

```yml
sidebar:
  # Sidebar Position, available value: left | right
  position: left
  #position: right

  # Sidebar Display, available value:
  #  - post    expand on posts automatically. Default.
  #  - always  expand for all pages automatically
  #  - hide    expand only when click on the sidebar toggle icon.
  #  - remove  Totally remove sidebar including sidebar toggle.
  #display: post
  display: always
  #display: hide
  #display: remove

  # Sidebar offset from top menubar in pixels.
  offset: 12
  offset_float: 0

  # Back to top in sidebar
  b2t: false

  # Scroll percent label in b2t button
  scrollpercent: false

```

我关注的是 `display: always`，就是无论浏览哪个页面和如何滚动，都不会省略侧边栏



#### 其他社交链接

```yaml
# Blog rolls
links_title: Links
#links_layout: block
#links_layout: inline
links:
  #Title: http://example.com/
  B站: https://space.bilibili.com/129371092

```

我挂了个B站作为例子



#### 背景动画

```yaml
# Canvas-nest
canvas_nest: true

# three_waves
three_waves: false

# canvas_lines
canvas_lines: false

# canvas_sphere
canvas_sphere: false

# Only fit scheme Pisces
# Canvas-ribbon
canvas_ribbon: false

```

这个我玩得最起劲，觉得贼几把炫酷 ~~





---

> 
>
> 其他一些高级功能我之后有时间再慢慢探索 ~~~
>
> 

---





# Github 和 Gitee 上的配置



总体而言，Github配置较为简单，因为Github服务器的配置环境版本较新，我加载下来没有出现乱码啥的<sub>（当然速度较慢是必然的）</sub>；而Gitee的环境版本较为落后，所以有时候我加载下来不对，会让有些小烦躁。

- 关于两者环境的具体说明：[this blog](https://www.cnblogs.com/xjtu-blacksmith/p/jekyll-of-pages.html)



### Github篇

1. 新建一个Repository，命名为 xxxxx.github.io（xxxxx为你的用户名）

2. 将你本地的博客全部 push 上去（关于 git 的概念和操作，建议从头学一遍[廖雪峰大佬的博客](https://www.liaoxuefeng.com/wiki/896043488029600)，毕竟作为程序员之后将与 git 常相伴随 ~~ ），具体而言

    - 安装好 git 

   - 在 git bash 中，进入你的博客所在的根目录

   - 输入命令，初始化仓库：

     ```
     git init
     ```

   - 输入命令，将仓库所有东西 add 到本地的暂存区

     ```
     git add .
     ```

   - 输入命令，将暂存区的东西提交，并且写一个备注"xxxx"

     ```
     git commit -m "xxxx"
     ```

   - 输入命令，查看本地仓库连接的远端服务器情况

     ```
     git remote -v
     ```

   - 如果之前已经设定过，就跳过；没有的话添加账户：（origin是自己给远端起的名字，xxxx是用户名）

     ```
     git remote add origin git@github.com:xxxx/xxxx.github.io.git
     ```

   - 输入命令，把本地的master，提交给远端的origin

     ```
     git push origin master
     ```

  

3. 到这里其实就应该算结束了，如果不出意外，输入xxxxx.github.io就能看到你的博客了





### Gitee篇



1. 首先，将本地博客根目录下的 _config.yml 文件中，url 等字段进行修改。

   例如我 Gitee 上个人博客的网址为：https://gitee.com/oldsandyoungman/blog，那么 _config.yml 文件中部分内容则修改如下：

   

   ```yaml
   # URL
   
   ## If your site is put in a subdirectory, set url as 'http://yoursite.com' and baseurl as '/child'
   
   url: http://oldsandyoungman.gitee.io/blog
   baseurl: /blog
   permalink: pretty
   ```

   

2. 之后的上传步骤同理，不过在添加远端名字时，需要将“Github”的网址，改成你Gitee上的相应网址，例如我 Gitee 上个人博客的网址为：https://gitee.com/oldsandyoungman/blog

    

    那么就将

    ```
    git remote add origin git@github.com:oldsandyoungman/oldsandyoungman.github.io.git
    ```

    改为

    ```
    git remote add origin git@gitee.com:oldsandyoungman/blog.git
    ```
    
     
    
    
    
3. 最后，在Gitee的你的仓库页面，点击“服务”，选中“Gitee Pages”，然后就可以生成你的博客啦 ~~


​    







