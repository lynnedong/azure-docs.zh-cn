


本文讨论了通过经典部署模型创建的 Azure 虚拟机的一些用户常见问题。

## <a name="can-i-migrate-my-vm-created-in-the-classic-deployment-model-to-the-new-resource-manager-model"></a>是否可以将经典部署模型中创建的 VM 迁移到新的 Resource Manager 模型？
是的。 有关如何迁移的说明，请参阅：

* [使用 Azure PowerShell 从经典部署迁移到 Azure Resource Manager 部署](../articles/virtual-machines/windows/migration-classic-resource-manager-ps.md)。
* [使用 Azure CLI 从经典部署迁移到 Azure Resource Manager 部署](../articles/virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)。

## <a name="what-can-i-run-on-an-azure-vm"></a>我可以在 Azure VM 上运行什么程序？
所有订户都可以在 Azure 虚拟机上运行服务器软件。 可以运行最新版本的 Windows Server 和各种 Linux 发行版。 有关支持详细信息，请参阅：

• Windows VM -- [Microsoft 服务器软件对 Azure 虚拟机的支持](http://go.microsoft.com/fwlink/p/?LinkId=393550)

• Linux VM -- [Azure 认可发行版中的 Linux](http://go.microsoft.com/fwlink/p/?LinkId=393551)

对于 Windows 客户端映像，某些版本的 Windows 7 和 Windows 8.1 可供 MSDN Azure 权益订户及 MSDN 开发和测试即用即付订户，用于开发和测试任务。 有关详细信息（包括说明和限制），请参阅[适用于 MSDN 订户的 Windows 客户端映像](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/)。

## <a name="why-are-affinity-groups-being-deprecated"></a>为什么要弃用地缘组？
地缘组是一个旧概念，指的是在 Azure 中客户的云服务部署和存储帐户的地理分组。 在早期的 Azure 网络设计中，它们最初用于改进 VM 到 VM 的网络性能。 它们还支持仅限于使用一个区域中的一小部分硬件的虚拟网络 (VNet) 的初始版本。

当前一个区域内的 Azure 网络已被设计为不再需要地缘组。 虚拟网络也在区域范围内，因此使用虚拟网络时不再需要地缘组。 由于这些改进，我们不再建议客户使用地缘组，因为在某些情况下它们可能存在一些限制。 使用地缘组将不必要地将 VM 和特定的硬件相关联，这样限制了可供你选择的 VM 大小。 当与地缘组相关联的特定硬件接近容量上限时，如果尝试添加新的 VM，还可能导致与容量相关的错误。

Azure Resource Manager 部署模型和 Azure 门户已弃用地缘组功能。 对于经典 Azure 门户，我们将禁止支持创建地缘组以及创建固定到地缘组的存储资源。 无需修改现有的使用地缘组的云服务。 但是，不能在新的云服务中使用地缘组，除非 Azure 支持专业人员建议使用它们。

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>使用虚拟机时，我可以使用多少存储？
每个数据磁盘的容量高达 1 TB。 可以使用的数据磁盘的数目取决于虚拟机的大小。 有关详细信息，请参阅[虚拟机大小](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

Azure 存储帐户提供可用于操作系统磁盘和任意数据磁盘的存储。 每个磁盘都是一个 .vhd 文件，以页 blob 形式存储。 有关定价详细信息，请参阅 [Storage Pricing Details](http://go.microsoft.com/fwlink/p/?LinkId=396819)（存储定价详细信息）。

## <a name="which-virtual-hard-disk-types-can-i-use"></a>可以使用哪些虚拟硬盘类型？
Azure 仅支持固定的 VHD 格式虚拟硬盘。 若要在 Azure 中使用 VHDX，需先使用 Hyper-V 管理器或 [convert-VHD](http://go.microsoft.com/fwlink/p/?LinkId=393656) cmdlet 对其进行转换。 然后，使用 [Add-AzureVHD](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet（在“服务管理”模式下）将 VHD 上传到 Azure 的存储帐户，用于虚拟机。

* 有关 Linux 说明，请参阅创建并上传包含 Linux 操作系统的虚拟硬盘。[](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* 有关 Windows 说明，请参阅[创建 Windows Server VHD 并将其上传到 Azure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。

## <a name="are-these-virtual-machines-the-same-as-hyper-v-virtual-machines"></a>这些虚拟机是否与 Hyper-V 虚拟机相同？
在许多方面，它们与“第 1 代”Hyper-V VM 类似，但并非完全相同。 两种类型的虚拟机都提供虚拟化硬件，并且兼容 VHD 格式虚拟硬盘。 这意味着可以在 Hyper-V 与 Azure 之间交换使用它们。 同时存在以下三大区别，有时也会使 Hyper-V 用户感到惊讶：

* Azure 不提供对虚拟机的控制台访问。 在 VM 完成启动前，无法对其进行访问。
* 大多数[大小](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)的 Azure VM 只有 1 个虚拟网络适配器，这意味着它们也只能具有 1 个外部 IP 地址。 （A8 和 A9 大小的 VM 可使用第二个网络适配器在实例之间进行应用程序通信，但仅限特定方案。）
* Azure VM 不支持第 2 代 Hyper-V VM 功能。 有关这些功能的详细信息，请参阅 [Virtual Machine Specifications for Hyper-V](http://technet.microsoft.com/library/dn592184.aspx)（Hyper-V 虚拟机规范）和 [Generation 2 Virtual Machine Overview](https://technet.microsoft.com/library/dn282285.aspx)（第 2 代虚拟机概述）。

## <a name="can-these-virtual-machines-use-my-existing-on-premises-networking-infrastructure"></a>这些虚拟机可否使用现有的本地网络基础结构？
对于通过经典部署模型创建的虚拟机，可以使用 Azure 虚拟网络扩展现有的基础结构。 该方法类似于设立分支机构。 可以预配和管理 Azure 中的虚拟专用网络 (VPN)，并将其安全连接到本地 IT 基础结构。 有关详细信息，请参阅 [Virtual Network Overview](../articles/virtual-network/virtual-networks-overview.md)（虚拟网络概述）。

创建虚拟机时，需要指定虚拟机所属的网络。 不能将现有虚拟机加入虚拟网络。 不过，将虚拟硬盘 (VHD) 与现有虚拟机分离即可解决此问题，然后可以用于创建具有所需网络配置的新虚拟机。

## <a name="how-can-i-access--my-virtual-machine"></a>如何访问虚拟机？
需要通过适用于 Windows VM 的远程桌面连接或适用于 Linux VM 的安全外壳 (SSH) 建立登录虚拟机所需的远程连接。 有关说明，请参阅：

* [如何登录到运行 Windows Server 的虚拟机](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。 除非将服务器配置为远程桌面服务会话主机，否则最多支持 2 个并发连接。  
* [如何登录到运行 Linux 的虚拟机](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。 默认情况下，SSH 允许的并发连接最多为 10 个。 通过编辑配置文件，可以增大此数目。

如果在使用远程桌面或 SSH 时遇到问题，请安装和使用 [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 扩展帮助解决问题。

对于 Windows VM，其他选项包括：

* 在 Azure 经典门户中找到 VM，并单击命令栏中的“重置远程访问”。
* 查看[解决远程桌面连接到基于 Windows 的 Azure 虚拟机的问题](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。
* 使用 Windows PowerShell 远程连接到 VM，或创建其他终结点以方便其他资源连接到 VM。 有关详细信息，请参阅 [How to Set Up Endpoints to a Virtual Machine](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)（如何设置虚拟机的终结点）。

如果熟悉 Hyper-V，可以寻找类似于 VMConnect 的工具。 Azure 不提供类似的工具，因为不支持通过控制台来访问虚拟机。

## <a name="can-i-use-the-temporary-disk-the-d-drive-for-windows-or-devsdb1-for-linux-to-store-data"></a>是否可以使用临时磁盘（Windows 的 D: 盘或 Linux 的 /dev/sdb1）存储数据？
不得使用临时磁盘（Windows 默认的 D: 盘或 Linux 的 /dev/sdb1）存储数据。 这些磁盘只是临时存储空间，因此存在丢失数据且无法恢复数据的风险。 将虚拟机迁移到其他主机时，可能会发生这种情况。 调整虚拟机大小，更新主机和主机硬件故障都是需要迁移动虚拟机的原因。

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>如何更改临时磁盘的驱动器号？
在 Windows 虚拟机中，可以通过移动页面文件和重新分配驱动器号更改驱动器号，但需要确保按特定顺序执行这些步骤。 有关说明，请参阅[更改 Windows 临时磁盘的驱动器号](../articles/virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。

## <a name="how-can-i-upgrade-the-guest-operating-system"></a>如何升级来宾操作系统？
术语“升级”通常是指迁移到更新的操作系统版本，同时保留原有硬件。 对于 Azure VM，迁移到更新的 Linux 版本和 Windows 版本是不同过程：

* 对于 Linux VM，可使用适用于发行版的包管理工具和过程。
* 对于 Windows 虚拟机，需要使用类似 Windows Server 迁移工具的工具迁移服务器。 请勿尝试升级来宾 OS（如果驻留在 Azure 上）。 由于存在失去虚拟机访问权限的风险，因此不支持该功能。 如果在升级过程中出现问题，可能无法启动远程桌面会话，并且无法解决这些问题。

有关 Windows Server 迁移工具和过程的一般详细信息，请参阅 [Migrate Roles and Features to Windows Server](http://go.microsoft.com/fwlink/p/?LinkId=396940)（将角色和功能迁移到 Windows Server）。

## <a name="whats-the-default-user-name-and-password-on-the-virtual-machine"></a>虚拟机的默认用户名和密码是什么？
Azure 提供的映像没有预先配置的用户名和密码。 使用其中一个映像创建虚拟机时，需提供用于登录到虚拟机的用户名和密码。

如果忘记用户名或密码且已安装 VM 代理，可以安装并使用 [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 扩展解决该问题。

其他详细信息：

* 对于 Linux 映像，如果用户使用 Azure 经典门户，系统会提供“azureuser”作为默认用户名，但用户可以更改该用户名，方法是使用“从库中”而非“快速创建”作为创建虚拟机的方法。 使用“从库中”，可以决定登录时是使用密码、SSH 密钥还是同时使用二者。 用户帐户是非特权用户，但具有运行特权命令所需的“sudo”访问权限。 “root”帐户已禁用。
* 对于 Windows 映像，需要在创建 VM 时提供用户名和密码。 该帐户将添加到管理员组。

## <a name="can-azure-run-anti-virus-on-my-virtual-machines"></a>Azure 能否在虚拟机上运行防病毒软件？
Azure 针对防病毒解决方案提供多种选项，但需要用户自行管理。 例如，可能需要另外订阅反恶意软件的软件，并需要自行决定运行扫描和安装更新的时间。 可以在创建 Windows 虚拟机时添加具有适用于 Microsoft 反恶意软件、Symantec Endpoint Protection 或 TrendMicro Deep Security Agent 的 VM 扩展的防病毒支持，也可以稍后进行。 Symantec 和 TrendMicro 扩展允许使用免费的限时试用订阅或使用现有的企业订阅。 Microsoft 反恶意软件是免费的。 有关详细信息，请参阅：

* [如何在 Azure VM 上安装和配置 Symantec Endpoint Protection](http://go.microsoft.com/fwlink/p/?LinkId=404207)
* [如何在 Azure VM 上安装和配置 Trend Micro Deep Security 即服务](http://go.microsoft.com/fwlink/p/?LinkId=404206)
* [在 Azure 虚拟机上部署反恶意软件解决方案](https://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)

## <a name="what-are-my-options-for-backup-and-recovery"></a>哪些选项可用于备份和恢复？
在某些区域，可将 Azure 备份用作预览。 有关详细信息，请参阅[备份 Azure 虚拟机](../articles/backup/backup-azure-vms.md)。 认证合作伙伴提供其他解决方案。 若要了解当前提供的内容，请搜索 Azure Marketplace。

其他选项使用 blob 存储的快照功能。 为此，需要在进行任何依赖 blob 快照的操作前关闭 VM。 这会保存挂起的数据写入，并将文件系统保持为一致状态。

## <a name="how-does-azure-charge-for-my-vm"></a>Azure 如何为 VM 收费？
Azure 根据 VM 的大小和操作系统按小时价格计费。 对于不足一小时的部分，Azure 将仅根据使用的分钟数计费。 如果使用包含特定预安装软件的 VM 映像创建 VM，则还会按小时收取额外软件费用。 对于 VM 的操作系统和数据磁盘，Azure 单独收取存储费用。 临时磁盘存储免费。

当 VM 状态为“正在运行”或“已停止”时计费，但是当 VM 状态为“已停止”（“已释放”）时不计费。 要将 VM 置于“已停止”（“已释放”）状态，请执行以下操作之一：

* 从 Azure 经典门户关闭或删除 VM。
* 使用 Azure PowerShell 模块中提供的 Stop-AzureVM cmdlet。
* 使用服务管理 REST API 中的关闭角色操作，并为 PostShutdownAction 元素指定 StoppedDeallocated。

有关更多详细信息，请参阅 [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/)（虚拟机定价）。

## <a name="will-azure-reboot-my-vm-for-maintenance"></a>Azure 是否会重新启动 VM 进行维护？
在进行 Azure 数据中心定期计划内维护更新时，Azure 有时会重新启动 VM。

当 Azure 检测到影响 VM 的严重硬件问题时，可能会发生计划外维护事件。 对于计划外事件，Azure 自动将 VM 迁移到正常运行的主机，并重新启动 VM。

对于任何独立的 VM（该 VM 不属于可用性集），Azure 在进行计划内维护前提前至少 1 周向订阅服务管理员发送电子邮件通知，因为在更新期间各个 VM 可能会重新启动。 在 VM 上运行的应用程序可能会遭遇停机。

因计划内维护而重新启动时，还可以使用 Azure 经典门户或 Azure PowerShell 查看重新启动日志。 有关详细信息，请参阅[查看 VM 重新启动日志](https://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/)。

要提供冗余，可将两个或多个采用类似配置的 VM 放到同一个可用性集中。 这可以确保在计划内或计划外维护期间至少有一个 VM 可用。 对于此配置，Azure 可以保证某些级别的 VM 可用性。 有关详细信息，请参阅[管理虚拟机的可用性](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

## <a name="additional-resources"></a>其他资源
[关于 Azure 虚拟机](../articles/virtual-machines/virtual-machines-linux-about.md)

[使用 Azure CLI 创建和管理 Linux VM](../articles/virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[使用 Azure PowerShell 创建和管理 Windows VM](../articles/virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

