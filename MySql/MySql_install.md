## linux 安装MySql

### 安装前说明
***

这里选择的安装版本是Percona mysql 8.0，建议选择稳定版本，目前已知的稳定版本是 8.0.18 , 本文介绍操作使用的是8.0.16-7

安装mysql的方式有很多种，本文介绍的是基于二进制包解压安装的方式，无需编译

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

3： 程序账户设置 -- 添加mysql 用户组

`groupadd -r mysql && useradd -r -g mysql -s /bin/false -M mysql`

4:  安装目录配置 --创建MySQL数据库文件的存放路径以及相关安全配置

```
mkdir -p /data2/mysql/3306/{conf,db,log,backup,redolog,undolog,tmp,binlog}
chown -R mysql:mysql /usr/local/mysql/ && touch /data2/mysql/3306/log/error.log
chown -R mysql:mysql /data2/mysql/ && chmod -R go-rwx /data2/mysql/
```

5:  将程序的相关配置写入系统环境 - 将mysql相关命令配置成系统指令

`echo -e '\n\nexport PATH=/usr/local/mysql/bin:$PATH\n' >> /etc/profile && source /etc/profile` 

6:  程序配置档修改  -- 配置mysql conf文档

    mysql的配置档可以专门一文来讲解，其中很多重点参数的设置，这里只是牵扯到安装的话，暂时不讲解这么详细
    
    在/data2/mysql/3306/conf/ 路径下新建一个my_3306.cnf文档，注意里面的几个重点参数需要修改

7： 启动程序

```
/usr/local/mysql/bin/mysqld --defaults-file=/data2/mysql/3306/conf/my_3306.cnf --initialize-insecure --user=mysql  --初始化mysql 环境，8.0初始化默认root登录无密码

mysqld_safe --defaults-file=/data2/mysql/5306/conf/my_3306.cnf &  -- 开启mysql
```

8： 测试使用

```
mysql -S /data2/mysql/5306/mysql_5306.sock -h localhost -u root -p  -- 直接回车登录，刚初始化设置root 无密码登录
ALTER USER USER() IDENTIFIED BY 'XXXX';  -- 修改root密码
```


#### 配置档

### 总结
***

linux下安装MySql算是安装一些数据库环境中操作步骤较为规范和完整的，涉及的安装操作基本上和后续的很多其他安装软件的风格很像的，类比和迁移，也会很快掌握很多其他的安装软件，比如redis，ES, clickhouse等等。
