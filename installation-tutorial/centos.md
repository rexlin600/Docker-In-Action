# CentOS

Docker 支持 64 位版本的 CentOS7/8，并且内核版本要求不低于 3.10。CentOS 7 满足最低内核的要求，但由于内核版本比较低，部分功能（如 `overlay2` 存储层驱动）无法使用，并且部分功能可能不太稳定。

## 卸载旧版本

旧版本的 Docker 被称为 `docker` 或 `docker-engine`，如下：

```text
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

## 替换 CentOS 源

```text
sudo yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

sudo sed -i 's/download.docker.com/mirrors.aliyun.com\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo

# 官方源
# sudo yum-config-manager \
#     --add-repo \
#     https://download.docker.com/linux/centos/docker-ce.repo
```

## CentOS 8 额外设置

由于 CentOS8 防火墙使用了 `nftables`，但 Docker 尚未支持 `nftables`，因此我们需要修改一个配置文件：`/etc/firewalld/firewalld.conf`，我们可以使用如下设置使用 `iptables`：

```text
# FirewallBackend=nftables
FirewallBackend=iptables
```

或者执行下面的命令：

```text
firewall-cmd --permanent --zone=trusted --add-interface=docker0
firewall-cmd --reload
```

## 安装 Docker

```text
# 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 添加软件源信息
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 更新并安装 Docker-CE
sudo yum makecache fast

# 查找 Docker-CE 版本
yum list docker-ce.x86_64 --showduplicates | sort -r

# 安装指定版本，替换下面的 [VERSION] 为上面查看列表中你需要的 Docker 版本
sudo yum -y install docker-ce-[VERSION]

# 启动 docker
sudo systemctl enable docker
sudo systemctl start docker
```

## 添加用户组

默认情况下，只有 root 用户以及 docker 组的用户才可以访问 Docker 引擎的 `Unix Socket`，出于安全考虑，我们一般不建议直接使用 root 用户。当然，笔者也知道很多公司为了简单直接是直接使用 root 用户的，还是看习惯及管理方式吧。

但是笔者还是建议将需要使用 docker 的用户加入到 docker 用户组，避免直接使用 root 用户：

```text
# 建立 docker 用户组
sudo groupadd docker

# 将当前用户加入到用户组
sudo usermod -aG docker $USER
```

