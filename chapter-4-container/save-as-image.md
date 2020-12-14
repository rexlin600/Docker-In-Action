# 保存为镜像

我们可以将容器使用 `commit` 指令将其保存为镜像，命令格式如下：

```text
docker commit --help

Usage:	docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

从容器创建一个镜像

Options:
  -a, --author string    镜像作者名称
  -c, --change list      将Dockerfile指令应用于创建的映像
  -m, --message string   提交镜像的Message
  -p, --pause            在提交镜像期间暂停容器（默认）
```

使用 `commit` 命令提交镜像时，我们更多的是从一个容器去提交镜像，使用时需要注意对于镜像的命令都是两段式命名，即 `<仓库名>:<标签>` ，示例如下：

```text
# 从本地运行的容器提交一个镜像
$ docker commit -a "rexlion600@gmail.com" -m "test" b3991b9dbf88  registry.cn-chengdu.aliyuncs.com/ms4cloud/docker-docs:latest 
sha256:...
```

