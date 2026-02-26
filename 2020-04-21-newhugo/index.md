# 继续折腾博客


# 为什么折腾？

没多久之前，刚折腾了新鲜的玩意儿──Gridea，利用这个软件搭建了Github Page。

搭建完之后，又图新鲜买了个域名耍耍。之前，都是利用互联网资源蹭免费的域名玩。

<!-- more -->

再然后呢？折腾完没几天，在博客也没输出些实质内容，就又开始瞎折腾了，折腾利用Hugo搭建博客。在遇到Gridea之前，搜索🔍到Hexo、Hugo等教程类文章。但是，苦于各类代码，看到头疼，就望而却步。

但是，为什么现在反而又要尝试折腾了？因为，无意间发现很多Hugo搭建的博客，~~皮肤~~ theme真漂亮。可能Gridea的theme不仅仅局限于官方的主题市场，但是受限于自己的技术能力，学习成本显然比较高，暂时无法利用其他资源去更换新皮肤，虽然现在用的`Fly`还是不错的。

另一方面，还是有兴趣去接触一下这类好玩的东西，毕竟兴趣是做好事情的根本动力。

# 怎么折腾？

折腾的过程比较复杂

## 起步

在开始之前，Google了很多技术类文章，初步了解了一下大致方法，感觉还是能够驾驭操作的。主要参考的文章就是 [使用 Hugo + GitHub Pages 搭建个人博客](https://mogeko.me/2018/018/)，基本上按照这个方法可以顺利完成搭建。当初也是因为看中这个博客的~~皮肤~~ theme，才起了念头。

## 意外

### 问题1

在开始搭建之初，根本没有注意到一个GitHub账户只能搭建一个pages的问题。因此，一开始就在利用之前的Github账户新开了一个仓库，但是hugo渲染之后发现跟Gridea的站点冲突了，而且无法直接用*.github.io直接访问。但是，以之前搭建Gridea的经验，操作程序应该没问题。所以怀疑是不是Github的问题，万能Google告诉我，你蒙对了。

于是，我的解决办法是，重新开一个Github账户。

### 问题2

在解决了问题1之后，按照教程，安装hugo、初始化、安装主题、新建文章、配置，然后卡在了发布阶段。

```plaintext
git remote add origin https://github.com/[Github 用户名]/[Github 用户名].github.io.git
```

在用上述命令将本地库关联到远程库时，发生了错误。提示`ERROR: Permission to XXX.git denied to YYY`。然后，在这个步骤卡了很久，Google了很多，最后发现是因为SSH密钥设置问题。因为之前可能已经用自己的电脑设置过GitHub，所以这台电脑只认之前的Github账户，不认我新设的账户。解决办法就是新设一个密钥，然后绑定到新设Github账户，教程在这里：

1. [Git 最著名报错 “ERROR: Permission to XXX.git denied to user”终极解决方案](https://www.jianshu.com/p/12badb7e6c10)。
2. [Git配置多个SSH-Key](https://javahikers.github.io/2019/06/15/git-is-configured-with-multiple-ssh-keys/)。
3. [一台电脑使用两个/多个GitHub账号部署两个/多个Hexo博客](https://www.itrhx.com/2019/01/18/A16-deploy-two-or-more-hexo-blogs/)。

解决了这个问题之后，就可以正常将本地库同步至远程库，完成搭建。

### 问题3

博客设置当中的主要问题有：头像图标、浏览器图标以及文章分类问题。

所用的 [leaveit](https://github.com/liuzc/leaveit)主题，头部`Blog`栏出现404，无法显示我已经发布的文章。最后的症结解决问题是，在文章目录新建posts文件夹，新建的文章放到这个文件夹下，那么发布之后，文章才会出现在`Blog`这一栏。

### 问题4

在搭建完之后，发现发布文章每次都要输入很多命令，有点繁琐。是不是有自动化的发布工具？万能Google帮助我。发现很多的第三方工具，还有GitHub Action。但是，教程显示都是在新建博客之前，设置好一系列的自动化流程，没有发现在现有博客基础上去构建，于是放弃了使用Github Action自动化部署。

最终老实的用了脚本，方法：

1. 在本地库根目录下新建txt文档；

2. 将以下代码Copy：

   ```plaintext
   #!/bin/sh
   
   # If a command fails then the deploy stops
   set -e
   
   printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"
   
   # Build the project.
   hugo # if using a theme, replace with `hugo -t <YOURTHEME>`
   
   # Go To Public folder
   cd public
   
   # Add changes to git.
   git add .
   
   # Commit changes.
   msg="rebuilding site $(date)"
   if [ -n "$*" ]; then
       msg="$*"
   fi
   git commit -m "$msg"
   
   # Push source and build repos.
   git push origin master
   
   cd ..
   ```

3. 重命名txt文档为`deploy.sh`

4. 使用`chmod +x deploy.sh`使它可执行

5. 新建文章，通过`hugo new`或直接将md文档放入文件夹

6. 输入`./deploy.sh`

相比较之前，可以少输入一些命令，新建文章、输入`./deploy.sh`。

Gridea需要：新建文章、打开客户端、点击同步。

# 接下来

完成初步搭建之后，觉得还是学到一些知识或是方法。

现在，购买的域名还是绑定在原来的Gridea上，看情况是不是解析到新建站。先两边同步更新一段时间，说不定过段时间，还是觉得Gridea来得简单。

![](https://i.loli.net/2020/04/22/OKRyYu9AEbN2UQv.png)
