---
title: "了解如何为 Azure 托管应用程序创建 UI 定义 | Microsoft Docs"
description: "介绍了如何为 Azure 托管应用程序创建 UI 定义"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 176b891538f85c5638a2321561c3d8bd377d245b
ms.contentlocale: zh-cn
ms.lasthandoff: 05/11/2017

---
# <a name="getting-started-with-createuidefinition"></a>CreateUiDefinition 入门
本文档介绍了 CreateUiDefinition 的核心概念，该元素由 Azure 门户用来生成用于创建托管应用程序的用户界面。

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

CreateUiDefinition 始终包含三个属性： 

* handler
* version
* parameters

对于托管应用程序，handler 应当始终为 `Microsoft.Compute.MultiVm`，支持的最新版本为 `0.1.2-preview`。

parameters 属性的架构取决于所指定的 handler 和 version 的组合。 对于托管应用程序，支持的属性为 `basics`、`steps` 和 `outputs`。 basics 和 steps 属性包含要在 Azure 门户中显示的_元素_，例如文本框和下拉列表。 outputs 属性用来将指定元素的输出值映射到 Azure Resource Manager 部署模板的参数。

建议包括 `$schema`，但这是可选的。 如果指定，则 `version` 的值必须与 `$schema` URI 中的版本匹配。

## <a name="basics"></a>Basics
Basics 步骤始终是 Azure 门户在分析 CreateUiDefinition 时生成的向导的第一个步骤。 除了会显示 `basics` 中指定的元素外，该门户还会为用户注入其他元素以用于为部署选择订阅、资源组和位置。 通常，对部署范围内的参数进行查询的元素（例如群集名称或管理员凭据）应当放在此步骤中。

如果元素的行为依赖于用户的订阅、资源组或位置，则不能在 basics 中使用该元素。 例如，**Microsoft.Compute.SizeSelector** 需要依赖于用户的订阅和位置来确定可用大小的列表。 因此，**Microsoft.Compute.SizeSelector** 只能用于 steps 中。 通常，只有 **Microsoft.Common** 命名空间中的元素可以用于 basics 中。 但是也允许其他命名空间中不依赖于用户上下文的某些元素（例如 **Microsoft.Compute.Credentials**）。

## <a name="steps"></a>Steps
steps 属性可以包含要在 basics 后显示的零个或多个其他步骤，每个步骤都包含一个或多个元素。 请考虑按所部署的应用程序的角色或层添加步骤。 例如，针对群集中的主节点添加一个用于输入的步骤，针对辅助角色节点添加另一个步骤。

## <a name="outputs"></a>Outputs
Azure 门户使用 `outputs` 属性来将 `basics` 和 `steps` 中的元素映射到 Azure Resource Manager 部署模板的参数。 此字典中的键是模板参数的名称，值是所引用元素中的输出对象的属性。

## <a name="functions"></a>函数
与 Azure Resource Manager 中的[模板函数](resource-group-template-functions.md)类似（在语法和功能方面），CreateUiDefinition 提供了用于处理元素的输入和输出的函数，以及条件语句等诸多功能。

## <a name="next-steps"></a>后续步骤
CreateUiDefinition 本身具有一个简单的架构。 它的实际深度来自于所有受支持的元素和函数，下面的文档中非常详细地介绍了这些元素和函数：

- [元素](managed-application-createuidefinition-elements.md)
- [函数](managed-application-createuidefinition-functions.md)

以下位置提供了CreateUiDefinition 的当前 JSON 架构：https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json。 

更高版本也将在同一位置提供。 请将 URL 的 `0.1.2-preview` 部分和 `version` 值替换为你打算使用的版本标识符。 当前支持的版本标识符为 `0.0.1-preview`、`0.1.0-preview`、`0.1.1-preview` 和 `0.1.2-preview`。
