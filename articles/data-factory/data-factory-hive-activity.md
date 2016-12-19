---
title: "Hive 活动"
description: "了解如何使用 Azure 数据工厂中的 Hive 活动在按需/你自己的 HDInsight 群集上运行 Hive 查询。"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: a4121f8857fa9eaeb1cf1bca70e29666f6a04f63
ms.openlocfilehash: 6f5aecf7ac50258e8f744a7bbf9c78d46413069d


---
# <a name="hive-activity"></a>Hive 活动
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Hadoop 流式处理](data-factory-hadoop-streaming-activity.md)
> * [机器学习](data-factory-azure-ml-batch-execution-activity.md) 
> * [存储过程](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> * [.NET 自定义](data-factory-use-custom-activities.md)
> 
> 

数据工厂[管道](data-factory-create-pipelines.md)中的 HDInsight Hive 活动会在[你自己](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或基于 Windows/Linux 的[按需](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight 群集上执行 Hive 查询。 本文基于[数据转换活动](data-factory-data-transformation-activities.md)一文，它概述了数据转换和受支持的转换活动。

## <a name="syntax"></a>语法
    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }

## <a name="syntax-details"></a>语法详细信息
| 属性 | 说明 | 必选 |
| --- | --- | --- |
| name |活动的名称 |是 |
| description |描述活动用途的文本 |否 |
| type |HDinsightHive |是 |
| inputs |Hive 活动使用的输入 |否 |
| outputs |Hive 活动生成的输出 |是 |
| linkedServiceName |参考在数据工厂中注册为链接服务的 HDInsight 群集 |是 |
| 脚本 |指定 Hive 脚本内联 |否 |
| 脚本路径 |将 Hive 脚本存储在 Azure blob 存储中，并提供该文件的路径。 使用“script”或“scriptPath”属性。 两者不能一起使用。 文件名称需区分大小写。 |否 |
| 定义 |使用“hiveconf”在 Hive 脚本中指定参数作为密钥/值对，以便参考 |否 |

## <a name="example"></a>示例
请考虑游戏日志分析的示例，在此示例中，你想要确定用户用于玩你的公司发行的游戏的时间。 

以下日志是一个示例游戏日志，它以逗号（`,`）隔开，并且包含以下字段 - ProfileID、SessionStart、Duration、SrcIPAddress 和 GameType。

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

用于处理此数据的 **Hive 脚本**：

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID        string, 
        SessionStart     string, 
        Duration         int, 
        SrcIPAddress     string, 
        GameType         string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 

    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (    
        ProfileID     string, 
        Duration     int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';

    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

若要在数据工厂管道中执行此 Hive 脚本，需要执行以下操作

1. 创建链接服务以注册[自己的 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或配置[按需 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 让我们称此链接服务为“HDInsightLinkedService”。
2. 创建[链接服务](data-factory-azure-blob-connector.md)以配置托管数据的 Azure Blob 存储的连接。 让我们称此链接服务为“StorageLinkedService”
3. 创建指向输入和输出数据的[数据集](data-factory-create-datasets.md)。 让我们称输入数据集为“HiveSampleIn”，称输出数据集为“HiveSampleOut”
4. 将 Hive 查询作为文件复制到步骤 2 中配置的 Azure Blob 存储。 如果承载数据的存储不同于承载此查询文件的存储，则创建单独的 Azure 存储链接服务，并在活动中引用它。 使用 **scriptPath ** 来指定 hive 查询文件的路径，使用 **scriptLinkedService** 来指定包含脚本文件的 Azure 存储。 
   
   > [!NOTE]
   > 还可以通过使用**脚本**属性在活动定义中提供 Hive 脚本内联。 不建议使用此方法，因为需要转义 JSON 文档内的脚本中的所有特殊字符，并且可能会导致调试问题。 最佳做法是遵循步骤 #4。
   > 
   > 
5. 创建 HDInsightHive 活动的管道。 活动处理/转换数据。
   
     {
   
       "name": "HiveActivitySamplePipeline",
       "properties": {
         "activities": [
           {
             "name": "HiveActivitySample",
             "type": "HDInsightHive",
             "inputs": [
               {
                 "name": "HiveSampleIn"
               }
             ],
             "outputs": [
               {
                 "name": "HiveSampleOut"
               }
             ],
             "linkedServiceName": "HDInsightLinkedService",
             "typeproperties": {
               "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
               "scriptLinkedService": "StorageLinkedService"
             },
                "scheduler": {
                   "frequency": "Hour",
                   "interval": 1
             }
           }
         ]
       }
     }
6. 部署管道。 有关详细信息，请参阅[创建管道](data-factory-create-pipelines.md)一文。 
7. 使用数据工厂监视和管理视图来监视管道。 有关详细信息，请参阅[监视和管理数据工厂管道](data-factory-monitor-manage-pipelines.md)一文。 

## <a name="specifying-parameters-for-a-hive-script"></a>为 Hive 脚本指定参数
在此示例中，将每天向 Azure Blob 存储中引入游戏日志，并将游戏日志存储在使用日期和时间进行分区的文件夹中。 需要对 Hive 脚本进行参数化和在运行时动态传递输入文件夹位置，并生成使用日期和时间进行分区的输出。

若要使用参数化的 Hive 脚本，请执行以下操作

* 定义 **defines** 中的参数。
  
        {
            "name": "HiveActivitySamplePipeline",
              "properties": {
            "activities": [
                 {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                          {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                          {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                          "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                          "scriptLinkedService": "StorageLinkedService",
                          "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                          },
                           "scheduler": {
                              "frequency": "Hour",
                              "interval": 1
                        }
                    }
                  }
            ]
          }
        }
* 在 Hive 脚本中，使用 **${hiveconf:parameterName}** 引用参数。 
  
        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID     string, 
            SessionStart     string, 
            Duration     int, 
            SrcIPAddress     string, 
            GameType     string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
  
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID     string, 
            Duration     int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
  
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID

## <a name="see-also"></a>另请参阅
* [Pig 活动](data-factory-pig-activity.md)
* [MapReduce 活动](data-factory-map-reduce.md)
* [Hadoop 流式处理活动](data-factory-hadoop-streaming-activity.md)
* [调用 Spark 程序](data-factory-spark.md)
* [调用 R 脚本](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)




<!--HONumber=Nov16_HO3-->

