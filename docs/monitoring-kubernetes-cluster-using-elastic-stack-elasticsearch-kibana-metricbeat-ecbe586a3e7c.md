# 使用弹性堆栈(Elastic search-ki Bana-metric beat)监控 Kubernetes 集群

> 原文：<https://medium.com/nerd-for-tech/monitoring-kubernetes-cluster-using-elastic-stack-elasticsearch-kibana-metricbeat-ecbe586a3e7c?source=collection_archive---------0----------------------->

## 技术基础

## 用橡皮筋监控

## 目标:

在这篇中型文章中，我们将部署用于监控 Kubernetes 集群的弹性堆栈(Elastic search-ki Bana-metric beat)。Metricbeat 将从 Kubernetes 集群收集指标，并将数据转发给 Elasticsearch 进行分析。Kibana 将允许我们以仪表板格式可视化数据。最棒的是，我们将部署这个…