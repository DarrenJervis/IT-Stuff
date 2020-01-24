##### Variables for common values
$resourceGroup = "DJGRP"
$location = "uksouth"
$vmName = "DJVM2"

##### Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

##### Create a resource group
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

##### Create a virtual machine
New-AzureRmVM `
  -ResourceGroupName $resourceGroup `
  -Name $vmName `
  -Location $location `
  -ImageName "Win2016Datacenter" `
  -VirtualNetworkName "DJVnet" `
  -SubnetName "DJSub" `
  -SecurityGroupName "DJNsg" `
  -PublicIpAddressName "DJPublic2" `
  -Credential $cred `
  -OpenPorts 80

##### Install IIS
$PublicSettings = '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}'

Set-AzureRmVMExtension -ExtensionName "IIS" -ResourceGroupName $resourceGroup -VMName $vmName `
  -Publisher "Microsoft.Compute" -ExtensionType "CustomScriptExtension" -TypeHandlerVersion 1.4 `
  -SettingString $PublicSettings -Location $location