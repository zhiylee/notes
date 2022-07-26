## 特点

- 分布式 水平拓展
- 推模式 低时延
- 基于内存 满了也持久化到硬盘

## 组成

- **nsqlookupd** 是管理拓扑信息的守护进程，客户端询问 nsqlookupd 来发现特定话题的 nsqd 生产者，nsqd 节点通过它广播话题和频道信息。
- **nsqd** 是接收、排队和将消息传达至客户端的守护进程，可以单独运行但是通常配合 nsqlookupd 形成集群。
- **nsqadmin** 是 NSQ 的 Web UI，用于实时查看集群的汇总数据并执行管理任务。





https://blog.crazytaxii.com/posts/an_introduction_2_nsq/