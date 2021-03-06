---
title: "教程：在 Azure Active Directory 中配置 ZenDesk 实现自动用户预配 | Microsoft Docs"
description: "了解如何将 Azure Active Directory 配置为自动将用户帐户预配到 ZenDesk 和取消其预配。"
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 1a1414eefd20e6d7c025da08cfd5ae7c45daad33
ms.contentlocale: zh-cn
ms.lasthandoff: 07/20/2017

---

# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>教程：配置 ZenDesk 实现自动用户预配


本教程旨在说明从 Azure AD 自动将用户帐户预配到 ZenDesk 和取消其预配需要在 ZenDesk 和 Azure 中执行的步骤。 

## <a name="prerequisites"></a>先决条件

在本教程中概述的方案假定您已具有以下各项：

*   Azure Active Directory 租户
*   启用了[企业计划](https://www.zendesk.com/product/pricing/)或更佳计划的 ZenDesk 租户 
*   ZenDesk 中具有管理员权限的用户帐户 

> [!NOTE]
> Azure AD 预配集成依赖于 [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api)，其可供拥有基本计划或更佳计划的 ZenDesk 团队使用。

## <a name="assigning-users-to-zendesk"></a>将用户分配到 ZenDesk

Azure Active Directory 使用称为“分配”的概念来确定哪些用户应收到对所选应用的访问权限。 在自动用户帐户预配的上下文中，只同步已“分配”到 Azure AD 中应用程序的用户和组。 

配置和启用预配服务前，需确定 Azure AD 中的哪些用户和/或组表示需要访问 ZenDesk 应用的用户。 确定后，可以按照此处的说明将这些用户分配到 ZenDesk 应用：

[向企业应用分配用户或组](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>将用户分配到 ZenDesk 的重要提示

*   建议将单个 Azure AD 用户分配到 ZenDesk 以测试预配配置。 其他用户和/或组可以稍后分配。

*   如将用户分配到 ZenDesk，必须在分配对话框中选择用户角色或其他特定于应用程序的有效角色（如有）。 “默认访问权限”角色不可用于预配，将跳过这些用户。

> [!NOTE]
> 作为新增功能，预配服务将读取 Zendesk 中定于的所有自定义角色，并将其导入 Azure AD，可在其中的“选择角色”对话框中对其进行选择。 启用预配服务并完成一个同步周期后，这些角色将在 Azure 门户中可见。

## <a name="configuring-user-provisioning-to-zendesk"></a>向 ZenDesk 配置用户预配 

本部分将指导完成将 Azure AD 连接到 ZenDesk 的用户帐户预配 API 并配置预配服务，以便在 ZenDesk 中根据 Azure AD 中的用户和组分配来创建、更新和禁用分配的用户帐户。

> [!TIP] 
> 还可选择按照 [Azure 门户](https://portal.azure.com)中提供的说明为 ZenDesk 启用基于 SAML 的单一登录。 可以独立于自动预配配置单一登录，尽管这两个功能互相补充。


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a>在 Azure AD 中向 ZenDesk 配置自动用户帐户预配


1. 在 [Azure 门户](https://portal.azure.com)中，浏览到“Azure Active Directory”>“企业应用”>“所有应用程序”部分。

2. 如果已为 ZenDesk 配置单一登录，请使用搜索字段搜索 ZenDesk 实例。 否则，请选择“添加”并在应用程序库中搜索“ZenDesk”。 从搜索结果中选择 ZenDesk，并将其添加到应用程序列表。

3. 选择 ZenDesk 实例，并选择“预配”选项卡。

4. 将“预配模式”设置为“自动”。

    ![ZenDesk 预配](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. 在“管理员凭据”分区下，输入 ZenDesk 帐户生成的管理员用户名、令牌密钥和域（可在帐户下找到该令牌：“管理员” > “API” > “设置”）。 

    ![ZenDesk 预配](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. 在 Azure 门户中单击“测试连接”，确保 Azure AD 可以连接到 ZenDesk 应用。 如果连接失败，请确保 ZenDesk 帐户具有管理员权限，然后重试步骤 5。

7. 在“通知电子邮件”字段中输入应接收预配错误通知的个人或组的电子邮件地址，并选中复选框“发生故障时发送电子邮件通知”。

8. 单击“保存” 。 

9. 在“映射”部分下，选择“将 Azure Active Directory 用户同步到 ZenDesk”。

10. 在“属性映射”部分，查看从 Azure AD 同步到 ZenDesk 的用户属性。 选为“匹配”属性的特性用于匹配 ZenDesk 中的用户帐户以执行更新操作。 选择“保存”按钮以提交任何更改。

11. 若要为 ZenDesk 启用 Azure AD 预配服务，请在“设置”部分中将“预配状态”更改为“启用”

12. 单击“保存” 。 

这会开始“用户和组”部分中分配给 ZenDesk 的任何用户和/或组的初始同步。 初始同步执行的时间比后续同步长，只要服务正在运行，大约每隔 20 分钟就会进行一次同步。 可以使用“同步详细信息”部分监视进度并跟踪指向预配活动报告的链接，这些报告描述了预配服务执行的所有操作。

若要详细了解如何读取 Azure AD 预配日志，请参阅[有关自动用户帐户预配的报告](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting)。


## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](active-directory-enterprise-apps-manage-provisioning.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](active-directory-saas-provisioning-reporting.md)

