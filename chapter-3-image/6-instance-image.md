# 运行镜像

Docker 运行镜像使用 `docker run` 命令，Docker 运行镜像实际上就是实例化镜像为容器的过程。

换句话说，使用 `docker run` 命令运行镜像会生成一个运行中的容器

{% hint style="warning" %}
`docker run` 命令实际上相当于先执行了`docker create（在镜像上创建读写层）`，接着再执行了`docker start（启动容器），如下图：`
{% endhint %}

![](../.gitbook/assets/image%20%288%29.png)

## 命令格式

使用 `docker run` 命令可以将一个镜像实例化为容器，命令格式如下：

```text
# 命令格式
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

可以看见，我们还**可以在 docker run 命令的最后添加一条 `command` 以及一些 param**，这在有些场景下是非常有用的，后续大家在学习使用 `docker-compose.yaml` 文件时就知道启动容器往往需要一些命令的辅助，而类似这样的 `command` 是非常有用的。 

## 常用选项

学习 `docker run` 命令最值得关注的实质上是一些 `OPTIONS` 的使用，这里列举出一些常用的选项帮助大家快速掌握 `docker run` 命令：

```text
OPTIONS：
  --restart	       容器退出时重新启动策略以应用 yes或no

  -d               后台运行容器并返回容器ID
  -i               交互模式运行容器，常常与 -t 同时使用
  -t               为容器分配一个伪输入终端，常常与 -i 同时使用
  
  -P               随机端口映射，容器内部端口映射到主机端口
  -p               指定端口映射，格式：<主机端口>:<容器端口>
  
  --label , -l		 在容器上设置元数据
  --name           指定容器名称
  
  -m               容器使用的最大内存值
  
  --entrypoint		 覆盖容器的默认ENTRYPOINT
  -e               指定环境变量，如：-e TZ="Asia/Shanghai"
  --mount		       将文件系统挂载附加到容器
  --env-file=[]    从指定文件读入环境变量

  
  --net="bridge"   指定容器的网络连接类型，支持 bridge/host/none/container 四种
  
  --link=[]        添加链接到另一个容器
  --expose=[]      开发一个端口或一组端口
  
  --voule,-v       绑定一个卷     
  --volumes-from	 从指定的容器装载卷
  
  --workdir,-w	   容器内的工作目录
```

## 运行示例

更丰富的示例请参考 [Docker 官方示例](https://docs.docker.com/engine/reference/commandline/run/#examples)。

```text
# 分配名称
ocker run --name test -it debian

# 指定端口
$ docker run -p 127.0.0.1:80:8080/tcp ubuntu bash
$ docker run --expose 80 ubuntu bash

# 设置环境变量
$ docker run -e MYVAR1 --env MYVAR2=foo --env-file ./env.list ubuntu bash
$ docker run --env VAR1=value1 --env VAR2=value2 ubuntu env | grep VAR
VAR1=value1
VAR2=value2

# 设置工作目录
$ docker  run -w /path/to/dir/ -i -t  ubuntu pwd

# 添加元数据
$ docker run -l my-label --label com.example.foo=bar ubuntu bash

# 将容器连接到指定网络
$ docker run -itd --network=my-net busybox
$ docker run -itd --network=my-net --ip=10.10.9.75 busybox

# 挂载
$ docker run -v /doesnt/exist:/foo -w /foo -i -t ubuntu bash
$ docker run -t -i -v /var/run/docker.sock:/var/run/docker.sock \
-v /path/to/static-docker-binary:/usr/bin/docker \
busybox \
sh

# 使用 mount 绑定
$ docker run --read-only \
--mount type=volume,target=/icanwrite \
busybox \
touch /icanwrite/here

# 重新启动
$ docker run --restart=always redis
```

## 精品案例

{% hint style="warning" %}
**下面我使用的镜像均为** `registry.cn-chengdu.aliyuncs.com/ms4cloud/<软件名>:<标签>`**，请替换为相应的官方镜像即可**
{% endhint %}

### Docker 文档

```text
docker run -d --name="docker-docs" \
-p 4000:4000 \
--restart="always" \
-e TZ="Asia/Shanghai" \
registry.cn-chengdu.aliyuncs.com/ms4cloud/docker-docs:latest
```

### Eureka

你需要提前构建好自己的 Eureka 镜像，构建 Eureka 镜像的 Dockerfile 请参考文末。

```text
docker run -d --name="eureka-server" \
-p 8761:8761 \
--restart="always" \
-e TZ="Asia/Shanghai" \
registry.cn-chengdu.aliyuncs.com/ms4cloud/eureka:latest
```

### MySQL

运行前请先创建目录、拷贝配置文件、修改权限，运行镜像命令如下：

```text
docker run -d --name="mysql-server" \
-p 3306:3306 \
--restart="always" \
-v "~/opt/mysql/data:/var/lib/mysql" \
-v "~/opt/mysql/conf/my.cnf:/root/.my.cnf:ro" \
-e "TZ=Asia/Shanghai" \
-e MYSQL_ROOT_PASSWORD="root" \
-e MYSQL_USER="root" \
-e MYSQL_PASSWORD="123456" \
registry.cn-chengdu.aliyuncs.com/ms4cloud/mysql-server:5.7.32
--default-authentication-plugin=mysql_native_password --character-set-server=utf8 --collation-server=utf8_general_ci --explicit_defaults_for_timestamp
```

### Portainer

运行前请先创建目录、拷贝配置文件、修改权限，运行镜像命令如下：

```text
docker run -d --name="portainer" \
--restart="always" \
-p 9000:9000 \
-e TZ="Asia/Shanghai" \
-v "/var/run/docker.sock:/var/run/docker.sock" \
-v "~/opt/portainer/data:/data:rw" \
-v "~/opt/portainer/public:/public:rw" \
registry.cn-chengdu.aliyuncs.com/ms4cloud/portainer:1.24.1
```

### RabbitMQ

运行前请先创建目录、拷贝配置文件、修改权限，运行镜像命令如下：

```text
docker run -d --name="rabbitmq-server" \
--restart="always" \
-p 5672:5672 -p 15672:15672 \
-e TZ="Asia/Shanghai" \
-v "~/opt/rabbitmq/data:/var/lib/rabbitmq" \
registry.cn-chengdu.aliyuncs.com/ms4cloud/rabbitmq:3.8.9-management
```

### Redis

运行前请先创建目录、拷贝配置文件、修改权限，运行镜像命令如下：

```text
docker run -d --name="redis-server" \
--restart="always" \
-p 6379:6379 \
-e TZ="Asia/Shanghai" \
-v "~opt/redis/conf/redis.conf:/usr/local/etc/redis/redis.conf" \
-v "~/opt/redis/data:/data" \
registry.cn-chengdu.aliyuncs.com/ms4cloud/redis:6.0.4
```

## 参考

### Eureka Dockerfile

只将需要 `Dockerfile` 和 `/jar/eureka-server.jar` 包放在同一目录 `eureka` 下即可构建，Dockerfile文件如下：

```text
FROM anapsix/alpine-java:8_server-jre_unlimited

LABEL author.name = "rexlin600"\
      email = "rexlin600@gmail.com"

ENV TZ=Asia/Shanghai
ENV JAVA_OPTS="-Xms128m -Xmx256m -Djava.security.egd=file:/dev/./urandom"
ENV APP="eureka-server"

RUN ln -sf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN mkdir -p /app

WORKDIR app

EXPOSE 8761

ADD ./jar/${APP}.jar ./

CMD sleep 10;java $JAVA_OPTS -jar ${APP}.jar
```

文件目录及构建命令参考：

```text
# 文件目录
$ tree
.
├── Dockerfile
├── jar
│   └── eureka-server.jar

1 directory, 4 files

# 进入 Dockerfile 所在目录

# 构建命令
docker build .
```



