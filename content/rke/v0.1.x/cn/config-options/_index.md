---
title: 配置
weight: 200
---
当配置RKE的cluster.yml时，有大量不同的选项可以配置，这些选项决定了RKE怎样安装K8S.
When setting up your `cluster.yml` for RKE, there are a lot of different options that can be configured to control the behavior of how RKE launches Kubernetes.

有些选项可以在集群配置选项中配置。有几个如下路径配置示例({{< baseurl >}}/rke/v0.1.x/cn/example-yamls/)，示例里包含了所有选项。
There are several options that can be configured in cluster configuration option. There are several [example yamls]({{< baseurl >}}/rke/v0.1.x/cn/example-yamls/) that contain all the options.

配置节点
### Configuring Nodes

节点
* [Nodes]({{< baseurl >}}/rke/v0.1.x/cn/config-options/nodes/)

受支持的docker版本
* [Ignoring unsupported Docker versions](#supported-docker-versions)

私人仓库
* [Private Registries]({{< baseurl >}}/rke/v0.1.x/cn/config-options/private-registries/)

集群级别SSH秘钥路径
* [Cluster Level SSH Key Path](#cluster-level-ssh-key-path)

SSH客户端
* [SSH Agent](#ssh-agent)

堡垒机
* [Bastion Host]({{< baseurl >}}/rke/v0.1.x/cn/config-options/bastion-host/)


配置k8s集群
### Configuring Kubernetes Cluster

集群名称
* [Cluster Name](#cluster-name)

k8s版本
* [Kubernetes Version](#kubernetes-version)

系统镜像
* [System Images]({{< baseurl >}}/rke/v0.1.x/cn/config-options/system-images/)

服务
* [Services]({{< baseurl >}}/rke/v0.1.x/cn/config-options/services/)

扩展参数、监听、环境变量
* [Extra Args and Binds and Environment Variables]({{< baseurl >}}/rke/v0.1.x/cn/config-options/services/services-extras/)

外部ETCD
* [External Etcd]({{< baseurl >}}/rke/v0.1.x/cn/config-options/services/external-etcd/)

认证
* [Authentication]({{< baseurl >}}/rke/v0.1.x/cn/config-options/authentication/)

搜权
* [Authorization]({{< baseurl >}}/rke/v0.1.x/cn/config-options/authorization/)

云支持
* [Cloud Providers]({{< baseurl >}}/rke/v0.1.x/cn/config-options/cloud-providers/)

额外
* [Add-ons]({{< baseurl >}}/rke/v0.1.x/cn/config-options/add-ons/)

	任务超时
  * [Add-ons Jobs Timeout](#add-ons-jobs-timeout)
  
	网络插件
  * [Network Plugins]({{< baseurl >}}/rke/v0.1.x/cn/config-options/add-ons/network-plugins/)
  
	Ingress控制器
  * [Ingress Controller]({{< baseurl >}}/rke/v0.1.x/cn/config-options/add-ons/ingress-controllers/)
  
	定义用户
  * [User-Defined-Add-ons]({{< baseurl >}}/rke/v0.1.x/cn/config-options/add-ons/user-defined-add-ons/)

集群级别选项
## Cluster Level Options

集群名称
### Cluster Name

默认情况下，你的集群名称为lcoal，如果你想要改成其他，你的集群生成的kubeconfig文件中，通过cluster_name设置.
By default, the name of your cluster will be `local`. If you want a different name, you would use the `cluster_name` directive to change the name of your cluster. The name will be set in your cluster's generated kubeconfig file.

```yaml
cluster_name: mycluster
```
受支持的docker版本
### Supported Docker Versions

默认情况下，RKE会检查所有机器上安装的docker版本，如果该版本不被k8s支持，这将会报错。这里列出了受支持的docker版本https://github.com/rancher/rke/blob/master/docker/docker.go#L29。如果你不想检查docker版本，你可以将ignore_docker_version配置为true
By default, RKE will check the installed Docker version on all hosts and fail with an error if the version is not supported by Kubernetes. The list of [supported Docker versions](https://github.com/rancher/rke/blob/master/docker/docker.go#L29) are set specifically for each Kubernetes version. To override this behavior, set this option to `true`.

The default value is `false`.

```yaml
ignore_docker_version: true
```

k8s版本
### Kubernetes Version

你可以选择一个k8s版本用作集群安装。这些选项在Rancher 2.x中都是可用的。默认情况下RKE使用的k8s版本为v1.10.3-rancher2-1，如果没有定义kubernetes_version，RKE将采用默认值。
You can select which version of Kubernetes to install for your cluster. These options are the Kubernetes versions made available in Rancher v2.x. The current default Kubernetes version used by RKE is `v1.10.3-rancher2-1`. If a version is defined in `kubernetes_version` and is not found in this list, the default is used.

 Kubernetes version|
 -----------------|
 v1.10.3-rancher2-1|
 v1.10.1-rancher2-1|
 v1.10.0-rancher1-1|
 v1.9.7-rancher2-1|
 v1.9.5-rancher1-1|
 v1.8.11-rancher2-1|
 v1.8.10-rancher1-1|

<br>

这里有2种方式选择k8s的版本
There are two ways to select a Kubernetes version:
使用k8s镜像，在系统镜像里定义。
- Using the Kubernetes image defined in [system images](#rke-system-images)
使用kubernetes_version配置选项
- Using the configuration option `kubernetes_version`

```yaml
kubernetes_version: "v1.10.3-rancher2-1"
```
如果2者都配置了，系统镜像配置生效。
In case both are defined, the system images configuration will take precedence over `kubernetes_version`.

集群级别SSH秘钥路径
### Cluster Level SSH Key Path

RKE连接主机使用的是SSH，每个节点都会有一个独立的ssh 秘钥路径：nodes选项下的 ssh_key_path。如果你一个ssh 秘钥能访问所有主机，你可以在配置结构顶端配置这个ssh_key_path。
RKE connects to host(s) using `ssh`. Typically, each node will have an independent path for each ssh key, i.e. `ssh_key_path`, in the `nodes` section, but if you have a SSH key that is able to access **all** hosts in your cluster configuration file, you can set the path to that ssh key at the top level. Otherwise, you would set the ssh key path in the [nodes]({{< baseurl >}}/rke/v0.1.x/cn/config-options/nodes/).

如果ssh_key_path同时定义在集群级别和主机级别，主机级别的定义优先。
If ssh key paths are defined at the cluster level and at the node level, the node-level key will take precedence.

```yaml
ssh_key_path: ~/.ssh/test
```

SSH客户端
### SSH Agent

RKE支持使用本地ssh客户端ssh连接配置。这个选项的默认值为false，如果你想使用本地ssh客户端，你需要把ssh_agent_auth设置为true
RKE supports using ssh connection configuration from a local ssh agent. The default value for this option is `false`. If you want to set using a local ssh agent, you would set this to `true`.

```yaml
ssh_agent_auth: true
```
如果你的ssh私钥加密过，你需要添加环境变量配置SSH_AUTH_SOCK
If you want to use an SSH private key with a passphrase, you will need to add your key to `ssh-agent` and have the environment variable `SSH_AUTH_SOCK` configured.

```
$ eval "$(ssh-agent -s)"
Agent pid 3975
$ ssh-add /home/user/.ssh/id_rsa
Enter passphrase for /home/user/.ssh/id_rsa:
Identity added: /home/user/.ssh/id_rsa (/home/user/.ssh/id_rsa)
$ echo $SSH_AUTH_SOCK
/tmp/ssh-118TMqxrXsEx/agent.3974
```
额外任务超时
### Add-ons Job Timeout

你可以在集群启动后定义它，默认任务超时30秒。
You can define [add-ons]({{< baseurl >}}/rke/v0.1.x/cn/config-options/add-ons/) to be deployed after the Kubernetes cluster comes up, which uses Kubernetes [jobs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/). RKE will stop attempting to retrieve the job status after the timeout, which is in seconds. The default timeout value is `30` seconds.

```yaml
addon_job_timeout: 30
```
