---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2017-10-03T09:24:38Z"
guid: http://trukhanov.com/?p=157
id: 157
title: Association between virtual ethernet and virtual switch
url: /2017/10/association-between-virtual-ethernet-and-virtual-switch/
---
You&#8217;ve just created couple of internal network switches in your Hyper-V Manager. Or you&#8217;ve just installed Docker for Windows and played with it a little. Now want to create NAT to allow your fresh new containers and/or virtual machines to have access to the Internet. Unfortunately there is no simple way to say which of the HNS Internal NIC adapters associated with proper vSwitch.
<!--more-->

I&#8217;ve created simple script to show associations between virtual Ethernet interface and vSwitch. Here it is:

```powershell
$net_adapters = Get-NetAdapter
foreach($ethernet_port in gwmi -Namespace Root\Virtualization\V2 -Class Msvm_InternalEthernetPort){
	$endpoint_physical = gwmi -Namespace Root\Virtualization\V2 -Query "ASSOCIATORS OF {$ethernet_port} WHERE ResultClass=Msvm_LANEndpoint AssocClass=Msvm_EthernetDeviceSAPImplementation"
	$endpoint_virtual = gwmi -Namespace Root\Virtualization\V2 -Query "ASSOCIATORS OF {$endpoint_physical} where ResultClass=Msvm_LANEndpoint assocclass=Msvm_ActiveConnection"
	$ethernetswitchport = gwmi -Namespace Root\Virtualization\V2 -Query "ASSOCIATORS OF {$endpoint_virtual}  WHERE ResultClass=Msvm_EthernetSwitchPort AssocClass=Msvm_EthernetDeviceSAPImplementation"
	$vswitch = gwmi -Namespace Root\Virtualization\V2 -Query "ASSOCIATORS OF {$ethernetswitchport} WHERE ResultClass=Msvm_VirtualEthernetSwitch"
	
	$net_adapter = $net_adapters | ?{($_).MacAddress -replace '-','' -eq $ethernet_port.PermanentAddress}
	Write-Host "Adapter:" $net_adapter.Name 
	Write-Host "Switch:" $vswitch.ElementName
	Write-Host
}
```
