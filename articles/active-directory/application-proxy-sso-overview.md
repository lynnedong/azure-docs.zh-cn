---
title: "管理 Azure AD 应用程序代理的 SSO | Microsoft 文档"
description: "了解有关使用应用程序代理进行单一登录的基础知识"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 1deb3d91049d45fe26791783e13bd23e0a7d9f95
ms.contentlocale: zh-cn
ms.lasthandoff: 08/25/2017

---

# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Azure AD 应用程序代理如何提供单一登录？

单一登录是 Azure AD 应用程序代理的一个关键要素。  它提供最佳的用户体验，因为用户只需登录到云中的 Azure Active Directory。 用户在 Azure Active Directory 中完成身份验证后，应用程序代理连接器会处理本地应用程序的身份验证。 后端应用程序无法辨别远程用户通过应用程序代理登录与在已加入域的设备上使用普通登录方法的差异。 

若要使用 Azure Active Directory 单一登录到应用程序，需要选择“Azure Active Directory”作为预身份验证方法。 如果选择“直通”，则用户根本不会向 Azure Active Directory 进行身份验证，而是直接定向到应用程序。 可以在首次发布应用程序时配置此设置，或者在 Azure 门户中导航到自己的应用程序，并编辑应用程序代理设置。 

若要查看单一登录选项，请遵循以下步骤：

1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 导航到“Azure Active Directory” > “企业应用程序” > “所有应用程序”。
3. 选择想要管理其单一登录选项的应用。
4. 选择“单一登录”。

   ![SSO 下拉菜单](./media/application-proxy-sso-overview/single-sign-on-mode.png)

下拉菜单显示了五个用于单一登录到应用程序的选项：

* Azure AD 单一登录已禁用
* 基于密码的登录
* 联合登录
* Windows 集成身份验证
* 基于标头的登录

## <a name="azure-ad-single-sign-on-disabled"></a>Azure AD 单一登录已禁用

如果不想要使用 Azure Active Directory 集成单一登录到应用程序，请选择“Azure AD 单一登录已禁用”。 选择此选项后，用户可能会执行身份验证两次。 他们首先向 Azure Active Directory 进行身份验证，然后登录到应用程序本身。 

如果本地应用程序不需要用户进行身份验证，但你希望将 Azure Active Directory 添加为远程访问的安全层，则这是一个适当的选项。 

## <a name="password-based-sign-on"></a>基于密码的登录

若要使用 Azure Active Directory 作为本地应用程序的密码保管库，请选择“基于密码的登录”。 如果应用程序使用用户名/密码的组合而不是访问令牌或标头进行身份验证，则这是一个适当的选项。 使用基于密码的登录时，用户在首次访问应用程序时需要登录。 此后，Azure Active Directory 会代表用户提供用户名和密码。 

有关设置基于密码的登录的信息，请参阅[使用应用程序代理通过密码保管库进行单一登录](application-proxy-sso-azure-portal.md)。

## <a name="linked-sign-on"></a>联合登录

如果已经为本地标识设置了单一登录解决方案，请选择“链接登录”。 此选项可让 Azure Active Directory 利用现有的 SSO 解决方案，但仍允许用户远程访问应用程序。 

有关链接登录的信息（以前称为现有单一登录），请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)。

## <a name="integrated-windows-authentication"></a>Windows 集成身份验证

如果本地应用程序使用 Windows 集成身份验证 (IWA)，或者你要使用 Kerberos 约束委派 (KCD) 进行单一登录，请选择“Windows 集成身份验证”。 使用此选项时，用户只需向 Azure Active Directory 进行身份验证，然后，应用程序代理连接器会模拟用户获取 Kerberos 令牌并登录到应用程序。 

有关设置 Windows 集成身份验证的信息，请参阅[使用应用程序代理通过 Kerberos 约束委派进行单一登录](active-directory-application-proxy-sso-using-kcd.md)。

## <a name="header-based-sign-on"></a>基于标头的登录 

如果应用程序使用标头进行身份验证，请选择“基于标头的登录”。 使用此选项时，用户只需在 Azure Active Directory 中进行身份验证。 Microsoft 与名为 PingAccess 的第三方身份验证服务合作，已将 Azure Active Directory 访问令牌转换为应用程序的标头格式。 

有关设置基于标头的身份验证的信息，请参阅[使用应用程序代理通过基于标头的身份验证进行单一登录](application-proxy-ping-access.md)。

## <a name="next-steps"></a>后续步骤

- [使用应用程序代理通过密码保管库进行单一登录](application-proxy-sso-azure-portal.md)
- [使用应用程序代理通过 Kerberos 约束委派进行单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [使用应用程序代理通过基于标头的身份验证进行单一登录](application-proxy-ping-access.md) 

