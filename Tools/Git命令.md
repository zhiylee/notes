## 报家门

```shell
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

## 创建SSH Key

```shell
ssh-keygen -t rsa -C "youremail@example.com"
```

## 基本命令

````shell
git init

git add <file>

git commit -m <∫>
git commit --amend  #修改commit内容

git status  #查看工作区状态

git diff  #查看文件修改的内容

git log  #查看提交历史
git log --graph  #分支合并图

git reflog  #查看命令历史

git reset --hard commit_id  #版本穿梭

git checkout -- filename  #用版本库里的版本替换工作区的版本


#分支
git branch  #cat

git branch <name>  #creat

git checkout <name>  #switch
git switch <name>

git merge <name>  #合并 <name> 分支到当前分支，需手动处理冲突；加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并；修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除

git branch -d <name>  #delete
git branch -D <name>  #强制删

git branch -m old-branch new-branch #重命名分支 要先返回master


#标签
git tag <tagname>  #用于新建一个标签，默认为HEAD，也可以指定一个commit_id

git tag -a <tagname> -m "blablabla..."  #指定标签信息

git tag  #可以查看所有标签

git tag -d <tagname>  #可以删除一个本地标签

git push origin <tagname>  #可以推送一个本地标签
git push origin --tags  #可以推送全部未推送过的本地标签
git push origin :refs/tags/<tagname>  #可以删除一个远程标签



#reset 撤销commit
git reset --soft HEAD^  #不删除工作空间改动代码，撤销commit，不撤销git add .

git reset HEAD^  #不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
git reset --mixed HEAD^  #同上


git reset --hard HEAD^  #危！！删除工作空间改动代码，撤销commit，撤销git add . ,恢复到了上一次的commit状态


# stash
git stash              # 默认不包含新建的文件
git stash -u           # 包含xi
git stash save "name"  # 命名
git stash pop          # 取第一个
git stash list         # list
git stash drop         # 删 可带名字
git stash clear        # 删所有
git stash show         # 看变更
git stash show -p      # 所有的变更
````

## 关联远程仓库

``` shell
git remote add origin git@server-name:path/repo-name.git

git push -u origin master #第一次推送master分支的所有内容

git push origin master

git pull  #抓取远程的新提交，可能要处理冲突

git remote set-url origin [url] #修改仓库

git remote rm origin #删除关联

git remote -v  #查看关联

git checkout -b branch-name origin/branch-name  #本地创建和远程分支对应的分支，本地和远程分支的名称最好一致

git branch --set-upstream branch-name origin/branch-name  #建立本地分支和远程分支的关联
```

## Commit Message格式

type : subject

type 提交类型：

- feature:     新特性
- fix:     修改问题
- style:     代码格式修改
- test:     测试用例修改
- docs:     文档修改
- refactor:     代码重构
- misc:     其他修改, 比如构建流程, 依赖管理

subject 提交描述

对应内容是commit 目的的简短描述，一般不超过50个字符

## 中文无法显示

```shell
git config --global core.quotepath false
```

## 其他

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

开发一个新feature，最好新建一个分支；