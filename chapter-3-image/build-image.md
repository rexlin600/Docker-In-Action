# 构建镜像

Docker 构建镜像需要使用到 Dockerfile 文件，后续会有专门的章节讲解 [Dockerfile](../chapter-9-best-practices/best-practice-dockerfile.md) 的使用。这里大家先熟悉 Docker 构建镜像的命令。

## 命令格式

```text
# 命令格式
Usage:  docker build [OPTIONS] PATH | URL | -

Build an image from a Dockerfile

Options:
    --tag, -t                 镜像的名字及标签，通常 name:tag 或者 name 格式（重要）
                              可以在一次构建中为一个镜像设置多个标签。
    -f                        指定要使用的Dockerfile路径（重要）
    --build-arg=[]            设置镜像创建时的变量
    --cpu-shares              设置 cpu 使用权重
    --cpu-period              限制 CPU CFS周期
    --cpu-quota               限制 CPU CFS配额
    --cpuset-cpus             指定使用的CPU id
    --cpuset-mems             指定使用的内存 id
    --disable-content-trust   忽略校验，默认开启
    --force-rm                设置镜像过程中删除中间容器
    --isolation               使用容器隔离技术
    --label=[]                设置镜像使用的元数据
    -m                        设置内存最大值
    --memory-swap             设置Swap的最大值为内存+swap，"-1"表示不限swap
    --no-cache                创建镜像的过程不使用缓存
    --pull                    尝试去更新镜像的新版本
    --quiet, -q               安静模式，成功后只输出镜像 ID
    --rm                      设置镜像成功后删除中间容器
    --shm-size                设置/dev/shm的大小，默认值是64M
    --ulimit                  Ulimit配置。
    --squash                  将 Dockerfile 中所有的操作压缩为一层。
    --network                 默认 default。在构建期间设置RUN指令的网络模式
```

## 示例

```text
# 以当前目录的 Dockerfile 构建
docker build -t ubuntu:latest .

# 使用 URL 构建
docker build github.com/creack/docker-firefox

# 指定 Dockerfile 构建
docker build -f <YOUR PATH>/Dockerfile .
```

更详细的使用 docker build 构建镜像的命令，请参考 [docker build](https://docs.docker.com/engine/reference/commandline/build/)。

