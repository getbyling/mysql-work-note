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
remote: Counting objects: 73, done.
remote: Compressing objects: 100% (55/55), done.
remote: Total 73 (delta 8), reused 45 (delta 2)
Unpacking objects: 100% (73/73), done.
From 192.168.0.236:zm/zmaccount
 0d0dfb3a..0757d619  develop       -> origin/develop
 7f24245f..6aaef8b5  feature/agent -> origin/feature/agent
 f80c0643..f446d3af  feature/live  -> origin/feature/live
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (feature/zmaccount2.0)
$ git push
Everything up-to-date

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (feature/zmaccount2.0)
$ git checkout develop
Your branch is behind 'origin/develop' by 41 commits, and can be fast-forwarded.
(use "git pull" to update your local branch)
Switched to branch 'develop'

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git pull
Updating 8e97dae4..0757d619
Fast-forward
.../biz/ZmAccountInfoManageService.java            |   3 +-
.../biz/impl/ZmAccountInfoManageServiceImpl.java   |   3 +-
.../accountting/web/ZmAccountInfoManageAction.java |   6 +-
.../common/constants/ZmMemberResourceCodeEnum.java | 139 +++++++----
4 files changed, 9 insertions(+), 7 deletions(-)

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git diff

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git add .

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git merge feature/zmaccount2.0
Merge made by the 'recursive' strategy.
.../biz/ZmAccountInfoManageService.java            |  14 +-
.../biz/impl/ZmAccountInfoManageServiceImpl.java   |  79 ++++++++++-
.../accountting/dao/ZmAccountInfoMapper.java       | 158 +++++++++++----------
.../accountting/vo/ZmAccountManageListVo.java      | 118 +++++++++++++++
.../accountting/web/ZmAccountInfoManageAction.java |  14 +-
.../oauth/biz/impl/ZmAccountAuthServiceImpl.java   |   2 +-
.../account/accountting/ZmAccountInfoMapper.xml    |  40 ++++++
.../account/oauth/ZmAccountCertificateMapper.xml   |   2 +-
8 files changed, 329 insertions(+), 98 deletions(-)
create mode 100644 src/main/java/cn/gaiay/business/zm/account/accountting/vo/ZmAccountManageListVo.java

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git pull
remote: Counting objects: 30, done.
remote: Compressing objects: 100% (22/22), done.
remote: Total 30 (delta 8), reused 0 (delta 0)
Unpacking objects: 100% (30/30), done.
From 192.168.0.236:zm/zmaccount
 0757d619..4afbeb03  develop       -> origin/develop
 6aaef8b5..aec7d584  feature/agent -> origin/feature/agent
Merge made by the 'recursive' strategy.
.../order/biz/impl/OpenZmAccountServiceImpl.java   | 24 ++++++++++++++++------
1 file changed, 18 insertions(+), 6 deletions(-)

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git diff

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git add .

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git commit -m 'merge feature/zmaccount2.0 branch'
On branch develop
Your branch is ahead of 'origin/develop' by 3 commits.
(use "git push" to publish your local commits)
nothing to commit, working tree clean

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git commit
On branch develop
Your branch is ahead of 'origin/develop' by 3 commits.
(use "git push" to publish your local commits)
nothing to commit, working tree clean

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git push
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (10/10), done.
Writing objects: 100% (18/18), 1.32 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote:
remote: To create a merge request for develop, visit:
remote:   http://192.168.0.236/zm/zmaccount/merge_requests/new?merge_request%5Bsource_branch%5D=develop
remote:
To 192.168.0.236:zm/zmaccount.git
 4afbeb03..21f28507  develop -> develop

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/zmaccount (develop)
$ git checkout feature/zmaccount2.0
Your branch is up-to-date with 'origin/feature/zmaccount2.0'.
Switched to branch 'feature/zmaccount2.0'
```