---
title: "流分析作业的可视化和故障排除 | Microsoft 文档"
description: "了解如何使用诊断关系图功能对自助服务的流分析作业管道的故障排除进行可视化。"
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 820b73a5dbf9bb108e189313cf6ee2b924ab04c7
ms.contentlocale: zh-cn
ms.lasthandoff: 08/29/2017

---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>流分析作业的可视化和故障排除
与其他基于云的技术一样，在流分析中，故障排除有时需要深入了解作业没有生成预期的输出（或者是该问题的任何输出）的原因。 考虑到这一点，流分析提供了可视化流作业的功能。 该功能作为建模工具使用起来也很方便，并且对需要其工作文档的人员来说具有附带好处。

可在可视化面板中看到输入以及正在执行的查询和配置的所有输出。 连接或配置问题变得更加明显，对配置的可视化表示形式也很有用。

## <a name="using-the-diagnosis-diagram-tool"></a>使用诊断关系图工具
要访问此可视化工具，只需单击流分析作业的“设置”区域中的“诊断关系图”按钮。

![stream-analytics-troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

每个输入和输出都进行了颜色编码以表示组件的当前状态，如下所示。

![stream-analytics-troubleshoot-visualization-input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

当用户想要查看中间查询步骤，以了解作业内的数据流模式时，该可视化工具提供了将查询细分为其组件步骤的视图和流序列。 单击每个查询步骤会在查询编辑窗格中显示相应的部分，如图所示。 

![stream-analytics-troubleshoot-visualization-intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>后续步骤
* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)


