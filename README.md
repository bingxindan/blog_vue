
@[冰心丹](入站须知)
# 一.个人博客简介


## 1.1 博客主要页面：
### &ensp;1.1.1 首页

### &ensp;1.1.2 分类页
### &ensp;1.1.3 分类页
### &ensp;1.1.6 关于我
### &ensp;1.1.9 博客详情页面


## 1.2 博客后台管理页面：
> &ensp;后台管理主要有三大模块构成：用户管理，博客管理，数据统计构成。
> 
### &ensp;1.2.1 用户管理模块
### &ensp;1.2.2 博客管理模块
- 发布博客页面
- 查看博客页面
- 标签管理
- 分类管理

### &ensp;1.2.3 数据统计模块
- 博文数据
- 单篇博客分析

## 1.4 网站聊天室模块：
### 1.4.1 初始界面
### 1.4.2 群聊界面
### 1.4.3 私聊界面

## 1.5 功能介绍：
&ensp;本博客简单实现了博客展示、后台管理、发布博客还有评论等功能，其中后台管理、发布博客和评论功能要在用户登录后才可使用，而后台管理的某些功能普通用户只有查看的权限，并没有分配增删改的权限。
## 1.6 博客介绍
&ensp;由于博客是由博主一人完成的，所以暂且只做了一些简单的功能，部分地方还是有不完善的地方甚至有bug，欢迎各位在本篇博文下评论处指出。

## 1.7 Tips
### &ensp;1.7.1 
&ensp;编写博客的markdown编辑器在文章过长时，编写栏和预览栏可能会有错位，此时可手动拉动预览栏滚动条）
### &ensp;1.7.2 
&ensp;暂未设置图片上传功能，涉及图片的上传和使用建议使用网络地址。推荐的图片地址（https://picsum.photos/images#1），使用的时候，将右侧链接的(https://unsplash.it/100/100?image=1002) ***1002*** 改成自己的图片id即可，100/100是图片的尺寸，即长宽。

# 二.前端开发：
## 2.1 简介：
&ensp;***https://github.com/asiL-tcefreP/blog-vue***（前端源码地址）
&ensp;采用了vue.js，前端框架采用了semantic-ui和element-ui，此外还有一些关于页面动态和渲染的js和css类似(animate.css,pricsm等)。此外，需要说明的是，本人后端狗一枚，页面样式是基于网上部分模板样式的修改，其余开发是独立完成的。
## 2.2 项目介绍
&ensp;项目结构采用的是vue-cli3，值得一提的是其中用到的插件还是不错的。

[编辑器 Markdown](https://pandao.github.io/editor.md/)

[内容排版 typo.css](https://github.com/sofish/typo.css)

[动画 animate.css](https://daneden.github.io/animate.css/)

[代码高亮 prism](https://github.com/PrismJS/prism)

[目录生成 Tocbot](https://tscanlin.github.io/tocbot/)

[滚动侦测 waypoints](http://imakewebthings.com/waypoints/)

[平滑滚动 jquery.scrollTo](https://github.com/flesler/jquery.scrollTo)

[二维码生成 qrcode.js](https://davidshimjs.github.io/qrcodejs/)

[弹幕效果 vue-baberrage](https://blog.csdn.net/Dlihctcefrep/article/details/113773737)

[背景的彩带效果 ribbon](https://gitee.com/aaajkcn/ribbon)

[统计图 echarts](https://echarts.apache.org/zh/tutorial.html#5%20%E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B%20ECharts)

# 三.后端开发：
## 3.1 简介：
&ensp;***https://github.com/asiL-tcefreP/blog***（后端源码地址）
>- 大致框架采用了SpringBoot+MybatisPlus+SpringCloud(Eureka)+ElasticSearch完成的，用redis做缓存中间件，采用微服务的架构。
>- 安全方面采用了SpringSecurity和BCEncrypt
>- 用了jwt来请求访问接口
>- 利用RSA算法对前端发送的重要参数进行加密，经过网关解密后把参数发送到后端服务器。
>- 由于服务器内存和配置的原因，服务器只上线了四个模块
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210321201208846.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210520155833969.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RsaWhjdGNlZnJlcA==,size_16,color_FFFFFF,t_70)

>项目是由八个模块组成的，
>- blog-common: 博客服务端的实体类
>- blog-encrypt: 博客的服务代理类（从前端接收请求，网关RSA解密后转发给服务端接口）
> - blog-eureka: 微服务注册中心server
> - blog-server: 主体服务端
> - blog-extension: 拓展服务端（留言和友链功能），上线的版本集成了blog-search-api模块，因为阿里云服务器内存太小了
> - **blog-search-api:** **ElasticSearch的服务端，分出一个模块是为了更清晰的展现微服务架构，但是服务器内存太小，所以集成在上述模块中，自己开发可以直接使用本模块**
> - blog-article-crawler：爬虫和人工智能模块，用的webmagic框架爬取数据，deeplearning4j做文本分类
> - blog-ai：里面的服务类调用了py脚本来实现古诗词生成

## 3.3 开发中遇到的一些问题：
### 3.3.1 关于jwt与zuul
&ensp;本人使用自定义注解@LoginRequired来对某些类或者接口进行jwt验证，但是在一开始加入网关微服务的时候，发现后端用了jwt验证的接口一直访问不通过。在浏览器看，发的请求的请求头明明都带上了token，这是一开始百思不得其解的地方之一。
>&ensp;后来才得知，原来是在网关转发前端的请求后，再把请求转发给后端服务器时，请求头中的token丢失，于是只能在网关filter里面，**在转发请求给后端前，手动的把token加到头部。**

### 3.3.2 关于RSA解密与Json
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210193541608.png)
&ensp;获取到前端的加密请求参数时，还有对其进行URLdecode解码，然而加密后的数据中的空格依旧没有转换成加号，此时就得自己用字符串替换。前端传来的数据要进行解码否则就会有%2F,%3D等出现，其次base64编码的+号会变成空格，要对字符串进行处理重新变为＋号，关于转码问题可以参考: [这篇文章](https://www.cnblogs.com/hongdada/p/10338015.html)。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021019475237.png)

### 3.4 有意思的插件
-  captcha：自动生成验证码
- commonmark：将markdown格式的文章转换成html格式的显示在页面
- NeteaseCloudMusicApi：网易云音乐后台数据的api接口
### 3.5 关于ELK
&emsp;之前一直没做项目关于ElasticSearch的整合，是因为不知道项目如果采用了ES后，对数据库的操作该怎么实现。
>例如：当我更改数据库的数据时，还要同步ES索引中的数据，这也未免太过繁琐。此外，数据库中的多对多、一对多关系在ES索引中该如何表示？Linux系统下该怎么部署ES？
