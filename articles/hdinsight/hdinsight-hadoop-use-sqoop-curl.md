---
title: "在 HDInsight 中将 Hadoop Sqoop 与 Curl 配合使用 - Azure | Microsoft Docs"
description: "了解如何使用 Curl 向 HDInsight 远程提交 Sqoop 作业。"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 0975aedf58c6e110726dd3308eae5f9ad3907cc7
ms.contentlocale: zh-cn
ms.lasthandoff: 07/28/2017

---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>使用 Curl 在 HDInsight 中的 Hadoop 上运行 Sqoop 作业
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

学习如何使用 Curl 在 HDInsight 中的 Hadoop 群集上运行 Sqoop 作业。

本文档使用 Curl 演示如何使用原始 HTTP 请求来与 HDInsight 交互，以便运行、监视和检索 Sqoop 作业的结果。 若要执行这些操作，需要使用 HDInsight 群集提供的 WebHCat REST API（前称 Templeton）。

> [!NOTE]
> 如果已熟悉如何使用基于 Linux 的 Hadoop 服务器，但刚接触 HDInsight，请参阅 [Information about using HDInsight on Linux](hdinsight-hadoop-linux-information.md)（关于使用 HDInsight on Linux 的信息）。
> 
> 

## <a name="prerequisites"></a>先决条件
若要完成本文中的步骤，你将需要：

* HDInsight 群集上的 Hadoop（基于 Linux 或 Windows）
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a>使用 Curl 提交 Sqoop 作业
> [!NOTE]
> 使用 Curl 或者与 WebHCat 进行任何其他形式的 REST 通信时，必须提供 HDInsight 群集管理员用户名和密码对请求进行身份验证。 此外，还必须使用群集名称作为用来向服务器发送请求的统一资源标识符 (URI) 的一部分。
> 
> 对本部分中的所有命令，请将 **USERNAME**替换为在群集上进行身份验证的用户，并将 **PASSWORD** 替换为用户帐户的密码。 将 **CLUSTERNAME** 替换为群集名称。
> 
> REST API 通过 [基本身份验证](http://en.wikipedia.org/wiki/Basic_access_authentication)进行保护。 始终应该使用安全 HTTP (HTTPS) 来发出请求，以确保安全地将凭据发送到服务器。
> 
> 

1. 在命令行中，使用以下命令验证你是否可以连接到 HDInsight 群集。
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    应会收到类似于下面的响应：
   
        {"status":"ok","version":"v1"}
   
    此命令中使用的参数如下：
   
   * **-u** - 用来对请求进行身份验证的用户名和密码。
   * **-G** - 指出这是 GET 请求。
     
     所有请求的 URL 开头都是 **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**。 路径 **/status** 指示请求将返回服务器的 WebHCat（也称为 Templeton）状态。 
2. 使用以下命令 sqoop 作业：

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    此命令中使用的参数如下：

    * **-d** - 由于未使用 `-G`，请求将按默认使用 POST 方法。 `-d` 指定与请求一起发送的数据值。

        * **user.name** - 正在运行命令的用户。

        * **command** - 要执行的 Sqoop 命令。

        * **statusdir** - 此作业的状态要写入到的目录。

    此命令应会返回可用来检查作业状态的作业 ID。

        {"id":"job_1415651640909_0026"}

1. 若要检查作业的状态，请使用以下命令。 将“JOBID”替换为上一步骤返回的值。 例如，如果返回值为 `{"id":"job_1415651640909_0026"}`，则 **JOBID** 将是 `job_1415651640909_0026`。
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    如果作业已完成，状态将是 **SUCCEEDED**。
   
   > [!NOTE]
   > 此 Curl 请求返回具有作业相关信息的 JavaScript 对象表示法 (JSON) 文档；使用 jq 可以仅检索状态值。
   > 
   > 
2. 在作业的状态更改为“SUCCEEDED”后，可以从 Azure Blob 存储中检索作业的结果。 随查询一起传递的 `statusdir` 参数包含输出文件的位置；在本例中，位置为 wasb:///example/curl。 此地址会将作业的输出存储在 HDInsight 群集所用的默认存储容器的 **example/curl** 目录中。
   
    可以使用 [Azure CLI](../cli-install-nodejs.md) 列出并下载这些文件。 例如，若要列出 **example/curl** 中的文件，请使用以下命令：
   
        azure storage blob list <container-name> example/curl
   
    若要下载文件，请使用以下命令：
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > 必须使用 `-a` 和 `-k` 参数指定包含 blob 的存储帐户名称，或者设置 **AZURE\_STORAGE\_ACCOUNT** 和 **AZURE\_STORAGE\_ACCESS\_KEY** 环境变量。 请参阅 <a href="hdinsight-upload-data.md" target="_blank" 了解详细信息。
   > 
   > 

## <a name="limitations"></a>限制
* 批量导出 - 在基于 Linux 的 HDInsight 上，用于将数据导出到 Microsoft SQL Server 或 Azure SQL 数据库的 Sqoop 连接器目前不支持批量插入。
* 批处理 - 在基于 Linux 的 HDInsight 上，如果执行插入时使用 `-batch` 开关，Sqoop 将执行多次插入而不是批处理插入操作。

## <a name="summary"></a>摘要
如本文档中所示，可以使用原始 HTTP 请求来运行、监视和查看 HDInsight 群集上的 Sqoop 作业的结果。

有关本文中使用的 REST 接口的详细信息，请参阅 <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API 指南</a>。

## <a name="next-steps"></a>后续步骤
有关将 Hive 与 HDInsight 配合使用的一般信息：

* [将 Sqoop 与 HDInsight 上的 Hadoop 配合使用](hdinsight-use-sqoop.md)

有关 HDInsight 上的 Hadoop 的其他使用方法的信息：

* [将 Hive 与 Hadoop on HDInsight 配合使用](hdinsight-use-hive.md)
* [将 Pig 与 Hadoop on HDInsight 配合使用](hdinsight-use-pig.md)
* [将 MapReduce 与 HDInsight 上的 Hadoop 配合使用](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx



