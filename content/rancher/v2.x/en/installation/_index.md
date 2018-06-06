---
title: 安装Rancher
weight: 50
---
本章节包含在开发和生产环境中安装Rancher的说明。

从以下安装选择中选择：

- [单节点安装]({{< baseurl >}}/rancher/v2.x/cn/installation/single-node-install)

	在这个简单的安装场景中，您可以在单个Linux主机上安装RANCHER。
	
- [Single Node Installation with External Loadbalancer]({{< baseurl >}}/rancher/v2.x/cn/installation/single-node-install-external-lb)

	In this scenario, you install Rancher on a single Linux host and access it using an external loadbalancer/proxy.

	In this scenario, you install Rancher on a single Linux host and access it using an external load balancer/proxy.

-  [High Availability Installation]({{< baseurl >}}/rancher/v2.x/en/installation/ha-server-install/)

 	This install scenario creates a new Kubernetes cluster dedicated to running Rancher Server in a high-availabilty (HA) configuration.

-  [High Availability Installation with External Load Balancer]({{< baseurl >}}/rancher/v2.x/en/installation/ha-server-install-external-lb)

 	This install scenario creates a new Kubernetes cluster dedicated to running Rancher Server in a high-availabilty (HA) configuration. A load balancer is placed in front of the HA configuration.

-  [Air Gap Installation]({{< baseurl >}}/rancher/v2.x/en/installation/air-gap-installation/)

 	We also have instructions for a more specialized use case where you install Rancher Server in an environment without an Internet connection.

This section also includes help content for Rancher configuration and maintenance.

-  [Backups and Restoration]({{< baseurl >}}/rancher/v2.x/en/installation/backups-and-restoration/)

 	This page lists the ports you must open to operate Rancher.

-  [Port Requirements]({{< baseurl >}}/rancher/v2.x/en/installation/references/)

 	This page lists the ports you must open to operate Rancher.

-  [Rancher HTTP Proxy Configuration]({{< baseurl >}}/rancher/v2.x/en/installation/proxy-configuration/)

	If your Rancher installation runs behind a proxy, this page provides information on how to configure Rancher for your proxy.
