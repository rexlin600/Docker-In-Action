# 网络

我们知道，要实现网络通信，机器至少需要一个网络接口（物理接口或虚拟接口）来收发数据包；此外，如果不同子网之间需要进行通信，还需要路由机制。

{% hint style="success" %}
Docker 正是利用命名空间这项技术得以将一个 Linux 系统抽象出多个网络子系统，各子系统间都有自己的网络设备，协议栈等，彼此之间互不影响。各个命名空间之间需要通信，便需要利用到 veth-pair 作桥梁。
{% endhint %}

## veth-pair 技术

Docker 中的网络接口都是虚拟接口，虚拟接口的优势在于转发效率高。Linux 通过在内核中进行数据复制来实现虚拟接口之间的数据转发，发送接口的发送缓存中的数据包被直接复制到接收接口的接收缓存中。对于本地系统和容器内系统看来就像是一个正常的以太网卡，只是它不需要真正同外部网络设备通信，速度要快很多。

前面讲了，Docker 容器网络就利用了命名空间、veth-pair 实现了不同命名空间之间的相互通信。

具体来说：在本地主机和容器内分别创建一个虚拟接口，并让它们彼此连通（这样的一对接口叫做 **veth pair**）；Docker 在容器以及宿主机内分别创建一个虚拟接口并让他们彼此联通，如下图（图片来自互联网）：

![](../.gitbook/assets/image%20%2813%29.png)

我们可以发现一个很典型的问题就是：多个命名空间进行通信，会非常复杂（每个命名空间之间均需要建立 veth-pair 进行通信），这样毫无疑问是非常繁杂的，那么如何解决这个问题那么？答案就是**网桥**！

但是在介绍网桥之前我们需要先了解 Docker 的四种网络模式：

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

## 网桥

如果 Docker 的容器通过 Linux 的命名空间完成了与宿主机进程的网络隔离，但是却有没有办法通过宿主机的网络与整个互联网相连，就会产生很多限制，所以 Docker 虽然可以通过命名空间创建一个隔离的网络环境，但是 Docker 中的服务仍然需要与外界相连才能发挥作用。

{% hint style="info" %}
Docker 的默认网络模式为网桥模式
{% endhint %}

在 Docker 的默认网桥模式下，除了分配隔离的网络命名空间之外，Docker 还会为所有的容器设置 IP 地址。当 Docker 服务器在主机上启动之后会创建新的虚拟网桥 **docker0**，随后在该主机上启动的全部服务在默认情况下都与该网桥相连，如下图（图片来自互联网）：

![](../.gitbook/assets/image%20%2817%29.png)

在默认情况下，每一个容器在创建时都会创建一对虚拟网卡，两个虚拟网卡组成了数据的通道，其中一个会放在创建的容器中，会加入到名为 docker0 网桥中。

我们可以使用如下命令查看当前的网桥接口：

```text
$ brctl show
```

docker0 会为每一个容器分配一个新的 IP 地址并将 docker0 的 IP 地址设置为默认的网关。网桥 docker0 通过 iptables 中的配置与宿主机器上的网卡相连，所有符合条件的请求都会通过 iptables 转发到 docker0 并由网桥分发给对应的机器。

```text
iptables -t nat -L
```

我们在当前的机器上使用 `docker run -d -p 6379:6379 redis` 命令启动了一个新的 Redis 容器，在这之后我们再查看当前 iptables 的 NAT 配置就会看到在 DOCKER 的链中出现了一条新的规则：

```text
DNAT       tcp  --  anywhere             anywhere             tcp dpt:6379 to:192.168.0.4:6379
```

  
上述规则会将从任意源发送到当前机器 6379 端口的 TCP 包转发到 192.168.0.4:6379 所在的地址上。  
  
这个地址其实也是 Docker 为 Redis 服务分配的 IP 地址，如果我们在当前机器上直接 ping 这个 IP 地址就会发现它是可以访问到的。

{% hint style="info" %}
Docker 是如何将容器的内部的端口暴露出来并对数据包进行转发？
{% endhint %}

当有 Docker 的容器需要将服务暴露给宿主机器，就会为容器分配一个 IP 地址，同时向 iptables 中追加一条新的规则。

当我们使用 redis-cli 在宿主机器的命令行中访问 127.0.0.1:6379 的地址时，经过 iptables 的 NAT PREROUTING 将 ip 地址定向到了 192.168.0.4，重定向过的数据包就可以通过 iptables 中的 FILTER 配置，最终在 NAT POSTROUTING 阶段将 ip 地址伪装成 127.0.0.1，到这里虽然从外面看起来我们请求的是 127.0.0.1:6379，但是实际上请求的已经是 Docker 容器暴露出的端口了。

```text
$ redis-cli -h 127.0.0.1 -p 6379 ping
PONG
```

  
Docker 通过 Linux 的命名空间实现了网络的隔离，又通过 iptables 进行数据包转发，让 Docker 容器能够优雅地为宿主机器或者其他容器提供服务。

