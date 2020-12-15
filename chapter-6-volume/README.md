# 卷管理

卷（`volume`）是用于持久化 Docker 容器生成以及使用的数据的首选机制。Docker 总共提供了三种挂载数据的方式，下面将详细介绍着三者的区别于使用场景，在容器中管理数据一般有两种方式：

* 数据卷（`Volumes`）
* 挂载主机目录（`Bind mounts`）

下面将详细介绍着 Docker 提供的三种挂载方式的区别于使用场景。

## 挂载方式

我们需要知道 Docker 有哪些挂载方式，各种挂载方式的异同点。如下图（图片来自互联网）：

![](../.gitbook/assets/image%20%2818%29.png)

### **区别**

* **volumes：卷**由 Docker 管理（可通过 `docker volume create` 命令创建），卷存储在主机的某个地方（在 Linux 上是`/var/lib/docker/volumes/`）；非 Docker 程序不能改动这一位置的数据；一个 `volume` 可以同时被挂载到几个容器中，即使没有运行中的容器使用 `volume`，它依然会存在（可以使用 `docker volume prune` 清除不再使用的 volume）；
* **bind mounts：**通过这种方式可以将 Docker 生成及使用的数据存放在主机的任何地方。数据甚至可以是重要的系统文件或目录。非 Docker 应用程序可以改变这些数据（因此也存在一定的安全性问题）；
* **tmpfs mounts：**这种挂载方式的数据只存储在宿主机的内存中，不会写入到宿主机的文件系统。

### 适用场景

#### **volumes**

* 在不同的容器中共享数据
* 当需要迁移或备份数据时

#### bind mounts

* 主机和容器共享配置文件

#### tmpfs mounts

* 不想在容器、主机上写任何数据时

## 卷的优点

Docker 鼓励大家使用卷来管理数据，因此列举了一堆卷的优点，主要如下：

* 与 `bind mounts` 相比，卷更易于备份或迁移；
* 您可以使用 Docker CLI 命令或 Docker API 管理卷；
* 卷在 Linux 和 Windows 容器上均可工作；
* 可以在多个容器之间更安全地共享卷；
* 卷驱动程序使您可以将卷存储在远程主机或云提供程序上，以加密卷内容或添加其他功能；
* 新卷可以通过容器预先填充其内容；
* Docker Desktop 上的卷比 Mac 和 Windows 主机上的绑定挂载具有更高的性能。

此外，与将数据持久保存在容器的可写层中相比，卷通常是更好的选择，因为：**卷不会增加使用卷的容器的大小，并且卷的内容存在于给定容器的生命周期之外**。

## 理解-v与--mount

总的来说，`--mount`是更明确和冗长的。最大的区别是该`-v`语法将所有选项合并在一个字段中，而`--mount` 语法将它们分开。这是每个标志的语法比较。

如果需要指定音量驱动程序选项，则必须使用`--mount`。

* **`-v`或`--volume`**：由三个字段组成，以冒号（`:`）分隔。字段必须以正确的顺序排列，并且每个字段的含义不是立即显而易见的。
  * 对于命名卷，第一个字段是卷的名称，在给定的主机上是唯一的。对于匿名卷，将省略第一个字段。
  * 第二个字段是文件或目录在容器中的安装路径。
  * 第三个字段是可选的，并且是用逗号分隔的选项列表，例如`ro`。这些选项将在下面讨论。
* **`--mount`**：由多个键值对组成，这些对值之间用逗号分隔，每个键值对都由一个`<key>=<value>`元组组成。`--mount`比`-v`或`--volume`语法更加清晰、易懂：
  * `type`：其可以是[`bind`](https://docs.docker.com/storage/bind-mounts/)，`volume`，或 [`tmpfs`](https://docs.docker.com/storage/tmpfs/)，本主题讨论卷，因此类型始终为 `volume`。
  * `source`：对于命名卷，这是卷的名称；对于匿名卷，将省略此字段。可以指定为`source` 或`src`；
  * `destination`：其中的文件或目录被安装在容器的路径。可以指定为`destination`，`dst`或`target`；
  * `readonly`：（如果存在）会使绑定安装以[只读](https://docs.docker.com/storage/volumes/#use-a-read-only-volume)方式[安装到容器中](https://docs.docker.com/storage/volumes/#use-a-read-only-volume)。
  * `volume-opt`：可以多次指定的选项采用由选项名称及其值组成的键值对。

### 示例

{% tabs %}
{% tab title="--mount" %}
```text
$ docker run -d \
  --name=nginxtest \
  --mount source=nginx-vol,destination=/usr/share/nginx/html,readonly \
  nginx:latest
```
{% endtab %}

{% tab title="-v" %}
```text
$ docker run -d \
  --name=nginxtest \
  -v nginx-vol:/usr/share/nginx/html:ro \
  nginx:latest
```
{% endtab %}
{% endtabs %}

## 卷管理

这里给出几个卷的管理的简单使用，如下：

```text
# 创建一个卷
$ docker volume create my-vol

# 查看卷
$ docker volume ls
local               my-vol

# 查看卷内容
$ docker volume inspect my-vol
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": {},
        "Scope": "local"
    }
]

# 删除卷
$ docker volume rm my-vol
```



