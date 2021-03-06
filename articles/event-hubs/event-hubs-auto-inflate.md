---
title: "自动增加 Azure 事件中心吞吐量单位 |Microsoft Docs"
description: "在命名空间上启用自动膨胀，自动增加吞吐量单位"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: shvija;sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: b085091ea7bfd601efb0eee84144ddd091422d6e
ms.contentlocale: zh-cn
ms.lasthandoff: 06/13/2017

---

# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>自动增加 Azure 事件中心吞吐量单位

## <a name="overview"></a>概述

Azure 事件中心是高度可缩放的数据流式处理平台。 因此，事件中心客户通常会在加入该服务后提高自己的使用量。 要提高使用量需要增加预先确定的吞吐量单位 (TU)，从而缩放事件中心和处理更大的传输速率。 事件中心的自动膨胀功能可自动增加 TU 数量，以便满足使用量需求。 增加 TU 可防止出现限制情况，这些情况包括：

* 数据入口速率超过设置 TU。
* 数据出口请求速率超过设置 TU。

## <a name="how-auto-inflate-works"></a>自动膨胀的工作原理

事件中心流量由吞吐量单位控制。 单个 TU 允许 1 MB 入口量/秒（是出口量的两倍）。 标准事件中心可以配置 1-20 个吞吐量单位。 利用自动膨胀，可从所需吞吐量单位的最小值开始。 然后此功能会自动将所需吞吐量单位增加到最大值，具体取决于增加的流量。 自动膨胀具有以下优势：

- 高效的缩放机制，可从少量开始并随流量增长而增加。
- 自动增加到指定上限，没有限制问题。
- 更好地控制缩放，你可控制缩放的时间和程度。

## <a name="enable-auto-inflate-on-a-namespace"></a>在命名空间上启用自动膨胀

可使用下列方法之一在命名空间上启用或禁用自动膨胀：

1. [Azure 门户](https://portal.azure.com)。
2. Azure Resource Manager 模板。

### <a name="enable-auto-inflate-through-the-portal"></a>通过门户启用自动膨胀

创建事件中心命名空间时，可在命名空间上启用自动膨胀功能：
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

启用此选项后，可从少量吞吐量单位开始并随所需使用量的增长而增加。 膨胀的上限不会影响定价，定价取决于每小时使用的 TU 量。

还可以使用门户中“设置”边栏选项卡上的“缩放”选项启用自动膨胀：
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>使用 Azure Resource Manager 模板启用自动膨胀

可在 Azure Resource Manager 模板部署期间启用自动膨胀。 例如，将 `isAutoInflateEnabled` 属性设置为“true”并将 `maximumThroughputUnits` 设置为 10。

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

有关完整的模板，请参阅 GitHub 上的[创建事件中心命名空间和启用膨胀模板](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate)。

## <a name="next-steps"></a>后续步骤

访问以下链接可以了解有关事件中心的详细信息：

* [事件中心概述](event-hubs-what-is-event-hubs.md)
* [创建事件中心](event-hubs-create.md)

