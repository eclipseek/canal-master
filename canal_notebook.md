## 整体流程

### cannel 和 mysql master 交互
1. canal 启动时向 master 注册，发送 com_register_slave 命令。
2. canal 服务向 master 发送 com_binlog_dump 命令，在master 无新 event 时，在连接上挂着（MysqlConnection.dump --> DirectLogFetcher.fetch），
有 event 时能及时接收到。
3. 收到 event 后解析处理成 CanalEntry.Entry 对象：AbstractEventParser.parseAndProfilingIfNecessary 方法。
4. Entry 对象入缓存 EventTransactionBuffer.add 方法。

### 客户端和 canal server 交互

1. cannal server 启动设置 PipelineFactory：      
![](.\image\8fe733c9.png)
1. 客户端 tcp 链接后，服务端要求发送 handshake 报文（HandshakeInitializationHandler）
2. 客户端发送认证报文之后，服务端处理认证报文（ClientAuthenticationHandler）
认证之后发送 ack 报文（NettyUtils.ack )，并设置回调对象：在发送完之后，将 HandshakeInitializationHandler、ClientAuthenticationHandler
这两个 handler 从 pipelie 中移除，并添加 IdleStateHandler 和 IdleStateAwareChannelHandler 两个handler 用于处理空闲连接的释放。
3. 收到客户端请求在 SessionHandler.messageReceived 中处理
    * 订阅请求
    * 取消订阅
    * GET 请求
    * ACK 请求
    * 回滚请求
    
    
## 参考文档
* http://blog.csdn.net/lvzhuyiyi/article/details/51842697
* http://www.tianshouzhi.com/api/tutorials/canal/380
* http://blog.csdn.net/varyall/article/details/79208574
    

