# Rails Action Cable

## 关键定义

**Pub/Sub(Publish-Subscribe)**: Publishers 消息发送者 将消息发送到接收者 recipients(subscribers)。使用该方式在服务器及客户端沟通

## 服务端相关组件

* Connections 连接 每个接收服务器的WebSocket，都会实例化一个连接对象
