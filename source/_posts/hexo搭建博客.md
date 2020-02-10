---
title: hexo搭建博客
date: 2017-09-10 13:48:42
tags:
- hexo
keywords: hexo,yilia,SEO,github,sitemap
description: hexo,github搭建个人博客,使用yilia主题,SEO优化
categories: hexo
---

### 1.在github上新建项目：

**项目名称：**github用户名称.github.io
example: aloneSingingStar.github.io

**注意:**最好创建空项目，不带一个文件


### 2.本地新建目录
```
随意创建，我的是aloneSingingStar.github.io
```

### 3.进入aloneSingingStar.github.io目录，初始化hexo
```
hexo init
```
<!-- more -->

### 4.安装依赖
```
npm install
```

### 5.安装博客部署插件
```
npm install hexo-deployer-git --save
```

### 6.配置_comfig.xml，设置部署分支为master

```
deploy:
  type: git
  repository: git@github.com:aloneSingingStar/aloneSingingStar.github.io.git
  branch: master
```

### 7.将项目添加到github

* 初始化为git项目
```
git init
```

* 添加该目录下的所有文件到本地仓库（会根据.ignore文件过滤）
```
git add .
```

* 提交代码到本地仓库
```
git commit -m ‘初始化hexo’
```

* 关联本地仓库代码到远程仓库
```
git remote add origin git@github.com:aloneSingingStar/aloneSingingStar.github.io.git
```

* 提交本地仓库代码到远程仓库
```
git push -u origin master -f (必须加上-f,而且执行这句后，之前在github上的原有文件会丢失)
```

在github上创建项目时，里面有一个readme.md文件，而本地项目git init时，里面没有这个文件，当使用（git push -u origin master）把本地文件提交上，就会有如下问题：
To github.com:aloneSingingStar/aloneSingingStar.github.io.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:aloneSingingStar/aloneSingingStar.github.io.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.


### 8.在本地和远程创建hexo分支,并且本地切换到hexo分支,并拉取代码
* 本地仓库创建hexo分支
```
git checkout -b hexo
```

* 推送本地hexo分支到远程仓库
```
git push origin hexo
```

* 从远程hexo分支拉取代码
```
git pull origin hexo
```
(从hexo分支拉取代码，git pull无效果，可以git branch --set-upstream-to=origin/hexo(跟踪hexo的流)，并且git branch --unset-upstream master(取消对master的跟踪)，这样的话，就可以直接执行git pull、git push直接提交代码到hexo分支)


### 9.启动本地服务器测试
```
hexo server

访问 http://localhost:4000/ 预览效果
```

### 10.预览没有问题后，执行如下操作部署到github

* hexo clean

* hexo generate

* hexo deploy

* 访问http://aloneSingingStar.github.io

### 11.配置主题

**主题网址：**[主题网址](https://hexo.io/themes/)
我使用的是yilia,步骤如下：

1.使用ssh方式克隆项目到themes目录下的yilia目录
git clone git@github.com:litten/hexo-theme-yilia.git themes/yilia

2.修改aloneSingingStar.github.io目录下的_config.yml中的主题
theme:yilia

3.具体样式可以修改yilia目录下的_config.yml文件


### 12.配置百度统计（只能后台统计，无法前台展示）

* 注册百度统计站长版
* 注册成功后，会得到一段代码，其中有一段是：hm.src = "https://hm.baidu.com/hm.js?这里是你的唯一code;
把code填写到yilia主题目录下的_config.yml中的
```
# Miscellaneous
baidu_analytics: '填写你的code'
google_analytics: false
```

* [百度统计网址](https://tongji.baidu.com)

### 13.配置不蒜子，在网页显示访问量
我修改的是/Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io/themes/yilia/layout/_partial/footer.ejs文件
其中不蒜子我没有下载到本地，是直接引用的<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>

参考的是这个人的配置（https://github.com/sssvip/blog-data/blob/master/themes/yilia/layout/_partial/footer.ejs）

### 14.提交hexo分支上的修改

* 1.git status 查看代码修改

➜ /Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io git:(hexo) ✗ >git status
On branch hexo
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   _config.yml
        modified:   package.json
        new file:   "source/_posts/hexo\346\220\255\345\273\272\345\215\232\345\256\242.md"

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        themes/hexo-theme-spfk/
        themes/next/
        themes/yilia/

* 2.可以看到，有2个modified,一个new file,还有未被加入本地仓库的文件夹，这3个文件夹是我下载的主题，其中我配置了一些私密信息，比如，百度统计的唯一code、支付宝、微信打赏图片等，我不想上传，如果你想上传，可以使用:[git add .]将所有这个文件夹下的文件提交到本地仓库

* 3.git commit -m "描述"
如果这个文件已经提交到了远程仓库，本地做了修改，想再提交到远程仓库，如果只执行 git commit -m "描述" 是不行的，会报如下问题：

➜ /Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io git:(hexo) ✗ >git commit -m "配置说明修改"
On branch hexo
Changes not staged for commit:
        modified:   "source/_posts/hexo\346\220\255\345\273\272\345\215\232\345\256\242.md"

Untracked files:
        themes/hexo-theme-spfk/
        themes/next/
        themes/yilia/

no changes added to commit

说明已跟踪文件的内容发生了变化，但还没有放到暂存区。要暂存这次更新，需要运行 git add 命令，然后再提交

* 4.如果修改了很多个文件，那么一个个的去[git add 被修改的文件],然后再提交，会很麻烦，所以可以先使用[git status],查看所有未提交的文件，然后把不想提交的文件或者文件夹在[.gitignore]文件中过滤掉，这样的话，就可以直接使用[git add .]将所有未提交的提交到本地仓库

* 5.git push origin hexo

* 6.git pull origin hexo

### 15.在博客的md文件中，加入截断标签

如果没有加，一篇博客有多长，就展示多长，我们想要的效果是，在主页每篇博客只显示一部分，点击more后再进入详细页面

1.在需要截断的地方加入如下标签：<!-- more -->

2.在/Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io/themes/yilia/_config.yml文件中，加入：
excerpt_link: more

### 16 给文章设置tag,用于搜索

每篇生成的博客都有如下格式：可添加多个tag

---
title: hexo搭建博客
date: 2017-09-10 13:48:42
tags:
- tag1
- tag2
- tag3
---

### 17 绑定域名

* 1.购买域名，我购买的是阿里云的
* 2.域名解析：进入阿里云控制台，点击添加解析，【记录类型：A】【主机记录：@】【解析线路：默认】【记录值：ip(ping alonesingingstar.github.io得到的IP）】，然后保存即可
注：记录类型:CNAME , 记录值:alonesingingstar.github.io  是不行的，因为alonesingingstar.github.io不是顶级域名，没有备案的
* 3.在alonesingingstar.github.io/source目录下新建CNAME文件，里面内容是你买的域名
* 4.重新部署，然后用你的域名访问网站，我的是：http://alonesingingstar.site


### 18 网站SEO优化

#### 方式一
http://zhanzhang.baidu.com/college/articleinfo?id=1003

* 1.进入百度站长平台：http://zhanzhang.baidu.com/dashboard/index
* 2.点击添加站点，输入你购买的域名
* 3.勾选站点属性
* 4.验证网站，我选择的是【CNAME验证】,具体做法：在购买域名的网站（我的是阿里云）进行域名解析：点击添加解析，【记录类型：CNAME】【主机记录：C3bHznfyDD(这个值我乱写的，真实值按百度站长平台提供的来写)】【解析线路：默认】【记录值：zz.baidu.com(必须是这个，之前我写成了我自己的域名)】，然后保存即可

CNAME验证
请将 C3bHznfyDD.alonesingingstar.site 使用CNAME解析到zz.baidu.com
完成操作后请点击“完成验证”按钮。
为保持验证通过的状态,成功验证后请不要删除该DNS记录

结果：不到一分钟前alonesingingstar.site使用CNAME验证验证失败，原因：没有找到对应的DNS CNAME记录。
问题分析&解决办法： 请检查dns域名指向是否正确，dns生效一般需要几分钟到1天左右，请耐心等待。

等待一段时间后：

alonesingingstar.site验证成功！
该网站为主站，您可以批量添加子站并查看数据，
无需再次验证。帮助

* 5.打开百度，搜索 【site:你的域名】，看能不能搜索到


#### 方式二
进入百度统计的管理页面：https://tongji.baidu.com
点击新增网站，【网站域名：你购买的域名】,其他随便填，然后你点击获取代码,找到如下代码：hm.src = "https://hm.baidu.com/hm.js?这里是你的唯一code;然后可参照上面 12.配置百度统计（只能后台统计，无法前台展示）的配置

结果：一直没有统计数据

可能原因：可能要等一段时间，待验证


### 19 生成网站地图

* 1.安装插件：npm i hexo-generator-sitemap hexo-generator-baidu-sitemap -S
* 2.配置根目录下的_config.yml

sitemap:
    path: sitemap.xml
baidusitemap:
    path: baidusitemap.xml

url: http://alonesingingstar.site/(你的网址)
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

* 3.部署，部署后会生成sitemap.xml、baidusitemap.xml文件（这两者的区别在于 baidusitemap.xml 是百度搜索引擎的专用文件,另一个是通用）


### 20 向百度提交链接(在百度站长平台设置)

http://zhanzhang.baidu.com/linksubmit/index
#### 推送方式

* 1、主动推送：最为快速的提交方式，推荐您将站点当天新产出链接立即通过此方式推送给百度，以保证新链接可以及时被百度收录。
* 2、自动推送：最为便捷的提交方式，请将自动推送的JS代码部署在站点的每一个页面源代码中，部署代码的页面在每次被浏览时，链接会被自动推送给百度。可以与主动推送配合使用。
* 3、sitemap：您可以定期将网站链接放到sitemap中，然后将sitemap提交给百度。百度会周期性的抓取检查您提交的sitemap，对其中的链接进行处理，但收录速度慢于主动推送。
* 4、手动提交：一次性提交链接给百度，可以使用此种方式。

具体配置方式可以参考百度站长平台/网页抓取/链接提交 中


#### 推送方式--主动推送

* 1.安装插件:npm i hexo-baidu-url-submit -S
* 2.根目录下配置_config.yml
```
deploy:
- type: git
  repository: git@github.com:aloneSingingStar/aloneSingingStar.github.io.git
  branch: master
- type: baidu_url_submitter
#主动提交链接到百度
baidu_url_submit:
  count: 10 # 提交最新的链接数
  host: alonesingingstar.site # 在百度站长平台中注册的域名,虽然官方推荐要带有 www, 但可以不带.
  token: 密钥值 # 你的秘钥,每个人都不一样,在百度站长平台/网页抓取/链接提交/自动提交/主动推送 下面可以找到
  path: baidu_urls.txt # 文本文档的地址,新链接会保存在此文本文档里
```
* 3.重新部署，新的链接就会被推送上去
部署成功可以看到控制台有如下信息：
```
INFO  Deploying: baidu_url_submitter
INFO  Submitting urls
http://alonesingingstar.site/2017/09/10/hexo搭建博客/
http://alonesingingstar.site/2017/09/09/hello-world/
{"remain":4999998,"success":2}
INFO  Deploy done: baidu_url_submitter
```
到百度站长/站点管理/网页抓取/链接提交 中并没有看到提交的链接，需要等一段时间（可能要一两天），然后使用site:你的域名，才能搜索到
具体原因如下：http://tengj.top/2016/03/14/baidunoshouluresson/

9/12 下午16:29，使用百度搜索：site:alonesingingstar.site，已经可以搜索到了

### 21 向谷歌提交链接

具体步骤：进入谷歌站长页面（https://www.google.com/webmasters/，用你的谷歌账户登录，然后点击添加属性，输入你的网址）
1.下载HTML验证文件（在内容中加入layout: false,网上说hexo会编译这个文件，设置这个不让它编译）
2.将该文件放到/Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io/source目录下。
3.重新部署网站
4.通过在浏览器中访问 http://alonesingingstar.site/google2f21809f4cc6b2ea.html 确认上传成功。
6.点击验证
7.进入谷歌的Search Console，点击站点地图/添加站点地图,比如我的是http://alonesingingstar.site/sitemap.xml,添加后就能抓取到
结果：您无权使用此资源。请验证此资源，或请资源所有者将您添加为用户，要等一段时间
等一段时间后：
恭喜！您已成功验证您对 http://alonesingingstar.site/ 网站的所有权。
继续

使用谷歌搜索：site:alonesingingstar.site，暂时还搜索不到，先等吧

进入站长页面的Search Console，点击Google抓取工具，点击抓取，一定要请求将网址和链接页编入索引，然后使用site:alonesingingstar.site搜索，就能搜索到了

### 22.提升排名
* 博客根目录 _config.yml 文件进行如下修改，关键字英文逗号隔开：
```
# Site
title: 网站名称
description: 网站描述
author: 作者姓名
subtitle: 作者简介
language: zh-CN
timezone:
keywords: Web,HTML # 博客关键字
```

* 文章中加入关键字
```
---
title: ###
date: ###
categories: ###
tags: ###
keywords: ###
description: ###
---
```

* 文章路径简化
```
Hexo 默认的文章链接形式为 domain/year/month/day/postname，默认就是一个四级 url，并且可能造成 url 过长，对搜索引擎是十分不友好的。我们可以改成 domain/postname 的形式。编辑站点 _config.yml 文件，修改其中的 permalink 字段改为:
permalink: :title.html
```

### 23 加入友言评论系统
* 1.注册账号：http://www.uyan.cc/
* 2.注册成功后，可以看到一段代码，复制下来，在/Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io/themes/yilia/layout/_partial/post下新建uyan.ejs文件,将内容粘贴进去
* 3.找到/Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io/themes/yilia/layout/_partial/article.ejs文件，找到【<% if (!index && post.comments){ %>】这行代码,在后面加入：【<% if (theme.uyan){ %><%- partial('post/uyan', {key: post.slug,title: post.title,url: config.url+url_for(post.path)}) %><% } %> 】
* 4.进入后台管理，可以看到你的用户ID，复制这个ID，然后在/Users/aloneSingingStar/xyb/blog/aloneSingingStar.github.io/themes/yilia/_config.yml中加入：uyan: '你的ID'
* 5.重新部署

### 24 博客中引用图片
* 1._config.xml中开启文章资源文件夹
```
post_asset_folder: true

```
* 2.每次执行 hexo new "文章名字" 生成md文件时，会在同级生成"文章名字"文件夹,将资源放入该文件夹即可

* 3.引用方式
```
{% asset_img 资源名称 描述 %}

比如：{% asset_img example.jpg This is an example image %}

上面这种方式在网页端访问没有问题，但是手机RSS订阅会有问题，上面那种写法，atom.xml中显示为
<img src="/Tigase开发-Tigase服务器搭建/serverConfiguration.gif">,根本无法访问该链接，
可以使用如下方式访问：

![ServerConfguration](http://alonesingingstar.site/Tigase开发-Tigase服务器搭建/serverConfiguration.gif)

或者：

{% img http://alonesingingstar.site/Tigase开发-Tigase服务器搭建/serverConfiguration.gif %}
```
* 4.修改node_modules/hexo/lib/models/post_asset.js文件
将如下代码进行修改
```
 PostAsset.virtual('path').get(function() {
     var Post = ctx.model('Post');
     var post = Post.findById(this.post);
     if (!post) return;

     // PostAsset.path is file path relative to `public_dir`
     // no need to urlescape, #1562
     return pathFn.join(post.path, this.slug);
   });
```
改为：
```
PostAsset.virtual('path').get(function() {
    var Post = ctx.model('Post');
    var post = Post.findById(this.post);
    if (!post) return;
    // PostAsset.path is file path relative to `public_dir`
    // no need to urlescape, #1562
      //如果生成的文章路径是以html结尾的, 如:  2016/10/13/byte-order.html,
      // 则对应的资源路径应该是: 2016/10/13/byte-order + this.slug
      var reg = new RegExp("html" + "$");
      if(reg.test(post.path)) {
    var assetPath = post.path.substr(0, post.path.lastIndexOf("."));
    return pathFn.join(assetPath, this.slug);
      }
      return pathFn.join(post.path, this.slug);
  });
```
如果不改，执行hexo generate时会报错：Error: ENOTDIR: not a directory

参考(https://leokongwq.github.io/2016/10/14/hexo-post-asset-folder-html.html)