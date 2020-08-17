# Computer Network



### Http/2 多路复用

在一个Http连接上，多路“HTTP消息“同时工作

![截屏2020-08-16 下午7.46.39](/Users/gefeiyang/Desktop/截屏2020-08-16 下午7.46.39.png)

二进制帧结构：

length: 定义真实帧的长度

type: 帧的类型

flags：帧的标识

stream ID：每个流的唯一ID

Frame Payload: 真实帧的长度

