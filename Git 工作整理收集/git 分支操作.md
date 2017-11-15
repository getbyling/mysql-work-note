git 分支操作
============

#### 1 查看远程分支
```git
git branch -a  
* br-2.1.2.2  
  master  
  remotes/origin/HEAD -> origin/master  
  remotes/origin/br-2.1.2.1  
  remotes/origin/br-2.1.2.2  
  remotes/origin/br-2.1.3  
  remotes/origin/master 
``` 

#### 2 查看本地分支
```git
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git branch  
* br-2.1.2.2  
  master
```  

#### 3 创建分支
```git
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git branch test  
  
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git branch  
* br-2.1.2.2  
  master  
  test  
```
>下面是把分支推到远程分支 
```git
git push origin test  
```

#### 4 切换分支
```git
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git branch  
* br-2.1.2.2  
  master  
  test  
  
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git checkout test  
M       jingwei-server/src/main/java/com/taobao/jingwei/server/service/cmd/GetCustomerTarCmd.java  
M       jingwei-server/src/main/java/com/taobao/jingwei/server/util/ServerUtil.java  
Switched to branch 'test'  
  
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (test)  
git branch  
  br-2.1.2.2  
  master  
* test  
```

#### 5 删除本地分支
```git
l@l-PC MINGW64 /d/GaiayCode/origincode/extends (develop)
$ git branch -d hotfix/sp32.0.1
Deleted branch hotfix/sp32.0.1 (was 076cda7).
```

>M 表示cong 原来分支（上一次修改没有提交br-2.1.2.2）带过来的修改
```git
git checkout br-2.1.2.2  
M       jingwei-server/src/main/java/com/taobao/jingwei/server/service/cmd/GetCustomerTarCmd.java  
M       jingwei-server/src/main/java/com/taobao/jingwei/server/util/ServerUtil.java  
Switched to branch 'br-2.1.2.2'  
  
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git br  
* br-2.1.2.2  
  master  
  test  
  
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git br -d test  
Deleted branch test (was 17d28d9).  
  
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)  
git br  
* br-2.1.2.2  
  master  
```

>查看本地和远程分支  -a。前面带*号的代表你当前工作目录所处的分支
```git
remotes/origin/HEAD -> origin/master #啥意思呢？  
```
>“在clone完成之后，Git 会自动为你将此远程仓库命名为origin（origin只相当于一个别名，运行git remote –v或者查看.git/config可以看到origin的含义），并下载其中所有的数据，建立一个指向它的master 分支的指针，我们用(远程仓库名)/(分支名) 这样的形式表示远程分支，所以origin/master指向的是一个remote branch（从那个branch我们clone数据到本地）”
   这个是执行git remote -v 的结果，看出来origin其实就是远程的git地址的一个别名。
```git
git remote  -v  
origin git@xxxx/jingwei.git (fetch)  
origin git@xxxx/jingwei.git (push)  
```

```git
shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (test)  
git branch -a  
  br-2.1.2.2  
  master  
* test  
  remotes/origin/HEAD -> origin/master  
  remotes/origin/br-2.1.2.1  
  remotes/origin/br-2.1.2.2  
  remotes/origin/br-2.1.3  
  remotes/origin/master  
```

#### 6 删除远程版本
```git
git push origin :br-1.0.0  
```

#### 7 删除远程分支
```git
git branch -r -d origin/branch-name  
git push origin :branch-name  
```

参考资料
--------
- [git 查看远程分支、本地分支、创建分支、把分支推到远程repository、删除本地分支](http://blog.csdn.net/arkblue/article/details/9568249/)
