# 镜像标签

可以使用 `docker image tag` 或 `docker tag` 命令为镜像建立一个标签并指向源镜像。给镜像打标签的命令格式如下：

```text
# 下面两种给镜像打标签的方式都可以
Usage:  docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

# 笔者更加倾向于不带 image 的命令
Usage:  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

打标签的目的可以理解为将镜像进行归档，将镜像纳入到不同的仓库中进行管理

```text
# 示例
$ docker tag 0e5574283393 fedora/httpd:version1.0
$ docker tag httpd fedora/httpd:version1.0
$ docker tag httpd:test fedora/httpd:version1.0.test
```

