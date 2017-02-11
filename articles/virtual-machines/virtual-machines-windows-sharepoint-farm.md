---
title: "创建 SharePoint 服务器场 | Microsoft Docs"
description: "在 Azure 中快速创建新的 SharePoint 2013 或 SharePoint 2016 场。"
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
translationtype: Human Translation
ms.sourcegitcommit: 0f8bc125855bc5a5b67fde5b0b742c73b5da7610
ms.openlocfilehash: 7c0746840dd45229dac491b03925349e6d9a44de


---
# <a name="create-sharepoint-server-farms"></a>创建 SharePoint 服务器场

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a>SharePoint 2013 场
使用 Microsoft Azure 门户应用商店，可以快速创建预配置的 SharePoint Server 2013 场。 需要基本或高可用性 SharePoint farm 用于开发和测试环境，或者将 SharePoint Server 2013 评估作为组织的协作解决方案时，这可以节约很多时间。

> [!NOTE]
> Azure 门户的 Azure 应用商店中的 **SharePoint 服务器场**项已删除。 该项被替换为“SharePoint 2013 非 HA 场”和“SharePoint 2013 HA 场”。
>
>

基本 SharePoint 场在此配置中包括 3 个虚拟机。

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

使用该场配置可为 SharePoint 应用程序开发或你的第一次 SharePoint 2013 评估简化设置。

若要创建基本（三服务器）SharePoint 场，请执行以下操作：

1. 单击[此处](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/)。
2. 单击“部署”。
3. 在“SharePoint 2013 非 HA 场”窗格中，单击“创建”。
4. 在“创建 SharePoint 2013 非 HA 场”窗格的步骤中指定设置，然后单击“创建”。

高可用性 SharePoint 场在此配置中包括 9 个虚拟机。

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

使用该场配置可测试更高的客户端负载、外部 SharePoint 站点的高可用性以及 SharePoint 场的 SQL Server AlwaysOn 可用性组。 还可以将此配置用于高可用性环境中的 SharePoint 应用程序开发。

若要创建高可用性（九服务器）SharePoint 场，请执行以下操作：

1. 单击[此处](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/)。
2. 单击“部署”。
3. 在“SharePoint 2013 HA 场”窗格中，单击“创建”。
4. 在“创建 SharePoint 2013 HA 场”窗格的 7 个步骤中指定设置，然后单击“创建”。

> [!NOTE]
> 在 Azure 免费试用版中无法创建 **SharePoint 2013 非 HA 场** 或 **SharePoint 2013 HA 场**。
>
>

Azure 门户可以在具有面向 Internet 的网络影响力、仅限云的虚拟网络中创建这两种场。 没有站点到站点 VPN 或 ExpressRoute 连接重新连回到你的组织网络。

> [!NOTE]
> 使用 Azure 门户创建基本或高可用性 SharePoint 场时，无法指定现有的资源组。 若要解决此限制，请使用 Azure PowerShell 创建这些场。 有关详细信息，请参阅[使用 Azure PowerShell 创建 SharePoint 2013 开发/测试场](https://technet.microsoft.com/library/mt743093.aspx#powershell)。
>
>

## <a name="sharepoint-2016-farms"></a>SharePoint 2016 场
请参阅[此文](https://technet.microsoft.com/library/mt723354.aspx)，获取有关构建以下单服务器 SharePoint Server 2016 场的说明。

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>管理 SharePoint 场
你可以通过远程桌面连接来管理这些场的服务器。 有关详细信息，请参阅[登录到虚拟机](virtual-machines-windows-hero-tutorial.md#connect-to-the-virtual-machine-and-sign-on)。

在管理中心 SharePoint 站点中，可以配置“我的站点”、SharePoint 应用程序和其他功能。 有关详细信息，请参阅 [Configure SharePoint](http://technet.microsoft.com/library/ee836142.aspx)（配置 SharePoint）。

## <a name="next-steps"></a>后续步骤
* 发现 Azure 基础结构服务中的其他 [SharePoint 配置](https://technet.microsoft.com/library/dn635309.aspx)。



<!--HONumber=Nov16_HO3-->

