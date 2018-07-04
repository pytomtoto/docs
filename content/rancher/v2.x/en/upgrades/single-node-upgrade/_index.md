---
title: 单节点升级
weight: 1010
---

>**先决条件:** 打开Rancher web并记下浏览器左下方显示的版本号（例如:`v2.0.0`) ，在升级过程中您需要此号码。

1. 运行以下命令，停止当前运行Rancher Server的容器。

      ```bash
      docker stop <RANCHER_CONTAINER_ID>
      ```
      >**提示：** 您可以输入以下命令获取Rancher容器的ID : `docker ps`

2. 创建当前Rancher Server容器的数据卷容器，以便在升级Rancher Server中使用，命名为rancher-data容器。

    - 替换<RANCHER_CONTAINER_ID>为上一步中的相同容器ID。
    - 替换<RANCHER_CONTAINER_TAG>为您当前正在运行的Rancher版本，如上面的先决条件中所述。

      ```bash
      docker create --volumes-from <RANCHER_CONTAINER_ID> \
      --name rancher-data rancher/rancher:<RANCHER_CONTAINER_TAG>
      ```

3. 创建当前Rancher数据的另一个容器。但是，如果升级失败，此容器是用于还原Rancher Server的备份。命名容器rancher-data-snapshot-<CURRENT_VERSION>。

    - 替换<RANCHER_CONTAINER_ID>为上一步中的相同ID。
    - 替换<CURRENT_VERSION>为当前安装的Rancher版本的标记。
    - 替换<RANCHER_CONTAINER_TAG>为当前正在运行的Rancher版本，如先决条件中所述 。

      ```bash
      docker create --volumes-from <RANCHER_CONTAINER_ID> \
      --name rancher-data-snapshot- <CURRENT_VERSION> rancher / rancher：      <RANCHER_CONTAINER_TAG>
      ```
4. 拉取Rancher的最新镜像。

      ```bash
      docker pull rancher/rancher：最新
      ```

    >注意Air Gap用户：如果您访问此页面以完成Air Gap升级，请在运行docker run命令时将您的私有注册表URL添加到映像。
    >
    >例： <registry.yourdomain.com:port>/rancher/rancher:latest

5. 使用rancher-data容器启动新的Rancher Server 容器。

docker run -d --volumes-from rancher-data --restart = unless-stopped \
-p 80:80 -p 443：443牧场主/牧场主：最新
注意：
即使升级过程看起来比预期的要长，也不要在启动升级后停止升级。停止升级可能会在将来的升级过程中导致数据库迁移错误。 

注意：
升级Rancher Server后，升级后的服务器中的数据现在会保存到rancher-data容器中，以便将来升级。

删除以前的Rancher Server容器。

如果您只停止以前的Rancher Server容器（并且不删除它），则容器可能会在下次服务器重新启动后重新启动。

登录牧场主。通过检查浏览器窗口左下角显示的版本，确认升级成功。

结果： Rancher Server已升级到最新版本。

注意：
如果升级未成功完成，则可以将Rancher Server及其数据回滚到上一个健康状态。有关更多信息，请参阅还原备份 - 单节点安装。