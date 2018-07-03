---
title: 架构设计
weight: 1
---
#

本节介绍Rancher如何与Docker和Kubernetes两种j技术进行交互。

## Docker

Docker是容器打包和runtime标准。开发人员从Dockerfiles构建容器镜像，并从Docker镜像仓库中分发容器镜像。[Docker Hub](http://hub.docker.com)是最受欢迎的公共镜像仓库，许多组织还设置私有Docker镜像仓库。Docker主要用于管理各个节点上的容器。

>**Note:** 虽然Rancher 1.6支持Docker Swarm集群技术，但由于Rancher 2.0基于Kubernetes调度引擎，所以Rancher 2.0不再支持Docker Swarm。

## Kubernetes

Kubernetes已成为容器集群管理标准，通过YAML文件来管理配置应用程序容器和其他资源。Kubernetes执行诸如调度，扩展，服务发现，健康检查，密文管理和配置管理等功能。

一个Kubernetes集群由多个节点组成:

- **etcd database**

  通常在一个节点上运行一个etcd实例服务，但生产环境上，建议通过3个或5个(奇数)以上的节点来创建ETCD HA配置。

- **Master nodes**

  主节点是无状态的，用于运行API Server，调度服务和控制器服务。

- **Worker nodes**

  工作负载在工作节点上运行。
  
  默认情况下Master节点也会有工作负载调度上去， 

## Rancher

The majority of Rancher 2.0 software runs on the Rancher Server.  Rancher Server includes all the software components used to manage the entire Rancher deployment.

The figure below illustrates the high-level architecture of Rancher 2.0. The figure depicts a Rancher Server installation that manages two Kubernetes clusters: one created by RKE and another created by GKE.

![Architecture]({{< baseurl >}}/img/rancher/rancher-architecture.png)

In this section we describe the functionalities of each Rancher server components.

### Rancher API服务器

Rancher API server is built on top of an embedded Kubernetes API server and etcd database. It implements the following functionalities:

- **Rancher API服务器**

  Rancher API server manages user identities that correspond to external authentication providers like Active Directory or GitHub.
- **认证授权**

  Rancher API server manages access control and security policies.
- **项目**

  A _project_ is a group of multiple namespaces and access control policies within a cluster.
- **节点**

  Rancher API server tracks identities of all the nodes in all clusters.

### 集群控制和Agent

The cluster controller and cluster agents implement the business logic required to manage Kubernetes clusters.

- The _cluster controller_ implements the logic required for the global Rancher install. It performs the following actions:

- Configuration of access control policies to clusters and projects.
- Provisioning of clusters by calling:
- The required Docker machine drivers.
- Kubernetes engines like RKE and GKE.
- A separate _cluster agent_ instance implements the logic required for the corresponding cluster. It performs the following activities:
- Workload Management, such as pod creation and deployment within each cluster.
- Application of the roles and bindings defined in each cluster's global policies.
- Communication between clusters and Rancher Server: events, stats, node info, and health.

### 认证代理

The _authentication proxy_ forwards all Kubernetes API calls. It integrates with authentication services like local authentication, Active Directory, and GitHub. On every Kubernetes API call, the authentication proxy authenticates the caller and sets the proper Kubernetes impersonation headers before forwarding the call to Kubernetes masters. Rancher communicates with Kubernetes clusters using a service account.
