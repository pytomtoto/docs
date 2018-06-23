---
title: Rancher HTTP代理配置
weight: 500
---

如果你的环境需要通过代理才可以连接互联网，那么需要配置

### 设置主机`http_proxy` 变量

#### Ubuntu

1. Check if `http_proxy` is still defined:

    ```
echo $http_proxy
    ```

    If it is empty, set the variable and store it in your account's environment using the following command:

    ```
echo "export http_proxy=http://<username>:<password>@<proxy url>:<proxy port>/" >> .profile
    ```
2. Logout and then log back in to activate your changes.

### Start Rancher Container with Proxy Information

Ensure that your `http_proxy` environment variable is visible inside of Rancher's Docker container:

```
sudo docker run -d --restart=unless-stopped \
--volumes-from rancher-data -p 80:80 -p 443:443 \
-e HTTP_PROXY=$http_proxy \
-e HTTPS_PROXY=$http_proxy \
-e http_proxy=$http_proxy \
-e https_proxy=$http_proxy \
-e NO_PROXY="localhost,127.0.0.1" \
-e no_proxy="localhost,127.0.0.1" \
rancher/rancher

```
