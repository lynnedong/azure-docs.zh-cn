---
title: "使用 Linux VM MSI 访问 Azure Key Vault"
description: "本教程逐步介绍了如何使用 Linux VM 托管服务标识 (MSI) 访问 Azure 资源管理器。"
services: active-directory
documentationcenter: 
author: elkuzmen
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: elkuzmen
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: dd2dfe20f86b3fac28871b27a1c2b66c2b4a4cd6
ms.contentlocale: zh-cn
ms.lasthandoff: 09/20/2017

---

# <a name="use-managed-service-identity-msi-with-a-linux-vm-to-access-azure-key-vault"></a>使用 Linux VM 托管服务标识访问 Azure Key Vault 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

本教程介绍了如何为 Windows 虚拟机启用托管服务标识 (MSI)，再使用此标识访问 Azure Key Vault。 托管服务标识由 Azure 自动管理，可用于通过支持 Azure AD 身份验证的服务的身份验证，这样就无需在代码中插入凭据了。 学习如何：

> [!div class="checklist"]
> * 在 Linux 虚拟机上启用 MSI 
> * 授予 VM 对 Key Vault 中存储的密钥的访问权限 
> * 使用 VM 标识获取访问令牌，并使用它来检索 Key Vault 中的密钥 
 


如果还没有 Azure 订阅，可以在开始前创建一个 [免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。


## <a name="sign-in-to-azure"></a>登录 Azure
登录 Azure 门户 ([https://portal.azure.com](https://portal.azure.com))。 

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>在新的资源组中创建 Linux 虚拟机

本教程将新建一个 Linux VM。 另外，还可以在现有 VM 上启用 MSI。

1. 单击 Azure 门户左上角的“新建”按钮。
2. 选择“计算”，然后选择“Ubuntu Server 16.04 LTS”。
3. 输入虚拟机信息。 对于“身份验证类型”，选择“SSH 公钥”或“密码”。 使用创建的凭据可以登录 VM。

    ![Alt 图像文本](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. 在“订阅”下拉列表中，选择虚拟机对应的订阅。
5. 若要在新资源组中创建虚拟机，请选择“资源组”中的“新建”。 完成后，单击“确定”。
6. 选择 VM 大小。 若要查看更多大小，请选择“全部查看”或更改“支持的磁盘类型”筛选器。 在“设置”页中保留默认值，然后单击“确定”。

## <a name="enable-msi-on-your-vm"></a>在 VM 上启用 MSI

通过虚拟机 MSI，可以从 Azure AD 获取访问令牌，而无需在代码中插入凭据。 幕后运行机制是，启用 MSI 可以在 VM 上安装 MSI VM 扩展，并为 VM 启用 MSI。  

1. 选择要在其上启用 MSI 的虚拟机。
2. 单击左侧导航栏中的“配置”。
3. 此时，将会看到“托管服务标识”。 若要注册并启用 MSI，请选择“是”，若要禁用，请选择“否”。
4. 务必单击“保存”，以保存配置。

    ![Alt 图像文本](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. 若要检查在此 Linux VM 上安装了哪些扩展，请单击“扩展”。 如果 MSI 已启用，列表中会显示“ManagedIdentityExtensionforLinux”。

    ![Alt 图像文本](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)


## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>授予 VM 对 Key Vault 中存储的密钥的访问权限  

使用 MSI，代码可以获取访问令牌，通过支持 Azure Active Directory 身份验证的资源的身份验证。 但是，并非所有 Azure 服务都支持 Azure AD 身份验证。 若要将 MSI 用于不支持它的服务，可以在 Azure Key Vault 中存储这些服务所需的凭据，并使用 MSI 对 Key Vault 进行身份验证以检索凭据。 

首先，需要创建 Key Vault 并授予 VM 的标识对 Key Vault 的访问权限。   

1. 在左侧导航栏的顶部，依次选择“+ 新建”、“安全性 + 标识”以及“Key Vault”。  
2. 为新 Key Vault 提供一个名称。 
3. 定位到之前创建的 VM 所在的订阅和资源组中的 Key Vault。 
4. 选择“访问策略”，然后单击“添加新”。 
5. 在模板中的“配置”中，选择“密钥管理”。 
6. 选择“选择主体”，并在搜索字段中输入之前创建的 VM 的名称。  选择结果列表中的 VM，并单击“选择”。 
7. 单击“确定”完成添加新的访问策略，然后单击“确定”完成访问策略选择。 
8. 单击“创建”完成创建 Key Vault。 

    ![Alt 图像文本](media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)

接下来，将密钥添加到 Key Vault，以便稍后可以使用在 VM 中运行的代码检索此密钥： 

1. 选择“所有资源”，找到并选择刚刚创建的 Key Vault。 
2. 选择“密钥”，然后单击“添加”。 
3. 从“上传选项”中选择“手动”。 
4. 输入密钥的名称和值。  该值可以是任何需要的内容。 
5. 明确指定激活日期和到期日期，并将“已启用”设置为“是”。 
6. 单击“创建”以创建密钥。 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-retrieve-the-secret-from-the-key-vault"></a>使用 VM 标识获取访问令牌，并使用它来检索 Key Vault 中的密钥  

若要完成这些步骤，需要使用 SSH 客户端。  如果使用的是 Windows，可以在[适用于 Linux 的 Windows 子系统](https://msdn.microsoft.com/commandline/wsl/about)中使用 SSH 客户端。   
 
1. 在门户中，转到 Linux VM，并单击“概述”中的“连接”。 
2. 使用所选的 SSH 客户端连接到 VM。 
3. 在终端窗口中，使用 CURL 向本地 MSI 终结点发出请求，以获取访问 Azure Key Vault 所需的访问令牌。  
 
    下面是用于获取访问令牌的 CURL 请求。  
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true  
    ```
    响应包括访问资源管理器所需的访问令牌。 
    
    响应：  
    
    ```bash
    {"access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyIsImtpZCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3LyIsImlhdCI6MTUwNDEyNjYyNywibmJmIjoxNTA0MTI2NjI3LCJleHAiOjE1MDQxMzA1MjcsImFpbyI6IlkyRmdZTGg2dENWSzRkSDlGWGtuZzgyQ21ZNVdBZ0E9IiwiYXBwaWQiOiI2ZjJmNmU2OS04MGExLTQ3NmEtOGRjZi1mOTgzZDZkMjUxYjgiLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwib2lkIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwic3ViIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoib0U5T3JVZFJMMHVKSEw4UFdvOEJBQSIsInZlciI6IjEuMCJ9.J6KS7b9kFgDkegJ-Vfff19LMnu3Cfps4dL2uNGucb5M76rgDM5f73VO-19wZSRhQPxWmZLETzN3SljnIMQMkYWncp79MVdBud_xqXYyLdQpGkNinpKVJhTo1j1dY27U_Cjl4yvvpBTrtH3OX9gG0GtQs7PBFTTLznqcH3JR9f-bTSEN4wUhalaIPHPciVDtJI9I24_vvMfVqxkXOo6gkL0mEPfpXZRLwrBNd607AzX0KVmLFrwA1vYJnCV-sSV8bwTh2t6CVEj240t0iyeVWVc2usJ0NY2rxPzKd_UckQ_zzrECG3kS4vuYePKz6GqNJFVzm2w2c61lX0-O1CwvQ9w","refresh_token":"","expires_in":"3599","expires_on":"1504130527","not_before":"1504126627","resource":"https://vault.azure.net","token_type":"Bearer"} 
    ```
    
    可以使用此访问令牌对 Azure Key Vault 进行身份验证。  下一个 CURL 请求显示如何使用 CURL 和 Key Vault REST API 从 Key Vault 读取密钥。  将需要 Key Vault 的 URL，该 URL 位于 Key Vault 的“概述”页的“软件包”部分。  另外，还需要在前面的调用中获取的访问令牌。 
        
    ```bash
    curl https://<YOUR-KEY-VAULT-URL>/secrets/<secret-name>?api-version=2016-10-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    响应将如下所示： 
    
    ```bash
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
在检索 Key Vault 中的密钥后，可以使用该密钥对需要名称和密码的服务进行身份验证。


## <a name="related-content"></a>相关内容

- 有关 MSI 的概述，请参阅[托管服务标识概述](../active-directory/msi-overview.md)。

使用以下评论部分提供反馈，帮助我们改进内容。





