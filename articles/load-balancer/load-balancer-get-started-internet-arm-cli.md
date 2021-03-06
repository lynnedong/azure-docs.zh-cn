---
title: "创建面向 Internet 的负载均衡器 - Azure CLI | Microsoft 文档"
description: "了解如何使用 Azure CLI 在 Resource Manager 中创建面向 Internet 的负载均衡器"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ba36b7f6d2ae3cc4d63829ffb757ff7b311e467b
ms.contentlocale: zh-cn
ms.lasthandoff: 09/25/2017

---
# <a name="creating-an-internet-load-balancer-using-the-azure-cli"></a>使用 Azure CLI 创建内部负载均衡器

> [!div class="op_single_selector"]
> * [门户](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [模板](../load-balancer/load-balancer-get-started-internet-arm-template.md)


[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

本文介绍 Resource Manager 部署模型。 还可以[了解如何使用经典部署创建面向 Internet 的负载均衡器](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>使用 Azure CLI 部署解决方案

以下步骤说明如何使用 Azure Resource Manager 和 CLI 创建面向 Internet 的负载均衡器。 借助 Azure Resource Manager，可单独创建和配置每个资源，再将其合成一个新资源。

需要创建和配置以下对象才能部署负载均衡器：

* 前端 IP 配置 - 包含传入网络流量的公共 IP 地址。
* 后端地址池 - 包含从负载均衡器接收网络流量的虚拟机网络接口 (NIC)。
* 负载均衡规则 - 包含将负载均衡器上的公共端口映射到后端地址池中的端口的规则。
* 入站 NAT 规则 - 包含将负载均衡器上的公共端口映射到后端地址池中特定虚拟机的端口的规则。
* 探测器 - 包含用于检查后端地址池中虚拟机实例的可用性的运行状况探测器。

有关详细信息，请参阅 [Azure Resource Manager 对负载均衡器的支持](load-balancer-arm.md)。

## <a name="set-up-cli-to-use-resource-manager"></a>将 CLI 设置为使用 Resource Manager

1. 如果从未使用过 Azure CLI，请参阅[安装和配置 Azure CLI](../cli-install-nodejs.md)，并按照说明进行操作，直到选择 Azure 帐户和订阅。
2. 运行 **azure config mode** 命令以切换到 Resource Manager 模式，如下所示。

    ```azurecli
        azure config mode arm
    ```

    预期输出：

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>为前端 IP 池创建虚拟网络和公共 IP 地址

1. 使用名为 *NRPRG* 的资源组在美国东部位置创建名为 *NRPVnet* 的虚拟网络 (VNet)。

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    使用 *NRPVnet* 中 10.0.0.0/24 的 CIDR 块创建名为 *NRPVnetSubnet* 的子网。

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. 使用 DNS 名称 *loadbalancernrp.eastus.cloudapp.azure.com* 创建要由前端 IP 池使用的名为 *NRPPublicIP* 的公共 IP 地址。下面的命令使用静态分配类型和 4 分钟的空闲超时。

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > 负载均衡器使用公共 IP 的域标签作为其 FQDN。 这与经典部署不同，后者使用云服务作为负载均衡器完全限定的域名 (FQDN)。
   > 在本示例中，FQDN 是 *loadbalancernrp.eastus.cloudapp.azure.com*。

## <a name="create-a-load-balancer"></a>创建负载均衡器

以下命令会在美国东部 Azure 位置的 *NRPRG* 资源组中创建名为 *NRPlb* 的负载均衡器。

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>创建前端 IP 池和后端地址池
本示例演示如何创建前端和后端 IP 池，前者接收负载均衡器上的传入网络流量，后者发送负载均衡的网络流量。

1. 创建前端 IP 池，它与负载均衡器和上一步中创建的公共 IP 相关联。

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. 设置后端地址池，它用于接收前端 IP 池的传入流量。

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>创建 LB 规则、NAT 规则和探测器

下例会创建以下项。

* 用于将端口 21 上的所有传入流量转换到端口 22 的 NAT 规则<sup>1</sup>
* 用于将端口 23 上的所有传入流量转换到端口 22 的 NAT 规则
* 用于将端口 80 上的所有传入流量平衡到后端池中的地址端口 80 的负载均衡器规则。
* 用于检查 *HealthProbe.aspx* 页面上的运行状况状态的探测规则。

<sup>1</sup> NAT 规则将关联到负载均衡器后面的特定虚拟机实例。 到达端口 21 的网络流量发送至端口 22 上与该 NAT 规则关联的特定虚拟机。 必须为 NAT 规则指定协议（UDP 或 TCP）。 这两种协议不能分配给同一个端口。

1. 创建 NAT 规则。

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. 创建负载均衡器规则。

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. 创建运行状况探测器。

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. 检查设置。

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    预期输出：

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>创建 NIC

需要创建 NIC（或修改现有 NIC），并将其关联到 NAT 规则、负载均衡器规则和探测器。

1. 创建名为 *lb-nic1-be* 的 NIC，并将其与 *rdp1* NAT 规则和 *NRPbackendpool* 后端地址池相关联。

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    预期输出：

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. 创建名为 *lb-nic2-be* 的 NIC，并将其与 *rdp2* NAT 规则和 *NRPbackendpool* 后端地址池相关联。

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. 创建名为 *web1* 的虚拟机 (VM)，并将其与名为 *lb-nic1-be* 的 NIC 相关联。 名为 *web1nrp* 的存储帐户在运行以下命令之前已创建。

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > 负载均衡器中的 VM 需要在同一可用性集中。 使用 `azure availset create` 创建可用性集。

    输出应如下所示：

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > 应显示信息性消息**这是未配置公共 IP 的 NIC**，因为为连接到 Internet 的负载均衡器创建的 NIC 使用的是负载均衡器公共 IP 地址。

    由于 *lb-nic1-be* NIC 与 *rdp1* NAT 规则相关联，因此可以使用 RDP 通过负载均衡器上的端口 3441 连接到 *web1*。

4. 创建名为 *web2* 的虚拟机 (VM)，并将其与名为 *lb-nic2-be* 的 NIC 相关联。 名为 *web1nrp* 的存储帐户在运行以下命令之前已创建。

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>更新现有的负载均衡器
可添加引用现有负载均衡器的规则。 在下例中，向现有负载均衡器 **NRPlb** 添加了新的负载均衡器规则

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>删除负载均衡器
以下命令可删除负载均衡器：

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>后续步骤
[开始配置内部负载均衡器](load-balancer-get-started-ilb-arm-cli.md)

[配置负载均衡器分发模式](load-balancer-distribution-mode.md)

[配置负载均衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)

