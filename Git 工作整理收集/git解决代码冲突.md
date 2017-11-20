git 解决代码冲突
================

冲突解决方法（一）
-----------------
>将本地修改存储起来：
```git
git stash
```
>查看本地所有修改存储
```git
git stash list
```
>抓取当前分支代码：
```git
git pull
```
>还原暂存的内容
```git
git stash pop stash@{0}
```
>冲突系统提示如下：
```text
Auto-merging c/environ.c
CONFLICT (content): Merge conflict in c/environ.c
```
>手动解决冲突```-_-```

>删除 stash
```git
git stash drop stash@{0}
```
>如果不加stash编号，默认的就是删除最新的

>清除所有 stash
```git
git stash clear
```
>添加本地文件：
```git
git add .
```
>添加的本地文件提到仓库：
```git
git commit -m 'merge code'
```
>提到仓库：
```git
git commit
```
>提交代码：
```git
git push
```
>抓取验证代码：
```git
git pull
```

冲突解决方法（二）
-----------------
>抓取当前分支代码：
```git
git pull
```
>冲突内容如下：
```text
error: Your local changes to the following files would be overwritten by merge:
        src/main/resources/conf/dev/system_config.properties
Please move or remove them before you merge.
Aborting
```
>添加本地文件：
```git
git add .
```
>添加的本地文件提到仓库：
```git
git commit -m 'merge code'
```
>提到仓库：
```git
git commit
```
>抓取服务器分支代码：
```git
git pull
```
>手动解决冲突-_-

>提交代码：
```git
git push
```
>抓取验证代码：
```git
git pull
```