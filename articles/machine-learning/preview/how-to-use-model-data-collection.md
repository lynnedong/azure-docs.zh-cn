---
title: "如何使用 Azure 机器学习工作台中的模型管理数据收集功能 | Microsoft Docs"
description: "本文档介绍如何使用 Azure 机器学习工作台中的模型管理数据收集功能"
services: machine-learning
author: aashishb
ms.author: aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e751e2f0ec812de43a03749e04ff165fa62ec649
ms.contentlocale: zh-cn
ms.lasthandoff: 09/25/2017

---

# <a name="how-to-collect-model-data-using-data-collection"></a>如何使用数据收集来收集模型数据

使用模型数据收集功能可以将机器学习 Web 服务中的模型输入和预测存档。 本文档讨论模型数据收集的以下方面：

- 如何安装数据收集包
- 如何使用数据收集
- 如何查看和使用收集的数据

## <a name="how-to-install-the-data-collection-package"></a>如何安装数据收集包
数据收集库可以在 Linux 和 Windows 上本机安装。

### <a name="windows"></a>Windows
在 Windows 上，可使用以下命令安装数据收集器模块：

    pip install azureml.datacollector

### <a name="linux"></a>Linux
在 Linux 上，必须先安装 libxml++ 库。 运行以下必须在 sudo 下发出的命令：

    sudo apt-get install libxml++2.6-2v5

然后，发出以下命令：

    pip install azureml.datacollector
## <a name="how-to-use-data-collection"></a>如何使用数据收集

若要使用模型数据收集功能，需要对评分文件进行以下更改：

1. 在该文件顶部，添加以下代码
   
    ```python
    from azureml.datacollector import ModelDataCollector
    ```

2. 向 `init()` 函数添加以下代码行：
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

3. 向 `run(input_df)` 函数添加以下代码行：
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

    确保先初始化变量 `input_df` 和 `pred`（`model.predict()` 的预测值），然后再对它们调用 `collect()` 函数。

4. 将 `az ml service create realtime` 命令与 `--collect-model-data true` 交换机一起使用，创建实时 Web 服务。 这确保在运行该服务时可以收集模型数据。

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
5. 若要测试数据收集，请执行 `az ml service run realtime`。

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="how-to-view-and-consume-the-collected-data"></a>如何查看和使用收集的数据
若要在 blob 存储中查看收集的数据：

1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 单击“更多服务”。
3. 在搜索框中，键入“`Storage accounts`”，并按“Enter”
4. 在“存储帐户”搜索边栏选项卡上，选择“存储帐户”资源。 为了确定存储帐户，请使用以下步骤：

    a. 转到 Azure ML 工作台，选择正在处理的项目，然后打开“文件”菜单中的命令行提示
    
    b. 键入“`az ml env show -v`”并查看 *storage_account* 值。 这是存储帐户的名称

5. 单击资源边栏选项卡菜单中的“容器”，然后单击名为“modeldata”的容器。 提出第一个 Web 服务请求后，可能最多要等待 10 分钟才能看到数据开始传播到存储帐户。 数据通过以下容器路径流入 blob：

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

使用 Microsoft 软件和开源工具，可通过多种方式从 Azure blob 中使用数据。 下面是一些示例：
 - Azure ML 工作台 - 通过将 csv 文件添加为数据源，在 Azure ML 工作台中打开 csv 文件
 - Excel - 将日常 csv 文件作为电子表格打开
 - [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/) - 使用从 blob 中 csv 数据提取的数据创建图表
 - [Spark](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-overview) - 使用大部分 csv 数据创建数据帧
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started) - 将 csv 数据加载到 hive 表中，并直接对 blob 执行 SQL 查询


