---
title: 重建hexo博客记
date: 2021-02-25 11:23:07
tags:
---
工作几年后做了一次人生重大的选择，终于有时间和精力来搞点业余的东西，就想到了曾经短暂玩过的博客。打开自己的github，看到托管在pages上的hexo博客上一篇还在遥远的2016年，还是不免有些感慨，时间过得真的好快。当时的自己无论如何也想不到再写下一篇博客的时候会是现在这样吧。不过先不想这么多，是时候恢复hexo博客，开启新的篇章了！

# 0x00 恢复原博客
## 0 尝试直接github同步
太久没弄了，早忘了该怎么操作，先把github中的代码库拉下来看看

    git clone git@github.com:madgd/madgd.github.io.git

打开目录一看，发觉有点不太对。为什么只有html，没有md文件呢？？

![filetree](../img/oldBlogFileTree.png)

通过检索相关文档，才想起来md等数据是存在原始的hexo的工作目录中的，实际部署的博客目录是生成的`public`文件夹的结果。果断翻出旧硬盘找原始文件。但是只找到了若干md，并没用工作目录。
看来当年还是`too young`。

只能重新搭建hexo的工作目录，在把原来的md更新进去了。

## 1. 安装hexo
互联网上hexo的安装攻略一大把，随便搜一下照着来就行。

>官方的文档有点旧了，谨慎参考：[官方文档](https://hexo.io/zh-cn/docs/)\
参考这位老哥[使用hexo搭建个人博客并部署到GitHub](https://www.jianshu.com/p/282717c8da6c)\
windows环境可参考这位老哥 [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)

mac上比较简单：
```
brew install node
npm install -g hexo-cli
hexo init blog
cd blog && npm install
```
安装完成后验证：
```
hexo s
(hexo server)
```
执行成功后访问`http://localhost:4000`可验证
![starter](../img/hexostarter.png)

hints:
1. npm速度慢——[切换淘宝源](https://www.jianshu.com/p/63acb38962c6)
2. 运行时，npm提示缺哪个包就`npm install`就行了，有问题对症下药。

做一点站点的基础配置，编辑`_config.yml`:
```
title: madgd's blog
subtitle: ''
description: let's party
keywords:
author: madgd
language: zh
timezone: 'Asia/Shanghai'
```
# 2. 安装next theme
>hexo有大量主题资源，可以浏览并选择自己喜欢的主题安装[theme](https://hexo.io/themes/)

沿用原来使用的theme [next](https://github.com/theme-next/hexo-theme-next)
```
cd blog
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

编辑`_config.yml`
```
theme: next
```
在此`hexo s` 即可看到效果
![next](../img/nextstarter.jpeg)
# 3. 恢复原post
将原有post的md移动到`_posts`文件夹中，发现标题，时间不对：

![date_wrong](../img/oldpostDateWrong.png)

好解决，在md头部添加信息，即可修复标题、时间：
>\-\-\-\
title: test\
date: 2016-05-25 11:18:09\
tags:\
\-\-\-

# 4. 统一图片链接
在`source`下建立`img`目录，将所有图片移入，修改图片链接为相对目录：
```
![img](../img/win10bule2.jpg)
```

**至此原博客恢复完成**

***

# 0x01 若干升级
## 0. 统一图片路径

## 1. 引入评论模块

## 2. 
***

# 0x02 部署原代码
那么怎么保留hexo的原目录，以防在换电脑时避免文件丢失呢？

最好还是github代码库同步。但如果单独新建代码库，又略显多余。

一种主流方案是通过手动双`branch`实现：
>branch master 作为默认分支，放生成的博客文件\
>branch source 用于保存hexo的工作目录原文件

参考文档：
>[官方](https://hexo.io/zh-cn/docs/one-command-deployment)\
这位老哥的比较[详细](https://www.cnblogs.com/codecheng99/p/12380700.html#%E4%BA%8C%E3%80%81%E5%B0%86hexo%E9%83%A8%E7%BD%B2%E5%88%B0github%E4%B8%8A%E9%9D%A2%EF%BC%8C%E5%AE%9E%E7%8E%B0%E5%8F%8C%E5%88%86%E6%94%AF%E9%83%A8%E7%BD%B2)

另一种是官网推荐的用[Travis CI](https://travis-ci.com/)持续集成的，我们只需要保持hexo工作目录在github上更新即可，继续集成将生成站点目录并推送部署。（其实也可以双分支）
>[持续集成方案](https://hexo.io/zh-cn/docs/github-pages.html)



错误1：
fsevents not accessible from chokidar
The command "eval npm ci  " failed. Retrying, 2 of 3.


引用参考：