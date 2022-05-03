---
title: hexo博客搭建与配置
date: 2021-10-24 23:06:24
tags:
 - 
 - 
categories:
cover: false
---

# 前期准备

## 安装git

1. 下载：[https://git-scm.com/download](https://git-scm.com/download)
2. 安装：除了选择安装路径外，一直点next就行

可参考教程[https://blog.csdn.net/hahameier/article/details/86312530](https://blog.csdn.net/hahameier/article/details/86312530)


## 安装node.js

1. 下载：[https://nodejs.org/en/](https://nodejs.org/en/)
2. 安装：除了选择路径以外，一直点next就行



# hexo环境搭建

## 进入命令行
打开命令行窗口：按`win+r`键，输入`cmd`，回车，后续操作均在命令行进行

## 查看node.js版本
1. 查看node版本：输入`node -v`，回车
2. 查看npm版本：输入`npm -v`，回车

## 安装cnpm
1. 安装cnpm镜像源：输入`npm install -g cnpm --registry=http://registry.npm.taobao.org`，回车，等待...
2. 查看cnpm版本：输入`cnpm -v`，回车


## 安装hexo
1. 安装hexo框架：输入`cnpm install -g hexo-cli`，回车
2. 查看hexo版本：输入`hexo -v`，回车

## 新建blog文件
1. 以管理员权限打开 `cmd`
2. 输入`mkdir e:\blog`并回车，实现在`e:\`路径下创建`blog`文件夹
3. 配置路径：先输入`e:`回车，切换默认目录至`e`盘；然后输入`cd e:\blog`回车，切换至`e:\blog`目录

> `cmd` 切换目录：
>  - “盘符:”命令：进入指定硬盘区域，例：`D:`命令，切换到D盘
>  - “cd+空格+文件位置”命令：切换到该磁盘下的指定目录
>  - `dir`命令，显示当前目录下的所有文件
>  - `cd ..`命令：返回上一级目录

## 初始化
1. 初始化博客：输入`hexo init`，回车，等待...直到出现`INFO Starting blogging with Hexo`
2. 启动本地博客服务：输入`hexo s`，回车，得到本地访问地址`http://localhost:4000`
3. 访问本地博客：浏览器打开[https://localhost:4000/](https://localhost:4000/)

## 安装git插件
1. 配置到`blog`目录：在`cmd`命令行输入`e:`回车，然后输入`cd e:\blog`回车
2. 安装部署插件：输入`cnpm install --save hexo-deployer-git`并回车，等待安装完成，如若出现warning可直接忽略

## 配置git
1. 打开**git bash**
2. 输入`git config --global user.email YourEmail`，其中`YourEmail`是github邮箱
3. 输入`git config --global user.name YourName`，`YourName`是github用户名

## 创建github仓库
1. 登录 github 网页的个人账户
2. 右上角`New repository`新建仓库
3. `Repository name`命名为个人用户名`YourName`加上`.github.io`，即，仓库名为`YourName.github.io`

## 配置_config.yml
1. 打开`e:\blog`路径下的`_config.yml`文件
2. 找到`Deployment`板块，配置如下

		# Deployment
		## Docs: https://hexo.io/docs/deployment.html
		deploy: 
			type: git
			repo: https://github.com/YourGithubName/YourGithubName.github.io.git
			branch: master

其中，`repo`后面的链接为个人github仓库的链接

**注：冒号后面需要有英文环境下的空格**

## 部署到github仓库
1. `cmd`命令行配置到路径`e:\blog`
2. 部署：输入`hexo d`并回车，部署到 github 仓库里
3. 等待弹窗，输入 github 账户密码，即配置完成

## hexo相关命令
1. `hexo c`：清理
2. `hexo n`：新建
3. `hexo g`：生成博客
4. `hexo s`：启动本地博客服务
5. `hexo d`：部署到远端github博客


# 其他配置

## hexo主题

[主题Butterfly配置](https://butterfly.js.org/posts/21cfbf15/)

[hexo主题列表](https://hexo.io/themes/)


## 插件

[代码高亮插件: hexo-prism-plugin ](https://github.com/ele828/hexo-prism-plugin)

[中文链接转成拼音：hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin)

[本地搜索：hexo-generator-search](https://github.com/viko16/hexo-permalink-pinyin)

[帖子字数统计：hexo-wordcount](https://github.com/willin/hexo-wordcount)

[显示本地图片：hexo-asset-img](https://github.com/yiyungent/hexo-asset-img)

[渲染器：hexo-renderer-pug](https://github.com/hexojs/hexo-renderer-pug)

[渲染器：hexo-renderer-stylus](https://github.com/hexojs/hexo-renderer-stylus)

[数学公式：@neilsustc/markdown-it-katex](https://github.com/yzhang-gh/markdown-it-katex)

[数学公式：@upupming/hexo-renderer-markdown-it-plus](https://github.com/upupming/hexo-renderer-markdown-it-plus)

[文章置顶：hexo-generator-index](https://github.com/hexojs/hexo-generator-index)

[Mermaid流程图：hexo-filter-mermaid-diagrams](https://blog.csdn.net/wzh0709zml/article/details/103310405?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&utm_relevant_index=1)

# 多终端同步
通过维护两条git分支来实现：第一个分支是 github 上的 main 分支（注：最初的默认分支名为master，我此前改成了main），用于存放发布的博文的静态页面；另外再新建一个分支，比如命名为 hexo ，通过此分支存放主题、原始的博客文件等等，这些文件才是不同电脑需要同步的文件

当每次修改主题或者新增博文后，先将修改的主题文件（在themes文件夹下）或者新增博客（在source文件夹下），同步到远程的 hexo 分支，然后在通过 `hexo g -d` 命令发布博文，也就是将新增的博文的静态页面同步到 main 分支

## 配置

### 终端1：push 必要文件到 github 仓库的 hexo 分支

```git
git init  	      // 初始化本地仓库
git add source        // 将必要的文件（如 scaffolds、source、themes、_config.butterfly.yml、_config.yml、.gitignore、package.json 等）依次添加，这里也可以用`git add .`命令执行blog文件夹下全部内容的添加（默认的 .gitignore 已经配置了忽略规则）
git commit -m "Blog Source"
git branch hexo       // 新建hexo分支
git checkout hexo     // 切换到hexo分支上，可以通过 `git branch` 命令查看分支
git remote add origin git@github.com:yourname/yourname.github.io.git  // 将本地与github项目对接，可以通过 `git remote -v` 命令查看是否对接成功
git push origin hexo  // push到github项目的hexo分支上
```

注意：如果使用的是通过 `git clone` 下来的主题 theme（比如 butterfly），要把主题文件夹下面把 .git 文件夹（这是一个隐藏文件夹，vscode 以及 cmd 不可见，需要在文件资源管理器中查看“隐藏的项目”）删除掉，否则的话，主题无法 push 到远程仓库

### 终端2：将github的hexo分支clone到本地，并进行初次配置

```git
git clone -b hexo git@github.com:yourname/yourname.github.io.git  // 将Github中hexo分支clone到本地
cd  yourname.github.io  	// 切换到刚刚clone的文件夹内
npm install    			// 注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再init
hexo n "new blog name"   	// 新建一个.md文件，并编辑完成自己的博客内容
git add source  		// 每次只要更新source中的文件到github中即可(因为只是新建了一篇新博客)
git commit -m "XX"
git push origin hexo  		// 更新hexo分支
hexo g -d   			// 同步main分支
```

执行完上述操作，即实现了不同终端的配置，以后无需再重复


[为客户端及服务端添加ssh-key](https://blog.csdn.net/ywl470812087/article/details/104459288)

[如何解决github+Hexo的博客多终端同步问题](https://blog.csdn.net/Monkey_LZL/article/details/60870891?spm=1001.2014.3001.5506)

[Hexo+Git+Github博客在多台终端同步管理](https://blog.csdn.net/steven_zdg988/article/details/111240905?spm=1001.2014.3001.5506)

## 使用

不管是终端1还是终端2，后续如果修改了主题或者新增了博文，按照以下步骤即可：

```git
git pull origin hexo  	// 先git pull，将远端文件更新本地
hexo n "NEW POST"
git add source		// 添加source文件，或者通过 `git add .` 添加全部文件
git commit -m "XX"		// 提交至暂存区
git push origin hexo	// push到hexo分支
hexo g -d   		// 同步到main分支
```


# 参考资料

[视频教程](https://www.bilibili.com/video/BV1Yb411a7ty?p=1&share_medium=android&share_plat=android&share_source=WEIXIN&share_tag=s_i&timestamp=1635071237&unique_k=kYdWS1)


[个人博客地址](https://jiankychen.github.io/)

[解决 Github 连接不上的问题](https://jingyan.baidu.com/article/63f2362893f7e10208ab3df8.html)

[HOSTS文件配置](https://www.cnblogs.com/philobing/p/15670470.html)

[修改hosts文件提示没有权限保存怎么办](https://jingyan.baidu.com/article/3c343ff7b8fe550d377963e7.html)

[hexo+github本地和线上图片不显示问题](https://blog.csdn.net/z952957407/article/details/111642548?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_default&utm_relevant_index=1)

[添加 Waline 评论系统](https://blog.csdn.net/jiunian_2761/article/details/122621667)
 - [Waline邮箱设置](https://blog.csdn.net/hubojing/article/details/122659549)
 - [解决 LeanCloud 流控问题](https://www.aimtao.net/slef-wake-leancloud/)
 - [评论后台](https://waline-comments-jiankychen.vercel.app/ui/login)