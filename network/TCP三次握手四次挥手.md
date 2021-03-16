![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/370b0bfa7c8b47c6ad8d2a39f2d40f42~tplv-k3u1fbpfcp-zoom-1.image)



![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e465eb2997084086947fdaa053edb210~tplv-k3u1fbpfcp-zoom-1.image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e465eb2997084086947fdaa053edb210~tplv-k3u1fbpfcp-zoom-1.image)

## 为什么TCP4次挥手时需要经过2MSL( Maximum Segement Lifetime 最大报文生存周期)?

概念：MSL是报文在网络中最长生存时间，这是一个工程值(经验值)，不同的系统中可能不同。

场景：

1. A发出ACK后，等待一段时间T，确保如果B重传FIN自己一定能收到

分析：

1. ACK从A到B最多经过1MSL，超过这个时间B会重发FIN
2. B重发的FIN最多经过1MSL到达A

结论：如果B重发了FIN，且网络没有故障(重发的FIN被丢弃或错误转发)，那么A一定能在2MSL之内收到该FIN，因此A只需要等待2MSL。

## 为什么挥手是四次而不是三次?

当A发送FIN通知B我想关闭连接，B会发送一个确认位ACK给A表示我已经收到了你的要求，此时B有可能还有数据要传给A，当B把自己的数据传输完毕后，会发送FIN终止位给A通知A我准备好关闭了，A收到FIN后，A会发送ACK给B通知B关闭连接。

如果是三次的话，那么服务端的 ACK 和 FIN 合成一个挥手，那么长时间的延迟可能让 TCP 一位 FIN 没有达到服务器端，然后让客户的不断的重发 FIN

## 为什么握手是三次而不是两次?

如果首先客户端发送了 SYN 报文，但是滞留在网络中，TCP 以为丢包了，然后重传，两次握手建立了连接。

等到客户端关闭连接了。但是之后这个包如果到达了服务端，那么服务端接收到了，然后发送相应的数据表，就建立了链接，但是此时客户端已经关闭连接了，所以带来了链接资源的浪费。




