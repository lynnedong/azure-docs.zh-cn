---
title: "Azure AD v2 iOS 入门 - 配置 (ARP) | Microsoft Docs"
description: "iOS (Swift) 应用程序如何通过 Azure Active Directory v2 终结点调用需访问令牌的 API"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 5ce5d26943839d9fe53508d396825289d77cf2f8
ms.contentlocale: zh-cn
ms.lasthandoff: 08/15/2017

---
## <a name="add-the-applications-registration-information-to-your-app"></a>向应用添加应用程序的注册信息

在此步骤中，需将应用程序 ID 添加到项目：

1.  在 `ViewController.swift` 中，将以“`let kClientID`”开头的行替换为：
```swift
let kClientID = "[Enter the application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
按住 Control 键并单击 <code>Info.plist</code> 打开上下文菜单，然后单击：<code>Open As</code>> <code>Source Code</code>
</li>
<li>
在 <code>dict</code> 根节点下添加以下代码：
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Enter the application Id here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```

