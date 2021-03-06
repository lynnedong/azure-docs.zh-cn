---
title: "使用流分析中的操作和服务日志调试 | Microsoft 文档"
description: "如何使用流分析操作日志"
keywords: "服务日志"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 2ee0871752dc2a3da345339fb826340d44ae48d7
ms.contentlocale: zh-cn
ms.lasthandoff: 08/29/2017

---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>使用服务和操作日志调试流分析作业
所有 Azure 服务都向用户提供操作日志消息以记录与管理操作有关的详细信息。 在 Azure 流分析中，此信息用可于调试目的，例如查看作业状态、作业进度和失败消息，跟踪作业在一段时间内的进度（从开始、处理直到输出）。

## <a name="find-operation-logs-in-the-azure-portal"></a>在 Azure 门户中查找操作日志
可以通过两种方式访问操作日志：  

* 流分析作业的仪表板  
* Azure 经典门户中的管理服务  

## <a name="dashboard-of-the-stream-analytics-job"></a>流分析作业的仪表板
指向流分析作业相应日志的链接显示在该作业的“仪表板”选项卡上。如果单击该链接，它会按显示该特定作业的最新日志的方式来设置筛选器。

  ![选择管理服务日志](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>管理服务
若要在 Azure 经典门户中手动导航到流分析和其他服务的操作日志：

1. 单击 [Azure 经典门户](https://manage.windowsazure.com)中的“管理服务”。
2. 为“类型”选择“流分析”，并为“服务名称”选择作业名称。  
   
   ![选择流分析](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>在 Azure 门户中查找审核日志
要在 Azure 门户中查找流分析作业的操作日志，请单击“浏览”，并选择“审核日志”。

  ![Azure 门户 - 选择流分析](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

此操作会打开订阅中的所有资源最近七天的显示事件。  可以通过单击“筛选器”命令进行筛选，以查看特定类型或时间范围的事件。

  ![Azure 门户 - 选择流分析](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>获取日志详细信息
可以按时间范围和状态进行筛选，来查看作业的日志。

在 Azure 门户中，单击窗口底部的“详细信息”按钮可查看选定事件的更多信息。 

  ![选择详细信息](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

在 Azure 门户中，单击一个日志条目可查看它包含的详细事件。

  ![Azure 门户 - 选择详细信息](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

在这里，可以通过单击事件来打开“详细信息”显示。

  ![Azure 门户 - 选择详细信息](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>调试失败的作业
在 Azure 门户中，单击“搜索”图标并键入“failed”。 这样，系统会显示包含失败状态的所有日志。 

  ![调试失败的作业](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

在 Azure 门户中，可以按消息级别进行筛选，以查看“关键”事件。

  ![Azure 门户调试](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

可以选择任何一个失败，并单击“详细信息”以查看有关错误的更多信息。  某些错误消息还提供有关如何解决此问题的信息。 

  ![操作详细信息](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

如果需要联系[支持团队](https://azure.microsoft.com/support/options/)或通过 [MSDN 论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)向团队提供信息，请注明操作详细信息，尤其是**相关性 ID**。 

## <a name="get-help"></a>获取帮助
如需进一步的帮助，请尝试我们的 [Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>后续步骤
* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)


