## linux 安装MySql

### 安装前说明
***
linux下安装MySql算是安装一些数据库环境中操作步骤较为规范和完整的，涉及的安装操作基本上和后续的很多其他安装软件的风格很像的，类比和迁移，也会很快掌握很多其他的安装软件，比如redis，ES, clickhouse等等。

这里选择的安装版本是Percona mysql 8.0，建议选择稳定版本，目前已知的稳定版本是

### 安装环境说明
***
* 操作系统：centos 7
* MySQL版本：Percona 8.0.16-7

### 安装步骤
*** 
1： 下载二进制安装包

`wget https://www.percona.com/downloads/Percona-Server-LATEST/Percona-Server-8.0.16-7/binary/tarball/Percona-Server-8.0.16-7-Linux.x86_64.ssl101.tar.gz`

2:  将安装包解压到工作路径下，这里选择将二进制包解压到/usr/local

```
cd /usr/local  
tar xfz Percona-Server-8.0.16-7-Linux.x86_64.ssl102.tar.gz
cd Percona-Server-8.0.16-7  -- 查看文件夹内容是否完整
mv Percona-Server-8.0.16-7 mysql
```

3： 程序账户管理 -- 添加mysql 用户组

`groupadd -r mysql && useradd -r -g mysql -s /bin/false -M mysql`

4:  安装目录配置 --创建MySQL数据库文件的存放路径以及相关安全配置

```
mkdir -p /data2/mysql/{3306,3307,3308}/{conf,db,log,backup,redolog,undolog,tmp,binlog}
chown -R mysql:mysql /usr/local/mysql/ && touch /data2/mysql/5306/log/error.log
chown -R mysql:mysql /data2/mysql/ && chmod -R go-rwx /data2/mysql/
```

5:  

#### 配置档

### 总结

