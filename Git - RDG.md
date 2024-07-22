# Git实用指南  -- RDG

 

Git的本地结构

<img src="Git - RDG.assets/wps1.jpg" alt="img" style="zoom: 25%;" />

### 添加“新建/修改”

```shell
git add [file name]
```

将工作区的“新建/修改”添加到暂存区

git rm命令是把建立的版本库索引（index）和那个文件一起删除了。加上cached之后，就只删除索引，不删除文件本身。
与git add相应的取消操作并不是git rm，而是git rm –cached。这是需要非常注意的地方。

```shell
git rm --cached [file name]
```

撤销暂存区所有“新建/修改”

```shell
git reset --hard HEAD 
```

 

### 撤回 git add

使用场景你错误提交git add了N多文件,其中有你不希望提交的

放弃所有的暂存区修改可以使用, 

```shell
git reset HEAD . 放弃所有的暂存区修改可以使用
```

```shell
git reset HEAD readme.md 来放弃指定文件的缓存
```

### 状态查看

```shell
git status
```

查看工作区、暂存区状态

<img src="Git - RDG.assets/wps2.jpg" alt="img" style="zoom: 50%;" /> 

```shell
git add new.text
```

<img src="Git - RDG.assets/wps3.jpg" alt="img" style="zoom:50%;" /> 

 

<img src="Git - RDG.assets/wps4.jpg" alt="img" style="zoom:50%;" />  

### 提交

```shell
git commit -m "commit message" [file name]

git commit -am"commit message"
```

将暂存区的内容提交到本地库

修改提交message

```shell
git commit --amend
```

会弹出编辑窗口

### 撤回提交

1，还在本地没有推送到远端

```shell
git add . //添加所有文件
git commit -m "本功能全部完成"
```

此时还没git push

```shell
git reset --soft HEAD~1
```

如果是撤回两个连续提交 git reset --soft HEAD~2

这样就成功的撤销了你的commit

注意，仅仅是撤回commit操作，您写的代码仍然保留

2，错误提交, 并且推送到远端了,撤回的办法:

```shell
git reset --soft aa909cff
git push –-force 或 git push -f -u origin master
```

3,通过反做来撤销一个提交,会产生新的一个提交

```shell
git revert -n 8b89621019c9adc6fc4d242cd41daeb13aeb9861

git commit -m "revert add text.txt"

git push
```

### 拉取

git merge和git pull时代码与本地工作区未提交的代码有冲突 

git提示your local changes to the following files would be overwitten by merge
1,按照提示提交本地工作区的代码 , 再merge 
2,不提交代码 , git stash(隐藏当前工作区),再进行git merge,最后git stash pop(恢复隐藏的工作区);

### 合并

大多数情况是没有冲突的;

只解决一次冲突，分别对应的是当前分支最新提交和合并分支的最新提交的冲突

\- 合并之后产生一个新的提交

\- commit信息按照时间顺序合并

冲突未能解决可放弃本次合并

```shell
git merge --abort
```

 

### 解决冲突

冲突的表现

<img src="Git - RDG.assets/wps5.jpg" alt="img" style="zoom:50%;" /> 

冲突的解决

第一步：编辑文件，删除特殊符号

第二步：把文件修改到满意的程度，保存退出

第三步：git add [文件名]

第四步：git commit -m "日志信息"

注意：此时commit 一不能带具体文件名

 

### 撤销合并

reset 到 merge 前的版本，然后再重做接下来的操作，要求每个合作者都晓得怎么将本地的 [HEAD](https://www.baidu.com/s?wd=HEAD&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 都回滚回去：

```shell
git checkout [行merge操作时所在的分支]
git reset --hard [merge前的版本号]
```

### 查看历史记录

```shell
git log
git log --oneline
```

### 时光穿梭机(前进后退)



<img src="Git - RDG.assets/wps6.jpg" alt="img" style="zoom:50%;" /> 

 

![img](Git - RDG.assets/wps7.png)<img src="Git - RDG.assets/wps8.jpg" alt="img" style="zoom:50%;" /> 

基于索引值操作[推荐]

```shell
git reset --hard [局部索引值]
git reset --hard a6ace91
```

使用^符号：只能后退

```shell
git reset --hard HEAD^
```

注：一个^表示后退一步，n 个表示后退n 步

使用~符号：只能后退

```shell
git reset --hard HEAD~n
```

注：表示后退n 步

![img](Git - RDG.assets/wps9.png)![img](Git - RDG.assets/wps10.png)![img](Git - RDG.assets/wps11.png) 

reset 命令的三个参数对比 默认—mixed

--soft 参数  仅仅在本地库移动 HEAD 指针

--mixed 参数  在本地库移动 HEAD 指针 + 重置暂存区 

--hard 参数 在本地库移动 HEAD 指针 + 重置暂存区 + 重置工作区 

### 文件对比

```shell
git diff [本地库中历史版本] [文件名]
```

 

### 千里之行,始于切糕

1,切换分支

```shell
git checkout 分支名 
```

2,脱离分支切换历史版本

```shell
git checkout commit_id版本号
```

3,拷贝其他分支的文件到本地

```shell
git checkout 分支名 文件名
```

举个栗子:如果只是简单的将feature分支的文件f.txt copy到master分支上；

```shell
git checkout master
git checkout feature f.txt
如果突然不想合并过来 ,想还原master
git checkout master f.txt
```

4,拷贝其他历史版本的文件到本地

```shell
git checkout commit_id版本号  文件名
```

5,放弃某个文件的改动

```shell
git checkout文件名
```

6,放弃所有工作区的修改

```shell
git checkout . 
```

7,切换到指定标签

```shell
git checkout 标签名  
```

###  新建分支

```shell
Git checkout -b v1230 
Git push –set-upstream origin v1230 
```

 

### 便民套餐

```shell
git commit -am"关联马蜂窝" && git pull && git push 
```

 

### Work flow

<img src="Git - RDG.assets/wps12.jpg" alt="img" style="zoom: 33%;" />

<img src="Git - RDG.assets/22529981-95277b2c18d05043.webp" alt="22529981-95277b2c18d05043" style="zoom:50%;" />

 

### 热修复

Bug出现:

基于master创建hotfix分支

```shell
git checkout -b hotfix
```

1,微小改动,在本地环境就可以完成验证的

验证通过后,发布人员在master分支merge 该hotfix分支后发布;

2,需要在测试环境验证的

测试环境git checkout hotfix ,测试人员进行验证

验证通过后,发布官在master分支merge 该hotfix分支后发布;

```shell
git checkout master
git merge hotfix
git push
```

生产服务器 git pull

### 标签

```shell
git tag -a tagName -m "my tag" (新建本地标签)
```

```shell
git tag -a tagName 9fceb02 -m "my tag" (给指定commit版本打标签)
git push origin v20191205  (本地标签推送到远端服务器)
```

git tag  (显示所有标签) 

```shell
git show tagName  (显示标签详情)

git tag -d v0.1.2   (删除本地标签)

git push origin :refs/tags/<tagName>  (远端删除标签)

#example:  
git push origin :refs/tags/v0.1.2 远端删除
```

 

遇事不对 , 莫要慌 , 这本git宝典 , 翻开它

祝大家切糕愉快