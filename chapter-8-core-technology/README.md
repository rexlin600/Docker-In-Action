# 底层技术

Docker 使用 [Go 编程语言](https://golang.org/) 编写，并利用了 Linux 内核的多个功能来交付其功能。

Docker 的底层的核心技术如下四类：

* **命名空间（Namespaces）**： Docker 使用一种称为的技术`namespaces`来提供称为_容器_的隔离工作区；
* **控制组（Control Group）**： Linux上的Docker引擎还依赖于另一种称为_控制组_ （`cgroups`）的技术；
* **联合文件系统（Union FS）**：联合文件系统或UnionFS是通过创建图层进行操作的文件系统，使其非常轻便且快速；
* **容器格式**：Docker Engine 将命名空间、控制组、Union FS 组合到一个成为容器格式的包装器内， 默认容器格式为`libcontainer`。将来，Docker可以通过与 `BSD Jails` 或 `Solaris Zones` 等技术集成来支持其他容器格式。

下面，我们来分别来看下 Docker 的底层核心技术。

## 参考

* [Docker 核心技术与实现原理](https://www.cnblogs.com/davidshen/p/10490605.html)



