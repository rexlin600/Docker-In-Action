# 镜像导入

Docker 支持从 tarball 导入内容以创建文件系统镜像，命令格式如下：

```text
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
```

这个命令很简单，实际上就是从文件、URL 来创建一个镜像，下面给出一些丰富的示例：

```text
# 从归档文件导入
$ docker import http://example.com/exampleimage.tgz


```

