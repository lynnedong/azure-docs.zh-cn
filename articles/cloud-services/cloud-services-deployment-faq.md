---
title: "Microsoft Azure 云服务部署常见问题解答 | Microsoft 文档"
description: "本文将介绍一些关于 Microsoft Azure 云服务部署的常见问题解答。"
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 9b788b1d95c821a4bb76cd4dea1d689d36e2f92b
ms.contentlocale: zh-cn
ms.lasthandoff: 06/15/2017


---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure 云服务部署问题：常见问题解答 (FAQ)

本文包括一些关于 [Microsoft Azure 云服务](https://azure.microsoft.com/services/cloud-services)部署问题的常见问题解答。 还可以参阅[云服务 VM 大小页面](cloud-services-sizes-specs.md)，了解大小信息。

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>生产槽中已存在现有部署的情况下，为什么有时会在将云服务部署到过渡槽时出现资源分配错误？
如果某个云服务在任一槽中存在部署，则会将整个云服务固定到特定的群集。 这意味着，如果生产槽中已存在部署，则只能将新的过渡部署分配到生产槽所在的同一群集中。

当云服务所在的群集没有可满足部署请求的足够物理计算资源时，就会出现分配失败的情况。

为了帮助缓解这种分配失败情况，请参阅[云服务分配失败：解决方案](cloud-services-allocation-failures.md#solutions)。

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>为什么缩放或扩展云服务部署有时会导致分配失败？
当部署云服务时，通常会将其固定到特定集群。 这表示缩放/扩展现有云服务必须在同一群集中分配新实例。 如果群集容量趋于饱和或所需的虚拟机大小/类型不可用，则请求可能会失败。

为了帮助缓解这种分配失败情况，请参阅[云服务分配失败：解决方案](cloud-services-allocation-failures.md#solutions)。

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>为什么将云服务部署到地缘组有时会导致分配失败？
进行新的目标为空云服务的部署时，可以通过该区域任何群集中的结构对部署进行分配，除非已将云服务固定到地缘组。 将会在相同的群集中尝试部署到相同的地缘组。 如果群集已接近容量，则请求可能失败。

为了帮助缓解这种分配失败情况，请参阅[云服务分配失败：解决方案](cloud-services-allocation-failures.md#solutions)。

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>为什么更改虚拟机大小或将新的虚拟机添加到现有云服务有时会导致分配失败？
数据中心中的集群可能具有不同的计算机类型配置（例如 A 系列、Av2 系列、D 系列、Dv2 系列、G 系列、H 系列等）。 但并不是所有的群集都必须拥有所有类型的虚拟机。 例如，如果尝试将 D 系列虚拟机添加到仅部署在 A 系列群集中的云服务，则会出现分配失败。 尝试更改 VM SKU 大小（例如，从 A 系列切换到 D 系列）也会导致这种情况的发生。

为了帮助缓解这种分配失败情况，请参阅[云服务分配失败：解决方案](cloud-services-allocation-failures.md#solutions)。

要查看你所在区域的可用大小，请参阅 [Microsoft Azure：产品区域可用性](https://azure.microsoft.com/regions/services)。

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>为什么我的订阅或服务的限制/配额/约束有时会导致部署云服务失败？
如果需要分配的资源超过服务所在区域/数据中心级别允许的默认或最大配额，则云服务部署可能会失败。 有关详细信息，请参阅[云服务限制](../azure-subscription-service-limits.md#cloud-services-limits)。

你还可以在门户网站上跟踪订阅的当前使用情况/配额：Azure门户 => 订阅 => \<相应订阅> =>“使用情况 + 配额”。

资源使用情况/相关消耗信息也可以通过 Azure 计费 API 检索。 请参阅 [Azure 资源使用状况 API（预览）](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview)。

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>如何在不重新部署已部署云服务虚拟机的情况下更改其大小？
你无法在不重新部署已部署云服务虚拟机的情况下更改其大小。 虚拟机大小内置在 CSDEF 中，只能通过重新部署进行更新。

有关详细信息，请参阅[如何更新云服务](cloud-services-update-azure-service.md)。

 

