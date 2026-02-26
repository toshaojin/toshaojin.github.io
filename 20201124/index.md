# 尝试利用Github Action自动化部署Hugo


## 又开始折腾博客了

之前，一直想尝试利用Github Action自动化部署Hugo。但是，本地好不容易搭建，怕又被我自己手贱折腾坏了，然后又是瞎忙活失败。毕竟自己是小白，这类玩意还是需要一定基础的。虽然，自己有时候想是不是学一下这方面的基础，但是大法已经让我有点筋疲力尽的感觉，实在是没有这个精力（允许自己找个理由和接口）。

这次本着“即便搞砸也要尝试”的决心，还是动手尝试了。新鲜玩意儿，总有让人一股莫名的冲动。

## 利用submodules管理Theme

借鉴了很多文章，网络上太多大佬，这个还比较容易。主要参考了这篇文章[在 hexo 中使用 git submodules 管理主題](https://juejin.cn/post/6844903751908605965)。

### 使用

``git submodule add [主题链接] [主题存放目录] ``

如果是在博客根目录，则存放目录为themes/[主题名称]；如果是在themes目录，则存放目录只要写[主题名称]。根据之前看过的教程，自己选择的主题，fork了一份到自己的GitHub。

完成后，在博客根目录会产生一个.gitmodules的文件。内容为：

```bash
[submodule "themes/[主题名称]"] 
path = themes/[主题名称]
url = [主题链接]
```

### 更新

根据教程，更新也是比较容易的。若主题作者有更新， 可通过两种方法来拉取主题的更新内容.

1. 进入`themes` 下主题目录, 执行 `git fetch` 和 `git merge origin/master` 来 merge 上游分支的修改
2. 直接运行 `git submodule update --remote`, Git 将会自动进入子模块然后抓取并更新

更新后重新提交一遍, 子模块新的跟踪信息便也会记录到仓库中。像我采用fork了一份，应该重新fork一下就可以了吧？

```bash
git submodule init
git submodule update
```

### 删除子模块

因为自己在添加子模块过程添加错误，导致需要删除。于是，搜索了一番：

删除子模块相比于之前的操作要复杂些：

1. 删除`.gitmodules`中的子模块内容
2. 删除`.git/config`中的子模块内容
3. 删除`.git/modules`中的子模块对应文件夹
4. 运行`git rm --cached -r themes/*`删除对应子模块文件夹的索引
5. 删除子模块文件夹

 这时候，如果直接add子模块，提示`'sub_folder already exists in the index'`，那么运行一下 ` git rm --cached themes/*`。 然后可以确认一下，`git ls-files --stage  `，如果提示`Please stage your changes to .gitmodules or stash them to proceed`，就可以直接删掉.gitmodules文件。

接着，就可以重新添加了。

###对需要删除的文件、文件夹进行操作

参考文章[git 删除远程仓库文件](https://www.jianshu.com/p/de75a9e3d1e1)

预览将要删除的文件

```bash
git rm -r -n --cached 文件/文件夹名称 
加上 -n 这个参数，执行命令时，是不会删除任何文件，而是展示此命令要删除的文件列表预览。
```

确定无误后删除文件

```bash
git rm -r --cached 文件/文件夹名称
```

提交到本地并推送到远程服务器

```bash
git commit -m "提交说明"
git push origin master
```

## 利用GitHub Action自动化部署
之前，因为已经本地构建Hugo，并上传至GitHub，详见[之前的功课](https://shaojin.me/2020/2020-04-21-newhugo/)，从Gridea到自行搭建。最近没啥好写，于是就又想起来一直“耿耿于怀”的GitHub Action自动化部署，真的是每天不务正业。

一致纠结于GitHub Action是否能在已经部署了的GitHub Pages上进行构建，这次就不管三七二十一上了。因为已经在本地安装了Hugo并已经构建生成了public文件，所以可以直接略过Hugo的本地安装。

主要参考的文章：

1. [使用 Hugo + GitHub Pages 搭建个人博客](https://mogeko.me/2018/018/)
2. [使用 Travis CI 自动部署 Hugo 博客](https://mogeko.me/2018/028/)
3. [使用 GitHub Actions 部署 Hugo 博客到 GitHub Pages](https://io-oi.me/tech/deploy-hugo-to-github-pages-via-github-actions/)

最终成功部署是参考了最后一篇文章，因为跟我自己使用Hugo是一致的。但是，也是因为最后才搜到这篇文章，导致之前走了很多弯路，遭遇了很多错误，差点中途放弃了。

### GitHub多帐号的链接

这个问题之前也已经遇到过，所以这次遇到比较淡定，不过也看到了这篇文章，讲的比较好，[使用 SSH 连接到 GitHub（多帐号）](https://io-oi.me/tech/ssh-with-multiple-github-accounts/#%E5%8D%95%E5%B8%90%E5%8F%B7)。解决了我的很多问题。

### 设置代码仓库

需要设置两个库，一个是GitHub Pages展示博客的仓库，这个我之前就已经有了，是*.github.io命名的；另一个就是源码库，用来存放自己的原始文章。

如何通过配置公钥和密钥到仓库，关联这两个仓库，上面文章都有详细的讲述。只是我在看到这篇文章之前，很多文章都设置密钥名称为`ACTIONS_DEPLOY_KEY`，但我在这种过程中，却在自动化部署中会出错，最后还是按照文章3的教程，设置为`DEPLOY_KEY`。不知原因为何，反正成功了。

关联后，就可以将自己原来本地的博客文件夹下的源码推送至源码库。

### 已生成public文件情况下的处理

因为我已经在本地编译了Hugo，由此形成了`public`文件夹、存放主题的 `themes` 文件夹、主题产生的 `resources` 文件夹，而这部份文件是不需要上传至GitHub源码库的，所以需要设置一下。

设置一个`.gitignore` 文件来排除这些内容，在博客目录下创建并修改 `.gitignore`，然后提交到 GitHub。

```bash
public/*
themes/*
resources/*
```

如果之前已经删除了，建议用删除命令删掉远程源码库里的这些文件、文件夹，以便排除干扰因素。

### GitHub Action设置

网上有很多现成的workflows配置示范，但是我尝试很多，都出错了。在我设置过程中，参考样式，均设置`runs-on: ubuntu-latest`，但会出现错误：`Ubuntu-latest workflows will use Ubuntu-20.04 · Issue #1816 · actions/virtual-environments github.com`。

遍寻Google，没有找到中文释疑，最后看到一个建议，指定ubuntu版本号，然后就搞定了。

自己尝试过程中，试用了各种样本，都有各种各样的问题：`warning: url contains a newline in its host component: https://***`、`Process completed with exit code 1.` 等等，搞得我崩溃。

最终，还是按照文章3的样式，一举成功。叹气!






