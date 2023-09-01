## Linux系统部署Java环境

>参考：https://blog.csdn.net/angus_Lucky/article/details/81333452

### 1.下载Java Linux版本包

1.8版本下载地址：https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html<br>

Windows系统下载完成后上传到Linux系统，以下演示的是虚拟机上传的共享文件夹上传。其他方式有xshell、sftp等

### 2. 设置共享文件夹

设置共享文件夹：打开VM -> 虚拟机设置 -> 选项 -> 启用共享文件夹 -> 设置主机文件夹路径 <br>
Linux系统查看共享文件夹：cd mnt/hgfs

```shell
[root@localhost /]# ls
app  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@localhost /]# cd mnt
[root@localhost mnt]# ls
hgfs
[root@localhost mnt]# cd hgfs
[root@localhost hgfs]# ls
vm_share
[root@localhost hgfs]#
```


### 3. 复制文件到usr/local文件夹并解压

```shell
//进入共享文件夹
[root@localhost hgfs]# cd mnt/hgfs
//复制安装压缩包
[root@localhost hgfs]# cp jdk-8u181-linux-x64.tar.gz /usr/local/jdk-8u181-linux-x64.tar.gz
//解压
[root@localhost hgfs]# tar -zxvf jdk-8u181-linux-x64.tar.gz
//重命名
[root@localhost hgfs]# mv jdk1.8.0_181 jdk1.8

[root@localhost local]# ls
bin  etc  games  include  jdk1.8  jdk-8u181-linux-x64.tar.gz  lib  lib64  libexec  sbin  share  src

```

### 4. 配置环境变量

```shell
[root@localhost hgfs]# vim /etc/profile
```

```bash
//末尾添加
JAVA_HOME=/usr/local/jdk1.8
JRE_HOME=/usr/local/jdk1.8/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
```

让配置生效
```shell
[root@localhost local]# ls
bin  etc  games  include  jdk1.8  jdk-8u181-linux-x64.tar.gz  lib  lib64  libexec  sbin  share  src
//配置环境变量
[root@localhost local]# vim /etc/profile
//让配置生效
[root@localhost local]# source /etc/profile
```

### 5. 验证配置Java开发环境

```shell
[root@localhost local]# java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```
  