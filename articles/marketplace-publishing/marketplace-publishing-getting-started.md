---
title: "关于如何创建产品/服务并将其发布到 Marketplace 的概述 | Microsoft Docs"
description: "了解在 Azure Marketplace 中成为经批准的 Microsoft 开发人员，以及创建和部署虚拟机映像、模板、数据服务或开发人员服务所需的步骤"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 8fbf201343f6710d2781a4b56ae54833ed4c06cf
ms.contentlocale: zh-cn
ms.lasthandoff: 05/25/2017

---
# <a name="publish-and-manage-an-offer-in-the-azure-marketplace"></a>在 Azure Marketplace 中发布和管理产品/服务
本文旨在帮助开发人员创建、部署和管理在 Azure Marketplace 中列出的解决方案，以便其他 Azure 客户和合作伙伴可进行购买和使用。

## <a name="marketplace-publishing"></a>Marketplace 发布
作为 Azure 发布商，你可以在 Marketplace 中向其他开发人员、ISV 和 IT 专用人员分发和销售创新解决方案或服务。 可以通过 Marketplace 接触那些需要快速开发基于云的应用程序和移动解决方案的客户。 如果解决方案面向业务用户，则可能需要考虑 [AppSource](http://appsource.microsoft.com) Marketplace。


## <a name="supported-types-of-solutions"></a>支持的解决方案类型
作为一名发布商，首先要做的是定义公司将提供哪种类型的解决方案。 Marketplace 支持以下类型的产品/服务：

|解决方案类型|虚拟机|解决方案模板|
|---|---|---|
|**定义**|预配置的映像，具有一个完整安装的操作系统以及一个或多个应用程序。 虚拟机映像提供在 Azure 虚拟机服务中创建和部署虚拟机所需的信息。|一个数据结构，可以引用一个或多个不同的 Azure 服务，包括由其他销售者发布的服务。 有了它，Azure 订户就可以采用统一且协调的方式部署一个或多个产品/服务。|
|**示例**|作为 Azure 发布商，你已经创建 VM 并使用创新数据库服务验证该 VM。 其他 Azure 订户想要采购该 VM 并将其部署到云服务环境中。|作为 Azure 发布商，你已跨 Azure 捆绑服务集，从而可快速部署负载均衡的云服务、增强安全性和高可用性。 其他 Azure 订户可以采购满足其目标的解决方案模板，从而节省时间。 他们无需手动查找、采购、部署和配置相同的或类似的 Azure 服务。|

> [!NOTE]
> 一些步骤将在不同类型的解决方案之间共享，而另一些步骤则因解决方案类型而异。 本文简要概述了针对任何解决方案类型需要完成的步骤。

## <a name="publish-a-solution"></a>发布解决方案
![提名、注册、发布](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>提名解决方案以便预批准
若要向 Marketplace 发布虚拟机[解决方案](https://createopportunity.azurewebsites.net)，请完成 Microsoft Azure 认证**解决方案提名表**。

>[!NOTE]
> 如果正与合作伙伴客户经理或 DX 合作伙伴经理合作，请要求他们针对 Azure 认证计划推荐你的解决方案。 也可转到 [Microsoft Azure 认证](http://createopportunity.azurewebsites.net)网页，然后填写申请表。 在“Microsoft 发起人联系人”框中输入合作伙伴客户经理或 DX 合作伙伴经理的电子邮件。

如果你符合 [Azure Marketplace 参与策略](http://go.microsoft.com/fwlink/?LinkID=526833)中的资格条件，并且应用程序获得了批准，我们会协助你将解决方案发布到 Marketplace。

### <a name="register-your-account-as-a-microsoft-seller"></a>将你的帐户注册为 Microsoft 卖方
将 Microsoft 帐户注册为 [Microsoft 开发人员帐户](marketplace-publishing-accounts-creation-registration.md)。

### <a name="publish-your-solution"></a>发布解决方案
若要将解决方案发布到 Marketplace，请执行以下步骤：
1. 满足非技术要求。

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 满足[非技术先决条件](marketplace-publishing-pre-requisites.md)。

    b. 满足 [VM 技术先决条件](marketplace-publishing-vm-image-creation-prerequisites.md)。

    c. 满足[解决方案模板技术先决条件](marketplace-publishing-solution-template-creation-prerequisites.md)。

2. 创建产品/服务。

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 创建[虚拟机](marketplace-publishing-vm-image-creation.md)产品/服务。

    b. 创建[解决方案模板](marketplace-publishing-solution-template-creation.md)产品/服务。

3. 创建产品/服务[市场营销内容](marketplace-publishing-push-to-staging.md)。

4. 在过渡环境中测试产品/服务。

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在[过渡环境](marketplace-publishing-vm-image-test-in-staging.md)中测试 VM 产品/服务。

    b. 在[过渡环境](marketplace-publishing-solution-template-test-in-staging.md)中测试解决方案模板产品/服务。

5. 将产品/服务部署到 [Marketplace](marketplace-publishing-push-to-production.md)。


### <a name="create-and-manage-a-virtual-machine-image"></a>创建和管理虚拟机映像
通过以下资源创建和管理 VM 映像：
* 创建[本地](marketplace-publishing-vm-image-creation-on-premise.md) VM 映像。
* [在 Azure 门户中创建运行 Windows 的虚拟机](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。
* [在 Azure 门户中创建运行 Linux 的虚拟机](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
* 排查 [VHD 创建](marketplace-publishing-vm-image-creation-troubleshooting.md)期间遇到的常见问题。

## <a name="manage-your-solution"></a>管理解决方案
通过参阅以下资源来管理解决方案：
* [阅读虚拟机产品/服务后期制作指南](marketplace-publishing-vm-image-post-publishing.md)
* [更新产品/服务或 SKU 的非技术详细信息](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [更新产品/服务或 SKU 的技术详细信息](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [在已列出的产品/服务下添加新 SKU](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [更改已列出 SKU 的数据磁盘计数](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [从 Marketplace 中删除已列出的产品/服务](marketplace-publishing-vm-image-post-publishing.md)
* [从 Marketplace 中删除已列出的 SKU](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [从 Marketplace 中删除已列出的 SKU 的当前版本](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [将定价还原为生产值](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [将计费模型还原为生产值](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [将列出的 SKU 的可见性设置还原为生产值](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)
* [更改云解决方案提供商经销商激励](marketplace-publishing-csp-incentive.md)
* [了解付款报告](marketplace-publishing-report-payout.md)
* [作为发布者获取支持](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>其他资源
[设置 Azure PowerShell](marketplace-publishing-powershell-setup.md)

