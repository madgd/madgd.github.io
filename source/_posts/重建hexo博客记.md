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

# 4. 统一图片路径
在`source`下建立`img`目录，将所有图片移入，修改图片链接为相对目录：
```
![img](../img/win10bule2.jpg)
```

**至此原博客工作目录恢复完成，可以开始编辑新的博文了**

如何部署后文再讲

***

# 0x01 部署

官方介绍了多种[一键部署方案](https://hexo.io/zh-cn/docs/one-command-deployment.html), 这里我们使用github pages

# 0. 保留原代码
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


## 1. 采坑解决
### 1.1 部署报错

在按照[持续集成方案](https://hexo.io/zh-cn/docs/github-pages.html)部署时，

```
fsevents not accessible from chokidar
The command "eval npm ci  " failed. Retrying, 2 of 3.
```

检索该错误，未找到相关信息。检查travis 与本地环境diff，发现node版本不同：
```
node_js:
  - 10 # 官方文档版本
本地：
$ node -v
v15.3.0
```
修改版本一致: `- 15`

并添加`deploy`目录：
```
deploy:
  ...
  target: gh-page
```
### 1.2 修复图片路径错误

通过相对路径法生成的博客图片，在博客首页显示正常，但在博文详情页，缺发生了链接错误的情况。是相对路径导致。已有的主流解决方法参考：
>[hexo博客中插入图片失败——解决思路及个人最终解决办法](https://blog.csdn.net/m0_43401436/article/details/107191688)\
>[Hexo 中完美插入本地图片](https://andavid.github.io/2019/01/15/insert-local-image-in-hexo/)

几种方法或者需要舍弃md在本地图片的显示，或者需要为每篇博文单独建立目录，不利于图片的复用。

这里尝试一种新的思路：
>本地使用相对路径，支持md；再部署时批量去掉路径前的`..`，变成网站绝对目录

这样在本地编辑时，使用相对目录，可正常预览md：
```
![img](../img/win10bule2.jpg)
```
完成md编辑后，直接push到在github的master分支hexo工作目录，md也可以正常预览。

修改部署脚本`.travis.yml`, 在`hexo g`生成前，使用`sed` 正则匹配所有_post中的md文件, 原地批量替换：
```
sed -i '1,$s/](\.\.\/img/](\/img/g' source/_posts/*.md

```

>在mac上测试的时候会有一点问题，使用`-i`参数的时候会报错，可以试用`gsed`，参考：[mac sed -i 报错](https://www.whidy.net/macos-sed-command-notice)

## 2. 方案总结

总体步骤基本按照官方，需要修改`.travis.yml`. 以下为了方便看，直接复制过来：

1. 新建一个 repository。如果你希望你的站点能通过 `<你的 GitHub 用户名>.github.io` 域名访问，你的 repository 应该直接命名为 `<你的 GitHub 用户名>.github.io`， 如：
```
madgd/madgd.github.io
```
2. 将你的 Hexo 站点文件夹推送到 repository 中。默认情况下不应该`public` 目录将不会被推送到 repository 中，你应该检查 `.gitignore` 文件中是否包含 `public` 一行，如果没有请加上。
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```
3. 将 [Travis CI](https://github.com/marketplace/travis-ci) 添加到你的 GitHub 账户中。
![travisConf](../img/travisConf.png)
4. 前往 GitHub 的 [Applications settings](https://github.com/settings/installations)，配置 Travis CI 权限，使其能够访问你的 repository。
![travisConf2](../img/travisConf2.png)
5. 你应该会被重定向到 Travis CI 的页面。如果没有，请 [手动前往](https://travis-ci.com/)。
6. 在浏览器新建一个标签页，前往 GitHub [新建 Personal Access Token](https://github.com/settings/tokens)，只勾选 repo 的权限并生成一个新的 Token。Token 生成后请复制并保存好。
![](../img/githubPk4travis.png)
7. 回到 Travis CI，前往你的 repository 的设置页面，在 Environment Variables 下新建一个环境变量，Name 为 GH_TOKEN，Value 为刚才你在 GitHub 生成的 Token。确保 DISPLAY VALUE IN BUILD LOG 保持 不被勾选 避免你的 Token 泄漏。点击 Add 保存。
8. 在你的 Hexo 站点文件夹中新建一个 .travis.yml 文件, 经修改最终生成的`.travis.yml`文件：
```
sudo: false
language: node_js
node_js:
  - 15 # use nodejs v15 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - sed -i '1,$s/](\.\.\/img/](\/img/g' source/_posts/*.md
  - hexo cl
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
  target: gh-pages
```
9. 将`.travis.yml` 推送到 repository 中。Travis CI 应该会自动开始运行，并将生成的文件推送到同一 repository 下的`gh-pages` 分支下
10. 在 GitHub 中前往你的 repository 的设置页面，修改 `GitHub Pages` 的部署分支为 `gh-pages`。
11. 前往 `https://<你的 GitHub 用户名>.github.io` 查看你的站点是否可以访问。可能需要稍等一会儿。


**这样，每次更新博客的操作就是：**

1. hexo new "title"
2. 编辑md
3. 图片存在`source/img/` 目录下，md中使用相对路径`../img/pic`引入图片，本地可正常预览
4. 写完后
```
git add .
git commit -m "comment"
git push
```
5. 剩下的工作会由`travis`持续集成完成
![travisComplete](../img/travisComplete.png)

***
# 0x02 装修
## 0. 个性化设置
编辑`.config.yml`:
```
title: madgd's blog
subtitle: ''
description: let's party
keywords:
author: madgd
language: zh
timezone: 'Asia/Shanghai'

```

编辑`themes/next/_config.yml`:
```
scheme: Gemini

# Dark Mode
darkmode: true

language: zh-cn

position: right
# 头像
avatar:
  url: /img/avatar.png

author
```

可参考[next使用](http://theme-next.iissnan.com/getting-started.html)
## 1. 开启百度统计&添加阅读量统计

参考[next使用](http://theme-next.iissnan.com/getting-started.html)相关章节

效果如图：
![](../img/baiduAnalytics.png)

参考[添加文章阅读量统计](https://cloud.tencent.com/developer/article/1482008)

注意，按以上步骤配置后并不会立即生效，会提示：
```
Views: Counter not initialized! More info at console err msg.
```
查看console:
```
ATTENTION! LeanCloud counter has security bug, see how to solve it here: https://github.com/theme-next/hexo-leancloud-counter-security. 
 However, you can still use LeanCloud without security, by setting `security` option to `false`.
```

可直接选择修改配置:
```
  security: false
```

或者按照[hexo-leancloud-counter-security](https://github.com/theme-next/hexo-leancloud-counter-security) 安装并配置

效果如图：
![](../img/lcViewCounter.png)

## 2. 引入评论

常用评论模块有：[next评论系统](https://www.zhihu.com/question/267598518)

其中disqus需要翻墙，github类的gitment、gitalk、gitter需要github账号，valine无后端，对用户门槛最低。因此优先接入valine。

参考这位老哥的方案[Hexo博客进阶：为Next主题添加Valine评论系统](https://qianfanguojin.github.io/2019/07/23/Hexo%E5%8D%9A%E5%AE%A2%E8%BF%9B%E9%98%B6%EF%BC%9A%E4%B8%BANext%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0Valine%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F/)

![](../img/valine.png)

同时，为了支持带图片评论，接入[来必力](https://livere.com/), 方案参考这位老哥的[Hexo博客优化之实现来必力评论功能
](https://zhuanlan.zhihu.com/p/33617273)

## 2. 引入搜索服务

***
