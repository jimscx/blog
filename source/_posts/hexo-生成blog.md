---
title: hexo 如何生成blog
date: 2016-10-27 22:13:06
tags: utils
---
#### 1.环境
 首先你的本机需要有git node.js 环境这是必须

#### 2. hexo 配置
* 在mac环境下 `sudo npm install -g hexo`全局安装hexo
* `mkdir blog`用于存放你的hexo项目，`cd blog`进入目录执行`hexo init`
就是这样简单，你的blog就这样ok了，接下来执行`hexo g`生成静态页面，启动服务`hexo server`,现在就可以在本地浏览了
* 现在把它配置到自己的github仓库里，`vim _config.yml`编辑文件
```
deploy:
  type: git
  repo: https://github.com/jimscx/jimscx.github.io.git
  branch: master
```
所有的配置都在`_config.yml`文件中,尽情的修改~~~~
* 执行`npm install hexo-deployer-git --save`
然后，执行配置命令：`hexo deploy`,ok至此就可以访问了`https://github.com/jimscx.io`

#### 3.发表个文章
* `hexo new "name"`,看到在source目录下生成了一个md文档,打开文档编辑你的blog文章 
#### 发表也就是这几个命令
* `hexo clean` 
* `hexo generate` 简写为 `hexo g`
* `hexo deploy` 简写为 `hexo d`

#### 4.使用个美美的主题吧
* 可以去这里寻找喜欢的[主题](https://www.zhihu.com/question/24422335) next主题`git clone https://github.com/iissnan/hexo-theme-next themes/next`
* 修改配置文件_config
```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```