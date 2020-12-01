# 注册服务器

注册服务器（**Registry**），是一个无状态、高度可扩展的服务器端应用程序，它被用来存储、分发 Docker 镜像，注册服务器是开源的，我们也可以自己搭建私有注册服务器。

比较典型的注册服务器是 [**Docker Hub**](https://hub.docker.com/)，它是一个公共的注册服务器，它提供免费使用的托管注册表及其他功能（组织账户、自动构建等）。此外还有诸如 **Quay.io**、**Google Container Registry** 等比较知名的注册服务器。

众所周知，国内因为网络原因所以我们使用 Docker Hub 可能不是那么顺畅。鉴于此国内各大云服务商也提供了注册服务器镜像（**Registry Mirror**），这样我们可以借助于这个镜像中心帮助我们快速的拉取与推送镜像，比较常用的注册服务器镜像中心有：

* [阿里云加速器](https://www.aliyun.com/)（可登陆阿里云，在`容器镜像服务-镜像加速器` 中查看）
* [网易云加速器](https://hub-mirror.c.163.com/)
* [腾讯云加速器](https://mirror.ccs.tencentyun.com/)
* [DaoCloud加速器](http://f1361db2.m.daocloud.io)
* [百度云加速器](https://mirror.baidubce.com)



