---
title: "接收有关服务通知的活动日志警报 | Microsoft 文档"
description: "在 Azure 服务发生时，通过短信、电子邮件或 webhook 接收通知。"
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.contentlocale: zh-cn
ms.lasthandoff: 08/24/2017

---
# <a name="create-activity-log-alerts-on-service-notifications"></a>创建有关服务通知的活动日志警报
## <a name="overview"></a>概述
本文演示如何使用 Azure 门户设置活动日志警报，用于通知服务运行状况。  

当 Azure 将服务运行状况通知发送到 Azure 订阅时，你可以收到警报。 可以基于以下内容配置警报：

- 服务运行状况通知的类别（事件、维护、信息等）。
- 受影响的服务。
- 受影响的区域。
- 通知的状态（活动与已解析）。
- 通知的级别（信息性、警告、错误）。

还可以配置向其发送警报的人员：

- 选择现有操作组。
- 创建新操作组（可以用于将来的警报）。

若要了解有关操作组的详细信息，请参阅[创建和管理操作组](monitoring-action-groups.md)。

有关如何使用 Azure 资源管理器模板配置服务运行状况通知警报的信息，请参阅[资源管理器模板](monitoring-create-activity-log-alerts-with-resource-manager-template.md)。

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>使用 Azure 门户为新操作组创建有关服务运行状况通知的警报
1. 在[门户](https://portal.azure.com)中，选择“监视器”。

    ![“监视”服务](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. 在“活动日志”部分，选择“警报”。

    ![“警报”选项卡](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. 选择“添加活动日志警报”，并填写字段。

    ![“添加活动日志警报”命令](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. 在“活动日志警报名称”框中输入一个名称，然后提供“说明”。

    ![“添加活动日志警报”对话框](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. “订阅”框会自动填充当前订阅。 此订阅用于保存活动日志警报。 警报资源部署到此订阅，并在其中监视活动日志事件。

6. 选择在其中创建警报资源的“资源组”。 这不是由警报进行监视的资源组。 相反，它是警报资源所在的资源组。

7. 在“事件类别”框中，选择“服务运行状况”。 （可选）选择要接收服务运行状况通知的“服务”、“区域”、“类型”、“状态”和“级别”。

8. 在“报警方式”下，选择“新建”操作组按钮。 在“操作组名称”框中输入名称，然后在“短名称”框中输入名称。 在触发此警报时，将引用已发送通知中的短名称。

9. 通过提供接收方来定义接收方的列表：

    a. 名称：输入接收方的名称、别名或标识符。

    b. 操作类型：选择 SMS、电子邮件或 webhook。

    c. 详细信息：根据选择的操作类型，输入电话号码、电子邮件地址或 Webhook URI。

10. 选择“确定”以创建警报。

在几分钟内，警报将处于活动状态，并根据创建期间指定的条件开始触发。

有关活动日志警报的 webhook 架构的信息，请参阅 [Azure 活动日志警报的 Webhook](monitoring-activity-log-alerts-webhook.md)。

>[!NOTE]
>这些步骤中定义的操作组可以作为现有操作组重复用于所有未来的警报定义。
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>使用 Azure 门户为现有操作组创建有关服务运行状况通知的警报

1. 执行上一节中的步骤 1 至 7 来创建服务运行状况通知。 

2. 在“报警方式”下，选择“现有”操作组按钮。 选择适当的操作组。

3. 选择“确定”以创建警报。

在几分钟内，警报将处于活动状态，并根据创建期间指定的条件开始触发。

## <a name="manage-your-alerts"></a>管理警报

创建警报之后，可在“监视器”边栏选项卡的“警报”部分查看。 选择要管理的警报：

* 编辑它。
* 删除它。
* 如果要暂时停止或恢复接收警报的通知，可“禁用”或“启用”它。

## <a name="next-steps"></a>后续步骤
- 了解[服务运行状况通知](monitoring-service-notifications.md)。
- 了解[通知速率限制](monitoring-alerts-rate-limiting.md)。
- 查看[活动日志警报 webhook 架构](monitoring-activity-log-alerts-webhook.md)。
- 获取[活动日志警报概述](monitoring-overview-alerts.md)，了解如何接收警报。 
- 详细了解[操作组](monitoring-action-groups.md)。

