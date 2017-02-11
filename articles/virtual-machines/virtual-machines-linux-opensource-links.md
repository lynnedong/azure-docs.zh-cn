---
title: "Azure 上的 Linux 和开源计算 | Microsoft Docs"
description: "列出 Azure 上的 Linux 和开源计算文章，包括基本的 Linux 用法，关于在 Azure 上运行或上载 Linux 映像的一些基本概念，以及关于特定技术或优化的其他内容。"
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: a7e608b5-26ea-41e0-b46b-1a483a257754
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/27/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 680bb7dfb401decc554637c0094a06a0a6260288


---
# <a name="linux-and-open-source-computing-on-azure"></a>Azure 上的 Linux 和开源计算
查找在经典部署模型中创建和管理基于 Linux 的虚拟机所需的所有文档。

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>入门
* [Azure 上的 Linux 简介](virtual-machines-linux-intro-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [使用经典部署模型创建的 Azure 虚拟机的常见问题](virtual-machines-linux-classic-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [关于虚拟机的映像](virtual-machines-linux-classic-about-images.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [上载自己的发行版映像](virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)（以及使用 [Azure 认可的发行版](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)的说明）
* [使用 Azure 经典门户登录到 Linux VM](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="set-up"></a>设置
* [安装 Azure 命令行接口 (Azure CLI)](../xplat-cli-install.md)

## <a name="tutorials"></a>教程
* [在 Azure 中的 Linux 虚拟机上安装 LAMP 堆栈](virtual-machines-linux-create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VM 上的 Ruby on Rails Web 应用程序](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
* [如何：为 AMQP 和服务总线安装 Apache Qpid Proton-C](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>数据库
* [优化 Azure 上的 MySQL 的性能](virtual-machines-linux-classic-optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MySQL 群集](virtual-machines-linux-classic-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [在 Azure 上将 Cassandra 与 Linux 一起运行以及通过 Node.js 对其进行访问](virtual-machines-linux-classic-cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [创建多主的 Mariadb 群集](virtual-machines-linux-classic-mariadb-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="hpc"></a>HPC
* [Azure 的 HPC Pack 群集中的 Linux 计算节点入门](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [在 Azure 中的 Linux 计算节点上使用 Microsoft HPC Pack 运行 NAMD](virtual-machines-linux-classic-hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [设置 Linux RDMA 群集以运行 MPI 应用程序](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="docker"></a>Docker
* [从 Azure 命令行接口 (Azure CLI) 使用 Docker VM 扩展](virtual-machines-linux-classic-cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [在 Azure 门户中使用 Docker VM 扩展](virtual-machines-linux-classic-portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [如何在 Azure 上使用 docker-machine](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="ubuntu"></a>Ubuntu
* [如何：MySQL 群集](virtual-machines-linux-classic-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [如何：Node.js 和 Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="opensuse"></a>OpenSUSE
* [如何：安装和运行 MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="coreos"></a>CoreOS
* [如何：在 Azure 上使用 CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="planning"></a>规划
* [Azure 基础结构服务实施准则](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [选择 Linux 用户名](virtual-machines-linux-usernames.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [如何在经典部署模型中为虚拟机配置可用性集](virtual-machines-linux-classic-configure-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [如何在 Azure VM 上安排计划内的维护](virtual-machines-linux-planned-maintenance-schedule.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [管理虚拟机的可用性](virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure 中 Linux 虚拟机的计划内维护](virtual-machines-linux-planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="deployment"></a>部署
* [创建运行 Linux 的自定义虚拟机](virtual-machines-linux-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [基础知识：捕获 Linux VM 以创建模板](virtual-machines-linux-classic-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [有关未认可分发版的信息](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="management"></a>管理
* [SSH](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [如何为 Linux 重置密码或 SSH 属性](virtual-machines-linux-classic-reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [使用 Root](virtual-machines-linux-use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="azure-resources"></a>Azure 资源
* [Azure Linux 代理](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VM 扩展和功能](virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [将自定义数据注入到 VM 中以用于 Cloud-init](virtual-machines-windows-classic-inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="storage"></a>存储
* [将数据磁盘附加到 Linux VM](virtual-machines-linux-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [从 Linux VM 分离数据磁盘](virtual-machines-linux-classic-detach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [RAID](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>网络
* [如何在 Azure 中的经典虚拟机上设置终结点](virtual-machines-linux-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="troubleshooting"></a>故障排除
* [对于基于 Linux 的 Azure 虚拟机的 Secure Shell (SSH) 连接进行故障排除](virtual-machines-linux-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [排查在 Azure 中新建 Linux 虚拟机时遇到的经典部署问题](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)  
* [排查在 Azure 中重新启动或调整现有 Linux 虚拟机时遇到的经典部署问题](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) 

## <a name="reference"></a>引用
* [Azure 服务管理 (asm) 模式下的 Azure CLI 命令](../virtual-machines-command-line-tools.md)
* [Azure 服务管理 REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)

## <a name="general-links"></a>常规链接
下面是 Microsoft 博客、Technet 页面和外部站点（而非上述 Azure.com 文档）的链接。 由于 Azure 和开源计算都在快速发展，因此几乎可以肯定以下链接都已过时，*尽管*我们将尽最大努力继续添加更新的主题并删除过时的主题。 如果我们有遗漏，请在评论中告诉我们，或者向我们的 [GitHub 存储库](https://github.com/Azure/azure-content/)提交拉取请求。

* [使用 Docker 容器在 Linux 上运行 ASP.NET 5](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
* [如何从 OpenLogic 部署 CentOS VM 映像](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
* [SUSE 更新基础结构](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
* [SUSE Linux Enterprise Server for SAP Cloud Appliance Library](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
* [通过 12 个步骤在 Azure 上构建具有高可用性的 Linux](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
* [使用 Azure CLI、node.js、jhawk 在 Azure 上自动预配 Linux](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
* [Azure 上的 GlusterFS](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
* [在 Azure 中运行 FreeBSD](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
* [轻松部署 FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
* [部署自定义的 FreeBSD 映像](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
* [Kaspersky AV for Linux File Server](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL
* [8 种适用于 Azure 的开源 NoSql 数据库](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
* [Slideshare (MSOpenTech)：在 Azure 上体验 CouchDb](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
* [使用 node.js、CORS 和 Grunt 运行 CouchDB 即服务](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
* [Azure Redis 缓存服务中 Windows 上的 Redis](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
* [宣布推出适用于 Redis 预览版的 ASP.NET 会话状态提供程序](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
* [博客：RavenHQ 目前在 Azure 应用商店中可用了](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>大数据
* [在 Azure Linux VM 上安装 Hadoop](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
* [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>关系数据库
* [Microsoft Azure 中的 MySQL 高可用性体系结构](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
* [使用 ILB 通过 corosync、pg_bouncer 安装 Postgres](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux 高性能计算 (HPC)
* [快速入门模板：启动 SLURM 群集](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm)（和[博客文章](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)）
* [快速入门模板：使用 Linux 计算节点创建 HPC 群集](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>开发、管理和优化
进行开发、管理和优化涉及的方面十分广泛，而且各种情况变化很快，因此应将以下列表看作是学习的起点。

* [视频：Azure 虚拟机：使用 Chef、Puppet 和 Docker 管理 Linux VM](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)
* [使用 CoreOS 和 Weave 自动部署 Kubernetes 群集的完整指南](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
* [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [适用于 Azure 的 Jenkins Slave 插件](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
* [GitHub repo：适用于 Azure 的 Jenkins Storage 插件](https://github.com/jenkinsci/windows-azure-storage-plugin)
* [第三方：适用于 Azure 的 Hudson Slave 插件](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
* [第三方：适用于 Azure 的 Hudson Storage 插件](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [视频：Chef 是什么及其工作原理](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)
* [视频：如何在 Linux VM 上使用 Azure 自动化](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* [博客：如何执行 Powershell DSC for Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
* [GitHub：Docker 客户端 DSC](https://github.com/anweiss/DockerClientDSC)
* [适用于 Azure 的打包程序插件](https://github.com/msopentech/packer-azure)




<!--HONumber=Nov16_HO3-->

