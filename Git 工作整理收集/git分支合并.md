git 分支合并
----------------
>抓取当前分支文件：
```
git status
git pull
```
>提交当前分支文件：
```
git push
```
>切换合并分支：
```
git checkout develop
```
>抓取合并分支代码：
```text
git pull
```

>合并远程分支
```
git status
git merge origin/hotfix/sp31.0.2
```

>示例操作：
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

```