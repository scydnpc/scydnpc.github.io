---
title: Git Flow & 常用Git命名和操作
date: 2021-12-10
categories:
- git
tags:
- git flow
- git命令
---
## # 简述 Git Flow
简单来说，`Git Flow` 就是一套规范的代码管理流程
## # 分支应用情景
- 在`Git Flow`中，主要的分支有`master`、`develop`、`hotfix`、`release`、`feature` 这五种分支  
- `develop` 和 `master`会被保留，其他的随任务结束而删除  
### # master 分支
- 存放稳定上线版本
- 这个分支的来源只能从别的分支合并过来，开发者不会直接commit到这个分支上。
- 通常我们也会在这个分支上的提交打上版本号标签。
### # develop 分支
- 这个分支主要是所有开发的基础分支。
- 当要添加功能时，所有功能都是从这个分支切出去的，而功能分支实现后，也都会合并回来这个分支中。
### # hotfix 分支
- 用途：处理紧急线上问题
- 基于master分支创建
- 修复完成，合并到master分支和develop分支中
### # release 分支 (预上线分支)
- 当`develop`分支完成需求后，就可以从`develop`分支中开一个`release`分支，进行上线前最后的测试。
- 测试完成后，释放`release`分支将会同时合并到`master`以及`develop`分支中。
### # feature分支
- 当我们需要补充功能的时候，就会从develop分支中开一个feature分支进行功能开发。
- 当功能实现后，在将feature分支合并到develop分支中，等待最后的测试发布。
  
### # 具体场景
#### 1. 新功能开发
> 基于`develop`，新建功能分支 `f1-feature`，切换到该分支下进行开发
需要多人开发时，可以推到远端分支
完成功能开发，将该分支合并到`develop`分支，**并删除`f1-feature`**分支

**2021/12/24 -- 更新：使用 `rebase` 整理提交**  
关于新分支（命名`f1-feature` 或者 `feat/xxx` ）  
**1. develop有更新内容需要同步**  
```shell
# 提交代码
git add .
git commit -m 'message'

# 切换 develop 拉取代码
git checkout develop
git pull

# 切换 本地分支（例：feature），合并代码
git checkout feature
git rebase -i develop
```
-i 意为合并多次提交，运行后显示前几次的提交内容如下：  
```
pick 281812b message1  
pick e60c7f4 message2  
```
当需要合并所有提交的时候，保留一个`pick`，其余修改为 `s`，如下： 
```
pick 281812b message1  
s e60c7f4 message2  
```
`:wq` 提交修改

**代码冲突时，修改冲突后，重新`add`提交，执行`git commit`，展示`message`，根据需求自行修改，再 `git rebase --continue`**  

**2. 本地分支开发完毕需要合并到develop** 

```shell
# 先提交本地分支代码 add --> commit
git rebase -i develop
# 后面的操作如1所示
# 需要新增的步骤：切换到develop分支，merge 本地分支
git merge feature

# 最后 git push
```
需要注意的点：在 `git rebase -i develop` 的时候，根据自身情况选择对commit是否进行合并
#### 2. 发布上线
基于`develop`分支创建`release-0.1`分支作为预上线分支,有问题，在该分支上修改，最后切换到`develop`和`master`分支下合并该分支，删除`release-0.1`分支
#### 3.紧急bug修复
基于`master`分支新建`hotfix-bug1`分支进行修改，修改完切换到`develop`和`master`分支下合并该分支，删除`hotfix-bug1`分支
### # 常用git操作
**分支相关**
```
// 查看本地分支
git branch
返回🌰
*master

// 查看远程分支
git branch -r
返回🌰
origin/master

// 查看本地和远程分支
git branch -a
返回🌰
*master
 remotes/origin/master
 
// 切换分支
git checkout <branch-name>

// 创建并切换到该分支
git checkout -b <branch-name>

// 删除分支
git branch -d <branch-name>

// 删除远端分支
git push origin -d <branch-name>

// 重命名分支
git branch -m <oldbranch-name> <newbranch-name>

// 合并指定分支到当前分支
git merge <branch-name>
```



