# 玩转容器

容器是 Docker 中有一大核心概念，前面已经讲过容器是从镜像运行而来，那么容器运行之后又有哪些操作需要我们掌握呢，如下：

* **查看容器**
* **停止/删除容器**
* **理解容器的生命周期**
* **如何查看容器中的日志**
* **进入/退出容器**
* **保存容器为镜像**
* **导出容器**

{% hint style="info" %}
和操作镜像类似，操作容器的大多数命令格式都有 `docker container xxx`，笔者更加倾向于使用 `docker xxx`。比如 `docker logs` 与 `docker container pull` 实际上是一样的。
{% endhint %}

## Docker 命令图

老规矩，再次贴出 `Docker Commands Diagram` 这张图来进一步加深我们需要掌握的核心命令：

![](../.gitbook/assets/image%20%283%29%20%281%29.png)

