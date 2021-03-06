---
title: "在 Azure 中为 Window 创建虚拟机规模集 | Microsoft Docs"
description: "使用虚拟机规模集在 Windows VM 上创建和部署高度可用的应用程序"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 7fc2e841a193c219822e232fbc994df5e934ddc4
ms.contentlocale: zh-cn
ms.lasthandoff: 09/14/2017

---

# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>在 Windows 上创建虚拟机规模集和部署高度可用的应用
利用虚拟机规模集，可以部署和管理一组相同的、自动缩放的虚拟机。 可以手动缩放规模集中的 VM 数，也可以定义规则，以便根据资源使用情况（如 CPU 使用率、内存需求或网络流量）进行自动缩放。 在本教程中，会在 Azure 中部署虚拟机规模集。 学习如何：

> [!div class="checklist"]
> * 使用自定义脚本扩展定义要缩放的 IIS 站点
> * 为规模集创建负载均衡器
> * 创建虚拟机规模集
> * 增加或减少规模集中的实例数
> * 创建自动缩放规则

本教程需要 Azure PowerShell 模块 3.6 或更高版本。 可以运行 ` Get-Module -ListAvailable AzureRM` 来查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](/powershell/azure/install-azurerm-ps)。


## <a name="scale-set-overview"></a>规模集概述
利用虚拟机规模集，可以部署和管理一组相同的、自动缩放的虚拟机。 规模集中的 VM 将分布在逻辑容错域和更新域的一个或多个*放置组*中。 这些放置组由配置类似的 VM 组成，与[可用性集](tutorial-availability-sets.md)相似。

可以根据需要在规模集中创建 VM。 定义自动缩放规则，以控制如何以及何时在规模集中添加或删除 VM。 这些根据 CPU 负载、内存用量或网络流量等指标触发这些规则。

使用 Azure 平台映像时，规模集最多支持 1,000 个 VM。 对于有重要安装或 VM 自定义要求的工作负荷，可能需要[创建自定义 VM 映像](tutorial-custom-images.md)。 使用自定义映像时，在规模集中最多可以创建 300 个 VM。


## <a name="create-an-app-to-scale"></a>创建用于缩放的应用
创建规模集之前，需使用 [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建资源组。 以下示例在“EastUS”位置创建名为“myResourceGroupAutomate”的资源组：

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

在前面的教程中，已了解如何使用自定义脚本扩展来[自动执行 VM 配置](tutorial-automate-vm-deployment.md)。 创建规模集配置，并应用自定义脚本扩展，安装和配置 IIS：

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>创建规模负载均衡器
Azure 负载均衡器是位于第 4 层（TCP、UDP）的负载均衡器，通过在正常运行的 VM 之间分发传入流量提供高可用性。 负载均衡器的运行状况探测监视每个 VM 上的给定端口，并仅将流量分发给可操作的 VM。 有关详细信息，请参阅有关[如何负载均衡 Windows 虚拟机](tutorial-load-balancer.md)的下一个教程。

创建具有公共 IP 地址、在端口 80 上分布 Web 流量的负载均衡器：

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>创建规模集
现在，使用 [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm) 创建虚拟机规模集。 下列示例创建名为“myScaleSet”的规模集：

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

创建和配置所有的规模集资源和 VM 需要几分钟时间。


## <a name="test-your-app"></a>测试应用
要查看运行中的 IIS 网站，使用 [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取负载均衡器的公共 IP 地址。 以下示例获取创建为规模集一部分的“myPublicIP”的 IP 地址：

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

将公共 IP 地址输入到 Web 浏览器中。 将显示应用，包括负载均衡器将流量分发到的 VM 的主机名：

![运行 IIS 网站](./media/tutorial-create-vmss/running-iis-site.png)

若要查看规模集的运作方式，可以强制刷新 Web 浏览器，以查看负载均衡器如何在运行应用的所有 VM 之间分配流量。


## <a name="management-tasks"></a>管理任务
在规模集的整个生命周期内，可能需要运行一个或多个管理任务。 此外，可能还需要创建自动执行各种生命周期任务的脚本。 Azure PowerShell 提供一种用于执行这些任务的快速方法。 以下是一些常见任务。

### <a name="view-vms-in-a-scale-set"></a>查看规模集中的 VM
若要查看规模集中运行的 VM 列表，请使用 [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm)，如下所示：

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>增加或减少 VM 实例
若要查看规模集中当前包含的实例数，请使用 [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) 并查询 sku.capacity：

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

然后，可以使用 [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) 手动增加或减少规模集中虚拟机的数目。 以下示例将规模集中 VM 的数目设置为“5”：

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

这会花费数分钟来更新规模集中实例的指定数量。


### <a name="configure-autoscale-rules"></a>配置自动缩放规则
定义自动缩放规则，而不是手动缩放规模集中实例的数目。 这些规则监视规模集中的实例，并根据所定义的指标和阈值做出相应响应。 如果平均 CPU 负载高于 60% 且持续时间超过 5 分钟，以下示例将增加一个实例。 如果平均 CPU 负载低于 30% 且持续时间超过 5 分钟，则将减少一个实例：

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```

有关使用自动缩放的详细设计信息，请参阅[自动缩放最佳做法](/azure/architecture/best-practices/auto-scaling)。


## <a name="next-steps"></a>后续步骤
在本教程中，已创建虚拟机规模集。 你已了解如何：

> [!div class="checklist"]
> * 使用自定义脚本扩展定义要缩放的 IIS 站点
> * 为规模集创建负载均衡器
> * 创建虚拟机规模集
> * 增加或减少规模集中的实例数
> * 创建自动缩放规则

请继续学习下一篇教程，详细了解虚拟机的负载均衡概念。

> [!div class="nextstepaction"]
> [均衡虚拟机的负载](tutorial-load-balancer.md)

