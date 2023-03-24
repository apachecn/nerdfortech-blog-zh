# 使用 Terraform 构建两层架构以实现高可用性

> 原文：<https://medium.com/nerd-for-tech/building-a-two-tier-architecture-for-high-availability-using-terraform-ae6296fb2126?source=collection_archive---------1----------------------->

## TERRAFORM、CI/CD、AWS

## 在本文中，我将使用 Terraform 构建一个高度可用的双层架构。它将由三个公共子网、三个私有子网、一个公共子网中的堡垒主机、一个 Web 服务器自动扩展组(在私有子网中)和一个面向互联网的…