## MHA 高可用学习使用总结
### 前言 & 业务环境说明
***
公司业务的主从复制链路稍显复杂，因为是从sql server 迁移至mysql，有很多存储过程和跨库连接的使用，导致MySQL主从复制链路中存在较多的表复制和级联复制，所以MySQL高可用方案迟迟没有落实。

最近招入的DBA专家，在分析公司现有的业务数据框架后，提出了使用MHA的高可用策略，但是因为场景的复杂，框架需要对应调整。但MHA的使用需要熟悉加强。

于是。。。


**业务环境说明如下：**
- MySql 8.0.16-7
- Centos 7.6
- 阿里云自建ECS
### 正文
***
#### 背景--为什么需要MHA
  互联网业务的通用架构一般为一主一从或者一主一从一备，但是仅仅靠MySQL自带的binlog主从复制，当出现主库故障，仍需人员介入，手动配置检查提升备库甚至从库为主库。而人为介入操作哪怕再及时，也会出现不可控和疏忽的风险。
  
  此外，人为介入后也是执行规律性脚本和一些固定检查工作，**一切可以重复的人工工作，都尽量使用自动化程序替代才是最科学的做法**，所以引入自动化主备切换工具成为一种合理的需求，MHA应运而生。
  
  
#### MHA的实现原理
***
![](https://huaban.com/pins/3263072879/)
#### 概念解析
*** 
1. HA（High Availability)
```
* 高可用HA（High Availability）是分布式系统架构设计中必须考虑的因素之一，它通常是指，通过设计减少系统不能提供服务的时间。
* 假设系统一直能够提供服务，我们说系统的可用性是100%。如果系统每运行100个时间单位，会有1个 时间单位无法提供服务，我们说系统的可用性是99%。很多公司的高可用目标是4个9，也就是 99.99%，这就意味着，系统的年停机时间为8.76个小时。
```

2. MHA 
***
#### MHA 部署步骤和要点
***
#### 步骤
    * 配置MySQL主从复制关系 *具体操作不详细记录*
    
      配置后的主从关系如下
      
    * linux下安装MHA程序
    
    * MHA 进行配置
   
#### 要点
    * 
### 总结
***
  1. **MHA 自动化切换MySql主备切换的过程，程序化过程，保证故障切换的速度（一般自动切换过程可在30s内完成，不超过1分钟），从而提高业务系统的高可用性**
  2. **MHA 的manage node服务监控框架，方便进行MySQL的水平扩展高可用性保证，面向后续集群化管理显著提高运维管理效率**
  
  
### 升华小角落 & 未来遐想
***


  
#### 感谢以下参考资料
* [Mysql-MHA-高可用方案介绍以及配置-简单VIP方案](https://zhuanlan.zhihu.com/p/111668223)
* [MHA实现MySQL主从自动在线切换功能](https://blog.csdn.net/OH_ON/article/details/78820183?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase) 
* [使用MHA实现MySQL主从复制高可用](https://www.cnblogs.com/xuanzhi201111/p/4231412.html)
* [MHA实现mysql8主从故障切换](https://blog.csdn.net/qq_37369726/article/details/104462513)
