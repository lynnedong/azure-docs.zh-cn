---
title: "创建用于连接到 Azure 存储的 Azure Function | Microsoft Docs"
description: "Azure CLI 脚本示例 - 创建用于连接到 Azure 存储的 Azure Function"
services: functions
documentationcenter: functions
author: rachelappel
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: af90702601d1bd05836dbf2b20cd3e318832b07c
ms.contentlocale: zh-cn
ms.lasthandoff: 09/09/2017

---
# <a name="integrate-function-app-into-azure-storage-account"></a>将 Function App 集成到 Azure 存储帐户中

此示例脚本创建 Function App 和存储帐户。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( /cli/azure/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

此示例创建 Azure Function app，并将存储连接字符串添加到应用设置。

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组、应用服务应用以及所有相关资源：

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/#login) | 登录到 Azure。 |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | 使用相关位置创建资源组 |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account) | 创建存储帐户 |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | 创建一个新的 Function App |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | 清理 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Functions 文档](../functions-cli-samples.md)中找到其他 Azure Functions CLI 脚本示例。

