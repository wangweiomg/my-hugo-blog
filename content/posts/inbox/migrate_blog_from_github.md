---
title: "迁移博客到hugo"
date: 2023-03-31T14:05:03Z
draft: false 
toc: false
images:
tags:
  - untagged
---

听说过hugo很久了，能很快的构建博客，之前使用过 hexo 和 docsify 感觉都还不错，今天就开始折腾下hugo。

主要目标是:
1. 部署hugo，创建一个startup
2. 选择喜欢的模板并熟悉配置
3. 迁移老博客的md文件
4. 发布
5. 优化

### 1. 部署
这一步没啥难的，根据 gohugo.io 的quickstart 来搭建一个setup来试试。开了个免费的AWS EC2, 然后就开搞。
* 系统是ubuntu ， 使用snap install go hugo 
* 然后就是 hugo new site quickstart 来测试
* 下载默认主题，然后添加一个文件， hugo new posts/first.md 编写点内容
* 启动 hugo server 

过程中，主要问题是 hugo server启动默认 bind: 127.0.0.1 ，就是只接受本机访问， 在外界是访问不到的，需要把绑定改了
```shell
# 使用 -D 把 draft 状态的文章也发布了，因为默认不发布draft文章，并且md文件中 draft 默认是 true
hugo server --bind "0.0.0.0" -D
```
然后aws改下安全组策略，就能通过ip和默认的1313端口访问到了.
### 2. 主题选择
看过几个主题很美观，选择了个程序员类型的 hello-friend-ng , 最近更新是一个月前，还算活跃.
### 3. 迁移老博客
这里主要是大量的md 文件迁移到hugo里。这里最主要的是给md文件添加头信息。 也就是如下写法：
```
---
title:  'this is a title'
date: 2023-03-31 
draft: false
---
```
起初，计划给每个md文件拼上这个头信息, 然后复制到 content 目录，担心和 ```hugo new``` 的方式样式有问题(多余担心) , 就决定调用 hugo new 来生成md, 再拼接内容。
python相关逻辑很简单，就是读老博客 md 文件，写入到hugo生成的文件中，主要功能代码如下:
```python
with open(file_path, 'r') as file_a:
    content_a = file_a.read()
with open(new_path, 'a') as file_b:
    file_b.write(content_a)

```

### 4. 部署发布
使用 ```hugo``` 命令构建发布， 生成public 的静态html 文件. 然后使用nginx来代理这些静态文件。期间配置nginx遇到的主要问题是， 要记得改nginx.conf里的user .默认 user 是没有权限访问博客目录的，所以几次出现403问题。 之后在 cloudflare 上配置了域名解析，就可以用自己域名访问到博客了。

### 5. 后续优化
本次迁移是非常基本的迁移，有很多东西需要完善。比如分类、系列、标签几乎全是一样，另外就是时间是错的，都变成今天发布了。 一个好的方法是根据文件是创建时间来设置博客时间，这是下次优化的重点。
然后就是编写体验问题。本人喜欢用Typora在本地编写md文件，应该做到本地编写完，```git push ```到文档的repo里，git actions 启动，自动发布博客内容。

以后的目标是:
1. 修改博客文章日期
2. push后一键发布

