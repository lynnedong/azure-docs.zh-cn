---
title: Enable Azure CLI for Azure Stack users | Microsoft Docs
description: Learn how to use the cross-platform command-line interface (CLI) to manage and deploy resources on Azure Stack
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: d184bb9edbe2542d7321d8b9ccc5d23f2401f8d5
ms.contentlocale: zh-cn
ms.lasthandoff: 09/25/2017

---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Enable Azure CLI for Azure Stack users

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

There aren't any Azure Stack operator specific tasks that you can perform by using CLI. But before users can manage resources through CLI, Azure Stack operators must provide them with the following:

* **The Azure Stack CA root certificate** is required if your users are using CLI from a workstation outside the Azure Stack development kit.  

* **The virtual machine aliases endpoint** provides an alias, like "UbuntuLTS" or "Win2012Datacenter", that references an image publisher, offer, SKU, and version as a single parameter when deploying VMs.  

The following sections describe how to get these values.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Export the Azure Stack CA root certificate

The Azure Stack CA root certificate is available on the development kit and on a tenant virtual machine that is running within the development kit environment. Sign in to your development kit or the tenant virtual machine and run the following script to export the Azure Stack root certificate in PEM format:

```powershell
$label = "AzureStackSelfSignedRootCert"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Cerficate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Set up the virtual machine aliases endpoint

Azure Stack operators should set up a publicly accessible endpoint that hosts a virtual machine aliases file.  The virtual machine alias file is a JSON file that provides a common name for an image, which is subsequently specified when deploying a VM as an Azure CLI parameter.  

Before you add an entry to an alias file, make sure that you [download images from the marketplace]((azure-stack-download-azure-marketplace-item.md), or have [published your own custom image](azure-stack-add-vm-image.md).  If you publish a custom image, make note of the publisher, offer, SKU, and version information you specified during publishing.  If it is an image from the marketplace, you can view the information using the ```Get-AzureVMImage``` cmdlet.  
   
A [sample alias file](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) with many common image aliases is available, which you can use as a starting point.  You should host this file in a space that your CLI clients can reach it.  One way to do this, is to host in a blob storage account, and share the URL with your users:

1.  Download the [sample file](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) from GitHub.
2.  Create a new storage account in Azure Stack.  Once complete, create a new blob container.  Set the access policy to "public".  
3.  Upload the JSON file to the new container.  Once complete, you can view the URL of the blob by clicking the fblob name, and then selecting the URL from the blob properties.


## <a name="next-steps"></a>Next steps

[Deploy templates with Azure CLI](azure-stack-deploy-template-command-line.md)

[Connect with PowerShell](azure-stack-connect-powershell.md)

[Manage user permissions](azure-stack-manage-permissions.md)


