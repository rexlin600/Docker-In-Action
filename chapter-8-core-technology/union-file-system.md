# 联合文件系统

Docker 联合文件系统（Union File System），它是实现 Docker 镜像的技术基础。Docker 目前支持的联合文件系统包括 `OverlayFS`, `AUFS`, `Btrfs`, `VFS`, `ZFS` 和 `Device Mapper`。

联合文件系统是一种轻量级的高性能分层文件系统，支持将文件系统中的修改进行提交和层层叠加，这个特性使得镜像可以通过分层实现和继承。同时支持将不同目录挂载到同一个虚拟文件系统下。

## 基础镜像和父镜像

在 Docker 镜像中，镜像分为 **基础镜像**、**父镜像**，没有父镜像的镜像层被分为基础镜像。用户是基于基础镜像来制作各种不同的应用镜像。这些应用镜像共享同一个基础镜像层，提高了存储效率。

正是基于这个特性，Docker 中的镜像层能够憋复用，用户升级程序时并不需要替换整个镜像，只需要添加新层即可！

## AUFS

> 现在 Docker 推荐的存储驱动为 overlay2

在 Docker 中使用 AUFS（Another Union File System 或 Another Multilayered Unification File System）就是一种联合文件系统。简单来说就是：支持将不同目录挂载到同一虚拟文件系统下的文件系统。

AUFS 不仅可以设定对每一个目录只读、只写或读写的权限，还可以支持分层的机制！

传统的 Linux 启动到运行需要两个 FileSystem，BootFS 和 RootFS。不同的 Linux 发行版其 BootFS 基本一致，RootFS 有所差别。

BootFS 主要包含 BootLoader 以及Kernel，BootLoader 用来引导加载 Kernel，当引导成功后，Kernel 会被加载到内存中，BootFS就被 Unmout 了。

RootFS 可以被看成一个类 Unix 操作系统，其中包含 Linux 系统的诸如：/dev、/usr、/bin 等标准目录和文件。

基于此，Docker 从一个镜像启动容器时，会依次加载父镜像层直到基础镜像层，用户的进程运行在一个读写层的文件系统层中，所有的父镜像中的数据信息、ID、网络以及 LXC 管理的资源限制、容器的配置等等构成了在 Docker 概念上的容器。

## Linux 发行版与 Docker 存储驱动映射

| Linux 发行版 | Docker 推荐使用的存储驱动 |
| :--- | :--- |
| Docker CE on Ubuntu | `aufs`, `devicemapper`, `overlay2` \(Ubuntu 14.04.4 +, 16.04 +\), `overlay`, `zfs`, `vfs` |
| Docker CE on Debian | `aufs`, `devicemapper`, `overlay2` \(Debian Stretch\), `overlay`, `vfs` |
| Docker CE on CentOS | `devicemapper`, `vfs` |
| Docker CE on Fedora | `devicemapper`, `overlay2` \(Fedora 26 +\), `overlay` \(实验性支持\), `vfs` |

在可能的情况下，[Docker 官方推荐](https://docs.docker.com/storage/storagedriver/select-storage-driver/) 使用 `overlay2` 存储驱动，因为`overlay2` 是目前 Docker 默认的存储驱动，以前则是 `aufs`。你可以通过配置来使用以上提到的其他类型的存储驱动。

