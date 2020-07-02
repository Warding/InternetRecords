## linux 安装superset
***
公司数据分析人员需要将日常监控分析数据进行可视化，在踩了一些坑之后，终于在业务环境中搭建成功superet,后续复现两次流程也是成功的，分享一波。。。

### 业务环境说明
***
* 操作系统：centos 7
* python 3.6
### 安装步骤简介和重点
***
1. 安装python3以上 (网上教程一堆，请自行搜索，后面自己也计划汇总一版流程),**以下安装过程使用的均是python3**
2. 安装容器工具，建议直接按照以下指令顺序执行就好
    # yum upgrade python-setuptools
    # yum install gcc gcc-c++ libffi-devel python-devel python-pip python-wheel openssl-devel cyrus-sasl-devel openldap-devel
    # pip3 install cryptography
    # pip3 install virtualenv
    
3. 新建一个容器空间进行操作，（网上一堆的教程都是将superset安装在容器之中，可能是为了环境隔离吧，因为中间需要使用的第三方脚本还是很多的，避免影响了原始业务系统环境吧）
    ```
    # python3 -m venv venv  -- 新建一个名为venv的容器空间，同时也会新建在当前目录下新建一个venv的文件夹，请提前切换好工作文件夹
    # .  venv/bin/activate  -- 进入venv的容器环境
    # pip3 install superset -- 容器环境下安装superset
    ```
    **以下是重点，最多坑的地方，因为superset的运行依赖很多第三方库代码，所以安装python的依赖包并保证完整性很费事，有人整理了一个文档将这些依赖包进行汇总，下载此文档后，在本地按照文件安装依赖包就好，我将依赖包的汇总文档整理到了github,用以下指令下载**
    ```
    # wget https://github.com/Warding/InternetRecords/blob/master/%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8/superset_requirements.txt
    # pip3 install -r superset_requirements.txt  -- 安装依赖包
    # superset db upgrade                        -- 初始化db环境
    # export FLASK_APP=superset                  --账户设定
    # flask fab create-admin                     --创建账户按提示输入账密就好
    # superset load_examples                     --下载样例数据，可不执行
    # superset init                              --superset环境初始化
    # superset run -p 8088 -h 192.168.2.1 --with-threads  -- superset 启动（这里面我指定了端口和IP，分别是-p 和 -h 参数，建议指定成自己的服务器IP和某个端口，网页访问的时候，**还需要将这个端口对外开放，不然你本地登录网页访问会无法成功**）
    使用：输入上面的IP和端口对应的网址就行 http://192.168.2.1:8088/，   登录后输入刚刚建立的账密就好
    ```
    
### tips
***
* 出现superset 使用问题，比如缺失什么Python的第三方依赖库，请一定要在容器空间内安装所需的依赖包，例如需要连接mysql，你需要进入venv容器下进行安装pip3 install pymysql，然后需要重启superset
* 退出容器命令 deactivate
* 为避免出现关闭shell窗口导致superset无故退出，建议在容器下nohup 启动superset ,`# nohup superset run -p 8088 -h 192.168.2.1 --with-threads &`
   
### 小结
***
1：superset是第一次接触的在容器环境下使用的工具，说实话操作有点别扭，不论启动还是配置什么的，都需要切入对应容器空间进行操作，还需要注意配置的是不是容器的环境，有时候会将linux环境的操作误以为在容器环境也会生效，这点需要格外注意检查

2：资源包的整理需要及时，类似于上文中的python 的依赖包，第一次安装的当天依赖包还是有效的，等到第二天再去测试的时候，发现原链接下的这个依赖包已经变化，无法正常安装，赶紧从第一次成功的环境将依赖包文档本地保存一份并收藏，不然后面又要大折腾。所以日常应该注意，网络上收藏的网址资源并不是完全靠谱的，觉得很重要的，最好自己归档整理一次，不然后续麻烦的还是自己。
