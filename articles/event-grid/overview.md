---
title: "Azure 事件网格概述"
description: "介绍 Azure 事件网格及其概念。"
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/11/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: 1868e5fd0427a5e1b1eeed244c80a570a39eb6a9
ms.openlocfilehash: 358c1f4bca2ced207caf599db2fb1453ca8bc41e
ms.contentlocale: zh-cn
ms.lasthandoff: 09/19/2017

---

# <a name="an-introduction-to-azure-event-grid"></a>Azure 事件网格简介

通过 Azure 事件网格，可使用基于事件的体系结构轻松生成应用程序。 你可以选择要订阅的 Azure 资源，并提供要向其发送事件的事件处理程序或 WebHook 终结点。 事件网格包含来自 Azure 服务对事件的内置支持，如存储 blob 和资源组。 事件网格还可以使用自定义主题和自定义 webhook 对应用程序和第三方事件提供自定义支持。 

可以使用筛选器将特定事件路由到不同的终结点，多播到多个终结点，并确保事件可靠传送。 此外，事件网格还提供对自定义和第三方事件的内置支持。

预览版的事件网格支持“westus2”和“westcentralus”区域。 将添加其他区域。

本文将对 Azure 事件网格进行简要概述。 若要开始使用事件网格，请参阅[使用 Azure 事件网格创建和路由自定义事件](custom-event-quickstart.md)。

![事件网格功能模型](./media/overview/event-grid-functional-model.png)

目前，Blob 存储尚未公开用作发布服务器。 必须注册预览版本，才能对存储 blob 事件做出响应。 有关详细信息，请参阅[将 Blob 存储事件路由到自定义 Web 终结点（预览）](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)

## <a name="concepts"></a>概念

在可以开始进行操作的 Azure 事件网格中有五个概念：

* 事件 - 发生了什么。
* 事件源/发布者 - 事件发生的地点。
* 主题 - 其中发布者发送事件的终结点。
* 事件订阅 - 用于路由事件，有时用于多个处理程序的终结点或内置机制。 订阅还用于处理程序，以便智能地筛选传入事件。
* 事件处理程序 - 对事件作出反应的应用或服务。

有关这些概念的详细信息，请参阅 [Azure 事件网格中的概念](concepts.md)。

## <a name="capabilities"></a>功能

下面是 Azure 事件网格中的一些主要功能：

* 简洁性 - 指向并单击从 Azure 资源到任何事件处理程序或终结点的目标事件。
* 高级筛选 - 筛选事件类型或事件发布路径，以确保事件处理程序仅接收相关的事件。
* 扇出 - 订阅到相同事件的多个终结点，以将该事件的副本发送到所需的所有位置。
* 可靠性 - 会以指数退避算法在 24 小时内重试，以确保事件成功传送。
* 按事件支付 - 仅支付事件网格的使用量。
* 高吞吐量 - 通过对每秒数以百万计事件的支持，在事件网格上生成大量工作负荷。
* 内置事件 - 使用资源定义的内置事件快速启动和运行。
* 自定义事件 - 在应用中使用事件网格路由、筛选并可靠地传送自定义事件。

## <a name="built-in-publisher-and-handler-integration"></a>内置发布者和处理程序集成

Azure 使用多项服务提供内置事件支持，包括发布者和处理程序。

### <a name="publishers"></a>发布者

目前，以下 Azure 服务对事件网格的内置发布者提供支持：

* 资源组（管理操作）
* Azure 订阅（管理操作）
* 事件中心
* 自定义主题

本年度将添加其他 Azure 服务。

### <a name="handlers"></a>处理程序

目前，以下 Azure 服务对事件网格的内置处理程序提供支持： 

* Azure Functions
* 逻辑应用
* Azure 自动化
* Webhook
* Microsoft Flow

本年度将添加其他 Azure 服务。

## <a name="what-can-i-do-with-event-grid"></a>使用事件网格可以做什么？

Azure 事件网格提供了多个极大地改进无服务器的功能、运维自动化和集成工作： 

### <a name="serverless-application-architectures"></a>无服务器应用程序体系结构

![无服务器应用程序](./media/overview/serverless_web_app.png)

事件网格将数据源与事件处理程序连接。 例如，使用事件网格在每次向 Blob 存储容器添加新照片时，立即触发无服务器功能运行图像分析。 

### <a name="ops-automation"></a>运维自动化

![运维自动化](./media/overview/Ops_automation.png)

使用事件网格，可以加快自动化，简化策略执行。 例如，事件网格可在创建虚拟机或启动 SQL 数据库时通知 Azure 自动化。 这些事件可用于自动检查服务配置是否符合要求，是否将元数据放入操作工具、标记虚拟机或文件工作项中。

### <a name="application-integration"></a>应用程序集成

![应用程序集成](./media/overview/app_integration.png)

事件网格将应用与其他服务连接。 例如，创建一个自定义主题以将应用的事件数据发送到事件网格，并利用其可靠交付、高级路由和与 Azure 的直接集成。 或者，可使用具有逻辑应用的事件网格来处理任何位置的数据，而无需编写代码。 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a>事件网格与其他 Azure 集成服务有何不同？

事件网格是启用事件驱动、反应编程的事件底板。 它与 Azure 服务深度集成，并可与第三方服务集成。 事件消息包含你需要对服务和应用程序中的更改做出响应的信息。 事件网格不是数据管道，并且不提供已更新的实际对象。

服务总线非常适用于传统的企业应用程序，这些应用程序需要事务、排序、重复检测和即时一致性。 事件网格专为反应模型中的速度、规模、广度和低成本设计。 它非常适合无服务器体系结构。

事件网格补充了逻辑应用和事件中心等其他 Azure 服务。 事件网格触发逻辑应用，以开始其工作流。 事件中心与事件网格协同工作，使你能够对事件中心捕获的事件作出反应，并生成数据入口和转换管道。

## <a name="how-much-does-event-grid-cost"></a>事件网格的费用是多少？

Azure 事件网格使用按事件支付的定价模型，因此，你只需为你所使用的事件付费。

事件网格每百万个操作收取 $0.60（预览版为 $0.30），而每个月的前 100,000 个操作是免费的。 这些操作被定义为事件入口、高级匹配、交付尝试和管理调用。  更多详细信息，请查询[定价页面](https://azure.microsoft.com/pricing/details/event-grid/)。

## <a name="next-steps"></a>后续步骤

* [创建并订阅自定义事件](custom-event-quickstart.md)  
  立即开始使用 Azure 事件网格快速入门，将自己的自定义事件发送到任何终结点。
* [将逻辑应用用作事件处理程序](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  本教程介绍如何使用逻辑应用生成应用，并对事件网格推送的事件作出响应。
* [将大数据流式传输到数据仓库](event-grid-event-hubs-integration.md)  
  本教程介绍如何使用 Azure Functions 将数据从事件中心流式传输到 SQL 数据仓库。
* [事件网格 REST API 参考](/rest/api/eventgrid)  
  介绍有个 Azure 事件网格的更多技术信息，以及管理事件订阅、路由和过滤的参考。
