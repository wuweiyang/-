###计算机网络：网络层
####4.2.2分类的ip地址
*A类，B类C类地址的网络号字段分别为1,2,3字节长，而在网络号最前面1~3位的类别位，其数值分别规定为，0,10,110*

*A类B类C类地址的主机号字段分别为3个，2个，1个，字节长。*
<html>
<body>

<table border="1">
  <tr>
    <th>网络类别</th>
    <th>第一个可指派的网络号</th>
    <th>最后一个可指派的网络号</th>
    <th>每个网络中最大主机数</th>

  </tr>
  <tr>
    <td>126(2^7-2)</td>
    <td>1</td>
    <td>126</td>
    <td>16777214</td>
  </tr>
  <tr>
      <td>16383(2^14-1)</td>
      <td>128.1</td>
      <td>191.255</td>
      <td>65534</td>
  </tr>
  <tr>
      <td>2097151(2^21-1)</td>
      <td>192.0.1</td>
      <td>223.255.255</td> 
      <td>254</td>
  </tr>
</table>
</body>
</html>
#####ip地址的分类
* A,B,C类的资质的网络号字段分别为1,2,3,字节长，而在网络号字段的最前面有1~3位的的类别位，分别为0，10,110
* A，B,C类地址的主机号字段长为3个，2个，1个字。
* D类地址用于多播（前四位为1110）

#####IP地址与硬件地址
* 在发送数据时，数据从高层下到底层
* 使用IP地址的ip数据报到了数据链路层就会被封装成MAC帧
* MAC帧在传送时使用的源地址和目的地址都是硬件地址
* 源地址和目的地址都被写在了MAC帧的首部
* IP地址放在IP数据报的首部，硬件地址放在MAC帧的首部
* 网络层和网络层以上使用的是IP地址
* 数据链路层及以下使用的是硬件地址

####地址解析协议A RP
* ARP协议是在主机的ARP高速缓存中存放一个从ip地址到硬件地址的映射表，这些都是该主机目前知道的一些地址
####主机获取硬件地址的方法  
* 当主机A要向本局域网上的某个主机B发送IP数据报时，就先在其高速缓存中查看有无主机B的IP地址,如有就在ARP高速缓存中查出其对应的硬件地址再把这个硬件地址写入到mac帧，发往此硬件地址*
如果查不到主机的硬件地址，就根据以下步骤来获取B的硬件地址*
1. ARP进程再本局域网上广播发送一个ARP请求分组，请求分组的内容是：自己的ip和硬件地址，想要知道ip地址为x.x.x.x的主机的硬件地址
2. 在本局域网上的所有主机上运行的APR进程都会收到此APR请求分组。
3. 主机B在APR请求分中见到自己的IP地址，就向主机A发送ARP响应分组，并写入自己得到硬件地址，其余所有主机都不理睬这个ARP分组请求。ARP响应分组的内容:自己的ip和硬件地址。
4. 主机A收到B的ARP响应分组后，就在其ARP高速缓存中写入主机B的IP到硬件地址的映射。
<p>当主机A向B发送其ARP请求分组时，就把自己的IP到硬件地址的映射写入到ARP请求分组中，当B收到A的ARP请求分组时，就将主机A的这一地址映射写入到自己的ARP高速缓存中</p>
<font color="red">ARP是解决同一局域网上的主机或路由器的IP到硬件地址的映射问题</font>
#####使用ARP四种典型情况 
1. 发送方是主机，要把IP书记包发送到本网络上的一个主机。这时用ARP找到主机的硬件地址，
2. 发送方是主机，要把IP数据报发送到另一个网络上的一个主机。这时用ARP找到本网络上的一个路由器的硬件地址。剩下的工作由路由器完成
3. 发送方是路由器，要把IP数据发送到本网络上的一个主机。这时用ARP找到目的主机的硬件硬地址。
4. 发送方是路由器，要把ip数据包发送到另一网络上的一个主机，这时用ARP找到本网络上的一个路由器硬件地址，剩下的工作由这个路由器完成

#####ip数据报的格式
* 数据报是由首部和数据部分组成
* 首部固定长度<font color="red">20</font>字节
#######首部的组成
1. <B>版本</B> 占4位，指ip协议的版本。通信双方使用的版本必须一致‘
2.  <B>首部长度</B> 4位，可表示的最大的十进制数是15。这个字段表示的数的单位是字
（一个32位字长是4字节），当ip分组的首部长度不是4字节的整数倍时，必须利用最后的填充字段加以填充。因此数据部分永远在4的整数倍开始
3. <B>区分服务</B>
4. <B>总长度</B> 总长度指首部和数据之和的长度，单位为字节。总长度字段为16位
在ip层下面的每一种数据链路层都有自己的帧格式，其中包括<B>帧格式中的数据字段的最大长度</B> 称为<B>最大传送单元MTU</B>ip数据报封装成链路层的MAC帧时，数据的总长度不能超过MTU
* 当数据报总长度超过MTU时，必须把过长的数据报进行分片后才能在网络上传送。这是数据报首部中的总长度不是指为分片前的总的数据长度，而是指分片后每一个分片的首部和长度和数据长度的总和
5. <B>标识</B> 占16位。IP软件在储存器中维持一个计数器，每产生一个数据包计数器就+1，并将此值赋值给该字段。当数据报由于长度超过MTU而必须进行分片时，这个标识字段的值就被复制到所有数据报片的标识字段中，相同的标识字段的值使得分片后的各数据报片最后能正确的重装成原来的数据报。
6. <B>标识</B>占3位，目前只有两位有意义m
* 标志字段中的最低位为 <B>MF</B>。 MF=1表示后面还有 <B>”还有分片“</B>的数据报，MF=0时表示这已经若干数据报中的最后一个.
* 标志字段中间一位为<B>DF</B> 意思为“不能分片”，只有当 DF=0时才允许分片
7. <B>片偏移量</B> 占13位。片偏移量之初：较长的分组在分片后，某片在原分组中的相对位置，也就是说相对于用户数据字段的起点，该片从何处开始。片偏移量以八个字节为偏移单位。也就是说每一个分片的长度一定是8字节的整数倍数

###网际控制报文协议
报文的种类：<p>ICMP差错报文<p> <p>ICMP询问报文</p>
#####5种差错报文
1. 终点不可达：当路由器或主机不能交付数据报时就向源点发送终点不可到报文
2. 源点抑制：当路由器或主机由于拥塞而丢弃数据报时，就向源点发送源点抑制报文，使源点知道应当把数据报的发送速率放慢
3. 时间超过：当路由器收到生存时间为零的数据报时，除丢弃该数据报外，还要向源点发送时间超过报文。当终点不能在规定的时间内收到一个数据报的全部数据报片时，就把自己收到的全部数据报片全部丢弃，并向源点发送时间超过报文。
4. 参数问题：当路由器或主机收到的数据报的首部中的有的字段的值不正确时，就丢弃该数据报，并向源点发送参数问题报文
5. 改变路由器：路由器把路由器改变报文发送给主机，让主机知道下次应该把数据报发送给另外的主机
 

#####不应该发送ICMP差错报文的情况
 1. 对ICMP差错报告报文不再发送ICMP差错报文
 2. 对第一个分片数据报片的所有后续数据报片都不发送ICMP差错报告报文
 3. 对既有多播地址的数据包都不发送ICMP差错报告报文
 4. 对具有特殊地址的（如127.0.0.0或0.0.0.0）的数据不发送ICMP差错报告报文

###英特网路由选择协议

####两种路由选择协议
1. 内部网关协议IGP 在一个自治系统内部使用的路由选择协议，这与在互联网中其他自治系统使用什么路由选择协议无关 使用最多的是RIP和OSPF协议
2. 外部网关协议EGP源主机和目的主机处在不同的自制系统中，当数据传到一个自治系统的边界时，就需要一种协议将路由选择信息传递到另一个自治系统中。这种协议就是外部网关协议。目前使用最多的是BGP协议

* 自治系统之间的路由选择也叫做域间路由选择
* 自治系统中路由选择也叫做域内路由选择

####内部网关协议RIP
1. 工作原理
RIP协议要求网络中的每个路由器要维护它自身到每一个目的网络的距离记录

<font color="blue">RIP协议中的“距离”，也称为跳数，因为每经过一个路由器跳数就+1，RIP中认为好的路由就是通过路由器的跳数最少。RIP允许一条路径上最多只能包含15个路由器吗，因为距离等于16时相当于不可达，所以RIP只适合于小型的网络</font>

RIP不能在两个网络之间同时使用多条路由。RIP选择一条具有最少路由器的路由，哪怕还存在一条高速但路由器较多的路由。
#####<font color="red">RIP协议的特点</font>
<font color="red">
(1)仅和相邻路由器交换信息
(2)路由器就换的信息是当前路由器所知道的全部信息，既自己的路由表。也就是自己到本自治系统中所有的网络最短距离，以及到每个网络应该经过的下一跳路由器。
(3)按固定时间间隔交换路由信息。路由表中最重要的信息就是：到某个网络的最短距离。
</font>