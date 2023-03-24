# Kubernetes Helm 错误:升级失败:另一个操作(安装/升级/回滚)正在进行中。

> 原文：<https://medium.com/nerd-for-tech/kubernetes-helm-error-upgrade-failed-another-operation-install-upgrade-rollback-is-in-progress-52ea2c6fcda9?source=collection_archive---------0----------------------->

## 错误

最近在我的 AKS 集群上尝试 helm 命令时，我收到了以下错误:

```
Error: UPGRADE FAILED: another operation (install/upgrade/rollback) is in progress
```

我尝试的命令是将我的 kube-prometheus-stack 版本更新到 30.0.0 版本，但是当运行任何 helm 安装、升级或回滚操作时，您可能会看到这种情况。

```
helm upgrade…
```