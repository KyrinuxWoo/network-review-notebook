# 第三章 数据链路层

## 一般概念

1. 数据链路层使用的信道主要有以下两种类型：
    * 点对点信道：这种信道使用一对一的点对点通信方式。
    * 广播信道：这种信道使用一对多的广播通信方式。
1. 辨析：“链路”和“数据链路”
    * 链路：就是从一个结点到相邻结点的一段 *物理线路* ， 而中间没有任何其他的交换结点。
    * 数据链路：把实现 控制数据的传输的 协议的 硬件和软件加到链路上， 就构成了数据链路。
1. 对于通信质量良好的有线传输链路，并不要求数据链路层向网络层提供“可靠传输”的服务。

    但是对于质量差的无线传输链路，数据链路层协议使用确认和重传机制，向上提供“可靠传输”服务

## 重要概念

1. 三个基本问题
    1. 封装成帧： *帧的数据部分* （网络层的IP数据报）的前面和后面分别添加上首部和尾部，构成了一个完整的帧。
    1. 透明传输：无论什么样的比特组合数据，都能没有差错的通过数据链路层。如果数据中的某个字节的二进制代码恰好和SOH或EOT这种控制字符一样, 数据链路层就会错误地“找到帧的边界” ， 把部分帧收下，而把剩下的那部分数据丢弃。
    1. 差错检测：比特在传输过程中可能会产生差错，1可能会变成0, 而0也可能变成1。这就叫做比特差错。为了保证数据传输的可靠性， 在计算机网络传输数据时， 必须采用各种差错检测措施。**循环冗余检验CRC**。
1. 什么是“可靠传输”：数据链路层的发送端发送什么，在接收端就收到什么。
1. PPP协议
    * 特点：PPP协议就是数据链路层协议。
    * 适用于点对点的连路通信
    * 组成：
        1. 一个将IP 数据报封装到串行链路的方法
        1. 一个用来建立、配置和测试数据链路连接的链路控制协议LCP
        1. 一套网络控制协议NCP
    * 使用`0x7E`作为帧头和帧尾的定界符。
    * 字符填充：使用转义符`0x7D`来对数据部分可能出现的`0x7E`进行转义。
    * 零填充：由于`0x7E`定界符的二进制编码是`0111-1110`，所以如果找到数据部分中有连续五个1，就在其后插入一个0,这样出现`0x7E`时，就成了`0111-11[0]10`。接收端只要扫描到五个1，就把它之后的一个0删除，保证了透明传输。
1. 局域网的拓扑结构:
    * 星形网
    * 环形网
    * 总线网
1. 共享信道的方法：
    * 静态划分信道：如频分复用、时分复用等。不适合局域网。
    * 动态媒体接入控制：
        * 随机接入：所有用户可随机发送消息，但可能发生碰撞
        * 受控接入：不能随机发消息，必须接受一定控制。
1. CSMA/CD协议：载波监听多点接入/碰撞检测(Carrier Sense Multiple Access with Collision Detection)

## 简答题

1. 简述点对点信道的数据链路层在进行通信时的主要步骤：
    1. 结点A 的数据链路层把网络层交下来的IP数据报添加首部和尾部**封装成帧。**
    1. 结点A 把封装好的帧发送给结点B 的数据链路层。
    1. 若结点B 的数据链路层收到的帧无差错， 则从收到的帧中提取出IP 数据报上交给上面的网络层： 否则丢弃这个帧。

## 计算题

1. 循环冗余检验CRC。P<sub>75</sub>。注意做除法时，中间步骤是进行按位异或，而不是相减。