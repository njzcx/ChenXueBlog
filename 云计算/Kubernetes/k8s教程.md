> 原文：https://kubernetes.io/zh-cn/docs/home/supported-doc-versions/

# 1. K8S集群组件

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/云计算/Kubernetes/images/k8s教程/1741683757776-e84a12e0-c934-42aa-9058-178fd1edeb80.png)

控制平面组件（Control Plane Components）

管理集群的整体状态：

- kube-apiserver：公开 Kubernetes HTTP API 的核心组件服务器

- etcd：具备一致性和高可用性的键值存储，用于所有 API 服务器的数据存储

- kube-scheduler：查找尚未绑定到节点的 Pod，并将每个 Pod 分配给合适的节点。

- kube-controller-manager：运行控制器来实现 Kubernetes API 行为。

- cloud-controller-manager (optional)：与底层云驱动集成

Node 组件

在每个节点上运行，维护运行的 Pod 并提供 Kubernetes 运行时环境：

- kubelet：确保 Pod 及其容器正常运行。
- kube-proxy（可选）：维护节点上的网络规则以实现 Service 的功能。
- 容器运行时（Container runtime）：负责运行容器的软件，阅读容器运行时以了解更多信息。

## 1.1. Kubernetes对象管理

使用`kubectl`管理k8s对象。有3中管理方式：

- 指令式命令：直接通过`kubectl <动作> <对象>`管理对象
- 指令式对象配置：通过`kubectl <动作> -f <对象文件>` 实现管理对象
- 声明式对象配置：通过 `kubectl apply -f <目录>`实现管理对象 