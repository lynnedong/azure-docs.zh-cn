---
title: "了解 Azure 中的资源访问权限 | Microsoft Docs"
description: "本主题介绍有关使用订阅管理员在整个 Azure 门户中控制资源访问权限的概念"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: e5ac068e37d921530272e5eb3dc76d976a86a742
ms.contentlocale: zh-cn
ms.lasthandoff: 09/07/2017

---
# <a name="understanding-resource-access-in-azure"></a>了解 Azure 中的资源访问权限

Azure 中的访问控制首先体现在计费方面。 Azure 帐户的所有者（可通过访问 [Azure 帐户中心](https://account.windowsazure.com/subscriptions)进行访问）是帐户管理员 (AA)。 订阅是计费容器，但它们也可充当安全边界：每个订阅都有一个服务管理员 (SA)，此管理员可以使用 [Azure 门户](https://portal.azure.com/)在该订阅中添加、删除和修改 Azure 资源。 新订阅的默认 SA 是 AA，但 AA 可以在 Azure 帐户中心更改 SA。

<br><br>![Azure 帐户][1]

订阅也与目录相关联。 目录定义一组用户。 这些用户可以创建该目录的公司或学校的用户，也可以是外部用户（即 Microsoft 帐户）。 订阅可由这些已被指定为服务管理员 (SA) 或共同管理员 (CA) 的目录用户的子集来访问；唯一的例外是，为了保持向后兼容，可以将 Microsoft 帐户（以前称为 Windows Live ID）指定为 SA 或 CA，而这些帐户不必存在于目录中。

<br><br>![Azure 中的访问控制][2]

借助 Azure 经典门户中的功能，使用 Microsoft 帐户登录的 SA 可以使用“设置”中“订阅”页上的“编辑目录”命令来更改与订阅关联的目录。 注意，此操作会影响该订阅的访问控制。

> [!NOTE]
> 使用工作或学校帐户登录的用户不能使用 Azure 经典门户中的“编辑目录”命令，因为这些帐户只能登录到其所属的目录。
> 
> 

<br><br>![简单用户登录流程][3]

在这种简单的情况下，组织（如 Contoso）将对同一组订阅实行计费和访问控制。 也就是说，目录与由单个 Azure 帐户所拥有的订阅关联。 一旦成功登录到 Azure 经典门户，用户就可以看到两组资源（在前面的插图中以橘色表示）：

* 其用户帐户所在的目录（源用户或添加为外部主体）。 请注意，用于登录的目录与此计算无关，因此，目录将始终显示，而不考虑你登录的位置。
* 作为订阅一部分的资源，这些订阅与用于登录的目录关联且用户可以访问（对于此订阅，用户是 SA 或 CA）。

<br><br>![具有多个订阅和目录的用户][4]

跨多个目录具有订阅的用户可以通过使用订阅筛选器来切换 Azure 经典门户的当前上下文。 最终，这会导致单独登录到不同的目录，但这可以使用单一登录 (SSO) 无缝地实现。

由于这种单一的订阅目录视图所导致的结果，诸如在订阅之间移动资源的操作可能会更难以实现。 要执行资源传输，务必首先使用“设置”中“订阅”页上的“编辑目录”命令将订阅与相同目录关联。

## <a name="next-steps"></a>后续步骤
* 若要了解有关如何更改 Azure 订阅管理员的详细信息，请参阅 [如何添加或更改 Azure 管理员角色](../billing/billing-add-change-azure-subscription-administrator.md)
* 有关 Azure Active Directory 如何与 Azure 订阅相关联的详细信息，请参阅 [How Azure subscriptions are associated with Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)（Azure 订阅与 Azure Active Directory 的关联方式）
* 有关如何在 Azure AD 中分配角色的详细信息，请参阅 [在 Azure Active Directory 中分配管理员角色](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png

