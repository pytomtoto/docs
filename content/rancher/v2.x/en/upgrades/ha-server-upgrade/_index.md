---
title: HA升级
weight: 1020
---

>**先决条件:**
>
>- 创建[一次性etcd快照]({{< baseurl >}}/rancher/v2.x/cn/installation/backups-and-restoration/ha-backup-and-restoration). 如果在升级期间出现问题，此快照是一个还原点。
>- 在主机或者远程访问的笔记本上安装[kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)命令行工具
>- 确认系统存在以下路径：~/.kube/。如果没有，请自行创建。
>- 复制[Rancher Server安装]({{< baseurl >}}/rancher/v2.x/cn/installation/ha-server-install#part-11-backup-kube-config-rancher-cluster-yml)后自动生成的配置文件(`kube_config_rancher-cluster.yml`)副本到`~/.kube/` 目录.

1. 在安装了kubectl命令行工具的电脑上打开终端；

2. 输入以下命令：

    ```bash
    kubectl --kubeconfig=kube_config-rancher-cluster.yml set image deployment/cattle cattle-server=rancher/rancher:<VERSION_TAG> -n cattle-system
    ```
    替换`<VERSION_TAG>`为想要升级到的版本，可用镜像版本，可查阅[DockerHub](https://hub.docker.com/r/rancher/rancher/tags/)。请勿使用后缀为rc或者为master的镜像，具体详情请查看[版本标签]({{< baseurl >}}/rancher/v2.x/cn/installation/server-tags/)。

3. 登录Rancher UI,通过检查浏览器窗口左下角显示的版本，确认是否升级成功。
