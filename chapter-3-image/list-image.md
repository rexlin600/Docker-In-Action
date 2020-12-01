# 查看镜像

查看镜像具体来说可以再细分为如下几个具体的查看操作：

* 查看本地镜像列表（`docker images`、`docker image ls`）
* 查看镜像详细信息（`docker insepect`）
* 查看镜像历史（`docker history`）

下面我将分别介绍这几个指令如何使用，这样大家能够更加清楚自己在什么场景需要使用什么样的查看命令。

## 查看镜像列表

列出镜像列表我们可以使用如下两个命令都可以达到一样的效果：

* `docker image ls`
* `docker images`（笔者习惯使用这个命令）

我们先看下 docker images 命令：

```text
$ docker images --help

Usage:	docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             显示所有镜像，包含中间层镜像
      --digests         显示摘要
  -f, --filter filter   过滤输出
      --format string   格式化输出
      --no-trunc        不截断输出
  -q, --quiet           只显示镜像ID
```

下面给出一个使用 docker image ls 以及 docker images 的示例，可以看到效果是一样的：

```text
$ docker image ls
# 仓库名称     标签名称   镜像ID          创建时间       镜像大小
REPOSITORY    TAG      IMAGE ID        CREATED       SIZE
ubuntu        latest   f643c72bc252    5 days ago    72.9MB
<none>        <none>   a8a07b74a7fd    24 hours ago  79.1MB
...

$ docker images
# 仓库名称     标签名称   镜像ID          创建时间       镜像大小
REPOSITORY    TAG      IMAGE ID        CREATED       SIZE
ubuntu        latest   f643c72bc252    5 days ago    72.9MB
...
```

###  虚悬镜像

在下面的示例中可以看到显示了一个仓库名、标签均为 `<none>` 的镜像，这个镜像称之为 **dangling image**，也叫**虚悬镜像**。

虚悬镜像一般是因为拉取镜像时，官方维护的镜像更新了，因此旧的镜像上的这个名称就被取消了，新的镜像保留其名称。当然，在构建镜像时也可能会出现，原理是一样的。

```text
$ docker images
# 仓库名称     标签名称   镜像ID          创建时间       镜像大小
REPOSITORY    TAG      IMAGE ID        CREATED       SIZE
ubuntu        latest   f643c72bc252    5 days ago    72.9MB
<none>        <none>   a8a07b74a7fd    24 hours ago  79.1MB
...
```

我们可以通过使用如下命令来查看所有的虚悬镜像：

```bash
$ docker image ls -f dangling=true
```

一般来说，虚悬镜像是没有意义的，可以随意删除，可使用如下命令删除虚悬镜像：

```bash
$ docker image prune
```

### 中间层镜像

我们已经知道了 Docker 是使用分层存储的方式来管理镜像的，我们可以通过添加 `-a` 参数来显示所有镜像，包含中间层镜像，如下：

```bash
$ docker image ls -a
# 仓库名称     标签名称   镜像ID          创建时间       镜像大小
REPOSITORY    TAG      IMAGE ID        CREATED       SIZE
redis         latest   36t3242bc252    5 days ago    72.9MB
<none>        <none>   84937b74k89d    24 hours ago  79.1MB
<none>        <none>   csd37b74kd4g    24 hours ago  200MB
...
```

{% hint style="danger" %}
**这里显示的为 `<none>` 的镜像与虚悬镜像不同，它门是中间层镜像，是其他镜像依赖的镜像**
{% endhint %}

因为中间层镜像时其他镜像所依赖的镜像，因此中间层镜像不能随意删除，事实上也不需要删除，只需要删除相应的顶层镜像，其依赖的镜像会连带被删除。

### 查看占用空间

需要说明的是，Docker Hub 中指明的镜像并不等同于实际拉取下来的镜像大小，前者显示的体积是压缩后的体积，关注的是在网络传输中消耗的流量大小。

我们可以使用下述命令查看镜像、容器、数据卷所占用的空间：

```bash
$ docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              21                  2                   3.603GB             3.552GB (98%)
Containers          2                   2                   32.77kB             0B (0%)
Local Volumes       1                   0                   0B                  0B
Build Cache         0                   0                   0B                  0B
MacBook-Pro:opt rexlin600$
```

### 格式化输出

有时候我们可能会有这样的情况，电脑屏幕同时开了多个 shell 窗口，如果直接查看镜像的话内容会折叠起来，不方便查看。

我们可以使用格式化的方式来查看镜像，如下：

```bash
# 只查看 镜像ID、仓库:Tag 的格式化输出
$ docker images --format "{{.ID}}\t {{.Repository}}:{{.Tag}}"
6e254207f4e2	 registry.cn-chengdu.aliyuncs.com/ms4cloud/docker-docs:latest
6704fa7d9f30	 registry.cn-chengdu.aliyuncs.com/ms4cloud/portainer:1.24.1
...
```

## 查看详细信息









## 查看镜像历史



