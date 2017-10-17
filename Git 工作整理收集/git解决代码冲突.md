git 解决代码冲突
----------------
>抓取当前分支代码：
```
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
```
git add .
```
>添加的本地文件提到仓库：
```
git commit -m 'merge code'
```
>提到仓库：
```text
git commit
```

>抓取服务器分支代码：
```text
 git pull
```

>手动解决冲突``` -_- ```

>提交代码：
```text
git push
```

>抓取验证代码：
```text
git pull
```