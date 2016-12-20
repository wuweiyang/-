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

######首部的组成
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
8. <B>生存时间</B> 占8位，表明数据报在网络中的寿命。由发出这个数据报的源点设置这个字段。防止无法交付的数据报在英特网中无限制的兜圈子。TTL字段的功能改成了“跳数限制”，路由器在转发数据报之前就把数据报的TTL减1，若TTL值减小到0，就丢弃该数据报，不在转发。
9. <B>协议</B> 占8位 协议字段指出该数据报携带的数据使用的是何种协议，以便目的主机的ip层知道应该讲数据部分上交给那个处理过程
常用的一些协议及相应的协议字段值
<table>
  <tr>
    <th>协议名</th>
    <th>ICMP</th>
    <th>IGMP</th>
    <th>TCP</th>
    <th>EGP</th>
    <th>IGP</th>
    <th>UDP</th>
    <th>IPV6</th>
    <th>OSPF</th>
  </tr>
  <tr>
    <th>协议字段值</th>
    <td>1</td>
    <td>2</td>
    <td>6</td>
    <td>8</td>
    <td>9</td>
    <td>17</td>
    <td>41</td>
    <td>89</td>
  </tr></table>
 10. <B>首部检验和</B> 占16位。这个字段值检验数据报的首部，不包括数据部分。这是因为数据报每经过一个路由器。路由器要重新计算一下首部检验和。
 * ip数据报检验和的检验和的计算方法：在发送方，先把IP数据报划分成许多16位的序列，并把检验和字段归零。用反码算数吧16位字相加后，将得到的检验和的反码写入检验和字段。接收方收到数据报后，将首部的所有16位字再使用所有的反码算术相加一次，将得到的和取反码，即得到接受方的检验和计算结果。若首部未发生任何变化，则此结果必定为0,，就保留这个数据报。否则就认为出差错，将次数据报丢弃
 11. <B>源地址</B> 32位
 12. <B>目的地址</B> 32位
 
###ip层转发分组流程
 <b>分组转发的算法</b>
 1. 从数据报的首部提取目的主机的ip地址D，得出目的网络地址为N。
 2. 若N就是与此路由器直接连接的某个网络，则进行直接交付，不再经过其他路由器，直接把数据报交付目的主机（这里包括把目的主机地址转化为转化为具体的硬件地址，把数据报封装成MAC帧，再发送此帧）；否则就是间接交付，执行3
 3. 若路由表中有目的地址为D的特定主机路由，则把数据报传送给路由表中所指明的吓一跳路由器；否则执行4
 4. 若路由表中有到达网络N的路由，则把数据报传送给路由表中的所指明的吓一跳路由器；否则执行5
 5. 若路由表中有一个默认路由，则把数据报传送给路由表中所指明的默认路由器；否则执行6
 6. 报告转发分组出错


###划分子网和构造超网

####划分子网的基本思路
1. 一个拥有许多物理网络的单位，可将所属的物理网络划分成若干个子网，该单位对外仍 属于一个网络。
2. 划分子网的方法是从网络主机号借用若干位作为子网号，subnet-id，主机号也就减少相应的位数。
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
<font color="blue">路由器在开始工作的时候，只知道到相连网络的距离，接着每一个路由器也只和自己相邻的路由器交换信息并更新路由信息。经过若干次更新之后，所有的路由器都会知道到达本自制网络中的任何一个路由器的最短距离，和吓一跳路由器的地址。
路由表中最主要的信息就是：到某个网络的，以及应经过的吓一跳地址，路由表更新的原则是找出到每个墓地网路的最短距离，。这种算法称作距离向量算法。
</font>
<b>距离向量算法</b>
对每一个相邻路由器发过来的RIP报文，进行一下步骤:
1. 对地址为x的相邻路由器发来的RIP报文，先修好此报文中的所有项目：把“下一跳”字段中的地址都改为x，并把所有距离字段中的值都加1。每一个项目都有三个关键数据：到目的网络N，距离d，下一跳路由器是x
2. 对修改后的RIP报文中的每个项目进行一下步骤：
* 若原来路由表中没有目的网络N，则把该项目添加到路由表中。
* 否则（即在路由表中没有目的网络N，这时就查看下一跳路由表的地址）
      若下一跳路由器地址是X，则把收到的项目替换原来表中的项目 
      否则（即这个项目是：到目的网络N，单下一跳路由器不是X）
          若收到的项目中的距离d小于路由表中的项目， 则进行更新，
          否则什么也不做
3. 若三分钟还没有收到相邻路由器的更新路由表，则把相邻路由器记为不可达路由器，即把距离设置为16（距离16表示不可达）
4.返回  

<font color="blue">RIP存在问题：当网络出现故障时，要经过很长的时间才能将此信息传达到所有的路由器。
实例
网1-------R1-------网2-------R2-----网3
假设路由器R1到网1的链路出了故障，R1无法到达网1，于是路由器R1把到网1的距离设置为16，R1路由表中相应的项目变成了“1,16，直接”。但是很可能要经过30秒后R1才把更新信息发送给R2,。然而R2可能已经把自己的路由表发送给了R1,其中就有一项“1,2，R1”这一项
R1收到R2的更新报文后误认为可经R2到达网1，于是把收到的路由信息修改为“1,3，R2”，表明“我到网1的距离是3，下一跳经过的路由R2。”并把更新信息发给R2。
同理，R2又把自己的路由表修给为“1,4，R1”。
这样一直更新下去，知道R1和R2到网1的距离都增大到16时，R1，和R2才知道原来网1是不可达的。RIP这一特点叫做：好消息传播快，坏消息传播慢。 
</font>

####内部网关协议
OSPF协议的基本特点
<font>OSPF主要特征就是使用分布式的链路状态协议，而不是像RIP那样的距离向量协议</font>
<font color="blue">
######OSPF协议与RIP协议的三点不同

1. 向本自制系统中所有路由器发送信息。这里使用的方法是<b>洪泛法</b>这就是路由器向所有相邻的路由器发送信息，而每一个相邻的路由器又再将此信息发往其所有相邻的路由器（但不再发送给刚刚发送信息的那个路由器）这样，最终整个区域中的所有路由器都得到了这个信息的一个副本。
2. 发送的信息就是与本路由器相邻的所有路由器的链路状态，但这只是路由器知道的部分信息。所谓”链路状态“就是说明本路由器都和那些路由器相邻，以及链路的”度量“，OSPF将这个度量用来表示费用，距离，费用等等，这些都由网络管理人员来决定。
3. 只有当链路发生变化时，路由器之间路由器才向所有路由器用洪泛法发送此信息，而不像RIP那样，不管网络拓扑有无发生变化，路由器都要定期交换路由表的信息。
</font>

####外部网关协议
边界网关协议BGP只能是力求寻找一条能够到达目的网络且比较好的路由，而并非要寻找一条最佳路由。
<font color="blue">在配置BGP协议时，每一个自治系统的管理员要选择至少一个路由作为该自制系统的“<<b>BGP网络发言人</b>”。一般来说两个BGP发言人都是通过一个共享网络连接在一起，而BGP发言人往往就是BGP边界路由器，但也可以不是BGP边界路由器  
</font>
* 一个BGp发言人与其他AS的BGP发言人要交换路由信息，就要先建立TCP连接（端口号为179），然后在此连接上交换BGP报文，以建立BGP会话。
* 每一个BGP发言人除了要运行BGP协议外，还必须运行该自治系统的所用的内部网关协议，如RIP,OSPF.
* 边界网关BGP所交换的网络可达性的信息就是要到达某个网络的所要经过的一系列的自制系统。当BGP发言人互相交换了网络可达性的信息后，各BGP发言人就根据所采用的策略从收到的路由信息中找出到达各自制系统的较好的路由。
#####BGP的四种报文
1. OPEN(打开)报文，用来与相邻的另一个BGP的发言人建立关系，使通信 初始化。
2. UPDATA(更新)报文，用来通告某一路由信息，以及列出要撤销的多条路由。
3. KEEPALIVE(保活)报文，用来周期性的证实邻站的连通性
4. NOTIFICATION(通知)报文，用来发送检测到的差错
<font color="blue">
  若两个邻站属于两个不同的AS，而其中一个邻站打算和另一个邻站定期的交换路由信息，这就应该有一个商谈的过程。
  * 一开始向邻站进行商谈时就必须发送OPEN报文。如果邻站接受了这种关系，就用KEEPALIVE报文响应。这样两个BGP发言人的邻站关系就建立了。
  * 一旦邻站关系建立了，就要继续维持这种关系。双方中的每一方都需要确信对方是存在的，且一直在保持这邻站关系。为此两个BGP发言人彼此要周期性的交换KEEPALIVE报文（一般每隔30秒）。
  * UPDATE报文是BGP报文的核心内容。BGP发言人可以用UPDATE报文撤销它以前曾经通知过的路由，         也可以宣布增加新的路由。撤销路由可以一次撤销许多条，但增加新的路由时，每个更新报文只能增加一条。
</font>
  * OPEN报文共有6个字段，<b>版本 </b>，<b>本自治系统号</b>，（两字节，使用全球唯一的16位自治系统号）<b></b>（2字节，以秒计算的保持为邻站关系的时间）<b>BGP标识符</b>（4字节，通常是该路由器的ip地址）可选参数长度和可选参数。
  * UPDATE 报文共有5个字段
  1. 不可行路由长度（2字节，指明下一字段的长度）
  2. 撤销的路由：列出所有要撤销的路由
  3. 路径属性总长度：2字节指明下一字段的长度
  4. 网络层可达信息：定义发出此报文的网络，包括网络的前缀位数，ip地址前缀。
  * KEEPALIVE 报文只有BGP的19字节长的通用首部
  * NOTIFICATION 报文有三个字段
  1. 差错代码
  2. 差错子代码
  3. 差错数据（给出有关差错的诊断信息）
  
#第五章 运输层
###运输层的两个主要协议
1. 用户数据报协议 UDP
2. 传输控制协议TCP

* UDP在传送数据之前<b>不需要先建立连接</b>主机的运输层在收到UDP报文后，不需要给
任何确认。
* TCP<b>提供面向连接的服务</b>在传输之前必须先建立连接，数据传送结束后要释放链接，TCP不提供广播多播服务。

#####使用UDP和TCP协议的各种应用和应用层协议
<table>
<tr>
<th>应用</th>
<th>应用层协议</th>
<th>运输层协议</th>
</tr>
<tr>
<th>名字转换</th>
<th>DNS（域名系统）</th>
<th>UDP</th>
</tr>
<tr>
  <th>文件传送</th>
  <th>TFTP（简单文件传送协议）</th>
  <th>UDP</th>
</tr>
<tr>
  <th>路由选择协议</th>
  <th>RIP</th>
  <th>UDP</th>
</tr>
<tr>
  <th>IP地址配置</th>
  <th>DHCP(动态主机配置协议)</th>
  <th>UDP</th>
</tr>
<tr>
  <th>网络管理</th>
  <th>SNMP(简单网络管理协议)</th>
  <th>UDP</th>
</tr>
<tr>
<th>远程文件服务器</th>
<th>NFS（网络文件系统）</th>
<th>UDP</th>
</tr>
<tr>
  <th>IP电话</th>
  <th>专用协议</th>
  <th>UDP</th>
</tr>
<tr>
<th>流式多媒体通信</th>
<th>专用协议</th>
<th>UDP</th>
</tr>
<tr>
<th>多播</th>
<th>IGMP（网际组管理协议）</th>
<th>UDP</th>
</tr>
<tr>
<th>电子邮件</th>
<th>SMTP（简单邮件传送协议）</th>
<th>TCP</th>
</tr>
<tr>
  <th>远程终端接入</th>
  <th>TELNET</th>
  <th>TCP</th>
</tr>
<tr>
<th>万维网</th>
<th>HTTP(超文本传送协议)</th>
<th>TCP</th>
</tr>
<tr>
  <th>文件传送</th>
  <th>FTP（文件传送协议）</th>
  <th>TCP</th>
</tr>
</table>

####服务器使用的端口号
1. 熟知端口号：熟知为1~1023
<table>
  <tr>
    <th>应用程序</th>
    <th>FTP</th>
    <th>TELNET</th>
    <th>SMTP</th>
    <th>DNS</th>
    <th>TFTP</th>
    <th>HTTP</th>
    <th>SNMP</th>
    <th>SNMP(trap)</th>
  </tr>
  <tr>
    <th>熟知端口号</th>
    <th>21</th>
    <th>23</th>
    <th>25</th>
    <th>53</th>
    <th>69</th>
    <th>80</th>
    <th>161</th>
    <th>162</th>
  </tr>
</table>
2. 登记端口号 数值为1024~49151 这类端口号是为没有熟知端口号的应用程序使用
3. 客户端使用端口号 数值为49152~65535 这类端口号仅在客户进程运行时才动态选择，因此又叫短暂端口号，通信结束后刚才使用过的端口号就不存在了

##用户数据报协议UDP 
UDP概述
1. UDP是无连接的，即发送数据不需要建立连接，减少了开销和发送数据之前的时延
2. UDP使用<b>尽最大努力交付</b> 即不保证可靠交付
3. UDP是面向报文的 
4. UDP没有拥塞控制
5. UDP支持一对一，一对多，多对一，和多对多交互通信
6. UDP首部开销小 只有8个字节

###UDP首部格式
UDP首部字段，由4个字段组成，每个字段的长度都是两个字节
1. <b>源端口号</b> 在对方回信时使用，不需要是可用全0
2. <b>目的端口</b> 交付报文时使用
3. <b>长度</b> UDP用户数据报的长度，最小值是8（仅有首部）
4. <b>检验和</b> 检验UDP用户数据报在传输过程中是否有错，有错就丢弃

<font color="blue">当运输层从IP层收到UDP数据报时，就根据首部中的源端口，把UDP数据报通过相应的端口，上交到最后的终点-应用进程
如果接受方UDPfaint收到的报文中的目的端口号不正确，就丢弃该报文并由忘记控制报文协议ICMP发送端口不可达差错报文给发送方。
</font>
##传输控制协议TCP概述
###TCP最主要的特点
1. TCP是<b>面向连接的运输层协议</b>，应用程序在使用TCP协议之前要先建立连接，在通信结束之后要释放连接
2. 每一条TCP连接只能有两个端点，每一条TCP连接只能是点对点的。
3. TCP提供<b>可靠交付</b>的服务。通过TCP连接传送数据。
4. TCP提供<b>全双工通信</b>TCP允许通信双方的应用进程，在任何时间都能发送数据。TCP连接的两端都设有发送缓存和接收缓存，用来临时存放双向通信的数据。
5. <b>面向字节流</b> TCP把应用程序交下来的数据看成仅仅是一连串的无结构的字节流  TCP并不知道所传送的字节流的含义，TCP并不保证接收方应用程序所收到的数据块和发送方应用程序所发送的数据块具有对应的大小关系

###TCP的连接
* 套接字 socket = （IP地址：端口号）
* 每一条TCP连接唯一的被通信两端的两个端点（即两个套接字）所确定

##可靠传输的原理
###停止等待协议
每发送完一个分组就停止发送，等待对方的确认。在收到确认后再发送下一个分组。
1. <b>无差错情况</b>A发送分组M1，发送完成后就暂停发送，等待B的确认。A在收到了对M1的确认后，就在发送下一个分组。
2. <b>出现差错的情况</b> 只要A超过一定的时间没有收到B的确认，就认为刚才发送的分组丢失了，就重传前面的分组。这叫<b>超时重传</b>
 <font color="red">
   * A发送完一个分组后，必须展示保留自己发送的分组副本，（为超时重传使用）只有在收到了相应的确认之后才可以清除暂时保留的分组副本
   * 分组和确认分组都必须进行编号。这样才能明确哪一个是发送出去的分组收到了确认，而哪一个分组还没有收到确认。
   * 超时计时器设置的重传时间应当比数据在分组传送的平均往返时间更长一些。
 </font>
 3. 确认丢失和确认迟到
 B所发送的对M1的确认丢失。A在设定的超时重传时间没有收到确认，但无法知道是自己发送分组出错，丢失，或者B发送的确认丢失了。因此A在超时计时器到后期就要重传M1。假设B收到了重传分组M1，这是应采取两个行动
 第一，丢弃这个分组M1，不向上层交付。
 第二，向A发送确认。
 这种协议称为自动重传请求 ARQ协议。

##TCP报文段首部格式
1. 源端口号和目的端口号，TCP分用功能也是通过端口号实现的
2. <b>序号</b> 4字节。序号的范围是[0,2^32-1]，序号增加到2^32-1后，下一个序号就回到0。TCP是面向字节流的。在一个TCP连接中传送的字节流中的<b>每一个字节都按顺序编号</b>。整个要传送的字节流的起始序号必须在连接建立时设置。首部中的序号字段指的是<b>本报文段</b>所发送字节的第一个字节的序号。
3. <b>确认号</b> 四个字节，是<b>期望收到对方下一个报文段的第一个数据字节的序号</b>
4. <b>数据偏移量</b>
占4位。它指出TCP报文段数据起始处距离TCP报文段的起始处有多远。这个字段其实是指出TCP报文段的首部长度。由于首部中还有长度不确定的选项字段，因此数据偏移字段是必要的。但应注意数据偏移的单位是32位字。4位二进制能表示的最大的十进制数为15，因此数据偏移的最大字节是60字节，也是TCP首部的最大长度。
5. 保留 占6位，今后使用
6. <b>紧急URG</b> 当URG=1时，表示紧急字段指针有效。它告诉系统此报文中有紧急数据，应尽快传送(相当于高优先级的数据)。而不要按原来的顺序传送。
7. <b>确认ACK</b> 当ACK=1时确认号字段才有效。当ACK=0时，确认号无效。TCP规定，在连接建立后所有传送的报文段都必须把ACK置1。
8. <b>推送PSH</b>当两个进程进行通信交互式，有时在一端的应用进程希望在键入一个命令后立即就能够收到对方的响应。在这种情况下TCP可以使用推送操作。这时发送方TCP把PSH置为1，并立即发送一个报文发送出去。接收方TCP收到PSH的报文段，就尽快的交付接受应用进程，而不再等待整个缓存都填满了后再向上提交。
9. <b>复位</b>当RST=1时，表明TCP连接中出现严重差错(如由于主机崩溃或者其他原因)，必须释放，然后重新建立运输连接。RST置1还用来拒绝一个非法的报文段或拒绝打开一个连接。RST也可称为重建位或者重置位。
10. <b>同步SYN</b>在连接建立时用来同步序号。。当SYN=1时而ACK=0时表明这是一个连接请求报文段。对方若同意建立连接，则在响应的报文段中使SYN=1和ACK=1.因此SYN=1就表示这是一个连接请求或者连接接受报文。
11. <b>终止FIN</b> 用来释放一个连接。方FIN=1时，表明此报文段的发送方的数据已经发送完毕，并要求释放运输连接。
12. <b>窗口</b> 2字节。窗口值时[0,2^16-1]之间的整数。窗口指的是发送本报文段的一方的接受窗口(而不是自己的发送窗口)。窗口值告诉对方：从本报文段首部中的确认号算起，接收方允许对方发送的数据量。之所以要有这个限制，是因为接收方的数据缓存空间有限。<b>窗口值作为接收方让发送方设置其发送窗口的依据</b>
<font color="red">窗口字段明确指出了现在允许对方发送的数据量。窗口值是经常在动态变化着</font>
13. <b>检验和</b> 两字节，检验和字段检验的范围包括首部和数据这两部分。
14. <b>紧急指针</b>紧急指针仅在URG=1时才有意义，它指出本报文段中紧急数据的字节数（紧急数据结束后就是普通数据），<b>即使窗口值为0也可以发送紧急数据</b>
15. <b>选项</b> 长度可变最长可达40字节，当没有使用TCP选项是首部长度是20字节。
