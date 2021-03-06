---
title: "Log Analytics 新日志搜索常见问题解答 | Microsoft Docs"
description: "本文提供有关将 Log Analytics 升级到新的查询语言的常见问题解答。"
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 507136beef9718dc6a7f42a4b84f8030d4a60563
ms.contentlocale: zh-cn
ms.lasthandoff: 09/07/2017

---

# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Log Analytics 新日志搜索常见问题解答和已知问题

本文提供包括有关[将 Log Analytics 升级到新的查询语言](log-analytics-log-search-upgrade.md)的常见问题解答和已知问题。  决定升级工作区前，请通读全文。


## <a name="alerts"></a>警报

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>问：我有大量警报规则。 升级后，是否需要采用新的语言再次创建这些规则？  
不需要，在升级过程中，警报规则会自动转换为新的搜索语言。  


## <a name="computer-groups"></a>计算机组

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>问：我在尝试使用计算机组时收到了错误。  其语法是否已经更改？
是的，工作区升级后，使用计算机组的语法将更改。  请参阅 [Log Analytics 日志搜索中的计算机组](log-analytics-computer-groups.md)了解详细信息。

### <a name="known-issue-groups-imported-from-active-directory"></a>已知问题：从 Active Directory 导入的组
当前无法创建使用从 Active Directory 导入的计算机组的查询。  此问题未得到解决之前，应采用变通方法，使用导入的 Active Directory 组创建新的计算机组，然后在查询中使用新组。

以下是创建包括导入的 Active Directory 组的新计算机组的示例查询：

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>仪表板

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>问：是否可以在升级后的工作区中使用仪表板？
可以继续使用工作区升级前添加到“我的仪表板”的任何磁贴，但无法编辑这些磁贴或创建新磁贴。  可以继续使用[视图设计器](log-analytics-view-designer.md)创建和编辑视图，也可以在 Azure 门户中创建仪表板。


## <a name="log-searches"></a>日志搜索

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>问：已将搜索保存到升级的工作区之外。 是否可以自动将这些搜索转换到新的搜索语言？
可以在“日志搜索”页使用语言转换器工具转换每个搜索。  目前无法在不升级工作区的情况下自动转换多个搜索。

### <a name="question-why-are-my-query-results-not-sorted"></a>问：为什么我的查询结果未排序？
默认情况下，新的查询语言中不对结果进行排序。  使用 [sort 运算符](https://go.microsoft.com/fwlink/?linkid=856079)以便按一个或多个属性对结果进行排序。

### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>已知问题：列表中的搜索结果可能包括不含数据的属性
列表中的日志搜索结果可能会显示不含数据的属性。  升级之前，这些属性将不包括在内。  此问题将会得到解决，以便不会显示空的属性。

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>已知问题：在图表中选择值时不显示详细结果
升级之前，在图表中选择值时，它将返回与所选值匹配的记录的详细列表。  升级后，仅返回单个汇总行。  当前正在调查此问题。

## <a name="log-search-api"></a>日志搜索 API

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>问：升级后，日志搜索 API 是否会更新？
升级工作区时，旧的[日志搜索 API](log-analytics-log-search-api.md) 将不再工作。  有关新 API 的详细信息，请参阅 [Azure Log Analytics REST API](https://dev.loganalytics.io/)。


## <a name="portals"></a>门户

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>问：应使用新的高级分析门户还是继续使用日志搜索门户？
可以在 [Azure Log Analytics 中用于创建和编辑日志查询的门户](log-analytics-log-search-portals.md)参看两个门户的比较。  两者均具有明显的优势，因此可根据需求做出最佳选择。  在高级分析门户中写入查询，然后将其粘贴到其他位置（如视图设计器），这是一种很常见的做法。  执行该操作时，应了解[需要考虑的问题](log-analytics-log-search-portals.md#advanced-analytics-portal)。


## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>问：PowerBI 集成会发生改变吗？
是的。  升级工作区后，Log Analytics 数据导出到 Power BI 的过程将停止。  将禁用升级前创建的任何现有计划。  升级后，Azure Log Analytics 会将同一平台用作 Application Insights，将 Log Analytics 查询导出到 Power BI 的过程与[将 Application Insights 查询导出到 Power BI 的过程](../application-insights/app-insights-export-power-bi.md#export-analytics-queries)相同。

### <a name="known-issue-power-bi-request-size-limit"></a>已知问题：Power BI 请求大小限制
目前，可导出到 Power BI 的 Log Analytics 查询的大小限制为 8 MB。  很快将增加此限制。


##<a name="powershell-cmdlets"></a>PowerShell cmdlet

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>问：升级后，日志搜索 PowerShell cmdlet 是否会更新？
[Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) 尚未升级到新的搜索语言。  请继续通过此 cmdlet 使用旧的查询语言，即使工作区已经升级。  cmdlet 的更新文档将在其更新后可用。


## <a name="resource-manager-templates"></a>资源管理器模板

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>问：是否可以使用资源管理器模板创建一个已升级的工作区？
是的。  必须使用 2017-03-15-preview 的 API 版本，并且在模板中包括“功能”部分，如以下示例所示。

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>解决方案

### <a name="question-will-my-solutions-continue-to-work"></a>问：解决方案是否会继续有效？
所有解决方案在升级后的工作区中都会继续有效，尽管它们转换为新的查询语言后性能将提高。  某些已知问题已有解决方案，本部分将具体介绍。

### <a name="known-issue-capacity-and-performance-solution"></a>已知问题：容量和性能解决方案
[容量和性能](log-analytics-capacity.md)视图中的某些部分可能为空。  不久便会提供此问题的解决办法。

### <a name="known-issue-device-health-solution"></a>已知问题：设备运行状况解决方案
[设备运行状况解决方案](https://docs.microsoft.com/windows/deployment/update/device-health-monitor)不会在升级后的工作区中收集数据。  不久便会提供此问题的解决办法。

### <a name="known-issue-application-insights-connector"></a>已知问题：Application Insights 连接器
升级后的工作区中当前不支持 [Application Insights 连接器解决方案](log-analytics-app-insights-connector.md)中的观点。  目前正在对此问题的解决办法进行分析。

## <a name="upgrade-process"></a>升级过程

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>问：我有多个工作区。 是否可以同时升级所有工作区？  
不会。  升级适用于单次单个工作区。 当前没有方法可以一次性升级多个工作区。 请注意，所升级的工作区的其他用户也会受影响。  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>问：如果升级，是否将修改工作区中收集的现有日志数据？  
不会。 可用于工作区搜索的日志数据不受升级影响。 已保存的搜索、警报和视图将自动转换为新的搜索语言。  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>问：如果不升级工作区，会发生什么情况？  
旧的日志搜索将在几个月后弃用。 系统会自动升级此时间前仍未升级的工作区。

### <a name="question-i-didnt-choose-to-upgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>问：未选择升级，但工作区仍进行了升级！ 发生了什么情况？  
此工作区的另一个管理员可能升级了该工作区。 请注意，正式发布新语言后，系统会自动升级所有工作区。  

### <a name="question-i-have-upgraded-by-mistake-and-now-need-to-cancel-it-and-restore-everything-back-what-should-i-do"></a>问：不小心进行了升级，现在需要将其取消并还原所有内容。 我该怎么办？  
没问题。  由于升级之前创建了工作区快照，所以可将其还原。 请记住，升级后会丢失已保存的搜索、警报或视图。  若要还原工作区环境，请按照[升级后是否可以还原？](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade)中的步骤进行操作


## <a name="views"></a>视图

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>问：如何使用视图设计器创建新视图？
升级之前，可以使用视图设计器使用主仪表板上的磁贴创建新视图。  工作区升级后，将删除此磁贴。  可以通过单击左侧菜单中的绿色“+”按钮，使用视图设计器在 OMS 门户中创建新视图。

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>已知问题：视图中折线图的“查看全部”选项不产生折线图
单击视图中折线图部分底部的“查看全部”选项时，会出现一个表格。  升级之前，会出现一个折线图。  此问题正在分析当中以进行潜在修改。


## <a name="next-steps"></a>后续步骤

- 详细了解[将工作区升级到新的 Log Analytics 查询语言](log-analytics-log-search-upgrade.md)。

