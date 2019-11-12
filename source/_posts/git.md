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
  # 新建一个目录，将其初始化为Git代码库
  $ git init [project-name]
  mkdir robbin_site.git + cd robbin_site.git + git --bare init # 在服务器创建纯仓库
  ```

  

- `clone`

  ```shell
  # 下载一个项目和它的整个代码历史
  $ git clone [url]
  git clone https://github.com/AndyLee-Github/cartboon.git   #从远程仓库中克隆
  git clone --bare robbin_site robbin_site.git  # 用带版本的项目创建纯版本仓库
  scp -r my_project.git git@git.csdn.net:~      # 将纯仓库上传到服务器上
  ```

  

- `fetch`

  ```shell
  git fetch origin                 # 抓取远程仓库更新，加下一条指令等同于git pull
  # 下载远程仓库的所有变动
  $ git fetch [remote]
  ```

  

- `pull`

  ```shell
  git pull                         # 抓取远程仓库所有分支更新并合并到本地
  git pull --no-ff                 # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
  # 取回远程仓库的变化，并与本地分支合并
  $ git pull [remote] [branch]
  ```

  

- `push`

  ```shell
  git push                         # push所有分支
  git push origin branch   # 将本地分支推到远程分支
  git push –u origin branch   #推送本地分支到远程仓库，首次提交需要加-u 
  git push origin <local_branch>   # 创建远程分支， origin是远程仓库名
  git push origin <local_branch>:<remote_branch>  # 创建远程分支
  git push origin :<remote_branch>  #先删除本地分支(git br -d <branch>)，然后再push删除远程分支
  
  git push -u origin master                                      # 客户端首次提交
  git push -u origin develop  # 首次将本地develop分支提交到远程develop分支，并且track
  # 删除远程tag
  $ git push origin :refs/tags/[tagName]
  # 提交指定tag
  $ git push [remote] [tag]
  # 提交所有tag
  $ git push [remote] --tags
  
  # 上传本地指定分支到远程仓库
  $ git push [remote] [branch]
  
  # 强行推送当前分支到远程仓库，即使有冲突
  $ git push [remote] --force
  
  # 推送所有分支到远程仓库
  $ git push [remote] --all
  ```

  

- `remote`

  ```shell
  git remote  #查看远程库的信息
  git remote –v  #查看远程库地址和名称
  git remote show origin           # 查看远程服务器仓库状态
  git remote add origin git@github:robbin/robbin_site.git         # 添加远程仓库地址
  git remote set-url origin git@github.com:robbin/robbin_site.git # 设置远程仓库地址(用于修改远程仓库地址)
  git remote rm <repository>       # 删除远程仓库
  git remote add origin git@github.com:robbin/robbin_site.git    # 设置远程仓库地址
  git remote set-head origin master   # 设置远程仓库的HEAD指向master分支
  
  # 显示所有远程仓库
  $ git remote -v
  
  # 显示某个远程仓库的信息
  $ git remote show [remote]
  
  # 增加一个新的远程仓库，并命名
  $ git remote add [shortname] [url]
  
  ```

  

- `submodule`



## 配置gitconfig

用户的配置文件`~/.gitconfig`

```shell
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
git config --global user.name "xxx"   
git config --global user.email "xxxxxx@gmail.com"

git config --global color.ui true
git config --global alias.co checkout # 配置别名，co 配为checkout 别名， 不过我没用
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global core.editor "mate -w"    # 设置Editor使用textmate
git config -l  # 列举所有配置
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
     
     # 建立追踪关系，在现有分支与指定的远程分支之间
     $ git branch --set-upstream [branch] [remote-branch]
     
     # 新建一个分支，但依然停留在当前分支
     $ git branch [branch-name]
     # 新建一个分支，指向指定commit
     $ git branch [branch] [commit]
     # 新建一个分支，与指定的远程分支建立追踪关系
     $ git branch --track [branch] [remote-branch]
     $ git branch --set-upstream master origin/master
     $ git branch --set-upstream develop origin/develop
     
     # 删除分支
     $ git branch -d [branch-name]
     # 强制删除分支 (未被合并的分支被删除的时需要强制)
     $ git branch-D <branch>
     # 删除远程分支
     $ git push origin --delete [branch-name]
     $ git branch -dr [remote/branch]
     ```

   - `checkout`

     ```shell
     # 新建一个分支，并切换到该分支
     $ git checkout -b [branch]
     # 切换到指定分支，并更新工作区
     $ git checkout [branch-name]
     # 切换到上一个分支
     $ git checkout -
     # 基于branch创建新的new_branch
     $ git checkout -b <new_branch> <branch>
     # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除
     $ git checkout  $id
     # 把某次历史提交记录checkout出来，创建成一个分支
     $ git checkout  $id -b <new_branch>
     # 依据需要合并的最后一条 commit 创建新分支
     $ git checkout -b newbranch <last_commit>
     # 新建一个分支，指向某个tag
     $ git checkout -b [branch] [tag]
     
     git checkout -- xx  #撤销xx文件修改
     git checkout .     #撤销工作区修改
     git checkout ${commit} /path/to/file #撤销指定文件到指定版本
     
     git checkout   --track origin/branch     # 跟踪某个远程分支创建相应的本地分支
     git checkout   -b <local_branch> origin/<remote_branch>  # 基于远程分支创建本地分支，功能同上
     
     # 恢复暂存区的指定文件到工作区
     $ git checkout [file]
     
     # 恢复某个commit的指定文件到暂存区和工作区
     $ git checkout [commit] [file]
     
     # 恢复暂存区的所有文件到工作区
     $ git checkout .
     ```

     

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
     # 停止追踪指定文件，但该文件会保留在工作区，从版本库中删除文件，但不删除文件
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



  

   

   



