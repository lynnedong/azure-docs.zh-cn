---
title: "SQL Server 存储过程活动"
description: "了解如何使用 SQL Server 存储过程活动从数据工厂管道调用 Azure SQL 数据库或 Azure SQL 数据仓库中的存储过程。"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 1b2514e1e6f39bb3ce9d8a46f4af01835284cdcc
ms.openlocfilehash: 3510b0446cf3c1a7ffb3ff02a4d84d24ead1cae7


---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server 存储过程活动
> [!div class="op_single_selector"]
> [Hive](data-factory-hive-activity.md)  
> [Pig](data-factory-pig-activity.md)  
> [MapReduce](data-factory-map-reduce.md)  
> [Hadoop 流式处理](data-factory-hadoop-streaming-activity.md)
> [机器学习](data-factory-azure-ml-batch-execution-activity.md)
> [存储过程](data-factory-stored-proc-activity.md)
> [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> [.NET 自定义](data-factory-use-custom-activities.md)
>
>

可在数据工厂[管道](data-factory-create-pipelines.md)中使用 SQL Server 存储过程活动以调用以下数据存储之一的存储过程：

* Azure SQL 数据库
* Azure SQL 数据仓库  
* 你的企业或 Azure VM 中的 SQL Server 数据库。 需要在托管数据库的同一计算机上或在单独的计算机上安装数据管理网关，以避免与数据库争用资源。 数据管理网关是一种软件，以安全和托管的方式将托管在 Azure VM 中的本地数据源连接到云服务。 有关数据管理网关的详细信息，请参阅[在本地与云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)一文。

本文基于[数据转换活动](data-factory-data-transformation-activities.md)一文，它概述了数据转换和受支持的转换活动。

## <a name="walkthrough"></a>演练
### <a name="sample-table-and-stored-procedure"></a>示例表和存储过程
1. 在 Azure SQL 数据库中，使用 SQL Server Management Studio 或其他熟悉的工具，创建以下**表**。 Datetimestamp 列是生成相应 ID 的日期和时间。

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    ID 是唯一标识，而 datetimestamp 列是生成相应 ID 的日期和时间。
    ![示例数据](./media/data-factory-stored-proc-activity/sample-data.png)

   > [!NOTE]
   > 此示例使用的是 Azure SQL 数据库，但同样适用于 Azure SQL 数据仓库和 SQL Server 数据库。
   >
   >
2. 创建以下**存储过程**，将数据插入 **sampletable**。

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS

        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

   > [!IMPORTANT]
   > 参数（在本示例中为 DateTime）的**名称**和**大小写**必须与管道/活动 JSON 中指定的参数匹配。 在存储过程定义中，请确保使用 **@** 作为参数的前缀。
   >
   >

### <a name="create-a-data-factory"></a>创建数据工厂
1. 登录到 [Azure 门户](https://portal.azure.com/)。
2. 在左侧菜单中单击“新建”，然后依次单击“智能 + 分析”、“数据工厂”。

    ![新建数据工厂](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. 在“新建数据工厂”边栏选项卡中，输入 **SProcDF** 作为名称。 Azure 数据工厂名称必须**全局唯一**。 必须将你的姓名作为数据工厂的名称前缀，才能成功创建工厂。

   ![新建数据工厂](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. 选择 **Azure 订阅**。
5. 对于**资源组**，请执行以下步骤之一：
   1. 单击“新建”，然后为资源组输入名称。
   2. 单击“使用现有资源组”并选择一个现有的资源组。  
6. 选择数据工厂的**位置**。
7. 选择“固定到仪表板”以便在下次登录时在仪表板上看到数据工厂。
8. 在“新建数据工厂”边栏选项卡中单击“创建”。
9. 此时可在 Azure 门户的“仪表板”中看到所创建的数据工厂。 成功创建数据工厂后，将看到数据工厂页，其中显示了数据工厂的内容。
   ![数据工厂主页](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>创建 Azure SQL 链接服务
创建数据工厂后，可以创建 Azure SQL 链接服务，将 Azure SQL 数据库链接到数据工厂。 此数据库包含 sampletable 和 sp_sample 存储过程。

1. 在 **SProcDF** 的“数据工厂”边栏选项卡中，单击“编写和部署”，启动数据工厂编辑器。
2. 在命令栏上单击“新建数据存储”并选择“Azure SQL 数据库”。 在编辑器中，应会看到用于创建 Azure SQL 链接服务的 JSON 脚本。

   ![新建数据存储](media/data-factory-stored-proc-activity/new-data-store.png)
3. 在 JSON 脚本中，进行以下更改：

   1. 将 **&lt;servername&gt;** 替换为 Azure SQL 数据库服务器的名称。
   2. 将 **&lt;databasename&gt;** 替换为在其中创建表和存储过程的数据库。
   3. 将 **&lt;username@servername&gt;** 替换为有权访问数据库的用户帐户。
   4. 将 **&lt;password&gt;** 替换为用户帐户的密码。

      ![新建数据存储](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. 单击命令栏上的“部署”，部署链接服务。 确认在左侧的树视图中已显示 AzureSqlLinkedService。

    ![包含链接服务的树视图](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>创建输出数据集
1. 在工具栏上单击“...更多”，然后依次单击“新建数据集”和“Azure SQL”。 单击命令栏上的“新建数据集”并选择“Azure SQL”。

    ![包含链接服务的树视图](media/data-factory-stored-proc-activity/new-dataset.png)
2. 将以下 JSON 脚本复制/粘贴到 JSON 编辑器。

        {                
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
3. 单击命令栏上的“部署”来部署数据集。 确认树视图中显示了此数据集。

    ![包含链接服务的树视图](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>使用 SqlServerStoredProcedure 活动创建管道
现在，我们来使用 SqlServerStoredProcedure 活动创建管道。

1. 单击命令栏上的“...更多”并单击“新建管道”。
2. 复制/粘贴以下 JSON 代码段。 **StoredProcedureName** 设置为 **sp_sample**。 **DateTime** 参数的名称和大小写必须与存储过程定义中参数的名称和大小写相匹配。  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                 "start": "2016-08-02T00:00:00Z",
                 "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    如果需要为参数传递 null，请使用语法："param1": null（全部小写）。
3. 单击工具栏上的“部署”来部署管道。  

### <a name="monitor-the-pipeline"></a>监视管道
1. 单击“X”关闭“数据工厂编辑器”边栏选项卡，导航回到“数据工厂”边栏选项卡，然后单击“图示”。

    ![图示磁贴](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. 在“图示视图”中，可以看到管道的概述，以及本教程中使用的数据集。

    ![图示磁贴](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. 在“图示视图”中，双击数据集 **sprocsampleout**。 将看到切片处于“就绪”状态。 由于切片是在 JSON 中针对开始时间和结束时间之间的每一小时生成的，因此，应该有 5 个切片。

    ![图示磁贴](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. 切片处于“就绪”状态时，对 Azure SQL 数据库运行 **select * from sampletable** 查询，以验证存储过程是否已将数据插入到表中。

   ![输出数据](./media/data-factory-stored-proc-activity/output.png)

   如需了解有关监视 Azure 数据工厂管道的详细信息，请参阅[监视管道](data-factory-monitor-manage-pipelines.md)。  

> [!NOTE]
> 在此示例中，SprocActivitySample 没有输入。 如果要链接此活动与活动上游（即先前处理），可以使用上游活动的输出作为此活动的输入。 在这种情况下，上游活动完成且能使用上游活动的输出（处于就绪状态）之前，此活动不会执行。 输入无法直接作为存储过程活动的参数
>
>

## <a name="json-format"></a>JSON 格式
    {
        "name": "SQLSPROCActivity",
        "description": "description",
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON 属性
| 属性 | 说明 | 必选 |
| --- | --- | --- |
| name |活动的名称 |是 |
| description |描述活动用途的文本 |否 |
| type |SqlServerStoredProcedure |是 |
| inputs |可选。 如果指定了输入数据集，则它必须可供使用（“就绪”状态），存储过程才能运行。 该输入数据集无法在存储过程中用作参数。 它仅用于在开始存储过程活动前检查依赖项。 |否 |
| outputs |必须指定存储过程活动的输出数据集。 输出数据集为存储过程活动指定**计划**（每小时、每周、每月等）。 <br/><br/>输出数据集必须使用**链接服务**，其指代 Azure SQL 数据库或 Azure SQL 数据仓库或要在其中运行存储过程的 SQL Server 数据库。 <br/><br/>输出数据集可用于传递存储过程的结果，以供管道中另一活动（[链接活动](data-factory-scheduling-and-execution.md#run-activities-in-a-sequence)）进行后续处理。 但是，数据工厂不会自动将存储过程的输出写入此数据集。 它是写入输出数据集指向的 SQL 表的存储过程。 <br/><br/>在某些情况下，输出数据集可以是**虚拟数据集**，它仅用于指定运行存储过程活动的计划。 |是 |
| storedProcedureName |在 Azure SQL 数据库或 Azure SQL 数据仓库中指定存储过程的名称，用输出表使用的链接服务表示。 |是 |
| storedProcedureParameters |指定存储过程的参数值。 如果需要为参数传递 null，请使用语法："param1": null（全部小写）。 请参阅以下示例了解如何使用此属性。 |否 |

## <a name="passing-a-static-value"></a>传递静态值
现在，我们可以考虑在包含静态值“Document sample”的表中添加名为“Scenario”的另一个列。

![示例数据 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)

    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

现在，从存储过程活动传递 Scenario 参数和值。 在上述示例中，typeProperties 部分如以下代码片段所示：

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters":
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }



<!--HONumber=Nov16_HO3-->

