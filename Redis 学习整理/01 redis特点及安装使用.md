Redis 安装使用
==============

准备工作
--------
>通过以下方法输入当前管理员用户的密码就可以进到root用户
```text
sudo -i
```

执行工作
--------
>1.Ubuntu 切换到下载目录，并下载 Redis 压缩包，下载 stable 版本
```text
cd /vagrant/environment/

wget http://download.redis.io/releases/redis-4.0.7.tar.gz
```

>2.解压 Redis 安装包
```text
tar zxvf redis-4.0.7.tar.gz
```

>3.进入解压后的 Redis 文件，并查看
```text
cd redis-4.0.7/
ls
```
>返回结果
```text
ubuntu@ubuntu-xenial:/vagrant/environment/redis-4.0.7$ ls
00-RELEASENOTES  COPYING  Makefile   redis.conf       runtest-sentinel  tests
BUGS             deps     MANIFESTO  runtest          sentinel.conf     utils
CONTRIBUTING     INSTALL  README.md  runtest-cluster  src
```

>4.解压后的 Redis，可以看到并没有**config**文件。其实 Redis 解压后的文件是已经配置好的，所以不需要**config**文件。直接编译就可以了，下面是编译 Redis 文件
```text
make
```
>如果出现以下错误：
```text
ubuntu@ubuntu-xenial:/vagrant/environment/redis-4.0.7$ make
The program 'make' can be found in the following packages:
 * make
 * make-guile
Try: sudo apt install <selected package>
```
>解决方法，安装 build-essential，执行以下命令。安装过程中，有可能因为网速，要等待一会。
```text
sudo apt-get install build-essential
```
>如执行**sudo apt-get install build-essential**出现以下问题：
```text
404  Not Found [IP: 91.189.88.149 80]
Err:2 http://security.ubuntu.com/ubuntu xenial-security/main amd64 libc-dev-bin amd64 2.23-0ubuntu9
```
>可执行
>安装完成后，继续执行以上编译工作
```text
make
```

>5.**make**成功后，会提示以下内容，建议执行 make test 测试：
```text
Hint: It's a good idea to run 'make test' ;)
```

>6.执行**make test**
```text
make test
```
>执行**make test**后，返回以下内容：
```text
cd src && make test
make[1]: Entering directory '/vagrant/environment/redis-4.0.7/src'
    CC Makefile.dep
make[1]: Warning: File 'Makefile.dep' has modification time 0.02 s in the future
You need tcl 8.5 or newer in order to run the Redis test
Makefile:242: recipe for target 'test' failed
make[1]: *** [test] Error 1
make[1]: Leaving directory '/vagrant/environment/redis-4.0.7/src'
Makefile:6: recipe for target 'test' failed
make: *** [test] Error 2
```

>7.执行 yum install tcl
```text
yum install tcl
```
>如返回以下内容：
```text
The program 'yum' is currently not installed. You can install it by typing:
sudo apt install yum
```
>先执行 sudo apt install yum，再执行 yum install tcl
```text
sudo apt install yum
yum install tcl
```
>Ubuntu 的安装指令为：
```text
sudo apt-get install tcl
```



>5.
```text

```

