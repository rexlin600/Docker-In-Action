# 私有仓库

我们知道因为网络等诸多因素，有时候 `Docker Hub` 并不能很方便的访问。用户可以创建一个本地仓库供私人使用。

鉴于此，Docker 官方给我们提供了一个[私有仓库](https://docs.docker.com/registry/)工具，它可以让我们用于构建私有的镜像仓库。

## 安装和运行

我们可以使用如下方式使用官方的 `registry` 镜像来启动私有仓库：

```text
$ docker run -d -p 5000:5000 --restart=always --name registry registry
```

默认情况下，仓库会被创建在容器的 `/var/lib/registry` 目录下。你可以通过 `-v` 参数来将镜像文件存放在本地的指定路径。例如下面的例子将上传的镜像放到本地的 `/opt/data/registry` 目录：

```bash
$ docker run -d \
    -p 5000:5000 \
    -v /opt/data/registry:/var/lib/registry \
    registry
```

安装好私有仓库后我们就可以对镜像打标签、上传到私有仓库了。唯一需要注意的是对于使用私有仓库而言，我们给镜像打标签、拉取推送镜像等操作同样要适用于前面我们讲的[两段式命令](../chapter-1-basic-concepts/4-repository.md#ming-ming-gui-fan)。

## 注意事项

Docker 默认不允许非 `HTTPS` 方式推送镜像，我们可以修改配置选项`insecure-registries`来取消消这个限制（对于 `systemd` 的系统，在 `/etc/docker/daemon.json` 中配置即可）

