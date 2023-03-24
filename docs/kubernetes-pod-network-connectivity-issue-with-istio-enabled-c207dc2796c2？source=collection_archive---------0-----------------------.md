# 启用 Istio 时 Kubernetes Pod 网络连接问题

> 原文：<https://medium.com/nerd-for-tech/kubernetes-pod-network-connectivity-issue-with-istio-enabled-c207dc2796c2?source=collection_archive---------0----------------------->

当试图在 Istio 边车运行前连接网络时，K8s Pod 可能会返回错误。

许多应用程序在启动时执行命令或检查，这需要网络连接。如果`istio-proxy`边车容器没有准备好，这会导致应用程序容器挂起或重启。

本文演示了如何使用 Istio 的注释`holdApplicationUntilProxyStarts`来避免网络连接问题。

# 测试环境