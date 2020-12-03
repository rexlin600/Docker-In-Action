# 网络

我们知道，要实现网络通信，机器至少需要一个网络接口（物理接口或虚拟接口）来收发数据包；此外，如果不同子网之间需要进行通信，还需要路由机制。

Docker 中的网络接口都是虚拟接口，虚拟接口的优势在于转发效率高。Linux 通过在内核中进行数据复制来实现虚拟接口之间的数据转发，发送接口的发送缓存中的数据包被直接复制到接收接口的接收缓存中。对于本地系统和容器内系统看来就像是一个正常的以太网卡，只是它不需要真正同外部网络设备通信，速度要快很多。

{% hint style="success" %}
Docker 容器网络主要利用了 Linux 上的网络命名空间和虚拟网络设备（尤其是 veth pair）
{% endhint %}

Docker 容器网络就利用了这项技术。它在本地主机和容器内分别创建一个虚拟接口，并让它们彼此连通（这样的一对接口叫做 **veth pair**），Docker 便利用这项技术，在容器以及宿主机内分别创建一个虚拟接口并让他们彼此联通，如下图（图片来自互联网）：

![](../.gitbook/assets/image%20%2813%29.png)

## 网络模式

<table>
  <thead>
    <tr>
      <th style="text-align:left">Docker&#x7F51;&#x7EDC;&#x6A21;&#x5F0F;</th>
      <th style="text-align:left">&#x914D;&#x7F6E;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">host</td>
      <td style="text-align:left">--net=host</td>
      <td style="text-align:left">&#x5BB9;&#x5668;&#x548C;&#x5BBF;&#x4E3B;&#x673A;&#x5171;&#x4EAB;Network
        namespace</td>
    </tr>
    <tr>
      <td style="text-align:left">container</td>
      <td style="text-align:left">--net=container:NAME_or_ID</td>
      <td style="text-align:left">
        <p>&#x5BB9;&#x5668;&#x548C;&#x53E6;&#x5916;&#x4E00;&#x4E2A;&#x5BB9;&#x5668;&#x5171;&#x4EAB;
          Network namespace&#x3002;</p>
        <p>kubernetes &#x4E2D;&#x7684; pod &#x5C31;&#x662F;&#x591A;&#x4E2A;&#x5BB9;&#x5668;&#x5171;&#x4EAB;&#x4E00;&#x4E2A;
          Network namespace</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">none</td>
      <td style="text-align:left">--net=none</td>
      <td style="text-align:left">&#x5BB9;&#x5668;&#x6709;&#x72EC;&#x7ACB;&#x7684; Network namespace&#xFF0C;&#x4F46;&#x5E76;&#x6CA1;&#x6709;&#x5BF9;&#x5176;&#x8FDB;&#x884C;&#x4EFB;&#x4F55;&#x7F51;&#x7EDC;&#x8BBE;&#x7F6E;&#xFF0C;&#x5982;&#x5206;&#x914D;
        veth pair &#x548C;&#x7F51;&#x6865;&#x8FDE;&#x63A5;&#xFF0C;&#x914D;&#x7F6E;
        IP &#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">bridge</td>
      <td style="text-align:left">--net=bridge</td>
      <td style="text-align:left">&#x9ED8;&#x8BA4;&#x6A21;&#x5F0F;</td>
    </tr>
  </tbody>
</table>



