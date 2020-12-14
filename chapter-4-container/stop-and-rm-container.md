# 停止/删除容器

停止、删除容器也是我们常用的管理容器的命令，如下：

* **docker stop**：停止容器
* **docker rm**：删除容器

## 停止容器

停止运行的容器很简单，命令格式如下：

```text
$ docker stop --help

Usage:	docker stop [OPTIONS] CONTAINER [CONTAINER...]

停止一个或多个容器

Options:
  -t, --time int   等待数秒后kill掉容器，默认10s
```

这里我们停止一个运行中的容器，看看会发生什么：

```text
# 停止一个容器后会输出这个容器的12位ID
$ docker stop 6c08990e3389
6c08990e3389
```

## 删除容器

删除容器默认情况下只能删除已经停止的容器，当然你也可以强制删除一个处于运行中或已停止的容器，删除容器的格式如下：

```text
$ docker rm --help

Usage:	docker rm [OPTIONS] CONTAINER [CONTAINER...]

删除一个或多个容器

Options:
  -f, --force     强制删除一个容器
  -l, --link      删除link
  -v, --volumes   删除volumes
```

接下来我们试试删除容器：

```text
# 我们尝试删除一个处于 Pause 状态的容器，可以看到并不能删除
$ docker rm 6c08990e3389
Error response from daemon: You cannot remove a paused container 6c08990e3389c15d5f134101dacd1994f3269a53b8a6c3f137e3f3420ecb2633. Unpause and then stop the container before attempting removal or force remove

# 然后我们停止这个容器再尝试删除（可以看到停止后再删除就可以删除了）
$ docker stop 6c08990e3389
6c08990e3389
$ docker rm 6c08990e3389
6c08990e3389

# 再试试强制删除一个运行中的容器
docker rm -f e4cc9ec708d4
e4cc9ec708d4
```



