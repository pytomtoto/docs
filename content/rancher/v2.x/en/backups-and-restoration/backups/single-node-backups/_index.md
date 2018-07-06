---
title: 单节点备份
weight: 25
---

在完成Rancher的单节点安装后，或者在升级到更新版本的Rancher之前，请创建当前安装的备份。如果您在升级时遇到问题，请将此备份用作Rancher安装的恢复点。

>**Prerequisite:** 打开Rancher并记下浏览器左下角显示的版本号（例如：v2.0.0）,在备份过程中需要这个号码。

1. Stop the container currently running Rancher Server. Replace `<RANCHER_CONTAINER_ID>` with the ID of your Rancher container.

    ```
docker stop <RANCHER_CONTAINER_ID>
    ```

    >**Tip:** You can obtain the ID for your Rancher container by entering the following command: `docker ps`.

2. <a id="backup"></a>Create a backup container. This container backs up the data from your current Rancher Server, which you can use as a recovery point.

    - Replace `<RANCHER_CONTAINER_ID>` with the same ID from the previous step.
    - Replace `<RANCHER_VERSION>` and `<RANCHER_CONTAINER_TAG>` with the version of Rancher that you are currently running, as mentioned in the  **Prerequisite** above.

    ```
docker create --volumes-from <RANCHER_CONTAINER_ID> \
--name rancher-backup-<RANCHER_VERSION> rancher/rancher:<RANCHER_CONTAINER_TAG>
    ```

3. Restart Rancher Server. Replace `<RANCHER_CONTAINER_ID>` with the ID of your Rancher container.

    ```
docker start <RANCHER_CONTAINER_ID>
    ```

**Result:** A backup of your Rancher Server is created. If you ever need to restore your backup, see [Restoring Backups: Single Node Installs]({{< baseurl >}}/rancher/v2.x/en/upgrades/restorations/single-node-restoration).