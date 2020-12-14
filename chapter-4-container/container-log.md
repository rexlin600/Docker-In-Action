# 容器日志

在实际使用中，我们可能经常有这样的需求：不进入容器中而查看容器的日志。Docker 给我们提供了一个专门从容器中取出日志的命令：

```text
$ docker logs --help

Usage:	docker logs [OPTIONS] CONTAINER

从容器中拉取日志

Options:
      --details        显示提供给日志的额外详细信息
  -f, --follow         跟踪日志输出
      --since string   从指定时间后显示 (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
      --tail string    显示日志文件最后的多少行日志 (default "all")
  -t, --timestamps     显示时间戳
      --until string   从指定时间前显示 (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
MacBook-Pro:~ rexlin600$
```

比如，我们在 Docker 容器中运行了一个 Eureka 服务，我们需要查看 Eureka 服务的日志，如下：

```text
# 查看最后10行日志
docker logs --tail="10" eureka-server

# 跟踪日志输出
docker logs -f eureka-server
```

