# git使用总结

------

## 本地仓库使用

安装部分省略…….直接从使用开始

### 设置用户名和邮箱

因为我事先已经设置过用户名和密码 ，我们先来查看一下

```
$ git config user.name
Otaku_gsf

$ git config user.email
15156269956@163.com
```

若要设置则使用以下命令(使用以下命令也可以用来更改用户名和邮箱)

```html
git config --global "user.name"   <!-- user.name处改为自己的用户名即可 -->
git config --global "user.email"  <!-- 这里和上一步相同，改成自己的就ok -->
```

### 创建和初始化

创建一个文件夹，进入~

```html
cd 选择一个目录 
mkdir 创建一个文件夹	<!-- 这里我事先创建好了，右键菜单git bash在此处打开-->
```

初始化仓库

```html
$ git init
Initialized empty Git repository in F:/Git/.git/
```

先看下初始化后的目录

```html
$ ls -al
total 8
drwxr-xr-x 1 Otaku 197121 0  7月 21 16:05 ./
drwxr-xr-x 1 Otaku 197121 0  7月 21 15:52 ../
drwxr-xr-x 1 Otaku 197121 0  7月 21 16:05 .git/
<!-- 注意了！！！这里初始化后生成了.git文件夹-->
```

现在编辑一个文件看看,然后使用命令查看仓库文件状态有没有改变

每次使用之前都最好查看下仓库状态，防止你对象改了你文件

```html
$ vi test.txt

$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        test.txt

nothing added to commit but untracked files present (use "git add" to track)

```

这个时候你就可以提交文件到暂存区了

```html
$ git add test.txt
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory

<!--这里完事了，可以用git status查看  但是我忘了,然后提交了(反面教材) 我再创建一个新的~ -->
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   gsf.txt
```

用git commit -m 第一次提交的数据（将文件提交到git的管理区域）

```html
$ git commit -m 初始文件
[master (root-commit) a0a8725] 初始文件
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

以上都为正常操作，下面的操作就有点秀了

先来张图

![1563697415281](C:\Users\18211\AppData\Roaming\Typora\typora-user-images\1563697415281.png)

这是git的工作原理 

我们进行操作时，往往会遇到很多问题：

1. 修改了内容，但是没有提交到暂存区。

   `用git checkout 文件名 来丢弃工作区修改的数据，数据恢复到之前的`

   (没想到吧 我还能恢复)

   ```html
   $ vi test.txt	<!--这里我更改了文件内容-->
   
   $ git checkout test.txt
   Updated 1 path from the index    <!-- 这里回退了我的修改 -->
   
   Otaku@DESKTOP-N2ATK0I MINGW64 /f/Git (master)
   $ vi test.txt	<!-- 内容回退到了修改之前 -->
   ```

2. 你不仅修改了工作区的内容，同时你也使用git add 文件名 ，提交到了暂存区，此时就差一步commit 。你开始慌了，你写的东西有问题。

   `用git reset HEAD 文件名 把暂存区的内容回退到工作区` (这波操作6不6)

   ```
   $ git reset HEAD test.txt
   Unstaged changes after reset:
   M       test.txt
   ```

   这个时候就可以回到第一个问题

3. 你发现问题可能有点晚了，已经提交上去了，你慌得一批，这个时候你还能被拯救

   1. 可以使用git log 命令。查看各个修改的版本信息，以及各个版本

      ```HTML
      $ git log
      commit 823d605a8628ec40c2e2f8857bea94278c71776f (HEAD -> master)
      Author: Otaku_gsf <15156269956@163.com>
      Date:   Sun Jul 21 16:44:57 2019 +0800
      
          做了点修改
      
      commit a0a8725018dc010450aee255803c67ae1552b0d5
      Author: Otaku_gsf <15156269956@163.com>
      Date:   Sun Jul 21 16:17:54 2019 +0800
      
          初始文件
      
      ```

      然后你可以使用下面的命令回退到上一个版本

      ```html
      $ git reset --hard HEAD^
      HEAD is now at a0a8725 初始文件
      <!-- HEAD^指向master -->
      ```

   2. 使用git reflog查看所有的操作日志

      ```html
      $ git reflog
      a0a8725 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
      823d605 HEAD@{1}: commit: 做了点修改
      a0a8725 (HEAD -> master) HEAD@{2}: commit (initial): 初始文件
      ```

      然后可以根据版本号进行回退

      ```html
      $ git reset --hard a0a8
      HEAD is now at a0a8725 初始文件
      <!-- hard后面填写版本号  版本号为前面的一大串数字 你写几个就行了 -->
      ```

      万不得已，希望你有勇气重头再来，删除吧

      ```html
      $ rm test.txt
      
      $ git status
      On branch master
      Changes not staged for commit:
        (use "git add/rm <file>..." to update what will be committed)
        (use "git checkout -- <file>..." to discard changes in working directory)
      
              deleted:    test.txt
      
      no changes added to commit (use "git add" and/or "git commit -a")
      ```

      热血过头，删错了咋办，还能咋办，找回啊

      ```html
      $ git checkout test.txt
      Updated 1 path from the index
      
      $ git status
      On branch master
      nothing to commit, working tree clean
      
      $ cat test.txt
      我可真帅 哈哈哈
      
      ```

      你看，只要你努力，还是可以回头的么，但是你想彻底放弃也有办法

      ```html
      $ rm test.txt
      
      $ git rm test.txt
      rm 'test.txt'
      
      $ git commit -m 撒油那拉
      [master d874cac] 撒油那拉
       1 file changed, 1 deletion(-)
       delete mode 100644 test.txt
      
      ```

------

## 远程仓库链接

远程库的创建这里就不再描述，主要讲讲怎么连接

连接一般分两种 ssh 和 http  这里先讲哈http

### 上传到远程仓库

`使用git remote add origin  地址 ====>地址看地址栏`

```
$ git remote add origin https://github.com/Otaku-gsf/git_study
```

连接完成了就可以将本地仓库的内容上传了

`使用git push -u origin master`

```html
$ git push -u origin master
To https://github.com/Otaku-gsf/git_study
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/Otaku-gsf/git_study'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

报了一个错误 github中的README.md文件不在本地代码目录中，可以通过如下命令进行代码合并

```html
 git pull --rebase origin master
```

然后再上传即可成功

```html’
$ git push -u origin master
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (8/8), 814 bytes | 407.00 KiB/s, done.
Total 8 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/Otaku-gsf/git_study
   43f16e0..115bd18  master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

### 克隆到本地

`git clone https://github.com/Otaku-gsf/git_study`

```html
$ git clone https://github.com/Otaku-gsf/git_study
Cloning into 'git_study'...
remote: Enumerating objects: 11, done.
remote: Counting objects: 100% (11/11), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 11 (delta 1), reused 8 (delta 1), pack-reused 0
Unpacking objects: 100% (11/11), done.

```

### 分支管理与合并

为啥要使用分支呢？

原因很简单，因为安全，更改了内容不会对其他分支造成影响，而且对于git来说，创建，合并分支是一件非常简单的操作，所以建议使用分支进行操作，do you understand?

1. 创建切换分支  

   * `git branch dev 创建名为dev的分支 `

     `git checkout dev 将分支切换到dev分支上`

   * `git  checkout -b dev 创建dev分支并切换到dev分支上`

   ```html
   $ git branch dev
   
   $ git branch		<!-- git branch 查看分支 默认分支为master -->
     dev
   * master
   
   $ git checkout dev
   Switched to branch 'dev'
   
   $ git branch      	<!-- 这里切换到了dev分支  当前分支会有*号标记  -->
   * dev
     master
   
   <!-- 这里用了第一种 第二种你自己去尝试吧 反正都一样的 我有点懒 emmmm-->
   
   ```

2. 分支合并

   选择master分支，再将dev分支合并到master分支上

   `命令： git checkout master `   

   ​           `git merge dev`     

   ```html
   $ vi gsf.txt  		<!-- 这里我先在dev分支上做了修改 -->
   
   $ git checkout master
   Switched to branch 'master'
   M       gsf.txt
   Your branch is up to date with 'origin/master'.
   
   $ git merge dev
   Already up to date.
   
   $ cat gsf.txt		<!-- 查看内容  发现确实合并完成-->
   gsf 最帅
   我是最帅的  不容置疑		
   ```

## 多人协作

多人协作的时候，远程库也应建立着与自己相对应的分支，然后本地远程相连接

`git branch --set-upstream-to=origin/dev`

```html
$ git branch --set-upstream-to=origin/gsf
Branch 'dev' set up to track remote branch 'gsf' from 'origin'.

<!-- 这里可能出现以下情况 -->
$ git branch --set-upstream-to=origin/gsf
error: the requested upstream branch 'origin/gsf' does not exist
<!-- 这个时候很好解决  先看远程仓库有没有你想连接的分支  如果有 看我操作-->
$ git fetch				<!---->
From https://github.com/Otaku-gsf/git_study
 * [new branch]      gsf        -> origin/gsf
<!-- 这样就可以了，但如果没有，那你就建一个吧 哈哈-->
```

然后就可以拉去远程分支里的内容

```
$ git pull
Already up to date.
```

在多人维护一条分支时，可能会出现冲突，具体解决方案我不会，自行百度