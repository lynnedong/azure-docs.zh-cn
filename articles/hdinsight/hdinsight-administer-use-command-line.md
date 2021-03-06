---
title: "使用 Azure CLI 管理 Hadoop 群集 - Azure HDInsight | Microsoft Docs"
description: "了解如何使用 Azure 命令行接口管理 Azure HDInsight 中的 Hadoop 群集。 Azure CLI 适用于 Windows、Mac 和 Linux。"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: 4f26c79f-8540-44bd-a470-84722a9e4eca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: cc1b78e430ab46f367fafcc16e1240aa76a9ba64
ms.contentlocale: zh-cn
ms.lasthandoff: 08/29/2017

---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>使用 Azure CLI 管理 HDInsight 中的 Hadoop 群集
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

了解如何使用 [Azure 命令行接口](../cli-install-nodejs.md)管理 Azure HDInsight 中的 Hadoop 群集。 Azure CLI 是以 Node.js 实现的。 可以在支持 Node.js 的任意平台（包括 Windows、Mac 和 Linux）上使用它。 HDInsight 目前不支持 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview)。

本文仅介绍如何将 Azure CLI 与 HDInsight 配合使用。 有关如何使用 Azure CLI 的常规指南，请参阅[安装和配置 Azure CLI][azure-command-line-tools]。

## <a name="prerequisites"></a>先决条件
在开始阅读本文前，必须具有：

* **一个 Azure 订阅**。 请参阅 [获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
* **Azure CLI** - 有关安装和配置信息，请参阅 [安装和配置 Azure CLI](../cli-install-nodejs.md) 。
* 使用以下命令**连接到 Azure**：

    ```cli
    azure login
    ```
  
    有关使用工作或学校帐户进行身份验证的详细信息，请参阅 [从 Azure CLI 连接到 Azure 订阅](../xplat-cli-connect.md)。
* 使用以下命令**切换到 Azure Resource Manager 模式**：
  
    ```cli
    azure config mode arm
    ```

若要获得帮助，请使用 **-h** 开关。  例如：

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>使用 CLI 创建群集
请参阅[使用 Azure CLI 在 HDInsight 中创建群集](hdinsight-hadoop-create-linux-clusters-azure-cli.md)。

## <a name="list-and-show-cluster-details"></a>列出并显示群集详细信息
使用以下命令来列出和显示群集详细信息：

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![群集列表的命令行视图][image-cli-clusterlisting]

## <a name="delete-clusters"></a>删除群集
使用以下命令来删除群集：

```cli
azure hdinsight cluster delete <Cluster Name>
```

还可通过删除包含该群集的资源组来删除群集。 请注意，这会删除组中的所有资源（包括默认存储帐户）。

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>缩放群集
若要更改 Hadoop 群集大小，请执行以下操作：

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>启用/禁用对群集的 HTTP 访问

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="enabledisable-rdp-access-for-a-cluster"></a>启用/禁用对群集的 RDP 访问

```cli
azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
azure hdinsight cluster disable-rdp-access [options] <Cluster Name>
```

## <a name="next-steps"></a>后续步骤
在本文中，已了解如何执行不同的 HDInsight 群集管理任务。 若要了解更多信息，请参阅下列文章：

* [使用 Azure 门户管理 HDInsight][hdinsight-admin-portal]
* [使用 Azure PowerShell 管理 HDInsight][hdinsight-admin-powershell]
* [Azure HDInsight 入门][hdinsight-get-started]
* [如何使用 Azure CLI][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "列出并显示集群"

