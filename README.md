# Git 常用命令及基本概念

### 1. 创建版本库

* git init 
> 把当前所在目录变成 Git 可以管理的仓库（版本库）。Git 的版本库⾥里存了很多东西，其中最重要的就是称为 stage（或者叫index）的暂存区，还有 Git 为我们自动创建的第一个分⽀支 master，以及指向 master 的一个指针叫 HEAD 。

* git add fileName.xx
> 把文件添加到版本库。实际上就是把文件修改添加到版本库的暂存区。

* git commit -m "xxx"
> 把文件提交到版本库，-m 后面输入的是本次提交的说明。实际上就是把暂存区的所有内容提交到当前分⽀。

> 因为 commit 可以一次提交很多文件修改，所以你可以多次 add 不同的文件，⽐如:

```
$ git add file1.txt
$ git add file2.txt
$ git add file3.txt
$ git commit -m "add 3 files."
```

### 2. 时光穿梭机

* git status
> 可以让我们时刻掌握版本库当前的状态。比如，告诉我们 fileName.xx 被修改过了，但还没有提交修改。

* git diff fileName.xx
> 顾名思义就是查看 difference，可以用来查看文件具体修改了什么内容。

### 3. 版本回退

* git log
> 显示从最近到最远的提交⽇志。穿梭前，⽤此命令可以查看提交历史，以便确定要回退到哪个版本。

* git reset --hard HEAD^ ｜ git reset --hard [commit id]
> 回退至上一个版本。在 Git 中，用 HEAD 表示当前版本，也就是最新的提交，上一个版本就是 HEAD^ ，上上一个版本就是 HEAD^^ ，当然往上100个版本写100个 ^ 比较容易数不过来， 所以写成HEAD~100。

* git reflog
> 记录你的每一次命令。可用于查看 [commit id] ，并与上条命令配合使用，可用来在各个版本之间切换。穿梭后，用此命令查看命令历史，以便确定要回到未来的哪个版本。

### 4. 工作区和暂存区

* 执行 git add fileName.xx，会将工作区文件添加到版本库的暂存区。
> ![git add 命令是将工作区文件添加到版本库的暂存区](./GitImg/add2stage.png)

* 执行 git commit，会一次性将暂存区的所有修改提交到版本库的分⽀。
> ![一次性将暂存区的所有修改提交到版本库的分⽀](./GitImg/commit2branch.png)

### 5. 管理修改

> Git是如何跟踪修改的？每次修改，如果不 add 到暂存区，那后续 commit 时就不会将修改提交至版本库的分支中。

* git diff HEAD -- fileName.xx
> 查看工作区和版本库里面最新版本的区别。

### 6. 撤销修改

* git checkout -- fileName.xx
> 把文件在工作区的修改全部撤销，这里有两种情况：一种是 fileName.xx 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模⼀样的状态；一种是 fileName.xx 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。总之，就是让这个文件回到最近一次 git add 或 git commit 时的状态。

* git reset HEAD fileName.xx
> 把暂存区的修改撤销掉(unstage)，重新放回工作区。此时⽤用 git status 查看一下，提示现在暂存区是干净的，工作区有修改。还记得如何丢弃工作区的修改吗？git checkout -- fileName.xx 。

> 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 7. 删除文件

* git rm fileName.xx

* git commit -m "xxx"

> 从版本库中删除该文件，用命令 git rm 删掉，并且 commit 。

> 工作区中的文件被误删，用 git checkout -- fileName.xx 可将版本库里的版本恢复到工作区中，无论工作区是修改还是删除，都可以“一键还原”。

### 8. 添加远程库

* git remote add origin git@github.com:hezhang18/learngit.git
> 把一个已有的本地仓库与远程仓库关联。

* git push -u origin master
> 把本地库的内容推送到远程，⽤ git push 命令，实际上是把当前分支 master 推送到远程。

> 由于远程库是空的，第一次推送 master 分⽀时，加上了 -u 参数，这样 Git 不但会把本地的 master 分支内容推送到远程新的master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，在以后的推送或者拉取时就可以简化命令。

> 添加后，远程库的名字就是 origin，这是 Git 默认的叫法，也可以改成别的，但是 origin 这个名字一看就知道是远程库。

> 注意：
> 
> 一般由于先有本地仓库，然后创建远程仓库并用 git remote add 命令进行关联。此时，远程仓库存在 README.md，LICENSE，.gitignore文件，而本地仓库不存在，此时使用 git push 提交命令则会报错，错误内容如下所示：
>> 提示：更新被拒绝，因为远程仓库包含您本地尚不存在的提交。
>> 
>> 提示：一个仓库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更。
>> 
> 解决方案：
> 
>> 使用命令：git pull origin master --allow-unrelated-histories
>> 
>> 这样对本地仓库和远程仓库进行合并冲突后，就可以正常使用 git push命令了。
>> 

* git push origin master
> 把本地 master 分支的最新修改推送至 GitHub（只在第一次推送时加 -u 参数）。

* git remote rm origin
> 删除远程库。

### 9. 从远程库克隆

* git clone git@github.com:hezhang18/learngit.git
> 从远程库克隆至本地。

### 10. 分支管理
















难点：推送到远程仓库时总是默认使用遗弃账户hezhang94,搜索可以全局重新设置账户名称和邮箱，但是依然没有解决（账号都显示为新设置的账户），但仍使用以前的推送，搜索可以清除账号的缓存，重新安装GitHub并登录，依然无法解决；最后发现是https和ssh之间的问题，应使用后者进行推送（git remote add origin git@github.com:hezhang18/learngit.git；git push -u origin master）。

mkdir fileFolder
cd fileFolder
touch fileName.xx
vi fileName.xx; i; esc -> :wq;
cat fileName.xx
rm -rf fileFolder
rm fileName.xx