# 查看容器

仔细查看官方文档的同学可能会发现，查看容器的命令我们可以看到两个：

* **docker container ls**
* **docker ps**

## **查看容器**

上述的两个查看容器的命令实质上效果是一样的，我们可以使用 `docker ps --help` 命令查看更多：

```text
$ docker ps --help

Usage:	docker ps [OPTIONS]

查看容器列表

Options:
  -a, --all             展示所有运行中的容器
  -f, --filter filter   根据条件过滤
      --format string   格式化展示
  -n, --last int        展示最新创建的n个容器，默认全部
  -l, --latest          展示最新创建的容器（默认所有状态）
      --no-trunc        不截断输出
  -q, --quiet           只展示容器ID
  -s, --size            展示所有文件大小
```

查看容器的命令使用起来还是比较简单，如下：

```text
# 查看所有容器
$ docker ps -a
CONTAINER ID        IMAGE                                                          COMMAND                  CREATED             STATUS              PORTS                            NAMES
e4cc9ec708d4        registry.cn-chengdu.aliyuncs.com/ms4cloud/eureka:latest        "/bin/sh -c 'sleep 1…"   13 days ago         Up 8 hours          0.0.0.0:8761->8761/tcp           eureka-server
6c08990e3389        registry.cn-chengdu.aliyuncs.com/ms4cloud/docker-docs:latest   "/docker-entrypoint.…"   13 days ago         Up 8 hours          80/tcp, 0.0.0.0:4000->4000/tcp   docker-docs

# 查看最新创建的1个容器
$ docker ps -n 1
CONTAINER ID        IMAGE                                                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
e4cc9ec708d4        registry.cn-chengdu.aliyuncs.com/ms4cloud/eureka:latest   "/bin/sh -c 'sleep 1…"   13 days ago         Up 8 hours          0.0.0.0:8761->8761/tcp   eureka-server

# 格式化显示
docker ps -n 1 --format "{{.ID}}\t {{.Image}}"
e4cc9ec708d4	 registry.cn-chengdu.aliyuncs.com/ms4cloud/eureka:latest
```

