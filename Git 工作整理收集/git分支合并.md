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

>示例操作 1：
```git
l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop)
$ git branch
* develop
  feature/groupon
  master

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop)
$ git checkout feature/groupon
Your branch is up-to-date with 'origin/feature/groupon'.
Switched to branch 'feature/groupon'

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (feature/groupon)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (feature/groupon)
$ git checkout develop
Your branch is up-to-date with 'origin/develop'.
Switched to branch 'develop'

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop)
$ git merge feature/groupon
Auto-merging src/main/resources/conf/dev/system_config.properties
Auto-merging pom.xml
CONFLICT (content): Merge conflict in pom.xml
Auto-merging .gitignore
Automatic merge failed; fix conflicts and then commit the result.

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop|MERGING)
$ git add .

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop|MERGING)
$ git commit -m 'merge 分支'
[develop ab6f7a2] merge 分支

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop)
$ git push
Counting objects: 21, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (21/21), 1.71 KiB | 0 bytes/s, done.
Total 21 (delta 8), reused 0 (delta 0)
remote:
remote: To create a merge request for develop, visit:
remote:   http://192.168.0.236/server/extends/merge_requests/new?merge_request%5Bsource_branch%5D=develop
remote:
To 192.168.0.236:server/extends.git
   c6d4139..ab6f7a2  develop -> develop

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/ma (develop)
$ git push
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 747 bytes | 0 bytes/s, done.
Total 9 (delta 3), reused 0 (delta 0)
remote:
remote: View merge request for develop:
remote:   http://192.168.0.236/server/ma/merge_requests/14
remote:
To 192.168.0.236:server/ma.git
   c6fd0b3..d9a3b21  develop -> develop
```

>示例操作 2：
```git
l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git branch
  develop
  feature/groupon
  hotfix/sp32.1.5
* hotfix/sp32.1.7
  master
  release/sp32.1

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git pull
remote: Counting objects: 397, done.
remote: Compressing objects: 100% (262/262), done.
remote: Total 397 (delta 151), reused 243 (delta 61)
Receiving objects: 100% (397/397), 72.49 KiB | 0 bytes/s, done.
Resolving deltas: 100% (151/151), done.
From 192.168.0.236:server/extends
   5cb7591f..3979f6cc  develop                -> origin/develop
 * [new branch]        feature/LeavingMessage -> origin/feature/LeavingMessage
 * [new branch]        feature/custom_service -> origin/feature/custom_service
 * [new branch]        hotfix/sp32.1.8        -> origin/hotfix/sp32.1.8
 * [new branch]        hotfix/sp32.1.9        -> origin/hotfix/sp32.1.9
 * [new branch]        maser                  -> origin/maser
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git checkout -b hotfix/sp32.1.8 origin/hotfix/sp32.1.8
Branch hotfix/sp32.1.8 set up to track remote branch hotfix/sp32.1.8 from origin.
Switched to a new branch 'hotfix/sp32.1.8'

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.8)
$ git pull
Already up-to-date.

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.8)
$ git checkout hotfix/sp32.1.7
Your branch is up-to-date with 'origin/hotfix/sp32.1.7'.
Switched to branch 'hotfix/sp32.1.7'

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git merge hotfix/sp32.1.8
Auto-merging src/main/resources/spring/application-datasource.xml
CONFLICT (content): Merge conflict in src/main/resources/spring/application-datasource.xml
Automatic merge failed; fix conflicts and then commit the result.

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7|MERGING)
$ git add .

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7|MERGING)
$ git commit -m 'merge 分支'
[hotfix/sp32.1.7 174265e0] merge 分支

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git add .

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git commit -m '修改冲突'
[hotfix/sp32.1.7 d0f9aabd] 修改冲突
 1 file changed, 4 deletions(-)

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git push
Counting objects: 15, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (15/15), 1.22 KiB | 0 bytes/s, done.
Total 15 (delta 6), reused 0 (delta 0)
remote:
remote: To create a merge request for hotfix/sp32.1.7, visit:
remote:   http://192.168.0.236/server/extends/merge_requests/new?merge_request%5Bsource_branch%5D=hotfix%2Fsp32.1.7
remote:
To 192.168.0.236:server/extends.git
   893e5a2e..d0f9aabd  hotfix/sp32.1.7 -> hotfix/sp32.1.7

l@l-PC MINGW64 /d/GaiayCode/origincode/extends (hotfix/sp32.1.7)
$ git pull
Already up-to-date.
```