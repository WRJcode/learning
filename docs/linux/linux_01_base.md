## linux基本命令

### 文件管理

>ls 展示当前目录下文件和列表
```shell
[root@localhost /]# ls
app  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@localhost /]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
2e2e21736944        redis:6.0.8         "docker-entrypoint.s…"   14 hours ago        Up 14 hours         0.0.0.0:6379->6379/tcp   redis1
[root@localhost /]# cd app
[root@localhost app]# ls
redis
[root@localhost app]# cp redis
cp: missing destination file operand after ‘redis’
Try 'cp --help' for more information.
[root@localhost app]# cd redis
[root@localhost redis]# ls
data  redis.conf
[root@localhost redis]# cd data
[root@localhost data]# ls
dump.rdb
[root@localhost data]# clear
[root@localhost data]# cd /
[root@localhost /]# ls
app  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@localhost /]# ls -a
.  ..  app  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@localhost /]# ls ll
ls: cannot access ll: No such file or directory
[root@localhost /]# ls -ll
total 20
drwxr-xr-x.   3 root root   19 Jul 12 05:15 app
lrwxrwxrwx.   1 root root    7 Jul 12 10:23 bin -> usr/bin
dr-xr-xr-x.   5 root root 4096 Jul 12 06:35 boot
drwxr-xr-x.  19 root root 3160 Jul 12 06:35 dev
drwxr-xr-x.  79 root root 8192 Jul 12 06:35 etc
drwxr-xr-x.   3 root root   19 Jul 12 07:49 home
lrwxrwxrwx.   1 root root    7 Jul 12 10:23 lib -> usr/lib
lrwxrwxrwx.   1 root root    9 Jul 12 10:23 lib64 -> usr/lib64
drwxr-xr-x.   2 root root    6 Apr 11  2018 media
drwxr-xr-x.   3 root root   18 Jul 12 04:18 mnt
drwxr-xr-x.   2 root root    6 Apr 11  2018 opt
dr-xr-xr-x. 138 root root    0 Jul 12 06:00 proc
dr-xr-x---.   3 root root  179 Jul 12 06:47 root
drwxr-xr-x.  27 root root  800 Jul 12 06:35 run
lrwxrwxrwx.   1 root root    8 Jul 12 10:23 sbin -> usr/sbin
drwxr-xr-x.   2 root root    6 Apr 11  2018 srv
dr-xr-xr-x.  13 root root    0 Jul 12 06:34 sys
drwxrwxrwt.  22 root root 4096 Jul 12 06:48 tmp
drwxr-xr-x.  13 root root  155 Jul 12 10:23 usr
drwxr-xr-x.  19 root root  267 Jul 12 10:31 var
[root@localhost /]# ls -a -ll
total 20
```

>cp
```shell
[root@localhost home]# mkdir test1
[root@localhost home]# cp test1 /copy
cp: omitting directory ‘test1’
[root@localhost home]# mkdir copy
[root@localhost home]# cp test1 /copy
cp: omitting directory ‘test1’
[root@localhost home]# ls
copy  test1  user1
[root@localhost home]# cp -a test1 copy/test1_copy
[root@localhost home]# ls
copy  test1  user1
[root@localhost home]#
[root@localhost home]# cd copy
[root@localhost copy]# ls
test1_copy
[root@localhost copy]#
```


 

