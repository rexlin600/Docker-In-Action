# 仓库

仓库（**Repository**）是一个很多开始学习 Docker 的同学都会把它和 **Registry** 搞混的概念。简单来说：**仓库是指某一类镜像的聚合仓库，Registry 是注册服务器，是一个负责集中存储、分发镜像的服务！**

可通过下图（图片来自互联网）更加直观的体会 **Registry**、**Repository**、**Image**、**Container** 的关系：

![](../.gitbook/assets/image%20%284%29.png)

## 命名规范

镜像一般是两段式命名，即：`<仓库名>:<标签>`，而 `<仓库名>` 也常以两段形式出现，即：`<用户名>/<软件名>`，对于私有注册服务器而言，可能 `<仓库名>` 是三段命名，下面给出一些示例：

```text
# 官方仓库：两段命名(简化)
# 下述实际上等同于 library/ubuntu:latest，省略注册服务器
ubuntu:latest

# 官方仓库：两段命名
k8s.gcr.io/kube-proxy:v1.19.3

# 私有仓库：三段命名
registry.cn-chengdu.aliyuncs.com/ms4cloud/docker-docs:latest
```

## Docker Registry 公开服务

Docker Registry 公开服务是开放给用户使用、允许用户管理镜像的 Registry 服务。一般这类公开服务允许用户免费上传、下载公开的镜像，并可能提供收费服务供用户管理私有镜像。

最常使用的 Registry 公开服务是官方的 [Docker Hub](https://hub.docker.com/)，这也是默认的 Registry，并拥有大量的高质量的官方镜像。除此以外，还有 Red Hat 的 [Quay.io](https://quay.io/repository/)；Google 的 [Google Container Registry](https://cloud.google.com/container-registry/)，[Kubernetes](https://kubernetes.io/) 的镜像使用的就是这个服务。

由于某些原因，在国内访问这些服务可能会比较慢。国内的一些云服务商提供了针对 Docker Hub 的镜像服务（`Registry Mirror`），这些镜像服务被称为 **加速器**。常见的有 [阿里云加速器](https://cr.console.aliyun.com/#/accelerator)、[DaoCloud 加速器](https://www.daocloud.io/mirror#accelerator-doc) 等。使用加速器会直接从国内的地址下载 Docker Hub 的镜像，参考镜像加速器一节：

{% page-ref page="../installation-tutorial/mirror-accelerator.md" %}

国内也有一些云服务商提供类似于 Docker Hub 的公开服务。比如 [网易云镜像服务](https://c.163.com/hub#/m/library/)、[DaoCloud 镜像市场](https://hub.daocloud.io/)、[阿里云镜像库](https://cr.console.aliyun.com) 等。

## 私有 Docker Registry

除了使用公开服务外，用户还可以在本地搭建私有 Docker Registry。Docker 官方提供了 [Docker Registry](https://hub.docker.com/_/registry/) 镜像，可以直接使用做为私有 Registry 服务。

开源的 Docker Registry 镜像只提供了 [Docker Registry API](https://docs.docker.com/registry/spec/api/) 的服务端实现，足以支持 `docker` 命令，不影响使用。但不包含图形界面，以及镜像维护、用户管理、访问控制等高级功能。在官方的商业化版本 [Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/2.0/) 中，提供了这些高级功能。

除了官方的 Docker Registry 外，还有第三方软件实现了 Docker Registry API，甚至提供了用户界面以及一些高级功能。比如，[Harbor](https://github.com/goharbor/harbor) 和 **Sonatype Nexus** 等。



