---
title: git
top: false
cover: false
toc: true
mathjax: false
date: 2019-11-07 20:36:23
tags: ['语言','版本库']
password:
summary:
categories:
---



# [Git](https://git-scm.com/docs/)

官方链接是如上，中文机翻版本是https://cloud.tencent.com/developer/doc/1096

## 导图

![导图](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

## 初始化init

- `init`

  ```shell
  # 在当前目录新建一个Git代码库
  $ git init
  # 在当前目录新创建一个名为repo且只包含 .git 子文件夹的空目录，将其初始化为Git代码库
  $ git init [repo]
  # 在服务器机器上创建裸仓库（因为服务器不需要对工作区直接操作，纯/裸仓库即普通仓库的.git文件夹）
  mkdir robbin_site.git + cd robbin_site.git + git --bare init 
  ```

  

- `clone`

  ```shell
  # 根据[url]从远程仓库中克隆下载一个项目和它的整个代码历史
  $ git clone [https://github.com/Hojondo/barerepo.git]
  # 用带版本的项目创建纯版裸仓库，不包括工作区直接就是版本库的内容
  $ git clone --bare [remote-bare-repo] [local-bare-repo.git]
  # 将纯仓库上传到服务器上 
  scp -r my_project.git git@git.csdn.net:~
```
  
  [注：] [--bare & --mirror的区别](https://www.worldhello.net/gotgit/02-git-solo/100-git-clone.html)
  
- `fetch`

  ```shell
  # 抓取下载远程仓库更新，加下一条指令等同于git pull
  # 与git pull相比git fetch相当于是从远程获取最新版本到本地，但不会自动merge
  $ git fetch [remote-repo]
  $ git merge [remote-repo]/master	# 将远程主分支合并到本地当前分支
  ```

  

- `pull`

  ```shell
  # 抓取远程仓库所有分支更新并合并到本地
  $ git pull
  # 取回远程仓库的某分支变化，并与当前本地分支合并
  $ git pull [remote-repo] [remote-branch]
  # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
  $ git pull --no-ff
  ```

  

- `push`

  ```shell
  # push所有分支
  $ git push
  # 客户端首次提交(没有master时)需要加-u
  $ git push –u [remote-repo] master
  # 将本地某分支推到远程分支 / 创建远程分支（远程没有对应分支时）
  $ git push [remote-repo] [local-branch]
  # 创建远程分支
  $ git push [remote-repo] <local-branch>:<remote-branch>
  # 强行推送当前分支到远程仓库，即使有冲突
  $ git push [remote-repo] --force
  # 推送所有分支到远程仓库
  $ git push [remote-repo] --all
  # 先删除本地分支，然后再push删除远程分支
  $ git branch -d <local-branch>
  $ git push [remote-repo] :<remote_branch>
  
  # 删除远程tag
  $ git push [remote-repo] :refs/tags/[tagName]
  # 提交指定tag
  $ git push [remote-repo] [tag]
  # 提交所有tag
  $ git push [remote-repo] --tags
  ```

  

- `remote`

  ```shell
  #查看远程库的信息
  $ git remote
  #查看所有远程库地址和名称
  $ git remote –v
  # 查看某个远程服务器仓库状态
  $ git remote show [remote-repo]
  # 增加一个新的远程仓库，并命名
  $ git remote add [remote-repo] [git@github:robbin/robbin_site.git]
  # 设置远程仓库地址(用于修改远程仓库地址)
  $ git remote set-url [remote-repo] [git@github.com:robbin/robbin_site.git]
  # 设置远程仓库的HEAD指向master分支
  $ git remote set-head [remote-repo] [master]
  # 删除远程仓库
  $ git remote rm [remote-repo]
  ```

  

- `submodule`



## 配置gitconfig

用户的配置文件`~/.gitconfig`

```shell
# 显示当前的Git配置,列举所有配置
$ git config --list
# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
git config --global user.name "xxx"   
git config --global user.email "xxxxxx@gmail.com"

# git status等命令自动着色
git config --global color.ui true
git config --global color.status auto
git config --global color.diff auto
git config --global color.branch auto
git config --global color.interactive auto
# 配置别名，co 配为checkout 别名，等等etc...
git config --global alias.co checkout 
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch

# 设置Editor使用textmate
git config --global core.editor "mate -w"

# remove  proxy configuration on git
git config --global --unset http.proxy
```

## 常用命令

1. 分支和合并

   - `branch`

     ```shell
     # 列出所有本地分支
     $ git branch
     # 列出所有远程分支
     $ git branch -r
     # 列出所有本地分支和远程分支
     $ git branch -a
     # 查看各个分支最后提交信息
     $ git branch -v
     # 查看已经被合并到当前分支的分支
     $ git branch --merged
     # 查看尚未被合并到当前分支的分支
     $ git branch --no-merged
     
     # 建立追踪关系，在现已有本地分支与指定的远程分支之间
     $ git branch --set-upstream-to [local-branch] [remote-branch]
     
     # 新建一个分支，但依然停留在当前分支
     $ git branch [local-branch]
     # 新建一个分支，指向指定commit
     $ git branch [local-branch] [commit-id]
     # 新建一个分支，与指定的远程分支建立追踪关系
     $ git branch --track [branch] [remote-branch]
     
     # 删除本地分支
     $ git branch -d [local-branch]
     # 强制删除分支 (未被合并的分支被删除时 需要强制)
     $ git branch -D <local-branch>
     # 删除远程分支
     $ git push [remote-repo] --delete [remote-branch]
     $ git push [remote-repo] :[remote-branch]
     $ git branch -dr [remote-branch]
     ```
     
   - `checkout`

     ```shell
     # 新建一个分支，并切换到该分支(-b等同于branch命令)
     $ git checkout -b [local-branch]
     # 切换到指定分支，并更新工作区
     $ git checkout [local-branch]
     # 切换到上一个分支
     $ git checkout -
     # 基于branch创建新的new_branch
     $ git checkout -b <new_branch> <branch>
     # 跟踪某个远程分支创建相应的本地分支
     $ git checkout --track [remote-repo]/<remote-branch>
     # 基于远程分支创建本地分支，功能同上
     $ git checkout -b <local_branch> [remote-repo]/<remote-branch>
     # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除
     $ git checkout  <commit-id>
     # 把某次历史提交记录checkout出来，创建成一个分支
     $ git checkout  <commit-id> -b <new_branch>
     # 依据需要合并的最后一条 commit 创建新分支
     $ git checkout -b newbranch <last_commit>
     # 新建一个分支，指向某个tag
     $ git checkout -b [branch] [tag]
     
     # 撤销工作区修改，恢复暂存区的所有文件到工作区
     $ git checkout .
     # 撤销xx文件修改，在工作区的修改全部撤销到暂存区状态
     $ git checkout -- xx
     # 恢复暂存区的指定文件到工作区
     $ git checkout [file]
     # 撤销指定文件到指定版本，恢复某个commit的指定文件到暂存区和工作区
     $ git checkout <commit-id> [/path/to/file]
     ```

     # todo

     

   - `merge`

     ```shell
     # 合并指定分支到当前分支
     $ git merge [branch]
     $ git merge origin/master          # 将远程主分支合并到本地当前分支
     
     # 不要Fast-Foward合并，这样可以生成merge提交
     $ git merge origin/master --no-ff
     # 选择一个其它分支的某一次提交commit，合并进当前分支(合并其它分支一系列提交，后缀为 first_commit ~ last_commit)
     $ git cherry-pick [commit]
     
     # 将master rebase到branch，等同于：
     #git checkout   <branch> + git rebase master + git checkout  master + git merge <branch>
     $ git rebase master <branch>
     # 从新分支中需要合并的第一条 commit 开始合并
     $ git rebase --onto master <first_commit>^
     ```

   - `stash`

     ```shell
     git stash  #暂存当前工作，恢复现场后可继续工作
     git stash list  #查看暂存文件列表
     git stash apply  #恢复暂存内容，暂存区不删除
     git stash drop  #删除暂存文件
     git stash pop  #恢复并删除文件
     
     # 暂时将未提交的变化移除，稍后再移入
     $ git stash
     $ git stash pop
     ```

     

   - `tag`

     ```shell
     # 列出所有tag
     $ git tag
     
     # 新建一个tag在当前commit
     $ git tag [tag]
     
     # 新建一个tag在指定commit
     $ git tag [tag] [commit]
     
     # 删除本地tag
     $ git tag -d [tag]
     
     # 删除远程tag
     $ git push origin :refs/tags/[tagName]
     
     # 查看tag信息
     $ git show [tag]
     
     # 提交指定tag
     $ git push [remote] [tag]
     
     # 提交所有tag
     $ git push [remote] --tags
     
     # 新建一个分支，指向某个tag
     $ git checkout -b [branch] [tag]
     ```

   - 

   

2. 基本快照操作

   - `add`

     ```shell
     # 添加指定文件到暂存区
     $ git add [file1] [file2] ...
     # 添加指定目录到暂存区，包括子目录
     $ git add [dir]
     # 添加当前目录的所有文件到暂存区
     $ git add .
     # 添加每个变化前，都会要求确认
     # 对于同一个文件的多处变化，可以实现分次提交
     $ git add -p
     ```

   - `rm`

     ```shell
     # 删除工作区文件，并且将这次删除放入暂存区
     $ git rm [file1] [file2] ...
     # 停止追踪指定文件，但该文件会保留在工作区，从远程版本库中删除文件，但不删除本地文件
     $ git rm --cached [file]
     # 改名文件，并且将这个改名放入暂存区
     $ git mv [file-original] [file-renamed]
     ```

   - `commit`

     ```shell
     #提交单个文件
     git commit  <file>
     #提交暂存区的文件到仓库区
     git commit –m “description”
     # 提交工作区自上次commit之后的变化，直接到仓库区，等同执行git add、 git rm及git commit
     git commit -a
     # 撤销最后一次提交记录，重新提交；如果代码没有任何新变化，则用来改写上一次commit的提交信息
     git commit --amend
     ```

   - `reset`

     ```shell
     git reset <file>    # 从暂存区恢复某一文件
     git reset -- .      # 从暂存区恢复所有文件
     git reset  –hard HEAD^/HEAD~  #回退到上一版本
     git reset  –hard <commit_id>    #回退到指定版本
     git reset HEAD file  #取消add文件
     # 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
     $ git reset [file]
     
     # 重置暂存区与工作区，与上一次commit保持一致
     $ git reset --hard
     
     # 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
     $ git reset [commit]
     
     # 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
     $ git reset --hard [commit]
     
     # 重置当前HEAD为指定commit，但保持暂存区和工作区不变
     $ git reset --keep [commit]
     ```

     

   - `status`

     ```shell
     git status        #查看仓库状态，显示有变更的文件
     ```

     

   - `diff`

     ```shell
     
     ```

     

   

   

   

3. 审核&比较

   - `show`

     ```shell
     git show ($id)  # 显示某次提交的内容
     
     # 显示某次提交的元数据和内容变化
     $ git show [commit]
     
     # 显示某次提交发生变化的文件
     $ git show --name-only [commit]
     
     # 显示某次提交时，某个文件的内容
     $ git show [commit]:[filename]
     ```

     

   - `log`

     ```shell
     git log   (file)       #查看（文件）提交记录
     git log -p <file>   # 查看每次详细修改内容的diff
     git log -p -2       # 查看最近两次详细修改内容的diff
     git log --stat      # 查看提交统计信息
     git log -g #同上，用'log'格式输出
     git log -- grep "name" # 搜索包含name的log 
     git log record-ID  -l -p #查看指定ID记录，-l:显示一行，-p:显示详细修改
     
     # 显示当前分支的版本历史
     $ git log
     
     # 显示commit历史，以及每次commit发生变更的文件
     $ git log --stat
     
     # 搜索提交历史，根据关键词
     $ git log -S [keyword]
     
     # 显示某个commit之后的所有变动，每个commit占据一行
     $ git log [tag] HEAD --pretty=format:%s
     
     # 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
     $ git log [tag] HEAD --grep feature
     
     # 显示某个文件的版本历史，包括文件改名
     $ git log --follow [file]
     $ git whatchanged [file]
     
     # 显示指定文件相关的每一次diff
     $ git log -p [file]
     
     # 显示过去5次提交
     $ git log -5 --pretty --oneline
     ```

   - `shortlog`

     ```shell
     # 显示所有提交过的用户，按提交次数排序
     $ git shortlog -sn
     ```

     

   - `diff`

     ```shell
     git diff <file>     # 比较当前文件和暂存区文件差异
     git diff   #比较所有文件
     git diff master..Andylee-Github/master #比较本地和远端仓库
     git diff <$id1> <$id2>   # 比较两次提交之间的差异
     git diff <branch1>..<branch2> #比较分支
     git diff --staged   # 比较暂存区和版本库差异
     git diff --cached   # 比较暂存区和版本库差异
     git diff --stat     # 仅仅比较统计信息
     
     # 显示暂存区和工作区的差异
     $ git diff
     
     # 显示暂存区和上一个commit的差异
     $ git diff --cached [file]
     
     # 显示工作区与当前分支最新commit之间的差异
     $ git diff HEAD
     
     # 显示两次提交之间的差异
     $ git diff [first-branch]...[second-branch]
     
     # 显示今天你写了多少行代码
     $ git diff --shortstat "@{0 day ago}"
     ```

4. 补丁Patch

      - `diff`

        ```shell
        git diff > ../sync.patch         # 生成补丁
        ```

      - `cherry-pick`

        ```shell
        
        ```

        

      - `apply`

        ```shell
        git apply ../sync.patch          # 打补丁
        git apply --check ../sync.patch  # 测试补丁能否成功
        ```

        

      - `revert`

        ```shell
        git revert <$id>    # 恢复某次提交的状态，恢复动作本身也创建了一次提交对象
        git revert HEAD     # 恢复最后一次提交的状态
        git revert <commit>  # 撤销提交
        git revert --continue # 处理完后，重新提交
        
        # 新建一个commit，用来撤销指定commit
        # 后者的所有变化都将被前者抵消，并且应用到当前分支
        $ git revert [commit]
        ```

        

      - `rebase`

        ```shell
        
        ```

      

5. 调试 Debug

   - `blame`

     ```shell
     # 显示指定文件是什么人在什么时间修改过
     $ git blame [file]
     ```

     

   - `grep`

6. 管理

   - `archive`
   - `bundle`
   - `clean`

7. Nothing



  

   

   



