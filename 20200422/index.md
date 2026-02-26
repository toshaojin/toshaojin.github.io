# 记录一下折腾过程中的一些代码


在这次折腾Hugo过程中，写到了一些命令，还是有必要备份一下，万一以后还会用到，就不用撞墙了。而且平时部署Hugo说不定还是频繁使用。

<!--more-->

## 文章发布

```plaintext
git add -A
git commit -m "[修订]"
git push -u origin master
```

## 删除文件 

1.克隆远程仓库到本地库。

```plaintext
git clone git@github.com:[用户名]/[仓库名].git
```

2.对需要删除的文件、文件夹进行操作:

```plaintext
git rm test.txt # 删除文件
git rm -r test # 删除文件夹
```

3.提交修改

```plaintext 
git commit -m "随便填写"
```

4.将修改提交到远程仓库

```plaintext
git push origin
```

## 密钥设置

解决`ERROR: Permission to xxxxxx/xxxxxx.github.io.git denied to xxxxxx.`报错问题

1. 终端输入`ls ~/.ssh/`查看当前已由的密钥，也可以忽略，直接为自己多个GitHub设定多个密钥。

2. `cd ~/.ssh/`进入ssh根目录。

3. 创建新的密钥

   `ssh-keygen -t rsa -C '根据自己需要设定第一个邮箱地址' -f ~/.ssh/github_[自己的id]`

   `ssh-keygen -t rsa -C '根据自己需要设定第二个邮箱地址' -f ~/.ssh/github_[自己的另一个id]`

   这样就新设了两个密钥。ssh根目录下就会新生成两对公钥、私钥文档。

4. 在ssh根目录下新建`config`文档，然后填入：

   ```plaintext
   # github第一个密钥
   Host github.com.one #可以自定义
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/[填入第一个的密钥名称]
   # github第二个密钥
   Host github.com.two #可以自定义
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/[填入第二个的密钥名称]
   ```

5. 然后分别进入两个GitHub账户──设置，将上述两个公钥分别复制粘贴到SSH Keys中。

6. 测试

   ```plaintext
   ssh -T git@github.com.one
   ssh -T git@github.com.two
   ```

   如果出现以下信息，说明成功了。

   ```plaintext
   Hi [你的GitHub用户名]! You've successfully authenticated, but GitHub does not provide shell access.
   ```

7. 替换

   ```plaintext
   git remote set-url origin git@github.com.one:[用户名]/[仓库名].git
   ```

   或

   ```plaintext
   git remote set-url origin git@github.com.two:[用户名]/[仓库名].git
   ```

   ## 错误：failed to push some refs 

   2020年4月26日更新

   出现该错误的话，一般是由于远程库与本地库不一致，需要使用命令

   ```plaintext
   git pull --rebase origin master
   ```

   '命令将远程库的更新合并到本地库。


