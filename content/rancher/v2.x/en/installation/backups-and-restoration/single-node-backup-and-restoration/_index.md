---
title: 单节点备份和恢复
weight: 365
---
#

在完成Rancher的单节点安装后，或者在升级到更新版本的Rancher之前，请创建当前安装的备份。如果您在升级时遇到问题，请将此备份用作Rancher安装的恢复点。

## 备份Rancher server数据

>**Prerequisite:** 打开Rancher并记下浏览器左下角显示的版本号（例如：v2.0.0）,在备份过程中需要这个号码。

1.停止当前运行的Rancher服务器容器；

```bash
docker stop <RANCHER_CONTAINER_ID>
```

>**Tip:** 您可以通过输入命令`docker ps`获取Rancher容器ID

2.创建一个备份容器，此容器备份当前Rancher服务器中的数据，您可以将其用作恢复点;

```bash
docker create \
--volumes-from <RANCHER_CONTAINER_ID> \
--name rancher-backup-<RANCHER_VERSION> \
rancher/rancher:<RANCHER_CONTAINER_TAG>
```

- 替换 `<RANCHER_CONTAINER_ID>`为上一步获取到的Rancher容器ID;
- 如上面的 **先决条件** 中所述，替换 `<RANCHER_CONTAINER_TAG>` 和 `<RANCHER_VERSION>`为Rancher的版本号；

3.重启Rancher server

```bash
docker start <RANCHER_CONTAINER_ID>
```

## 恢复 Rancher Server

1.停止当前运行的Rancher容器

```bash
docker stop <RANCHER_CONTAINER_ID>
```

2.通过备份的容器卷来重新运行Rancher server容器；

```bash
docker run -d --restart=unless-stopped \
-p 80:80 -p 443:443 \
--volumes-from rancher-backup-<RANCHER_VERSION> \
rancher/rancher:<CURRENT_RANCHER_VERSION>
```
