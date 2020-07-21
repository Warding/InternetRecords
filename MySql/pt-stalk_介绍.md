## MySQL pt-stalk 工具介绍

### 一、前言

MySQL的性能抖动偶尔会有出现，很多时候苦于监控系统的不灵敏和相关服务log的缺失，没法定位问题根源。且多数性能抖动的业务情景很是复杂，都是类似蝴蝶效应一般的连锁反应，很难后续进行测试复现。

**于是，自定义条件触发系统监控，log收集现场环境，方便后续定位问题，成为一种急迫的需求。**

当然，自己动手提供解决方案是可以的，自己写shell呗，部署在linux系统上，按照每秒监控系统或是mysql的一个指标，当满足条件后，触发后将mysql慢查时间更改为0，或者是其余的自定义动作，触发mysql和linux系统的log收集，后续进行MySQL慢查分析和服务器log分析，这当然是可以的，只是这部署和编码工作量似乎显得太多了。而仔细一看，这些功能似乎和以往接触的一款mysql的log收集工具很相似。

于是，再次仔细查阅这款工具pt-stalk的官方文档，发现一些额外的细节，**自定义触发条件和自定义触发之后的行为成为可能**，很是惊喜。

***

### 二、pt-stalk 介绍

- **来头**

下载Percona-Toolkit工具包可用，是Percona-Toolkit工具包中的一个工具。 *PT工具包，平时常用的 pt-query-digest、 pt-online-schema-change等工具都是出自于这个工具包*

- **它有什么用**

pt-stalk 的主要功能是在出现问题时收集 OS 及 MySQL 的诊断信息，这其中包括：

1： OS 层面的 CPU、IO、内存、磁盘、网络等信息；

2： MySQL 层面的行锁等待、会话连接、主从复制，状态参数等信息。

简单来说，就是上文中提到的解决方案的一个已经封装好的轮子，**pt-stalk挂在后台可以不间断监控系统或者mysql的某个指标，当满足你设定的触发条件时，可以启动收集linux系统、mysql层面的log信息，存入本地，后续供复盘分析。**

- **如何使用**

MySQL实例服务下本地运行shell指令即可

命令格式如下：

`pt-stalk  --function status --variable Threads_connected --threshold 2500 --daemonize --user=root --password=######`

***

### 三、重点参数说明


1：--function --variable --threshold 这三个参数需要一起使用，用来控制触发条件

--function 设置触发条件的类别，包括status、processlist、自定义脚本

默认为 status，代表监控 SHOW GLOBAL STATUS 的输出；
    
也可以设置为 processlist，代表监控 show processlist 的输出；
    
**自定义脚本 --function=脚本名称，可以自定义监控指标，比如CPU 负载**
    
--variable 设置监控function返回的某一个监控指标，比如SHOW GLOBAL STATUS有很多返回指标，最终只监控Threads_connected的值就好

--threshold 设置对variable监控指标的值的上限，比如--function status --variable Threads_connected --threshold 2500 当监控连接数大于2500，可满足条件触发

2： --cycles --interval --sleep --iterations  这是一起的，一般用来控制频率相关

--interval 默认情况pt-stalk会每隔一秒检查一次状态数据，判断是否需要触发收集。该参数指定间隔时间，默认是1秒。

--cycles  默认情况pt-stalk只有连续观察到五次状态值满足触发条件时，才触发收集。该参数控制，需要连续几次满足条件，收集被触发，默认是5次。

--sleep   为防止一直触发收集数据，该参数指定在某次触发后，必须sleep一段时候才继续观察并触发收集。默认是300秒

--iterations 该参数指定pt-stalk在收集几次故障现场后就退出。默认pt-stalk会一直运行。

3： **--plugin  可自定义脚本，控制监控前后，log收集前后的行为**

***

### 四、Tips

1：灵活**使用--function --plugin这两个参数进行自定义逻辑控制**，可以实现定制化功能

2：默认产生的log在/var/lib/pt-stalk路径下

3：分析pt-stalk产生的log，可以使用pt-sift工具，这也是Percona-Toolkit工具包的一个

4：tail -f /var/log/pt-stalk.log  可以查看pt-stalk的运行log

5：pt-stalk核心运行流程说明

![pt-stalk核心运行流程说明](http://ww1.sinaimg.cn/large/005Uq6NBly1ggyober0vbj30pt0j00tn.jpg)

6：**PS**

- **--function 自定义脚本功能说明**

 shell脚本中**只需声明固定函数名trg_plugin，且函数必须返回数值**，方便--threshold 进行大小比较，自定义脚本时--variable 可随意

如下参数可以设置监控linux CPU负载大于3时，触发收集

`--function=/root/shell_cpuload.sh --variable=highcpu_load --threshold=3`

自定义脚本内容如下：

cat /root/shell_cpuload.sh

```

#!/bin/bash
# 获取CPU当前负载值
function trg_plugin(){
cpuload_all=(`cat /proc/loadavg`)
cpu_now=${cpuload_all[0]}
echo $cpu_now
}

```

- **--plugin 自定义脚本功能说明**




***

### 五、总结

提炼出问题的需求重点是第一步，之后善于搜索和归类，可能发现以往的轮子也能有意外的惊喜，或许会完美解决现有的痛点。


***

- 感谢如下参考文章链接


https://www.percona.com/doc/percona-toolkit/LATEST/pt-stalk.html

https://zhuanlan.zhihu.com/p/142796042

https://www.cnblogs.com/hfclytze/p/pt-stalk.html

http://blog.itpub.net/26250550/viewspace-2059636/
