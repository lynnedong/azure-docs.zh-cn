---
title: "使用 ASP.NET Core 创建 Azure Service Fabric 应用的 Web 前端 | Microsoft Docs"
description: "使用 ASP.NET Core 项目和服务间通信，通过 Service Remoting 将 Service Fabric 应用程序公开到 Web。"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: b19aaa652f2c15573ded632ca1348e1a6752f080
ms.contentlocale: zh-cn
ms.lasthandoff: 06/10/2017

---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>使用 ASP.NET Core 生成应用程序的 Web 服务前端
默认情况下，Azure Service Fabric 服务不提供用于访问 Web 的公共接口。 若要向 HTTP 客户端公开应用程序的功能，需要创建一个 Web 项目作为入口点，然后从该处与单个服务进行通信。

在本教程中，我们将弥补[在 Visual Studio 中创建第一个应用程序](service-fabric-create-your-first-application-in-visual-studio.md)教程中遗留的内容，在有状态计数器服务的前面添加一个 ASP.NET Core Web 服务。 如果你尚未学习上述教程，应该返回并逐步完成该教程中的步骤。

## <a name="set-up-your-environment-for-aspnet-core"></a>为 ASP.NET Core 设置环境

需要 Visual Studio 2017 来跟随完成本教程。 任何版本都可以。 

 - [安装 Visual Studio 2017](https://www.visualstudio.com/)

若要开发 ASP.NET Core Service Fabric 应用程序，必须安装以下工作负荷：
 - Azure 开发（位于 Web 和云下）
 - ASP.NET 和 Web 开发（位于 Web 和云下）
 - .NET Core 跨平台开发（位于其他工具集下）

>[!Note] 
>Visual Studio 2015 的 .NET Core 工具不再进行更新，但若仍使用 Visual Studio 2015，需要安装 [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core)。

## <a name="add-an-aspnet-core-service-to-your-application"></a>将 ASP.NET Core 服务添加到应用程序
ASP.NET Core 是轻量跨平台的 Web 开发框架，可用于创建现代 Web UI 和 Web API。 

让我们将 ASP.NET Web API 项目添加到现有的应用程序。

1. 在解决方案资源管理器中，右键单击应用程序项目中的“**服务**”，然后选择“**添加 > 新建 Service Fabric 服务**”。
   
    ![将一个新服务添加到现有应用程序][vs-add-new-service]
2. 在“**创建服务**”页上，选择 **ASP.NET Core** 并将它命名。
   
    ![在新建服务对话框中选择 ASP.NET Web 服务][vs-new-service-dialog]

3. 下一页将提供一组 ASP.NET Core 项目模板。 请注意，如果 ASP.NET Core 项目是在 Service Fabric 应用程序外部创建的，则在显示这些选项的同时，还会提供少量附加代码，用于向 Service Fabric 运行时注册该服务。 本教程中，选择“Web API”。 不过你也可以运用相同的思路来构建完整的 Web 应用程序。
   
    ![选择 ASP.NET 项目类型][vs-new-aspnet-project-dialog]
   
    创建 Web API 项目后，应用程序中会有两个服务。 随着继续生成应用程序，可采用完全相同的方式添加更多服务。 每个服务都可以单独进行版本控制和升级。

## <a name="run-the-application"></a>运行应用程序
为了感受一下我们的成果，让我们部署新的应用程序并看看 ASP.NET Core Web API 模板提供的默认行为。

1. 在 Visual Studio 中，按 F5 调试应用。
2. 部署完成后，Visual Studio 将启动浏览器并打开 ASP.NET Web API 服务的根路径。 ASP.NET Core Web API 模板不提供根路径的默认行为，因此将在浏览器中看到 404 错误。
3. 将 `/api/values` 添加到浏览器中的位置。 这将对 Web API 模板中的 ValuesController 调用 `Get` 方法。 它会返回模板提供的默认响应，即包含两个字符串的 JSON 数组：
   
    ![从 ASP.NET Core Web API 模板返回的默认值][browser-aspnet-template-values]
   
    在本教程结束后，本页将显示有状态服务的最新计数器值而非默认字符串。

## <a name="connect-the-services"></a>连接服务
在如何与 Reliable Services 通信方面，Service Fabric 提供十足的弹性。 在单个应用程序中，你可能拥有可通过 TCP 访问的服务、可通过 HTTP REST API 访问的其他服务，并且还有其他可通过 Web 套接字访问的服务。 有关可用选项和相关权衡取舍的背景信息，请参阅[与服务通信](service-fabric-connect-and-communicate-with-services.md)。 本教程使用 SDK 中提供的 [Service Fabric Service Remoting](service-fabric-reliable-services-communication-remoting.md)。

在 Service Remoting 方法（模仿远程过程调用或 RPC）中，定义一个接口以用作服务的公共约定。 然后使用该接口生成代理类，以便与服务交互。

### <a name="create-the-remoting-interface"></a>创建远程处理接口
首先创建接口以充当有状态服务与其他服务之间的约定，在此情况下是 ASP.NET Core Web 项目。 此接口必须由使用它来进行 RPC 调用的所有服务共享，因此我们将在其自身的类库项目中创建。

1. 在解决方案资源管理器中，右键单击你的解决方案并选择“**添加**” > “**新建项目**”。

2. 在左侧导航窗格中选择 **Visual C#** 条目，然后选择“**类库**”模板。 确保 .NET Framework 版本已设置为 **4.5.2**。
   
    ![为有状态服务创建接口项目][vs-add-class-library-project]

3. 安装 Microsoft.ServiceFabric.Services.Remoting NuGet 包。 在 NuGet 包管理器中搜索 Microsoft.ServiceFabric.Services.Remoting，并为解决方案中所有使用 Services.Remoting 的项目安装它，包括：
   - 包含服务接口的类库项目
   - 有状态服务项目
   - ASP.NET Core Web 服务项目
   
    ![添加服务 NuGet 包][vs-services-nuget-package]

4. 在类库中，使用单个方法 `GetCountAsync` 创建接口，并从 `Microsoft.ServiceFabric.Services.Remoting.IService` 扩展接口。 远程接口必须派生自此接口，表明它是 Service Remoting 接口。
   
    ```c#
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-the-interface-in-your-stateful-service"></a>在有状态服务中实现接口
现在我们已定义了接口，接下来需要在有状态服务中实现该接口。

1. 在有状态服务中，添加对包含此接口的类库项目的引用。
   
    ![在有状态服务中添加对类库项目的引用][vs-add-class-library-reference]
2. 找到继承自 `StatefulService` 的类（例如 `MyStatefulService`），然后扩展它以实现 `ICounter` 接口。
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. 现在实现 `ICounter` 接口中定义的单个方法，即 `GetCountAsync`。
   
    ```c#
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>使用服务远程侦听器公开有状态服务
`ICounter` 接口实现后，最后一步是打开 Service Remoting 信道。 对于有状态服务，Service Fabric 提供了名为 `CreateServiceReplicaListeners` 的可重写方法。 通过此方法，可以根据想要为服务启用的通信类型指定一个或多个通信侦听器。

> [!NOTE]
> 用于打开无状态服务的通信通道的等效方法名为 `CreateServiceInstanceListeners`。

在本例中，我们将替换现有的 `CreateServiceReplicaListeners` 方法，并提供 `ServiceRemotingListener` 的实例，该实例通过 `ServiceProxy` 创建可从客户端调用的 RPC 终结点。  

通过 `IService` 接口上的 `CreateServiceRemotingListener` 扩展方法，可以创建具有所有默认设置的 `ServiceRemotingListener`。 若要使用此扩展方法，确保导入了 `Microsoft.ServiceFabric.Services.Remoting.Runtime` 命名空间。 

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>使用 ServiceProxy 类来与服务交互
现在，有状态服务已准备好通过 RPC 接收来自其他服务的流量。 因此，剩下的操作便是从 ASP.NET Web 服务添加代码来与其通信。

1. 在 ASP.NET 项目中，添加对包含 `ICounter` 接口的类库的引用。

2. 前面已将 Microsoft.ServiceFabric.Services.Remoting NuGet 包添加到 ASP.NET 项目。 此包提供用于对有状态服务进行 RPC 调用的 `ServiceProxy` 类。 请确保 ASP.NET Core Web 服务项目中已安装有此 NuGet 包。

4. 在 **Controllers** 文件夹中，打开 `ValuesController` 类。 请注意，`Get` 方法目前只返回“value1”和“value2”的硬编码字符串数组，这符合前面在浏览器中看到的内容。 使用以下代码替换此实现：
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    第一行代码是关键代码。 若要创建有状态服务的 ICounter Proxy，必须提供两项信息：分区 ID 和服务名称。
   
    可以使用分区根据定义的键（例如客户 ID 或邮政编码）将有状态服务的状态划分为不同的桶，以此调整有状态服务。 在我们的简单应用程序中，有状态服务只有一个分区，所以键并不重要。 提供的任何键将导致分区相同。 若要深入了解分区服务，请参阅[如何为 Service Fabric Reliable Services 分区](service-fabric-concepts-partitioning.md)。
   
    服务名称是 fabric:/&lt;application_name&gt;/&lt;service_name&gt; 格式的 URI。
   
    使用这两项信息，Service Fabric 可唯一标识请求应发送到的计算机。 `ServiceProxy` 类还能顺利处理托管有状态服务分区的计算机发生故障的情况，另一台计算机必须进行升级才能取而代之。 这种抽象使得编写客户端代码来处理其他服务变得简单许多。
   
    拥有代理后，我们只需调用 `GetCountAsync` 方法并返回其结果。

5. 再次按 F5 运行修改后的应用程序。 像前面一样，Visual Studio 将自动启动浏览器并打开 Web 项目的根路径。 添加“api/values”路径，你应会看到返回的当前计数器值。
   
    ![浏览器中显示的有状态计数器值][browser-aspnet-counter-value]
   
    定期刷新浏览器，以查看计数器更新值。

## <a name="kestrel-and-weblistener"></a>Kestrel 和 WebListener

默认的 ASP.NET Core Web 服务器称为 Kestrel，[目前尚不支持处理直接 Internet 流量](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel)。 因此，Service Fabric 的 ASP.NET Core 无状态服务模板默认使用 [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener)。 

若要详细了解 Service Fabric 服务中的 Kestrel 和 WebListener，请参阅 [Service Fabric Reliable Services 中的 ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)。

## <a name="connecting-to-a-reliable-actor-service"></a>连接到 Reliable Actors 服务
本教程着重介绍添加与有状态服务通信的 Web 前端。 但是你可以遵循十分类似的模型来与执行组件交流。 创建 Reliable Actor 项目时，Visual Studio 自动生成接口项目。 你可以使用该接口在 Web 项目中生成执行组件代理来与执行组件通信。 系统将自动提供通信通道。 因此您无需像本教程中所述的处理有状态服务一样创建 `ServiceRemotingListener`。

## <a name="how-web-services-work-on-your-local-cluster"></a>Web 服务在本地群集上的工作方式
一般而言，你可以将完全相同的 Service Fabric 应用程序部署到多计算机群集，该群集在本地群集上部署，并且你可以确信它能按预期运行。 这是因为，本地群集只是折叠成单个计算机的五节点配置。

但是，提到 Web 服务，有一个重要的微妙之处。 当群集位于负载均衡器后面时，如同在 Azure 中一样，你必须确保 Web 服务已部署到每台计算机上，因为负载均衡器只将流量循环分配到各台计算机。 可以通过将服务的 `InstanceCount` 设置为特殊值“-1”来达到此目的。

相比之下，当你在本地运行 Web 服务时，必须确保服务只有一个实例正在运行。 否则，正在同一路径和端口上侦听的多个进程将发生冲突。 因此，本地部署的 Web 服务实例计数应设置为“1”。

若要了解如何针对不同环境配置不同的值，请参阅[管理多个环境的应用程序参数](service-fabric-manage-multiple-environment-app-configuration.md)。

## <a name="next-steps"></a>后续步骤
现在，已使用 ASP.NET Core 为应用程序设置 Web 前端，详细了解 [Service Fabric Reliable Services 中的 ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)，以便深入了解 ASP.NET Core 如何与 Service Fabric 一起工作。

接下来，通常[详细了解如何与服务通信](service-fabric-connect-and-communicate-with-services.md)可全面了解 Service Fabric 中服务通信的工作原理。

很好地了解服务通信的工作原理后，请[在 Azure 中创建群集并将应用程序部署到云](service-fabric-cluster-creation-via-portal.md)。

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows

