---
title: "Azure 搜索中的服务限制 | Microsoft Docs"
description: "用于容量计划的服务限制以及请求和响应 Azure 搜索的最大限制。"
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/07/2017
ms.author: heidist
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 60e63401e3915e62e1ec5ac03cd548c291580b24
ms.contentlocale: zh-cn
ms.lasthandoff: 07/26/2017

---
# <a name="service-limits-in-azure-search"></a>Azure 搜索中的服务限制
对存储、工作负荷以及索引、文档和其他对象数量的最大限制，取决于是在“免费”、“基本”还是“标准”定价层上[预配 Azure 搜索](search-create-service-portal.md)。

* **免费**层是 Azure 订阅随附的多租户共享服务。 它是为现有订阅者提供的选项（无需额外费用），以便可以在注册专用资源前试用服务。
* **基本**层为小规模生产工作负荷提供专用计算资源。
* **标准**层在专用计算机上运行，在每个级别上都具有更多存储和处理容量。 标准层共有四个级别：S1、S2、S3 以及 S3 高密度 (S3 HD)。

> [!NOTE]
> 服务已在特定层上进行预配。 如果需要跳转层来获取更多容量，必须预配新服务（无就地升级）。 有关详细信息，请参阅[选择 SKU 或层](search-sku-tier.md)。 若要详细了解在已预配的服务内调整容量，请参阅[缩放查询和为工作负荷编制索引的资源级别](search-capacity-planning.md)。
>

## <a name="per-subscription-limits"></a>每个订阅限制
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="per-service-limits"></a>每个服务限制
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="per-index-limits"></a>每个索引限制
索引限制与索引器限制之间存在一对一的对应关系。 假定限制为 200 个索引，则相同服务的索引器最大限制也是 200。

| 资源 | 免费 | 基本 | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| 索引：每个索引的最大字段 |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| 索引：每个索引的最大计分配置文件 |100 |100 |100 |100 |100 |100 |
| 索引：每个配置文件的最大函数 |8 |8 |8 |8 |8 |8 |
| 索引器：每次调用的最大索引编制负载 |10,000 个文档 |仅受最大文档的限制 |仅受最大文档的限制 |仅受最大文档的限制 |仅受最大文档的限制 |N/A <sup>2</sup> |
| 索引器：最长运行时间 | 1-3 分钟 <sup>3</sup> |24 小时 |24 小时 |24 小时 |24 小时 |N/A <sup>2</sup> |
| Blob 索引器：最大 blob 大小，MB |16 |16 |128 |256 |256 |N/A <sup>2</sup> |
| Blob 索引器：从 blob 中提取的内容的最大字符数 |32,000 |64,000 |4 百万 |4 百万 |4 百万 |N/A <sup>2</sup> |

<sup>1</sup> 基本层是唯一具有最低限制（每个索引 100 个字段）的 SKU。

<sup>2</sup> S3 HD 当前不支持索引器。 如果亟需此功能，请联系 Azure 支持。

<sup>3</sup> 对于免费层，索引器最长执行时间为 blob 源 3 分钟，所有其他数据源 1 分钟。

## <a name="document-size-limits"></a>文档大小限制
| 资源 | 免费 | 基本 | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| 每个索引 API 的单个文档大小 |<16 MB |<16 MB |<16 MB |<16 MB |<16 MB |<16 MB |

是指调用索引 API 时最大文档大小。 文档大小实际上是索引 API 请求主体大小的限制。 由于可以一次将多个文档的批次传递给索引 API，因此大小限制实际上取决于批次中的文档数。 对于具有单个文档的批次，最大文档大小是 16 MB JSON。

若要保持较低的文档大小，务必将不可查询数据从请求中排除。 图像和其他二进制数据不可直接查询，并且不应存储在索引中。 若要将不可查询的数据集成到搜索结果中，请定义用于存储资源的 URL 引用的不可搜索字段。

## <a name="workload-limits-queries-per-second"></a>工作负荷限制（每秒查询次数）
| 资源 | 免费 | 基本 | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| QPS |不适用 |每个副本大约 3 个 |每个副本大约 15 个 |每个副本大约 60 个 |每个副本大于 60 个 |每个副本大于 60 个 |

每秒查询次数 (QPS) 是基于启发所得的近似值，它使用模拟及实际的客户工作负荷获得估计值。 精确的 QPS 吞吐量因数据和查询性质而异。

虽然上面提供粗略的估计值，但实际查询率难以确定，尤其是在吞吐量基于可用带宽和系统资源竞争的免费共享服务中。 在免费层中，计算资源和存储资源由多个订阅者共享，因此解决方案 QPS 将始终随同时运行的其他工作负荷量的不同而有所不同。

在标准级别中，由于能够控制更多参数，因此可以更精确地评估 QPS。 有关如何计算工作负荷的 QPS 的指南，请参阅[管理搜索解决方案](search-manage.md)中的最佳实践部分。

## <a name="api-request-limits"></a>API 请求限制
* 每个请求最大 16 MB <sup>1</sup>
* 最大 8 KB URL 长度
* 每个索引上传、合并或删除的批次最多包含 1000 个文档
* $Orderby 子句中最多 32 字段
* 最大搜索词大小为 UTF-8 编码文本的 32,766 字节（32 KB 减 2 个字节）

<sup>1</sup> 在 Azure 搜索中，请求主体受 16 MB 上限的约束，这将针对不受理论限制约束的单个字段或集合的内容施加实际限制（有关字段组合和限制的详细信息，请参阅[支持的数据类型](https://msdn.microsoft.com/library/azure/dn798938.aspx)）。

## <a name="api-response-limits"></a>API 响应限制
* 每页搜索结果最多返回 1000 个文档
* 每个建议 API 请求最多返回 100 条建议

## <a name="api-key-limits"></a>API 密钥限制
API 密钥用于服务身份验证。 有两种类型。 管理密钥在请求标头中指定，并授予对服务的完全读写访问权限。 查询密钥是只读密钥并在 URL 上指定，并且通常分发给客户端应用程序。

* 每个服务最多 2 个管理密钥
* 每个服务最多 50 个查询密钥

