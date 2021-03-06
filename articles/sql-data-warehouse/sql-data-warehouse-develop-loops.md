---
title: "在 Azure SQL 数据仓库中利用 T-SQL 循环 | Microsoft Docs"
description: "有关在开发解决方案时使用 Azure SQL 数据仓库中的 Transact-SQL 循环和替换游标的技巧。"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 43ab6a2f71ab51c50847b1ba5249f51c48e03fea
ms.openlocfilehash: d6409f1eb87787e5e023aa53b7b264116c9d8026
ms.contentlocale: zh-cn
ms.lasthandoff: 01/24/2017


---
# <a name="loops-in-sql-data-warehouse"></a>SQL 数据仓库中的循环
SQL 数据仓库支持对重复执行的语句块使用 [WHILE][WHILE] 循环。 只要指定的条件都成立，或者在代码专门使用 `BREAK` 关键字终止循环之前，这些语句将不断继续。 循环特别适合用于替换 SQL 代码中定义的游标。 幸运的是，几乎所有以 SQL 代码编写的游标都是快进的只读变体。 因此，如果你发现自己必须替换一个游标，[WHILE] 循环是绝佳的替代方案。

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>利用循环和替换 SQL 数据仓库中的游标
但是，在深入学习之前，你应该先自问以下问题：“此游标是否可重写以使用基于集的操作？”。 在许多情况下，答案是肯定的，通常这也是最佳方法。 基于集的操作的执行速度通常比迭代性的逐行方法要快得多。

可以轻松使用循环构造来替换快进只读游标。 下面是一个简单的示例。 此代码示例将更新数据库中每个表的统计信息。 通过迭代循环中的表，我们就能够依次执行每个命令。

首先，创建一个临时表，其中包含用于标识各个语句的唯一行号：

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

其次，初始化执行循环所需的变量：

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

现在，每次对一个语句执行一次循环：

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

最后，将第一个步骤创建的临时表删除

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>后续步骤
有关更多开发技巧，请参阅[开发概述][development overview]。

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WHILE]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->

