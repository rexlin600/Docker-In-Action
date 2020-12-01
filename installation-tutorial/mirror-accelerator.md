# 镜像加速器

因为从国内直接从 Docker Hub 拉取镜像时非常困难，因此需要配置镜像加速器加速我们镜像的拉取。

## 加速器地址

国内众多云服务商均提供了免费的加速器服务，如下：

* [阿里云加速器](https://www.aliyun.com/)（可登陆阿里云，在`容器镜像服务-镜像加速器` 中查看）
* [网易云加速器](https://hub-mirror.c.163.com/)
* [腾讯云加速器](https://mirror.ccs.tencentyun.com/)
* [DaoCloud加速器](http://f1361db2.m.daocloud.io)
* [百度云加速器](https://mirror.baidubce.com)

## Ubuntu 16.04+、Debian 8+、CentOS 7+

几乎主流 Linux 发行版均已经使用 systemd 进行服务管理，因此我们可以借助其进行 Docker 的管理，这里演示如何在 Ubuntu 16.04+、Debian 8+、CentOS 7+ 等 Linux 发行版配置加速器：

```text
# 查看是否配置过 Docker 镜像加速器
systemctl cat docker | grep '\-\-registry\-mirror'

# 在 /etc/docker/daemon.json 添加加速器地址（没有则手动创建这个文件）
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ]
}

# 重启 Docker 服务
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## macOS

macOS 配置镜像加速器比较简单，如果是 Docker Desktop for Mac 的用户直接配置即可，如下图：

![](../.gitbook/assets/image%20%287%29.png)

## 检查

 在命令行界面使用 docker info 命令查看如果有类似下述输出及证明加速器配置成功：

```text
Registry Mirrors:
 https://hub-mirror.c.163.com/
 ...
```

