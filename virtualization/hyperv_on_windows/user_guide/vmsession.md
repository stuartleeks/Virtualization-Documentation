# Manage Windows with PowerShell Direct
 
You can use PowerShell Direct to remotely manage a Windows 10 or Windows Server Technical Preview virtual machine from a Windows 10 or Windows Server Technical Preview Hyper-V host. PowerShell Direct allows PowerShell management inside a virtual machine regardless of the network configuration or remote management settings on either the Hyper-V host or the virtual machine. This makes it easier for Hyper-V Administrators to automate and script virtual machine management and configuration.

There are many ways to run PowerShell Direct:  
* As an interactive session -- [go to this section](vmsession.md#create-and-exit-an-interactive-powershell-session) to create and exit a PowerShell Direct session using PSSession cmdlets
* To execute a set of commands or script -- [go to this section](vmsession.md#run-a-script-or-command-with-invoke-command) to run a script or command with the Invoke-Command cmdlet

**New PowerShell Direct features -- 14280 and later**

Starting in Windows builds 14280 and later, PowerShell Direct has received some new functionality:

* Moving data between the virtual machine and the Hyper-V host with persistent PowerShell Direct sessions and [Copy-Item](https://technet.microsoft.com/en-us/library/hh849793.aspx).  
  ``` PowerShell
  $s = New-PSSession -VMName <VMName>
  
  Copy-Item -ToSession $s -Path c:\test\host.txt -Destination c:\test
  Copy-Item -FromSession $s -Path c:\test\host.txt -Destination c:\test2
  ``` 


## Requirements
**Operating system requirements:**
* The host operating system must run Windows 10, Windows Server Technical Preview, or a higher version.
* The virtual machine must run Windows 10, Windows Server Technical Preview, or a higher version.

If you're managing older virtual machines, use Virtual Machine Connection (VMConnect) or [configure a virtual network for the virtual machine](http://technet.microsoft.com/library/cc816585.aspx). 

To create a PowerShell Direct session on a virtual machine,
* The virtual machine must be running locally on the host and booted. 
* You must be logged into the host computer as a Hyper-V administrator.
* You must supply valid user credentials for the virtual machine.

## Create and exit an interactive PowerShell session
1. On the Hyper-V host, open Windows PowerShell as Administrator.

3. Run the one of the following commands to create a session by using the virtual machine name or GUID:  
``` PowerShell
Enter-PSSession -VMName <VMName>
Enter-PSSession -VMGUID <VMGUID>
```

4. Run whatever commands you need to. These commands run on the virtual machine that you created the session with.
5. When you're done, run the following command to close the session:  
``` PowerShell
Exit-PSSession 
``` 

> Note:  If your session won't connect, make sure you're using credentials for the virtual machine you're connecting to -- not the Hyper-V host.

To learn more about these cmdlets, see [Enter-PSSession](http://technet.microsoft.com/library/hh849707.aspx) and [Exit-PSSession](http://technet.microsoft.com/library/hh849743.aspx). 

## Run a script or command with Invoke-Command

You can use the [Invoke-Command](http://technet.microsoft.com/library/hh849719.aspx) cmdlet to run a pre-determined set of commands on the virtual machine. Here is an example of how you can use the Invoke-Command cmdlet where PSTest is the virtual machine name and the script to run (foo.ps1) is in the script directory on the C drive:

 ``` PowerShell
 Invoke-Command -VMName PSTest -FilePath C:\script\foo.ps1 
 ```

To run a single command, use the **-ScriptBlock** parameter:

 ``` PowerShell
 Invoke-Command -VMName PSTest -ScriptBlock { cmdlet } 
 ```

## Troubleshooting

There are a small set of common error messages surfaced through PowerShell Direct.  Here are the most common, some causes, and tools for diagnosing issues.

### -VMName or -VMID parameters don't exist
**Problem:**  
`Enter-PSSession`, `Invoke-Command`, or `New-PSSession` do not have a `-VMName` or `-VMID` parameter.

**Potential causes:**  
The most likely issue is that PowerShell Direct isn't supported by your host operating system.

You can check your Windows build by running the following command:

``` PowerShell
[System.Environment]::OSVersion.Version
```

If you are running a supported build, it is also possible your version of PowerShell does not run PowerShell Direct.  For PowerShell Direct and JEA, the major version must be 5 or later.

You can check your PowerShell version build by running the following command:

``` PowerShell
$PSVersionTable.PSVersion
```


### Error: A remote session might have ended
**Error message:**
```
Enter-PSSession : An error has occurred which Windows PowerShell cannot handle. A remote session might have ended.
```

**Potential causes:**
* The VM is not running
* The guest OS does not support PowerShell Direct (see [requirements](#Requirements))
* PowerShell isn't available in the guest yet
  * The operating system hasn't finished booting
  * The operating system can't boot correctly
  * Some boot time event needs user input
* The guest credentials couldn't be validated
  * The supplied credentials were incorrect
  * There are no user accounts in the guest (the OS hasn't booted before)
  * If connecting as Administrator:  Administrator has not been set as an active user.  Learn more [here](https://technet.microsoft.com/en-us/library/hh825104.aspx).

You can use the [Get-VM](http://technet.microsoft.com/library/hh848479.aspx) cmdlet to check that the credentials you're using have the Hyper-V administrator role and to see which VMs are running locally on the host and booted.

### Error: Parameter set cannot be resolved

**Error message:**  
``` 
Enter-PSSession : Parameter set cannot be resolved using the specified named parameters.
```

**Potential causes:**  
`-RunAsAdministrator` is not supported when connecting to virtual machines.  

PowerShell Direct has different behaviors when connecting to virtual machines versus Windows containers.  When connecting to a Windows container, the `-RunAsAdministrator` flag allows Administrator connections without explicit credentials.  Since virtual machines do not give the host implied administrator access, you need to explicitly enter credentials.

Administrator credentials can be passed to the virtual machine with the `-Credential` parameter or by entering them manually when prompted.


## Samples

Checkout samples on [GitHub](https://github.com/Microsoft/Virtualization-Documentation/search?l=powershell&q=-VMName+OR+-VMGuid&type=Code&utf8=%E2%9C%93).

See [PowerShell Direct snippets](../develop/powershell_snippets.md) for numerous examples of how to use PowerShell Direct in your environment as well as tips and tricks for writing Hyper-V scripts with PowerShell.
