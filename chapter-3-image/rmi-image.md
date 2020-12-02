# 删除镜像

Docker 删除镜像可以使用 `docker rmi` 命令。除此之外还有一个容易与 `docker rmi` 混淆的 Docker 命令是 `docker rm`，下面是二者的区别：

* **docker rmi**：删除镜像，带 `i` 的是删除镜像
* **docker rm**：删除容器

## 命令格式

删除镜像的命令格式如下：

```text
docker rm [OPTIONS] CONTAINER [CONTAINER...]

Remove one or more containers

Options:
  -f, --force     强制删除
  -l, --link      删除指定的链接
  -v, --volumes   删除与容器关联的匿名卷
```

删除镜像相对比较简单，如下：

```text
[root@chengdu-1v2g ~]# docker rmi docs/docker.github.io:1.0.0
Untagged: docs/docker.github.io:1.0.0
[root@chengdu-1v2g ~]# 
```

