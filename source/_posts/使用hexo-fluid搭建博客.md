---
title: 使用hexo+fluid搭建博客
date: 2021-04-27 18:17:14
# hide: true
categories:
- 其他技术
tags: 
- Hexo
---

{% note success %}
如何使用Hexo+Fluid+Github Action搭建博客
{% endnote %}

<!-- more -->

## 搭建Hexo

因为Hexo是基于NodeJs的，所以电脑只要安装了NodeJs，就可以直接安装使用Hexo了，[NodeJs安装地址](http://nodejs.cn/download/)。因为搭建的是Github pages，所以还需要Git，[Git安装地址](https://git-scm.com/downloads)

我使用的NodeJs为**12.16.3**，hexo版本为**5.4.0**，系统是**Window**

### 1.安装Hexo

使用`npm i -g hexo-cli`安装，如果需要指定版本可以：`hexo-cli@版本号`

### 2.搭建Fluid主题网站

#### 1.hexo初始化

``` bash
# 初始化hexo项目,<folder> = 创建的项目的根目录，根据自己的想法命名
hexo init <folder>
cd <folder>
npm i
#安装fluid主题，推荐hexo>5.0
npm install --save hexo-theme-fluid
```

这时候根目录下有`_config.yml(博客配置)`和`_config.fluid.yml(主题配置)`2个配置文件，如果没有`_config.fluid.yml`文件，手动在根目录下创建一个。因为我们使用的是**npm直接安装的Fluid主题**,所以不需要在themes下包括fluid的配置文件。之后和主题有关的配置都在`config.fluid.yml`中配置

这时候安装了Fluid主题还不能直接用，需要在`_config.yml`配置一些信息

```yaml
theme: fluid  # 指定主题，执行hexi init的时候可能携带了默认主题，替换成这个就行了

language: zh-CN  # 指定语言，会影响主题显示的语言，按需修改
```

#### 2.创建关于页

因为首次搭建，关于页是需要自己创建的

```sh
hexo new page about
```

创建完成后会生成`/source/about/index.md`,这就是关于页面了，可以编写你自己的关于界面

还可以在`_config.fluid.yml`中编写about页面相关的配置

```yaml
about:
  banner_img: /img/about.png #关于页面的顶部图片
  avatar: /img/avatar.jpg #需要显示的自己头像
  name: "QiongXing" #名称
  intro: "彰显逼格的话"
  icons: #自己的相关链接
    - {
        class: "iconfont icon-github-fill",
        link: "https://github.com/qiongxing",
        tip: "GitHub",
      }
    - { class: "iconfont icon-qq-fill", qrcode: "/img/qq.png" }
```

#### 3.其他主题配置

因为搭建的Fluid会在归档，文章，分类，标签的页面都使用自带的默认图片，所以都需要自己修改主题配置，`banner_img`是每个页面默认顶部的配置图片

```yaml
#首页
index:
  #顶部图片
  banner_img: /img/home.png
#文章配置
post:
  banner_img: /img/home.png #文章顶部默认图片
  default_index_img: /img/posts/default.webp #文章列表左侧默认图片

#归档
archive:
  banner_img: /img/home.png

#标签
tag:
  banner_img: /img/home.png
#分类
category:
  banner_img: /img/home.png
```

其他的配置可以在[这里](https://hexo.fluid-dev.com/docs/guide/#%E5%85%B3%E4%BA%8E%E6%8C%87%E5%8D%97)查看。

在说下可能要用到的工具

[hexo-all-minifier](https://github.com/chenzhutian/hexo-all-minifier)：这个是压缩js，css，html，图片的工具

 通过 `npm i hexo-all-minifier --save`安装，但是可能会安装失败，因为它的ip地址好像在国内屏蔽了，看了网上的可以去查询源地址的ip，加在windows的host文件中，有些麻烦，而且换台设备就又要重新配置host，即使配置完成也可能会install到linux环境的包，在windows环境下不能使用，又会报错，所以我推荐用安装cnpm，然后执行`cnpm i hexo-all-minifier --save`

然后在博客配置下配置下就可以了

```yaml
#压缩资源
all_minifier: true
```

#### 4.完成

完成上面步骤就可以运行`hexo s`来看看本地的执行效果，然后下面就是部署了

## 部署到Github pages上

建立在各位都有github账号的基础上，如果你还没有github账号，可以到[Github](https://github.com/)上注册个账号

### 创建个人pages页面

创建pages库，仓库名必须是`<username>.github.io`，因为我已经有这个仓库，所以报错了，没有的创建是不会有提示的，库的状态必须是`public`,可以创建或者不用创建`README.md`，之后自己想写的时候补充也行

![创建共有库](https://cdn.jsdelivr.net/gh/qiongxing/qiongxing.github.io@main/source/img/posts/2021/git-create-repo.png)

然后在创建成功的仓库页面复制`SSH链接`，为什么不是`HTTPS`链接呢，因为Git对https的支持逐渐减弱，每次都要校验账户号密码会很麻烦。

不过SSH，也需要在电脑上创建一对密钥，将公钥配置到Github的`SSH keys`，但是好处就是一劳永逸

![创建共有库](https://cdn.jsdelivr.net/gh/qiongxing/qiongxing.github.io@main/source/img/posts/2021/git-ssh.png)

将Hexo项目关联到Git仓库

```sh
git init
```
