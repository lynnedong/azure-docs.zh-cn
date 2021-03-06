---
title: "Azure Functions HTTP 和 webhook 绑定 | Microsoft Docs"
description: "了解如何在 Azure Functions 中使用 HTTP 和 webhook 触发器和绑定。"
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: "Azure Functions, Functions, 事件处理, webhook, 动态计算, 无服务体系结构, HTTP, API, REST"
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: mahender
ms.translationtype: HT
ms.sourcegitcommit: 8ad98f7ef226fa94b75a8fc6b2885e7f0870483c
ms.openlocfilehash: 3c3247592cbe2bc382d220264b0c646ee566b8a7
ms.contentlocale: zh-cn
ms.lasthandoff: 09/29/2017

---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure Functions HTTP 和 webhook 绑定
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

本文介绍如何在 Azure Functions 中配置和使用 HTTP 触发器和绑定。
利用这些，可以通过 Azure Functions 生成无服务器 API 和响应 webhook。

Azure Functions 提供以下绑定：
- 借助 [HTTP 触发器](#httptrigger)，可以使用 HTTP 请求调用函数。 可进行自定义以响应 [webhook](#hooktrigger)。
- 利用 [HTTP 输出绑定](#output)，可响应请求。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>HTTP 触发器
HTTP 触发器会执行函数以响应 HTTP 请求。 可以对其自定义以响应特定的 URL 或 HTTP 方法集。 还可将 HTTP 触发器配置为响应 webhook。 

如果使用 Functions 门户，也可以立即开始使用预设的模板。 选择“新建函数”，并从“方案”下拉列表选择“API 和 Webhook”。 选择其中一个模板，并单击“创建”。

默认情况下，HTTP 触发器通过 HTTP 200 OK 状态代码和空白正文响应请求。 若要修改该响应，请配置 [HTTP 输出绑定](#output)

### <a name="configuring-an-http-trigger"></a>配置 HTTP 触发器
HTTP 触发器是通过 function.json `bindings` 数组中的 JSON 对象定义的，如下面的示例所示：

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
该绑定支持以下属性：

|属性  |说明  |
|---------|---------|
| **name** | 必需 - 在请求或请求正文的函数代码中使用的变量名称。 请参阅[通过代码使用 HTTP 触发器](#httptriggerusage)。 |
| **类型** | 必需 - 必须设置为 `httpTrigger`。 |
| **direction** | 必需 - 必须设置为 `in`。 |
| **authLevel** | 确定请求中需要提供的密钥（如果有），以便调用此函数。 该值可以是以下值之一： <ul><li><code>anonymous</code>&mdash;无需 API 密钥。</li><li><code>function</code>&mdash;特定于函数的 API 密钥是必需的。 如果未提供任何值，该值为默认值。</li><li><code>admin</code>&mdash;主密钥是必需的。</li></ul> 有关详细信息，请参阅[使用密钥](#keys)。 |
| **methods** | HTTP 方法的数组，该函数将响应此方法。 如果未指定，该函数将响应所有 HTTP 方法。 请参阅[自定义 HTTP 终结点](#url)。 |
| **route** | 定义路由模板，控制函数将响应的请求 URL。 如果未提供任何值，则默认值为 `<functionname>`。 有关详细信息，请参阅[自定义 HTTP 终结点](#url)。 |
| **webHookType** | 将 HTTP 触发器配置为充当指定提供程序的 webhook 接收器。 使用此设置时，请勿使用 methods 属性。 该值可以是以下值之一：<ul><li><code>genericJson</code>&mdash;不包含特定提供程序逻辑的常规用途 webhook 终结点。</li><li><code>github</code>&mdash;该函数会响应 GitHub webhook。 使用此值时，请勿使用 authLevel 属性。</li><li><code>slack</code>&mdash;该函数会响应 Slack webhook。 使用此值时，请勿使用 authLevel 属性。</li></ul> 有关详细信息，请参阅[响应 webhook](#hooktrigger)。 |

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>通过代码使用 HTTP 触发器
对于 C# 和 F# 函数，可以将触发器输入的类型声明为 `HttpRequestMessage` 或自定义 .NET 类型。 如果选择 `HttpRequestMessage`，会获得对请求对象的完全访问权限。 对于自定义 .NET 类型，函数会尝试分析 JSON 请求正文，以设置对象属性。 

对于 Node.js 函数，Functions 运行时提供请求正文而不是请求对象。 有关详细信息，请参阅 [HTTP 触发器示例](#httptriggersample)。


<a name="output"></a>
## <a name="http-response-output-binding"></a>HTTP 响应输出绑定
通过 HTTP 输出绑定响应 HTTP 请求发送者。 此绑定需要使用 HTTP 触发器，利用此绑定，可以自定义与触发器请求相关联的响应。 如果未提供 HTTP 输出绑定，HTTP 触发器将返回带空白正文的 HTTP 200 OK。 

### <a name="configuring-an-http-output-binding"></a>配置 HTTP 输出绑定
HTTP 输出绑定是通过 function.json `bindings` 数组中的 JSON 对象定义的，如下面的示例所示：

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
该绑定支持以下必需属性：

|属性  |说明  |
|---------|---------|
|**name** | 在响应对象的函数代码中使用的变量名。 请参阅[通过代码使用 HTTP 输出绑定](#outputusage)。 |
| **类型** |必须设置为 `http`。 |
| **direction** | 必须设置为 `out`。 |

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>通过代码使用 HTTP 输出绑定
输出参数可用于响应 http 或 webhook 调用方。 还可以使用语言标准响应模式。 有关示例响应，请参阅 [HTTP 触发器示例](#httptriggersample)和 [Webhook 触发器示例](#hooktriggersample)。


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a>响应 webhook
具有 webHookType 属性的 HTTP 触发器将配置为响应 [webhook](https://en.wikipedia.org/wiki/Webhook)。 基本配置使用“genericJson”设置。 这会将请求限制为仅请求使用 HTTP POST 以及内容类型为 `application/json`。

此外，触发器可以定制为特定 webhook 提供程序（例如，[GitHub](https://developer.github.com/webhooks/) 和 [Slack](https://api.slack.com/outgoing-webhooks)）。 如果指定了提供程序，则 Functions 运行时可以处理提供程序的验证逻辑。  

### <a name="configuring-github-as-a-webhook-provider"></a>将 GitHub 配置为 webhook 提供程序
要响应 GitHub webhook，首先请创建包含 HTTP 触发器的函数，并将 webHookType 属性设置为 `github`。 然后将其 [URL](#url) 和 [API 密钥](#keys)复制到 GitHub 存储库的“添加 webhook”页。 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

有关示例，请参阅[创建由 GitHub webhook 触发的函数](functions-create-github-webhook-triggered-function.md)。

### <a name="configuring-slack-as-a-webhook-provider"></a>将 Slack 配置为 webhook 提供程序
Slack webhook 为用户生成令牌，而非让用户指定它，所以必须使用 Slack 中的令牌配置特定于函数的密钥。 请参阅[使用密钥](#keys)。

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a>自定义 HTTP 终结点
默认情况下，创建 HTTP 触发器或 WebHook 的函数时，可通过以下格式的路由对该函数进行寻址：

    http://<yourapp>.azurewebsites.net/api/<funcname> 

在 HTTP 触发器的输入绑定中，可以使用可选 `route` 属性自定义此路由。 例如，以下 *function.json* 文件定义了 HTTP 触发器的 `route` 属性：

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

通过此配置，现在可以通过以下路由对该函数进行寻址，而不是通过原始路由寻址。

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

这使得函数代码可以支持地址中的两个参数：“category”和“id”。可以将任何 [Web API 路由约束](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints)与参数配合使用。 以下 C# 函数代码使用了这两个参数。

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

以下是使用相同路由参数的 Node.js 函数代码。

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

默认情况下，所有函数路由的前缀均为 *api*。 还可以使用 *host.json* 文件中的 `http.routePrefix` 属性自定义或删除前缀。 以下示例通过将空字符串用于 *host.json* 文件中的前缀删除 *api* 路由前缀。

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

若要深入了解如何更新函数的 *host.json* 文件，请参阅[如何更新 Function App 文件](functions-reference.md#fileupdate)。 

若要了解可在 *host.json* 文件中配置的其他属性，请参阅 [host.json 参考](functions-host-json.md)。


<a name="keys"></a>
## <a name="working-with-keys"></a>使用密钥
HTTP 触发器允许使用密钥提高安全性。 标准 HTTP 触发器可以将这些密钥用作 API 密钥，这需要在请求中提供此密钥。 Webhook 可以使用密钥以多种方式对请求授权，具体取决于提供程序支持何种方式。

密钥作为 Function App 的一部分存储在 Azure 中，并进行了静态加密。 要查看密钥，请创建新的密钥或将密钥滚动到新值，导航到门户内的其中一个函数并选择“管理”。 

有两种类型的密钥：
- **主机密钥**：由 Function App 中的所有函数共享这些密钥。 这些密钥用作 API 密钥时，可以访问 Function App 中的任何函数。
- **函数密钥**：这些密钥仅适用于在其下定义它们的特定函数。 这些密钥用作 API 密钥时，只允许访问该函数。

命名每个密钥方便引用，并且在函数和主机级别存在名为“default”的默认密钥。 主密钥是为每个函数应用定义的默认主机密钥，名为“_master”。 无法撤消此密钥。 它提供对运行时 API 的管理访问权限。 在绑定 JSON 中使用 `"authLevel": "admin"` 需要在请求中提供此密钥；任何其他密钥均会导致授权失败。

> [!IMPORTANT]  
> 由于提升的权限由主密钥所授予，因此不应与第三方共享此密钥或在本机客户端应用程序中分发此密钥。 选择管理员授权级别时，请务必审慎行事。

### <a name="api-key-authorization"></a>API 密钥的授权
默认情况下，HTTP 触发器要求 HTTP 请求中含有 API 密钥。 因此 HTTP 请求通常如下所示：

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

该密钥可以包含在名为 `code` 的查询字符串变量中（如上所示），也可以包含在 `x-functions-key` HTTP头中。 密钥的值可以为任意为函数定义的函数密钥，也可以为任意主机密钥。

可以允许匿名请求，它不需要密钥。 可能还需要使用主密钥。 可使用绑定 JSON 中的 `authLevel` 属性更改默认授权级别。 有关详细信息，请参阅 [HTTP 触发器](#httptrigger)。

### <a name="keys-and-webhooks"></a>密钥和 webhook
Webhook 授权由属于 HTTP 触发器的 webhook 接收器组件处理，其机制因 webhook 类型而异。 但是每种机制都依赖于一个密钥。 默认情况下，使用名为“default”的函数密钥。 要使用其他密钥，请将 webhook 提供程序配置为使用以下方式之一的请求发送密钥名称：

- **查询字符串**：提供程序通过 `clientid` 查询字符串参数（例如，`https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`）传递密钥名称。
- **请求头**：提供程序通过 `x-functions-clientid` 头传递密钥名称。

> [!NOTE]
> 函数密钥优先于主机密钥。 如果为两个密钥定义的名称相同，则使用函数密钥。


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>HTTP 触发器示例
假设在 function.json 的 `bindings` 数组中有以下 HTTP 触发器：

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

请参阅特定语言的示例，此示例在查询字符串或 HTTP 请求的正文中查找 `name` 参数。

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>C# 中的 HTTP 触发器示例 #
```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

此外，还可以绑定到 .NET 对象，而不是 HttpRequestMessage。 此对象从请求正文进行创建，并分析为 JSON。 同样，可以将类型传递给 HTTP 响应输出绑定，并作为响应正文随 200 状态代码一同返回。

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a>F# 中的 HTTP 触发器示例 #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

需要一个使用 NuGet 引用 `FSharp.Interop.Dynamic` 和 `Dynamitey` 程序集的 `project.json` 文件，如下所示：

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

这会使用 NuGet 来获取依赖项，并在脚本中引用它们。

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Node.JS 中的 HTTP 触发器示例
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```
     
<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a>Webhook 示例
假设在 function.json 的 `bindings` 数组中有以下 webhook 触发器：

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

请参阅特定语言的示例，它记录 GitHub 问题注释。

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>C# 中的 Webhook 示例 #
```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a>F# 中的 Webhook 示例 #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a>Node.JS 中的 Webhook 示例
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>后续步骤
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


