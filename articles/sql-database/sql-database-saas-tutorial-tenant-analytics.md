---
title: "针对多个 Azure SQL 数据库运行分析查询 | Microsoft Docs"
description: "将租户数据库中的数据提取到分析数据库以供离线分析"
keywords: "sql 数据库教程"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 4e32407d5f321198358e07980907c3420aaf56c6
ms.contentlocale: zh-cn
ms.lasthandoff: 06/17/2017

---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>将租户数据库中的数据提取到分析数据库以供离线分析

在本教程中，将使用弹性作业对每个租户数据库运行查询。 此作业会提取门票销售数据，并将数据加载到分析数据库（或数据仓库）中，以供分析。 然后查询分析数据库，以便从所有租户的日常运行数据中提取见解。


本教程将介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建租户分析数据库
> * 创建一个计划的作业，以便检索数据并填充分析数据库

若要完成本教程，请确保满足以下先决条件：

* Wingtip SaaS 应用已部署。 若要在五分钟内进行部署，请参阅[部署和浏览 Wingtip SaaS 应用程序](sql-database-saas-tutorial.md)
* Azure PowerShell 已安装。 有关详细信息，请参阅 [Azure PowerShell 入门](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* 已安装最新版的 SQL Server Management Studio (SSMS)。 [下载并安装 SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>租户运行分析模式

SaaS 应用程序提供的大好机会之一是使用存储在云中的大量租户数据。 利用该数据了解应用程序和租户的运行和使用情况。 该数据可指导应用和平台的功能开发、可用性改进和其他投资。 在单个多租户数据库中访问此数据很简单，但当数据大规模分布在可能数千个数据库中时便不那么容易了。 访问该数据的一个方法是使用弹性作业，以便在输出数据库和表中捕获执行作业时生成的返回结果式查询的结果。

## <a name="get-the-wingtip-application-scripts"></a>获取 Wingtip 应用程序脚本

Wingtip SaaS 脚本和应用程序源代码可在 [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) GitHub 存储库中找到。 [下载 Wingtip SaaS 脚本的步骤](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts)。

## <a name="deploy-a-database-for-tenant-analytics-results"></a>部署租户分析结果的数据库

本教程要求部署一个数据库，用于捕获以作业方式执行脚本后生成的结果，这些脚本包含返回结果式查询。 为此，请创建名为 tenantanalytics 的数据库。

1. 在 *PowerShell ISE* 中打开 …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* 并设置以下值：
   * **$DemoScenario** = **2** *部署运行分析数据库*
1. 按 **F5** 运行演示脚本（用于调用 *Deploy-TenantAnalyticsDB.ps1* 脚本），以便创建租户分析数据库。

## <a name="create-some-data-for-the-demo"></a>创建用于演示的一些数据

1. 在 *PowerShell ISE* 中打开 …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* 并设置以下值：
   * **$DemoScenario** = **1** *购买在所有地点举行的活动的票证*
1. 按 **F5** 运行脚本并创建票证购买历史记录。


## <a name="create-a-scheduled-job-to-retrieve-tenant-analytics-about-ticket-purchases"></a>创建计划的作业，以便检索有关票证购买的租户分析

此脚本创建一个作业，用于检索来自所有租户的购票信息。 聚合成单个表以后，即可获得各租户购票模式的相关指标，这些指标包含大量的见解。

1. 打开 SSMS 并连接到 catalog-&lt;user&gt;.database.windows.net 服务器
1. 打开 ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. 修改 &lt;User&gt;，使用在部署脚本 **sp\_add\_target\_group\_member** 和 **sp\_add\_jobstep** 顶部的 Wingtip SaaS 应用时使用过的用户名
1. 通过右键单击选择“连接”，然后连接到 catalog-&lt;User&gt;.database.windows.net 服务器（如果尚未连接）
1. 确保已连接到 **jobaccount** 数据库，然后按 **F5** 运行该脚本

* **sp\_add\_target\_group** 创建目标组名称 *TenantGroup*，现在需添加目标成员。
* **sp\_add\_target\_group\_member** 添加 *server* 目标成员类型，该类型认定在作业执行时位于该服务器（请注意，这是包含租户数据库的 customer1-&lt;User&gt; 服务器）中的所有数据库都应包括在作业中。
* **sp\_add\_job** 创建新的按周计划的作业，其名称为“来自所有租户的购票”
* **sp\_add\_jobstep** 创建的作业步骤包含 T-SQL 命令文本，该文本用于检索来自所有租户的购票信息，并将返回的结果集复制到名为 *AllTicketsPurchasesfromAllTenants* 的表中
* 脚本中的剩余视图显示存在的对象以及监视作业执行情况。 在用于监视状态的“生命周期”列中查看状态值。 状态为“已成功”表明作业已成功地在所有租户数据库以及两个包含引用表的其他数据库上完成。

成功运行该脚本生成的结果应类似：

![结果](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>创建一个作业，用于检索来自所有租户的购票情况的汇总计数

此脚本创建一个作业，用于检索来自所有租户的所有购票信息的汇总。

1. 打开 SSMS 并连接到 *catalog-&lt;User&gt;.database.windows.net* 服务器
1. 打开 …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql* 文件
1. 修改 &lt;User&gt;，使用在脚本（位于 **sp\_add\_jobstep** 存储过程中）中部署 Wingtip SaaS 应用时使用过的用户名
1. 通过右键单击选择“连接”，然后连接到 catalog-&lt;User&gt;.database.windows.net 服务器（如果尚未连接）
1. 确保已连接到 **tenantanalytics** 数据库，然后按 **F5** 运行该脚本

成功运行该脚本生成的结果应类似：

![结果](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job** 创建一个新的按周计划的作业，其名称为“ResultsTicketsOrders”

* **sp\_add\_jobstep** 创建的作业步骤包含 T-SQL 命令文本，该文本用于检索来自所有租户的购票信息，并将返回的结果集复制到名为 CountofTicketOrders 的表中

* 脚本中的剩余视图显示存在的对象以及监视作业执行情况。 在用于监视状态的“生命周期”列中查看状态值。 状态为“已成功”表明作业已成功地在所有租户数据库以及两个包含引用表的其他数据库上完成。


## <a name="next-steps"></a>后续步骤

本教程介绍了如何：

> [!div class="checklist"]
> * 部署租户分析数据库
> * 创建计划的作业，以便跨租户检索分析数据

祝贺你！

## <a name="additional-resources"></a>其他资源

* 其他[基于 Wingtip SaaS 应用程序编写的教程](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [弹性作业](sql-database-elastic-jobs-overview.md)

