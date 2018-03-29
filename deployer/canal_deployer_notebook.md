### 一、deployer 模块主要作用

deployer 模块主要作用包括：
1. 读取 canal.properties 配置文件；
2. 启动 canal server，监听 canal client 请求；
3. 启动 canal instance，链接 mysql 库，伪装成 slave，发送 dump 请求，获取 binlog 日志并解析、存储。
4. 在 canal 运行过程中，监听配置文件的变化。


### 启动后线程模型
![](.\image\99e50bcc.png)


### 二、CanalLauncher

是程序的启动入口类。主要完成：
1. 读取配置文件
2. 根据配置文件构造 CanalController 对象并启动（start）
3. 设置 jvm shutdown 的 hook，在虚拟机退出时能优雅结束程序。


### 三、CanalController

Canal 源码分析的核心类。

1. 在构造方法中完成启动前的准备：
    * 配置文件解析
    * 初始化成员变量
    
    
    

