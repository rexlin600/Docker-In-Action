# 玩转镜像

前面已经在基本概念章节介绍了 Docker 的一些核心组件，其中有一个组件就是镜像（Image），这章我们将深入学习关于镜像的使用：

* **镜像的拉取与推送**
* **查看镜像**
* **删除镜像**
* **镜像标签**
* **构建镜像**
* **运行镜像**
* **镜像导入**
* **镜像载入与保存**

{% hint style="info" %}
操作镜像的大多数命令格式都有 `docker image xxx`，笔者更加倾向于使用 `docker xxx`，后续不会每一个都做特殊说明。比如 `docker pull` 与 `docker image pull` 实际上是一样的。
{% endhint %}

## Docker 命令图

此处再次贴出 `Docker Commands Diagram` 这张图来学习我们需要掌握的核心命令：

![](../.gitbook/assets/image%20%283%29%20%281%29.png)

