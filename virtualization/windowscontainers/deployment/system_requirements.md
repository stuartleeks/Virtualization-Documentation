---
title: Windows Container Requirements
description: Windows Container Requirements.
keywords: metadata, containers
author: neilpeterson
manager: timlt
ms.date: 04/20/2016
ms.topic: deployment-article
ms.prod: windows-contianers
ms.service: windows-containers
ms.assetid: 3c3d4c69-503d-40e8-973b-ecc4e1f523ed
---

# Windows Container Requirements

**This is preliminary content and subject to change.** 

This guides list the requirements for a Windows Container Host.

## Windows Containers on a physical System

- The Windows Container role is only available on Windows Server 2016 TP4 (Full and Core) and Nano Server.
- If Hyper-V Containers will be run, the Hyper-V role will need to be installed.

## Windows Containers on a virtual system

If a Windows Container host will be run from a Hyper-V virtual machine, and will also be hosting Hyper-V Containers, nested virtualization will need to be enabled. Nested virtualization has the following requirements:

- At least 4 GB RAM available for the virtualized Hyper-V host.
- Windows Server 2016 Technical Preview 4, or Windows 10 build 10565 on the host system, and Windows Server Technical Preview 4 (Full, Core) or Nano Server in the virtual machine.
- A processor with Intel VT-x (this feature is currently only available for Intel processors).
- The Container host VM will also need at least 2 virtual processors.


## Supported OS Images

Windows Server Technical Preview 4 is being offered with two container OS Images, Windows Server Core and Nano Server. Not all configurations support both OS images. This table details the supported configurations.

<table border="1" style="background-color:FFFFCC;border-collapse:collapse;border:1px solid FFCC00;color:000000;width:75%" cellpadding="5" cellspacing="5">
<thead>
<tr valign="top">
<th><center>Host Operating System</center></th>
<th><center>Windows Server Container</center></th>
<th><center>Hyper-V Container</center></th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td><center>Windows Server 2016 Full UI</center></td>
<td><center>Core OS Image</center></td>
<td><center>Nano OS Image</center></td>
</tr>
<tr valign="top">
<td><center>Windows Server 2016 Core</center></td>
<td><center>Core OS Image</center></td>
<td><center> Nano OS Image</center></td>
</tr>
<tr valign="top">
<td><center>Windows Server 2016 Nano</center></td>
<td><center> Nano OS Image</center></td>
<td><center>Nano OS Image</center></td>
</tr>
</tbody>
</table>
