# 导出容器

如果要导出本地某个容器，可以使用 `docker export` 命令。

```bash
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:18.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
$ docker export 7691a814370e > ubuntu.tar
```

这样将导出容器快照到本地文件，如下：

```bash
# 导出容器
$ docker export -o ~/Desktop/docker-docs.tar e4cc9ec708d4

# 然后可以载指定目录下看到一个 docker-docs.tar 文件
```

