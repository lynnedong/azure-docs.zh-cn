<!--author=alkohli last changed: 9/17/15-->

#### 完成最低版本的 StorSimple 设备安装

1. 选择设备，然后单击“**快速启动**”。单击“**完成设备安装**”以启动配置设备向导。

2. 在配置设备向导的**基本设置**对话框中，执行以下操作：
  1. 为设备输入一个“**友好名称**”。默认设备名称应反映设备型号和序列号等信息。可指定最多包含 64 个字符的友好名称管理你的设备。
  2. 基于部署设备的地理位置设置“**时区**”。设备将此时区用于所有计划操作。
  3. 在“**DNS 设置**”下，为“**辅助 DNS 服务器**”提供地址。如果使用的是 IPv6，将基于 Windows PowerShell 接口中提供的 IPv6 前缀填充字段。如果未配置辅助 DNS 服务器，则不允许你保存设备配置。
  4. 在启用 iSCSI 的接口下，为 iSCSI 启用至少一个网络。至少一个网络接口需要启用云，一个接口需要启用 iSCSI。DATA 0 自动启用云。
 
      ![StorSimple 最低版本设备安装基本设置](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupBasicSettings1-include.png)

3. 单击箭头图标。![StorSimple 箭头图标](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4. 在“**网络接口**”对话框中，为控制器 0 和控制器 1 提供固定的 IP 地址。**控制器的固定 IP 地址需为子网内可由设备 IP 地址访问的可用 IP。** 如果已为 IPv4 配置 DATA 0 接口，则需要以 IPv4 格式提供固定的 IP 地址。如果已为 IPv6 配置提供前缀，则固定的 IP 地址将自动填充到这些字段中。


    ![StorSimple 最低版本设备安装网络接口](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

    控制器的固定 IP 地址用于向设备提供更新，因此固定 IP 必须可路由并能够连接到 Internet。可以使用 [Test-hcsmconnection][Test] cmdlet 来检查固定控制器 IP 是否可路由。下面的示例演示固定控制器 IP 路由到 Internet，并且可以访问 Microsoft Update 服务器。

     ![Test-HcsmConnection 显示可路由的 IP](./media/storsimple-complete-minimum-device-setup-u1/Test-HcsmConnectionOutputRegisteredDevice.png)

5. 单击选中图标 ![StorSimple 选中图标](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png)。将返回到设备“**快速启动**”页。

 > [AZURE.NOTE] 通过访问“**配置**”页可随时修改所有其他设备设置。

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx

<!---HONumber=AcomDC_0921_2016-->