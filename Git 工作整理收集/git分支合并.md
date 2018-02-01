git 分支合并
----------------
>抓取当前分支文件：
```git
git status
git pull
```
>提交当前分支文件：
```git
git push
```
>切换合并分支：
```git
git checkout develop
```
>抓取合并分支代码：
```git
git pull
```

>合并远程分支：
```git
git status
git merge origin/hotfix/sp31.0.2
```

>提交合并分支代码：
```git
git push
```
>查看尚未暂存的文件更新了哪些部分，不加参数直接输入
```git
 git diff
```
>此命令比较的是工作目录(Working tree)和暂存区域快照(index)之间的差异
也就是修改之后还没有暂存起来的变化内容。

小结
----
>Git鼓励大量使用分支：

>查看分支：git branch

>创建分支：git branch <name>

>切换分支：git checkout <name>

>创建+切换分支：git checkout -b <name>

>合并某分支到当前分支：git merge <name>

>删除分支：git branch -d <name>

>示例操作 1：(有待优化)
```git
l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (feature/zmaccount2.0)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (feature/zmaccount2.0)
$ git checkout develop
Your branch is up-to-date with 'origin/develop'.
Switched to branch 'develop'

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git merge feature/zmaccount2.0
Merge made by the 'recursive' strategy.
 .../biz/impl/ZmAccountInfoManageServiceImpl.java   |  16 +-
 .../accountting/web/ZmAccountInfoManageAction.java |  18 +--
 .../oauth/biz/impl/ZmAccountAuthServiceImpl.java   |  18 +--
 .../zm/account/oauth/web/ZmAccountAuthAction.java  |  95 ++----------
 .../business/zm/common/utils/ResultModelUtil.java  |  32 ----
 .../live/category/web/LivePlateformTypeAction.java | 135 ++--------------
 .../zm/live/category/web/LiveTypeAction.java       | 172 +++++----------------
 src/test/java/cn/gaiay/util/DateTimeTest.java      |  27 ++++
 8 files changed, 106 insertions(+), 407 deletions(-)
 create mode 100644 src/test/java/cn/gaiay/util/DateTimeTest.java

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git diff

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git add .

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git commit -m 'merge feature/zmaccount2.0 branch'
On branch develop
Your branch is ahead of 'origin/develop' by 4 commits.
  (use "git push" to publish your local commits)
nothing to commit, working tree clean

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git commit
On branch develop
Your branch is ahead of 'origin/develop' by 4 commits.
  (use "git push" to publish your local commits)
nothing to commit, working tree clean

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git push
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 468 bytes | 0 bytes/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote:
remote: To create a merge request for develop, visit:
remote:   http://192.168.0.236/zm/zmaccount/merge_requests/new?merge_request%5Bsource_branch%5D=develop
remote:
To 192.168.0.236:zm/zmaccount.git
   01190164..872a818c  develop -> develop

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git checkout feature/zmaccount2.0
Your branch is up-to-date with 'origin/feature/zmaccount2.0'.
Switched to branch 'feature/zmaccount2.0'
```