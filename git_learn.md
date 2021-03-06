### git 安装配置

windows下载gitbash。

git config 这个工具，可专门用来配置或读取相应的工作环境变量。

在 Windows 系统上，Git 会找寻用户主目录下的 .gitconfig 文件。主目录即 $HOME 变量指定的目录

```git
$git config
列出所有的选项

配置个人用户名和电子邮件
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com
--global 更改的配置文件就是位于你用户主目录下的那个，以后所有的项目都会默认使用这里配置的信息

查看已有的配置信息
$git config -l
查看配置文件
$vim ~/.gitconfig
查看某个环境变量
$git config user.name
```

### SSH Keys

```git
ssh-keygen -t rsa -C "your_email@example.com"  生成本地的ssh-key
```



### git 工作区，暂存区，缓存区

* 工作区：就是你在电脑里能看到的目录。
* 暂存区：英文叫 stage 或 index。一般存放在 .git 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
* 版本库：工作区有一个隐藏目录 .git，这个不算工作区，而是 Git 的版本库。
* Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD
* git 管理的是修改

### git创建仓库

git init 初始化一个git仓库

```git
git init     #使用当前目录作为Git仓库,会在当前目录出现一个名为.git的目录
git init newrepo       #使用我们指定目录作为Git仓库。 
```

git clone 从现有的git仓库在拷贝项目

```git
git clone <repo>
git clone <repo> <directory>     #克隆到指定的目录
```

### git基本操作

1. git add  添加一个或多个文件到暂存区

   ```git
   git add [file1] [file2] ...
   git add .      #添加所有文件到暂存区
   ```

   

2. git status 命令用于查看在你上次提交之后是否有对文件进行再次修改。

   ```git
   git status -s  #使用 -s 参数来获得简短的输出结果
   ```


3. git diff  较文件在暂存区和工作区的差异。

4. git commit 命令一次性地将暂存区内容添加到本地仓库中。

   ```python
   git commit -m [message]    #-m 可以额外增加备注信息
   git commit -a  #-a 参数设置修改文件后不需要执行 git add 命令，直接来提交
   ```

5. git log 查看历史提交记录（由近到远的显示）

   ```git
   git log --pretty=oneline      #单行输出信息
   git log --oneline            #也是单行输出，只不过版本号只显示前几个字符
   git log --graph              #命令可以看到分支合并图。
   ```

6. git reset 回退版本： 

   * 当前版本为HEAD, HEAD^ 上一个版本, HEAD^^ 上上一个版本
   * HEAD~0 表示当前版本, HEAD~1 上一个版本, HEAD^2 上上一个版本

   ```git
   git reset [--soft | --mixed | --hard] [HEAD]
   --mixed 为默认，可以不用带该参数，用于重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变。
   
   比如修改了a.txt然后使用了git add, 需要撤销stage区域的a.txt避免该文件被commit,
   可以使用git reset a.txt 其全称是git reset --mixed HEAD a.txt
   这样就不改变 work dir 中的任何数据，将 stage 区域中的 a.txt 文件还原成 HEAD 指向的 commit history 中的样子
   
   --soft 参数用于回退到某个版本
   --hard 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交
   ```


7. git checkout 命令可用于切换分支或回复工作树文件
   
   git checkout  filename 丢弃工作区的修改：让这个文件回到最近一次git commit或git add时的状态。需要注意只会把被修改的文件恢复成stage的状态，若新增了新文件这样操作是不会删除新文件的

   * 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
   * 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
   
8. 删除文件： 
   * rm file : 只删除了工作区的文件
   * git rm file: 从暂存区和工作区中删除文件
     * 若要从版本库中删除：就用命令git rm删掉，并且git commit
   * 若误删除了： git checkout filename         用版本库里的版本替换工作区的版本



### 远程仓库

1. 添加远程仓库。 git remote 用于在远程仓库的操作

   * origin 为远程地址的别名
   * git remote -v  : 显示远程库的信息
   * git remote rm name : 删除远程库

   ```git
   echo "# python_tools" >> README.md
   git init
   git add README.md
   git commit -m "first commit"
   git branch -M main         #重命名分支
   git remote add origin git@github.com:tongwwgit/python_tools.git #添加远程库
   git push -u origin main  
   #本地库推送到远程库，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来
   ```

2. 从远程库克隆
   
   * git clone name
   
3. git pull 从远程获取代码并合并本地的版本。   git pull <远程主机名> <远程分支名>:<本地分支名>

   ```git
   git pull origin master:brantest
   git pull origin master  #如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
   ```

4. git push 命用于从将本地的分支版本上传到远程并合并。 

   ```git
   git push <远程主机名> <本地分支名>:<远程分支名>
   git push <远程主机名> <本地分支名>    #若本地分支和远程分支同名可省略冒号后面
   git push origin master
   ```


5. 关联本地分支和远程分支 git branch --set-upstream-to=origin/<branchname>   local_branch_name

   这样可以直接使用git push或者git pull 不需要再增加远程主机名和分支名

### 分支管理

1. 创建和合并分支
   * Git为我们自动创建的第一个分支master(指向提交)，以及指向master的一个指针叫HEAD
   * git branch 命令列出分支
   * git branch branchname  创建分支
   * git checkout branchname 切换分支   或者使用git switch branchname
   * git checkout 命令加上-b参数表示创建并切换     或者 git swith -c branchname
   * git merge branchname   合并指定分支到当前分支
   * git branch -d (branchname) 删除分支

2. 多人协作
   * 首先，可以试图用git push origin <branch-name>推送自己的修改；
   * 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
   * 如果合并有冲突，则解决冲突，并在本地提交；
   * 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！