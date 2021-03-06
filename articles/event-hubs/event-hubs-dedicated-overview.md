---
title: "专用 Azure 事件中心容量概述 | Microsoft Docs"
description: "专用 Microsoft Azure 事件中心容量概述。"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: sethm;babanisa
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: db8b119178de0e565b2064e9a52d5e9989d60d38
ms.contentlocale: zh-cn
ms.lasthandoff: 08/30/2017

---

# <a name="overview-of-event-hubs-dedicated"></a>专用事件中心概述

针对需求最严苛的客户，专用事件中心容量提供单个租户部署。 完整规模的 Azure 事件中心可每秒传入超过两百万个事件或每秒高达 2 GB 的遥测，并具有完全持久的存储和次秒级的延迟。 通过在相同系统上实时和批处理，它还实现了集成的解决方案。 借助包含在产品中的[事件中心捕获](event-hubs-capture-overview.md)功能，单个流可以同时支持实时和基于批处理的管道，从而降低解决方案的复杂性。

下表比较了事件中心的各可用服务层。 不同于标准事件中心中大部分功能的使用定价，专用事件中心产品每月价格是固定的。 专用层提供标准计划的功能，但具有企业规模容量，以满足客户的工作负荷需求。 

| 功能 | 标准 | 专用 |
| --- |:---:|:---:|:---:|
| 入口事件 | 按每百万个事件支付 | 附送 |
| 吞吐量单位（传入为 1 MB/秒，传出为 2 MB/秒） | 按每小时支付 | 附送 |
| 消息大小 | 256 KB | 1 MB |
| 发布者策略 | 是 | 是 |   
| 使用者组 | 20 | 20 |
| 消息重播 | 是 | 是 |
| 最大吞吐量单位 | 20（可灵活调整至 100）   | 1 CU≈200 |
| 中转连接 | 包括 1000 | 包括 100 K |
| 其他中转连接 | 是 | 是 |
| 消息保留 | 包括 1 天 | 包括最长 7 天 |
| 捕获 | 按每小时支付 | 附送 |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>专用事件中心容量的优点

使用专用事件中心具有以下优点：

* 单个租户托管，免除来自其他租户的干扰。
* 与标准事件中心的 256 KB 相比，消息大小增至 1 MB。
* 每次可重复性能。
* 有保障的容量，满足迸发需求。
* 容量单位 (CU) 可在 1 至 8 之间缩放 - 提供每秒高达两百万个入口事件。
  * 容量单位 (CU) 管理专用事件中心的规模，其中，每个 CU 约可提供相当于 200 个吞吐量单位 (TU)。
* 零维护：由我们负责管理负载均衡、操作系统更新、安全修补程序及分区。
* 固定的月度定价。

专用事件中心还会删除一些标准产品的吞吐量限制。 基本层的吞吐量单位可达每秒 1000 个事件，或者每个 TU 每秒 1 MB 的流入量和两倍的流出量。 专用规模产品对入口和出口事件计数不设限制。 这些限制仅由购买的事件中心处理容量管理。

该服务针对最大的遥测用户，也可提供给具有企业协议的客户。

## <a name="how-to-onboard"></a>如何加入

专用事件中心平台通过企业协议提供，它具有不同大小的 CU。 每个 CU 提供约等于 200 吞吐量计价单位。 通过添加或删除 CU，可以在一个月内随时扩展或缩小容量，满足自身需求。 专用计划独一无二，用户可从事件中心产品团队处获得适合自己的灵活部署，提供一种亲身实践操作体验。 

## <a name="next-steps"></a>后续步骤
请与 Microsoft 销售代表或 Microsoft 支持部门联系，以获其他关于事件中心专用容量的详细信息。 还可访问以下链接，了解有关事件中心的详细信息：

有关定价的详细信息，请访问以下链接：

- [专用事件中心定价](https://azure.microsoft.com/pricing/details/event-hubs/)。 还可以联系 Microsoft 销售代表或 Microsoft 支持部门，获取关于专用事件中心容量的其他详细信息。
- [事件中心常见问题解答](event-hubs-faq.md)中包含了定价信息并解答了一些有关事件中心的常见问题。 

