## Centos 安装superset
***
公司数据分析人员需要将日常监控分析数据进行可视化，在踩了一些坑之后，终于在业务环境中搭建成功superet,复现两次流程也是成功的，分享一波。。。

### 业务环境说明
***
    * 操作系统：centos 7
    * python 3.6
### 安装步骤简介和重点
***
    1. 安装python3以上 (网上教程一堆，请自行搜索，后面自己也计划汇总一版流程)
    2. 安装容器工具，建议直接按照以下指令顺序执行就好
    ```
    # yum upgrade python-setuptools
    # yum install gcc gcc-c++ libffi-devel python-devel python-pip python-wheel openssl-devel cyrus-sasl-devel openldap-devel
    # pip3 install cryptography
    # pip3 install virtualenv
    
    ```
    3. 新建一个容器空间进行操作，（网上一堆的教程都是将superset安装在容器之中，可能是为了环境隔离吧，因为中间需要使用的第三方脚本还是很多的，避免影响了原始业务系统环境吧）
    # python3 -m venv venv  -- 新建一个名为venv的容器空间，同时也会新建在当前目录下新建一个venv的文件夹，请提前切换好工作文件夹
    # .  venv/bin/activate  -- 进入venv的容器环境
    # pip3 install superset -- 容器环境下安装superset
### 小结
***
