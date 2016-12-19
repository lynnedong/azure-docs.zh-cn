---
title: "教程：Azure Active Directory 与 EmpCenter 的集成 | Microsoft 文档"
description: "了解如何将 EmpCenter 与 Azure Active Directory 配合使用来启用单一登录、自动化预配及更多功能！"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b24490585019c049af4e1808bf980e36c5143e71


---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a>教程：Azure Active Directory 与 EmpCenter 的集成
本教程的目标是展示 Azure 与 EmpCenter 的集成。  
在本教程中概述的方案假定已具有以下各项：

* 有效的 Azure 订阅
* 启用了 EmpCenter 单一登录的订阅

完成本教程后，分配到 EmpCenter 的 Azure AD 用户将能够在你的 EmpCenter 公司站点上（服务提供商启动的登录）或者使用[访问面板简介](active-directory-saas-access-panel-introduction.md)以单一登录方式登录到应用程序。

在本教程中概述的方案由以下构建基块组成：

1. 为 EmpCenter 启用应用程序集成
2. 配置单一登录
3. 配置用户预配
4. 分配用户

![方案](./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")

## <a name="enabling-the-application-integration-for-empcenter"></a>为 EmpCenter 启用应用程序集成
本部分的目的是概述如何为 EmpCenter 启用应用程序集成。

### <a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>若要为 EmpCenter 启用应用程序集成，请执行以下步骤：
1. 在 Azure 经典门户的左侧导航窗格中，单击“Active Directory”。
   
   ![Active Directory](./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")
2. 从“目录”列表中，选择要为其启用目录集成的目录。
3. 若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
   ![应用程序](./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applications")
4. 在页面底部单击“添加”。
   
   ![添加应用程序](./media/active-directory-saas-empcenter-tutorial/IC749321.png "Add application")
5. 在“要执行什么操作”对话框中，单击“从库中添加应用程序”。
   
   ![从库中添加一个应用程序](./media/active-directory-saas-empcenter-tutorial/IC749322.png "Add an application from gallerry")
6. 在**搜索框**中，键入“EmpCenter”。
   
   ![应用程序库](./media/active-directory-saas-empcenter-tutorial/IC802917.png "Application Gallery")
7. 在结果窗格中，选择“EmpCenter”，然后单击“完成”以添加该应用程序。
   
   ![EmpCentral](./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
   

## <a name="configuring-single-sign-on"></a>配置单一登录

本部分的目的是概述如何让用户能够使用基于 SAML 协议的联合身份验证通过他们在 Azure AD 中的帐户向 EmpCenter 证明自己的身份。

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要配置单一登录，请执行以下步骤：
1. 在 Azure 经典门户中，在 **EmpCenter** 应用程序集成页上，单击“配置单一登录”，以打开“配置单一登录”对话框。
   
   ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802919.png "Configure Single Sign-On")
2. 在“你希望用户如何登录到 EmpCenter”页上，选择“Microsoft Azure AD 单一登录”，然后单击“下一步”。
   
   ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802920.png "Configure Single Sign-On")
3. 在“配置应用设置”页上，执行以下步骤：
   
   ![配置应用设置](./media/active-directory-saas-empcenter-tutorial/IC802921.png "Configure App Settings")
   
   1. 在“登录 URL”文本框中，键入用户用来登录 EmpCenter 应用程序的 URL（例如：*https://partner-authenticati.empcenter.com/workforce/SSO.do*）。
   2. 单击“下一步”
4. 在“在 EmpCenter 处配置单一登录”页上，若要下载元数据，请单击“下载元数据”，然后将元数据文件保存在计算机上。
   
   ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802922.png "Configure Single Sign-On")
5. 将下载的元数据文件发送给 EmpCenter 支持团队。
   
   > [!NOTE]
   > 必须由 EmpCenter 支持团队执行实际的 SSO 配置。
   > 在为订阅启用了 SSO 后，你将收到通知。
   > 
   > 
6. 在 Azure 经典门户中，选择“单一登录配置确认”，然后单击“完成”，关闭“配置单一登录”对话框。
   
   ![配置单一登录](./media/active-directory-saas-empcenter-tutorial/IC802923.png "Configure Single Sign-On")
   
## <a name="configuring-user-provisioning"></a>配置用户预配

为了使 Azure AD 用户能够登录到 EmpCenter，必须将其预配到 EmpCenter 中。  
对于 EmpCenter，需要由 EmpCenter 支持团队创建用户帐户。

> [!NOTE]
> 可以使用 EmpCenter 提供的任何其他 EmpCenter 用户帐户创建工具或 API 来预配 Azure Active Directory 用户帐户。
> 
> 

## <a name="assigning-users"></a>分配用户
若要测试配置，需要向希望能够通过应用程序访问配置的 Azure AD 用户分配访问权限，从而向他们授予该权限。

### <a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>若要将用户分配到 EmpCenter，请执行以下步骤：
1. 在 Azure 经典门户中，创建测试帐户。
2. 在 **EmpCenter** 应用程序集成页上，单击“分配用户”。
   
   ![分配用户](./media/active-directory-saas-empcenter-tutorial/IC802924.png "Assign Users")
3. 选择测试用户，单击“分配”，然后单击“是”确认分配。
   
   ![是](./media/active-directory-saas-empcenter-tutorial/IC767830.png "Yes")

如果要测试单一登录设置，请打开访问面板。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)（访问面板简介）。




<!--HONumber=Nov16_HO3-->

