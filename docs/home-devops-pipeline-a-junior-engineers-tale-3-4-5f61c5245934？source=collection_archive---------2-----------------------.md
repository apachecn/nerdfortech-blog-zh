# 家庭开发管道:一个初级工程师的故事(3/4)

> 原文：<https://medium.com/nerd-for-tech/home-devops-pipeline-a-junior-engineers-tale-3-4-5f61c5245934?source=collection_archive---------2----------------------->

*内容:* [*1*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-1-4-336ed07a6ec0) *，* [*2*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-2-4-7be3e3c292c) *，(* [*3*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-3-4-5f61c5245934) *)，* [*4*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-4-4-5db7c1610e3e)

在这一系列文章中，我将解释我是如何使用几个 Raspberry Pis 和许多软件构建自己的家庭开发环境的。在本文中，我将介绍管道的部署部分。

> [🔗在 Github 上获取 Home DevOps 管道代码📔](https://github.com/sk-t3ch/home-repo)

# 部署

## Ansible

关于 Drone 的另一个伟大的事情是可用的插件[——我们将使用](http://plugins.drone.io) [Ansible](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html) 插件作为在多台机器上运行 ssh 命令的工具。我们也将利用[无人机机密](https://docs.drone.io/secret/repository/)作为安全部署某些配置值的方法。

我见过的一般部署方法通常包括发布到 AWS，然后神奇的事情发生了，使用云形成模板，你的栈就创建好了。

然而，我们希望在房屋回购中保留房屋。为此，我们希望部署到本地网络上的特定机器上。对我来说，这是一个负载的其他 Raspberry Pis 可以做很多事情，从控制我的 3D 打印机，到作为 CCTV 摄像机的网络工作。

为此，我们将像这样使用 Ansible:

```
kind: pipeline
name: commitPipelinetrigger:
  event:
     - pushplatform:
  os: linux
  arch: armsteps:
  - name: trigger-rebuild-of-image
    image: plugins/ansible:1
    environment:
      repo_username:
        from_secret: repo_username
      repo_password:
        from_secret: repo_password
    settings:
      private_key:
        from_secret: ansible_private_key
      playbook: ansible/playbook.yml
      inventory: ansible/inventory.yml
```

Ansible 执行的动作在`playbook.yml`中找到，我们想要在其上执行的机器在`inventory.yml`中描述:

```
# playbook.yml
- hosts: camerastasks:- name: .ssh directory
  file:
    path: ~/.ssh
    owner: pi
    group: pi
    state: directory- name: put key in
  copy:
    content: "{{ lookup('env','ansible_private_key') }}"
    dest: ~/.ssh/deploy_key
    owner: pi
    group: pi
    mode: 0600
    become: pi- name: clone the repo
  git:  
    repo: ssh://git@git.<domain>/<username>/cctv.git
    dest: /home/pi/cctv
    key_file: ~/.ssh/deploy_key
    accept_hostkey: yes
    force: yes- name: Rebuild docker image
   shell:
   cmd: docker-compose up --remove-orphans --force-recreate --build -d
  chdir: /home/pi/cctv- name: remove private key
   shell:
   cmd: rm ~/.ssh/deploy_key
```

和

```
# invetory.yml
all:
  hosts:
    drone.<domain>:
  children:
    cameras:
      hosts:
        192.168.0.41:
        192.168.0.42:
      vars:
        ansible_ssh_user: pi
```

感谢 ansible，我的 CCTV 摄像机更新部署流程大大简化了。

## 部署到 AWS

在家部署很有趣，也很棒，但训练轮必须脱落。现实世界中的开发涉及到云，在我工作的公司，这意味着 AWS。

将您的服务部署到 AWS EC2 机器也应该有一个测试步骤，但是最常见的架构(x86)与 Raspberry Pi 上的 arm 架构非常不同。如果我们的构建将被部署到 x86 机器上，我们不应该使用 arm 机器来测试它们。我们必须部署 x86 无人驾驶飞机。

然而，情节再次扭曲，为了使用我们的环境部署无人机 x86 runner，我们需要使用仅针对 x86 而非 arm 构建的无人机/云编队映像。但是我们可以自己重建它，就像我在我的[代码](https://github.com/sk-t3ch/home-repo)中展示的那样。像一个好孩子一样，我也在 dockerhub 上发布了[的图片](https://hub.docker.com/r/t3chflicks/aws-cfn)供他人使用。

![](img/ab88159faa978001759e62fc3a23a987.png)

照片由[安德里亚斯·瓦格纳](https://unsplash.com/@waguluz_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

使用它，我们现在可以像这样部署 AWS 机器:

```
kind: pipeline
name: commitPipelinetrigger:
  event:
    - pushplatform:
  os: linux
  arch: armsteps:
  - name: validate-seeder-infrastructure
    image: registry.<domain>/aws-cfn
    settings:
      mode: validate
      template: infrastructure.yml
    environment:
      AWS_ACCESS_KEY_ID:
        from_secret: aws_id
      AWS_SECRET_ACCESS_KEY:
        from_secret: aws_key
```

部署完成后，我们可以通过在. drone.yml 中指定架构或删除任何架构声明来运行 x86 构建，因为 x86 是默认的。

我还在这里重建了一个 AWS-CLI 映像。

我们已经介绍了管道的部署部分，在本系列的下一篇文章中，我们将介绍监控方法并讨论一些问题。在这里查看。

*内容:* [*1*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-1-4-336ed07a6ec0) *，* [*2*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-2-4-7be3e3c292c) *，(* [*3*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-3-4-5f61c5245934) *)，* [*4*](/@t3chflicks/home-devops-pipeline-a-junior-engineers-tale-4-4-5db7c1610e3e)

> [🔗在 Github 上获取 Home DevOps 管道代码📔](https://github.com/sk-t3ch/home-repo)

# 感谢阅读

我希望你喜欢这篇文章。如果你喜欢这种风格，可以去看看[T3chFlicks.org](https://t3chflicks.org/Projects/home-devops-pipeline)了解更多专注于科技的教育内容( [YouTube](https://www.youtube.com/channel/UC0eSD-tdiJMI5GQTkMmZ-6w) 、 [Instagram](https://www.instagram.com/t3chflicks/) 、[脸书](https://www.facebook.com/t3chflicks)、 [Twitter](https://twitter.com/t3chflicks) )。