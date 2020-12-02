# 镜像导入与导出

Docker 支持镜像的导入与导出，实现这个功能主要有两组命令：

* `docker import` 与 `docker xport`
* `docker save` 与 `docker load`

## 联系与区别

### 联系

**导入：**是指将一个为 `.tar` 的文件或文件流通过 **import** 或 **load** 命令导入到镜像中管理；

**导出：**是指将一个 `container` 通过 **export** 导出为一个 `.tar` 文件，或者使用 **save** 命令将一个 `image` 镜像保存为 `.tar` 文件。

### 区别

* 使用 **export** 命令导出的 `tar` 文件大小略小于使用 **save** 命令导出的 `tar` 文件；
* **export** 是从容器导出 `tar` 文件，而 **save** 命令是从镜像导出 `tar` 文件；
* 【**重点**】使用 **export** 命令，从容器导出的镜像无法保留镜像的所有历史；而使用 **save** 命令因为是从镜像导出的，因此能够完整的保留下每一层的 `layer` 信息。

可以查看查看前面的 [Docker 命令图](./#docker-ming-ling-tu) 理解这两组命令的在使用上的区别。下面给出这两组命令在实际使用时的建议：

{% hint style="success" %}
1）**备份镜像及层变化**：使用 `save`、`load` 命令

2）**备份镜像及内容变化**：使用 `import`、`export` 命令
{% endhint %}

## save/load 命令组

我们先来看下 save/load 这一组命令，命令格式如下：

```text
# save 命令
docker save [OPTIONS] IMAGE [IMAGE...]

  -o, --output string   输出到文件
  
# load 命令
docker load [OPTIONS]

  -i, --input string   从指定文件读取
  -q, --quiet          抑制负载输出
```

可以看到，保存镜像的命令非常简单，下面给出使用示例：

```text
# 保存镜像，执行完成之后可以在当前命令的执行目录看到一个 ubuntun.tar 文件
$ docker save -o ubuntu.tar ubuntu:latest

# 然后我们再删除本地的 ubuntu 镜像
$ docker rmi ubuntu:latest

# 导入镜像
$ docker load -i ubuntu.tar
bacd3af13903: Loading layer [==================================================>]  75.27MB/75.27MB
9069f84dbbe9: Loading layer [==================================================>]  15.36kB/15.36kB
f6253634dc78: Loading layer [==================================================>]  3.072kB/3.072kB
Loaded image: ubuntu:latest
$

# 查看镜像历史
$ docker image history ubuntu:latest
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
f643c72bc252        6 days ago          /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>           6 days ago          /bin/sh -c mkdir -p /run/systemd && echo 'do…   7B
<missing>           6 days ago          /bin/sh -c [ -z "$(apt-get indextargets)" ]     0B
<missing>           6 days ago          /bin/sh -c set -xe   && echo '#!/bin/sh' > /…   811B
<missing>           6 days ago          /bin/sh -c #(nop) ADD file:4f15c4475fbafb3fe…   72.9MB
MacBook-Pro:Desktop rexlin600$
```

## import/export 命令组

接着我们再来看下 import/export 这一组命令，命令格式如下：

```text
# import 命令 
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]

  -c, --change list       将Dockerfile指令应用于创建的映像
  -m, --message string    使用镜像的提交信息进行导入
      --platform string   实验功能，如果服务器支持多平台，则设置平台
      
# export 命令
docker export [OPTIONS] CONTAINER

  -o, --output string   指定输出文件位置   
```

同样的，实际上在本地使用这组命令也非常简单，这里就不写示例了，建议读者可以实际操作体验一下。

