# 镜像拉取与推送

安装 Docker 之后，Docker 的默认 Registry 是 Docker Hub，因此默认拉取镜像是从 Docker Hub 中拉取。默认推送镜像也是推送到 Docker Hub（需登陆）。

## 拉取镜像

拉取镜像使用 `docker pull` 命令，可以查看 Docker 命令帮助了解更多细节，如下：

```text
docker pull --help

Usage:	docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --platform string         Set platform if server is multi-platform capable
  -q, --quiet                   Suppress verbose output
```

### 从 Docker Hub 拉取

关于拉取镜像时镜像的名称格式可以参考 [基本概念-仓库-命令格式](../chapter-1-basic-concepts/repository.md#ming-ming-gui-fan)，下面给出一个从 Docker Hub 拉取镜像的示例（拉取缓慢请先配置[镜像加速器](../chapter-2-install-tutorial/mirror-accelerator.md)）：

```text
# 拉取 ubuntu 镜像
$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
da7391352a9b: Pull complete
14428a6d4bcd: Pull complete
2c2d948710f2: Pull complete
Digest: sha256:c95a8e48bf88e9849f3e0f723d9f49fa12c5a00cfc6e60d2bc99d87555295e4c
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

上面演示了从 Docker Hub 拉取一个 ubuntu 镜像，由于没有指定 tag，因此默认是拉取 latest 标签的镜像。从上面的拉取过程我们可以归纳如下信息：

* 默认从 **Docker Hub Registry** 拉取镜像；
* 未指定用户名则默认为 `library`，表示拉取官方镜像；
* 未指定 `tag` 则拉取标签为 `latest` 的镜像；
* 镜像是分层存储的，下载过程中会给出 `12` 位长度的镜像 `ID`；下载完成后悔给出镜像的完整 `sha256` 摘要，确保下载一致性
* 如果你下载时和我这里的 `ID` 或 `sha256` 不一致是因为镜像会有官方持续胡，任何更改都会以原来的标签重新发布，从而确保用户能够一直使用这个标签获取最稳定、安全的镜像

### 从私有 Registry 拉取

如果你正在使用私有 Registry 或使用云服务商提供的容器服务，你可以通过指定一个地址拉取镜像，格式一般为： `<Registry 域名>:[端口]/<用户名/命名空间>/<软件名>:<标签>` 的方式拉取镜像，这里以从阿里云拉取镜像为例：

```text
# 登陆到私有 Registry
sudo docker login --username=<username> registry.cn-chengdu.aliyuncs.com

# 拉取镜像
sudo docker pull registry.cn-chengdu.aliyuncs.com/ms4cloud/eureka:[镜像版本号]
...
```

## 推送镜像

推送镜像使用 docker push 命令，可以查看 Docker 命令帮助了解更多细节，如下：

```text
$ docker push --help

Usage:	docker push [OPTIONS] NAME[:TAG]

Push an image or a repository to a registry

Options:
      --disable-content-trust   Skip image signing (default true)
```

无论你是想要将镜像推送到 Docker Hub 或者其他私有镜像仓库，都应该遵循如下基本步骤：

* 登陆到 Registry
* 给镜像打标签
* 推送镜像

下面是我将一个本地镜像推送到阿里云 Registry 的过程：

```text
# 登陆
$ docker login --username=rexlin600 registry.cn-chengdu.aliyuncs.com
Password:
Login Succeeded

# 打标签
docker tag eureka_eureka:latest registry.cn-chengdu.aliyuncs.com/rexlin600/eureka:latest

# 推送镜像
docker push registry.cn-chengdu.aliyuncs.com/rexlin600/eureka:latest
The push refers to repository [registry.cn-chengdu.aliyuncs.com/rexlin600/eureka]
ad2ddf25e2bb: Preparing
e602bce8fe9b: Preparing
b3a7582fea88: Preparing
a52fcbff5465: Preparing
767f936afb51: Preparing
denied: requested access to the resource is denied
MacBook-Pro:opt rexlin600$ docker rmi registry.cn-chengdu.aliyuncs.com/rexlin600/eureka:latest
Untagged: registry.cn-chengdu.aliyuncs.com/rexlin600/eureka:latest
```



