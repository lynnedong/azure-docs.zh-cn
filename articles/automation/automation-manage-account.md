---
title: "管理 Azure 自动化帐户 | Microsoft Docs"
description: "本文介绍如何管理自动化帐户的配置，例如证书的续订、删除和错误配置。"
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/13/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 41efdbcacede74bac038342688362ff480cadc7e
ms.contentlocale: zh-cn
ms.lasthandoff: 04/15/2017

---

# <a name="manage-azure-automation-account"></a>管理 Azure 自动化帐户
在自动化帐户过期之前的某个时间点，你需要续订证书。 如果你认为运行方式帐户已遭到入侵，可以删除然后重新创建它。 本部分介绍如何执行这些操作。

## <a name="self-signed-certificate-renewal"></a>自签名证书续订
为运行方式帐户创建的自签名证书自创建日期算起的一年后过期。 可以在该证书过期之前的任何时间续订。 续订时，将保留当前的有效证书，确保已排队的或正在运行的且使用运行方式帐户进行身份验证的任何 Runbook 不会受到影响。 该证书在过期之前将保持有效。

> [!NOTE]
> 如果已将自动化运行方式帐户配置为使用企业证书颁发机构颁发的证书并使用此选项，该企业证书将被自签名证书替换。

若要续订证书，请执行以下操作：

1. 在 Azure 门户中，打开自动化帐户。

2. 在“自动化帐户”边栏选项卡上“帐户属性”窗格的“帐户设置”下面，选择“运行方式帐户”。

    ![自动化帐户属性窗格](media/automation-manage-account/automation-account-properties-pane.png)
3. 在“运行方式帐户”属性边栏选项卡上，选择要为其续订证书的运行方式帐户或经典运行方式帐户。

4. 在所选帐户的“属性”边栏选项卡上，单击“续订证书”。

    ![续订运行方式帐户的证书](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. 证书续订过程中，可以在菜单的“通知”下面跟踪进度。

## <a name="delete-a-run-as-or-classic-run-as-account"></a>删除运行方式帐户或经典运行方式帐户
本部分介绍如何删除并重新创建运行方式帐户或经典运行方式帐户。 执行此操作时，将保留自动化帐户。 删除运行方式帐户或经典运行方式帐户后，可在 Azure 门户中重新创建它。

1. 在 Azure 门户中，打开自动化帐户。

2. 在“自动化帐户”边栏选项卡上的帐户属性窗格中，选择“运行方式帐户”。

3. 在“运行方式帐户”属性边栏选项卡上，选择要删除的运行方式帐户或经典运行方式帐户。 然后，在所选帐户的“属性”边栏选项卡上，单击“删除”。

 ![删除运行方式帐户](media/automation-manage-account/automation-account-delete-runas.png)

4. 帐户删除过程中，可以在菜单的“通知”下面跟踪进度。

5. 删除该帐户后，可以通过在“运行方式帐户”属性边栏选项卡中选择创建选项“Azure 运行方式帐户”来重新创建该帐户。

 ![重新创建自动化运行方式帐户](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>配置错误
在初始设置期间，运行方式帐户或经典运行方式帐户所需的某些配置项可能已被删除或未正确创建。 这些项包括：

* 证书资产
* 连接资产
* 运行方式帐户已从参与者角色中删除
* Azure AD 中的服务主体或应用程序

在上述和其他错误配置的实例中，自动化将检测更改，并在该帐户的“运行方式帐户”属性边栏选项卡中显示“不完整”状态。

![不完整的运行方式帐户配置状态](media/automation-manage-account/automation-account-runas-incomplete-config.png)

选择该运行方式帐户时，该帐户的“属性”窗格中会显示以下错误消息：

![不完整的运行方式配置警告消息](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png)。

可通过删除并重新创建运行方式帐户来快速解决该帐户的问题。

## <a name="next-steps"></a>后续步骤
* 有关服务主体的详细信息，请参阅 [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md)（应用程序对象和服务主体对象）。
* 有关 Azure 自动化中基于角色的访问控制的详细信息，请参阅 [Role-Based access control in Azure Automation](automation-role-based-access-control.md)（Azure 自动化中基于角色的访问控制）。
* 有关证书和 Azure 服务的详细信息，请参阅 [Azure 云服务证书概述](../cloud-services/cloud-services-certs-create.md)。
