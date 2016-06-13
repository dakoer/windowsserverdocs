---
title: Windows Time Service Tools and Settings
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: active-directory
ms.suite: na
ms.technology: 
  - active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b43a025f-cce2-4c82-b3ea-3b95d482db3a
author: femila
---
# Windows Time Service Tools and Settings
  
  
**In this section**  
  
-   [Windows Time Service Tools](#w2k3tr_times_tools_dyax)  
  
-   [Windows Time Service Registry Entries](#w2k3tr_times_tools_uhlp)  
  
-   [Windows Time Service Group Policy Settings](#w2k3tr_times_tools_vwtt)  
  
-   [Network Ports Used by the Windows Time Service](#w2k3tr_times_tools_suxb)  
  
-   [Related Information](#w2k3tr_times_tools_qoep)  
  
> [!NOTE]  
> This topic contains information only about tools and settings for Windows Time service \(W32Time\). If you only want to synchronize time for a domain\-joined client computer, see [Configure a client computer for automatic domain time synchronization](http://technet.microsoft.com/en-us/library/cc758905(v=ws.10)). For additional topics about how to configure Windows Time service, see the list of topics in the section [Where to Find Windows Time Service Configuration Information](Windows-Time-Service-Technical-Reference.md#BKMK_Config).  
  
> [!CAUTION]  
> You should not use the Net time command to configure or set time when the Windows Time service is running.  
  
Also, on older computers that run Windows XP or earlier, the command Net time \/querysntp displays the name of a Network Time Protocol \(NTP\) server with which a computer is configured to synchronize, but that NTP server is used only when the computer’s time client is configured as NTP or AllSync. That command has since been deprecated.  
  
Most domain member computers have a time client type of NT5DS, which means that they synchronize time from the domain hierarchy. The only typical exception to this is the domain controller that functions as the primary domain controller \(PDC\) emulator operations master of the forest root domain, which is usually configured to synchronize time with an external time source. To view the time client configuration of a computer, run the W32tm \/query \/configuration command from an elevated Command Prompt in starting in Windows Server 2008, and Windows Vista®, and read the **Type** line in the command output. For more information, see [How Windows Time Service Works](http://go.microsoft.com/fwlink/?LinkId=117753) \(http:\/\/go.microsoft.com\/fwlink\/?LinkId\=117753\). You can run the command **reg query HKLM\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Parameters** and read the value of **NtpServer** in the command output.  
  
> [!IMPORTANT]  
> The W32Time service is not a full\-featured NTP solution that meets time\-sensitive application needs and is not supported by Microsoft as such. For more information, see Microsoft Knowledge Base article 939322, [Support boundary to configure the Windows Time service for high\-accuracy environments](http://go.microsoft.com/fwlink/?LinkID=179459) \(http:\/\/go.microsoft.com\/fwlink\/?LinkID\=179459\).  
  
## <a name="w2k3tr_times_tools_dyax"></a>Windows Time Service Tools  
The following tools are associated with the Windows Time service.  
  
#### W32tm.exe: Windows Time  
**Category**  
  
This tool is installed as part of Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server® 2008 R2  default installations.  
  
**Version compatibility**  
  
This tool works on Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2  default installations.  
  
W32tm.exe is used to configure Windows Time service settings. It can also be used to diagnose problems with the time service. W32tm.exe is the preferred command line tool for configuring, monitoring, or troubleshooting the Windows Time service.  
  
The following tables describe the parameters that are used with W32tm.exe.  
  
**W32tm.exe Primary Parameters**  
  
|Parameter|Description|  
|-------------|---------------|  
|W32tm \/?|W32tm command line help|  
|W32tm \/register|Registers the time service to run as a service and adds default configuration to the registry.|  
|W32tm \/unregister|Unregisters the time service and removes all configuration information from the registry.|  
|w32tm \/monitor<br /><br />\[\/domain:<domain name>\] \[\/computers:<name>\[,<name>\[,<name>...\]\]\] \[\/threads:<num>\]|domain – specifies which domain to monitor. If no domain name is given, or neither the domain nor computers option is specified, the default domain is used. This option might be used more than once.<br /><br />computers – monitors the given list of computers. Computer names are separated by commas, with no spaces. If a name is prefixed with a ‘\*’, it is treated as a PDC. This option might be used more than once.<br /><br />threads – specifies the number of computers to analyze simultaneously. The default value is 3. Allowed range is 1\-50.|  
|w32tm \/ntte <NT time epoch>|Convert an NT system time, in \(10^\-7\)s intervals from 0h 1\-Jan 1601, into a readable format.|  
|w32tm \/ntpte <NTP time epoch>|Convert an NTP time, in \(2^\-32\)s intervals from 0h 1\-Jan 1900, into a readable format.|  
|w32tm \/resync<br /><br />\[\/computer:<computer>\]<br /><br />\[\/nowait\]<br /><br />\[\/rediscover\]<br /><br />\[\/soft\]|Tells a computer that it should resynchronize its clock as soon as possible, throwing out all accumulated error statistics.<br /><br />computer:<computer> – Specifies the computer that should resynchronize. If not specified, the local computer will resynchronize.<br /><br />nowait – do not wait for the resynchronize to occur; return immediately. Otherwise, wait for the resynchronize to complete before returning.<br /><br />rediscover – Redetect the network configuration and rediscover network sources, then resynchronize.<br /><br />soft – resynchronize using existing error statistics. Not useful, provided for compatibility.|  
|w32tm \/stripchart<br /><br />\/computer:<target><br /><br />\[\/period:<refresh>\]<br /><br />\[\/dataonly\]<br /><br />\[\/samples:<count>\]|Display a strip chart of the offset between this computer and another computer.<br /><br />computer:<target> – the computer to measure the offset against.<br /><br />period:<refresh> – the time between samples, in seconds. The default is 2s.<br /><br />dataonly – display only the data without graphics.<br /><br />samples:<count> – collect <count> samples, then stop. If not specified, samples will be collected until **Ctrl\+C** is pressed.|  
|w32tm \/config<br /><br />\[\/computer:<target>\]<br /><br />\[\/update\]<br /><br />\[\/manualpeerlist:<peers>\]<br /><br />\[\/syncfromflags:<source>\]<br /><br />\[\/LocalClockDispersion:<seconds>\]<br /><br />\[\/reliable:\(YES&#124;NO\)\]<br /><br />\[\/largephaseoffset:<milliseconds>\]|computer:<target> – adjusts the configuration of <target>. If not specified, the default is the local computer.<br /><br />update – notifies the time service that the configuration has changed, causing the changes to take effect.<br /><br />manualpeerlist:<peers> – sets the manual peer list to <peers>, which is a space\-delimited list of DNS and\/or IP addresses. When specifying multiple peers, this option must be enclosed in quotes.<br /><br />syncfromflags:<source> – sets what sources the NTP client should synchronize from. <source> should be a comma separated list of these keywords \(not case sensitive\):<br /><br />MANUAL – include peers from the manual peer list.<br /><br />DOMHIER – synchronize from a domain controller \(DC\) in the domain hierarchy.<br /><br />LocalClockDispersion:<seconds> – configures the accuracy of the internal clock that W32Time will assume when it can’t acquire time from its configured sources.<br /><br />reliable:\(YES&#124;NO\) – set whether this computer is a reliable time source.<br /><br />This setting is only meaningful on domain controllers.<br /><br />YES – this computer is a reliable time service.<br /><br />NO – this computer is not a reliable time service.<br /><br />largephaseoffset:<milliseconds> – sets the time difference between local and network time which W32Time will consider a spike.|  
|w32tm \/tz|Display the current time zone settings.|  
|w32tm \/dumpreg<br /><br />\[\/subkey:<key>\]<br /><br />\[\/computer:<target>\]|Display the values associated with a given registry key.<br /><br />The default key is HKLM\\System\\CurrentControlSet\\Services\\W32Time<br /><br />\(the root key for the time service\).<br /><br />subkey:<key> – displays the values associated with subkey <key> of the default key.<br /><br />computer:<target> – queries registry settings for computer <target>|  
|w32tm \/query \[\/computer:<target>\] {\/source &#124; \/configuration &#124; \/peers &#124; \/status} \[\/verbose\]|This parameter was first made available in the Windows Time client versions of Windows Vista, and  Windows Server 2008 .<br /><br />Display a computer's Windows Time service information.<br /><br />**computer:<target>** – Query the information of **<target>**. If not specified, the default value is the local computer.<br /><br />**Source** – Display the time source.<br /><br />**Configuration** – Display the configuration of run time and where the setting comes from. In verbose mode, display the undefined or unused setting too.<br /><br />**peers** – Display a list of peers and their status.<br /><br />**status** – Display Windows Time service status.<br /><br />**verbose** – Set the verbose mode to display more information.|  
|w32tm \/debug {\/disable &#124; {\/enable \/file:<name> \/size:<bytes> \/entries:<value> \[\/truncate\]}}|This parameter was first made available in the Windows Time client versions of Windows Vista, and  Windows Server 2008 .<br /><br />Enable or disable the local computer Windows Time service private log.<br /><br />**disable** – Disable the private log.<br /><br />**enable** – Enable the private log.<br /><br />-   **file:<name>** – Specify the absolute file name.<br />-   **size:<bytes>** – Specify the maximum size for circular logging.<br />-   **entries:<value>** – Contains a list of flags, specified by number and separated by commas, that specify the types of information that should be logged. Valid numbers are 0 to 300. A range of numbers is valid, in addition to single numbers, such as 0\-100,103,106. Value 0\-300 is for logging all information.<br /><br />**truncate** – Truncate the file if it exists.|  
  
For more information about **W32tm.exe**, see Help and Support Center in Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2 .  
  
## <a name="w2k3tr_times_tools_uhlp"></a>Windows Time Service Registry Entries  
The following registry entries are associated with the Windows Time service.  
  
This information is provided as a reference for use in troubleshooting or verifying that the required settings are applied. It is recommended that you do not directly edit the registry unless there is no other alternative. Modifications to the registry are not validated by the registry editor or by Windows before they are applied, and as a result, incorrect values can be stored. This can result in unrecoverable errors in the system.  
  
When possible, use Group Policy or other Windows tools, such as Microsoft Management Console \(MMC\), to accomplish tasks rather than editing the registry directly. If you must edit the registry, use extreme caution.  
  
> [!WARNING]  
> Some of the preset values that are configured in the System Administrative template file \(System.adm\) for the Group Policy object \(GPO\) settings are different from the corresponding default registry entries. If you plan to use a GPO to configure any Windows Time setting, be sure that you review [Preset values for the Windows Time service Group Policy settings are different from the corresponding Windows Time service registry entries in Windows Server 2003](http://go.microsoft.com/fwlink/?LinkId=186066) \(http:\/\/go.microsoft.com\/fwlink\/?LinkId\=186066\). This issue applies to  Windows Server 2008 R2 ,  Windows Server 2008 , Windows Server 2003 R2, and Windows Server 2003.  
  
Many registry entries for the Windows Time service are the same as the Group Policy setting of the same name. The Group Policy settings correspond to the registry entries of the same name located in:  
  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\**.  
  
There are several registry keys at this registry location. The Windows Time settings are stored in values across all of these keys.  
  
> [!NOTE]  
> Many of the values in the W32Time section of the registry are used internally by W32Time to store information. These values should not be manually changed at any time. Do not modify any of the settings in this section unless you are familiar with the setting and are certain that the new value will work as expected. The following registry entries are located under **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\**  
  
Some of the parameters are stored in clock ticks in the registry and some are in seconds. To convert the time from clock ticks to seconds:  
  
-   1 minute \= 60 sec  
  
-   1 sec \= 1000 ms  
  
-   1 ms \= 10,000 clock ticks on a Windows system, as described at [DateTime.Ticks Property](http://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx).  
  
For example, 5 minutes would become 5\*60\*1000\*10000 \= 3000000000 clock ticks.  
  
#### AllowNonstandardModeCombinations  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpServer**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry indicates that non\-standard mode combinations are allowed in synchronization between peers. The default value for domain members is 1. The default value for stand\-alone clients and servers is 1.  
  
#### AllowNonstandardModeCombinations  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry indicates that non\-standard mode combinations are allowed in synchronization between clients and servers. The default value for domain members is 1. The default value for stand\-alone clients and servers is 1.  
  
#### AnnounceFlags  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls whether this computer is marked as a reliable time server. A computer is not marked as reliable unless it is also marked as a time server.  
  
-   0x00 Not a time server  
  
-   0x01 Always time server  
  
-   0x02 Automatic time server  
  
-   0x04 Always reliable time server  
  
-   0x08 Automatic reliable time server  
  
The default value for domain members is 10. The default value for stand\-alone clients and servers is 10.  
  
#### CompatibilityFlags  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the following compatibility flags and values:  
  
-   DispersionInvalid: 0x00000001  
  
-   IgnoreFutureRefTimeStamp: 0x00000002  
  
-   AutodetectWin2K: 0x80000000  
  
-   AutodetectWin2KStage2: 0x40000000  
  
The default value for domain members is 0x80000000. The default value for stand\-alone clients and servers is 0x80000000.  
  
#### CrossSiteSyncFlags  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry determines whether the service chooses synchronization partners outside the domain of the computer. The options and values are:  
  
-   None: 0  
  
-   PdcOnly: 1  
  
-   All: 2  
  
This value is ignored if the NT5DS value is not set. The default value for domain members is 2. The default value for stand\-alone clients and servers is 2.  
  
#### DllName  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the location of the DLL for the time provider.  
  
The default location for this DLL on both domain members and stand\-alone clients and servers is %windir%\\System32\\W32Time.dll.  
  
#### DllName  
Registry path  
  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpServer**  
  
Version  
  
Windows Server 2003 Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the location of the DLL for the time provider.  
  
The default location for this DLL on both domain members and stand\-alone clients and servers is %windir%\\System32\\W32Time.dll.  
  
#### Enabled  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry indicates if the NtpClient provider is enabled in the current Time Service.  
  
-   Yes 1  
  
-   No 0  
  
The default value on domain members is 1. The default value on stand\-alone clients and servers is 1.  
  
#### Enabled  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpServer**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry indicates if the NtpServer provider is enabled in the current Time Service.  
  
-   Yes 1  
  
-   No 0  
  
The default value on domain members is 1. The default value on stand\-alone clients and servers is 1.  
  
#### EventLogFlags  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the events that the time service logs.  
  
-   Time Jump: 0x1  
  
-   Source Change: 0x2  
  
The default value on domain members is 2. The default value on stand\-alone clients and servers is 2.  
  
#### EventLogFlags  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the events logged by the Windows Time service.  
  
-   0x1 reachability changes  
  
-   0x2 large sample skew \(This is applicable to Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2  only\)  
  
The default value on domain members is 0x1. The default value on stand\-alone clients and servers is 0x1.  
  
#### FrequencyCorrectRate  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the rate at which the clock is corrected. If this value is too small, the clock is unstable and overcorrects. If the value is too large, the clock takes a long time to synchronize. The default value on domain members is 4. The default value on stand\-alone clients and servers is 4.  
  
> [!NOTE]  
> 0 is an invalid value for the FrequencyCorrectRate registry entry. On Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2 computers, if the value is set to 0 the Windows Time service will automatically change it to 1.  
  
#### HoldPeriod  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the period of time for which spike detection is disabled in order to bring the local clock into synchronization quickly. A spike is a time sample indicating that time is off a number of seconds, and is usually received after good time samples have been returned consistently. The default value on domain members is 5. The default value on stand\-alone clients and servers is 5.  
  
#### InputProvider  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry indicates if the NtpClient provider is enabled.  
  
-   Yes 1  
  
-   No 0  
  
The default value on domain members is 1. The default value on stand\-alone clients and servers is 1.  
  
#### InputProvider  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpServer**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry indicates if the NtpServer provider is enabled.  
  
-   Yes 1  
  
-   No 0  
  
The default value on domain members is 1. The default value on stand\-alone clients and servers is 1.  
  
#### LargePhaseOffset  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies that a time offset greater than or equal to this value in 10<sup>-7</sup> seconds is considered a spike. A network disruption such as a large amount of traffic might cause a spike. A spike will be ignored unless it persists for a long period of time. The default value on domain members is 50000000. The default value on stand\-alone clients and servers is 50000000.  
  
#### LargeSampleSkew  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows Server 2003 and  Windows Server 2008   
  
This entry specifies the large sample skew for logging in seconds. To comply with Security and Exchange Commission \(SEC\) specifications, this should be set to three seconds. Events will be logged for this setting only when EventLogFlags is explicitly configured for 0x2 large sample skew. The default value on domain members is 3. The default value on stand\-alone clients and servers is 3.  
  
#### LastClockRate  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry is maintained by W32Time. It contains reserved data that is used by the Windows operating system, and any changes to this setting can cause unpredictable results. The default value on domain members is 156250. The default value on stand\-alone clients and servers is 156250.  
  
#### LocalClockDispersion  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the dispersion \(in seconds\) that you must assume when the only time source is the built\-in CMOS clock. The default value on domain members is 10. The default value on stand\-alone clients and servers is 10.  
  
#### MaxAllowedPhaseOffset  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the maximum offset \(in seconds\) for which W32Time attempts to adjust the computer clock by using the clock rate. When the offset exceeds this rate, W32Time sets the computer clock directly. The default value for domain members is 300. The default value for stand\-alone clients and servers is 1.  
  
In order for W32Time to set the computer clock gradually, the offset must be less than the MaxAllowedPhaseOffset value and satisfy the following equation at the same time:  
  
|CurrentTimeOffset| \/ \(PhaseCorrectRate\*UpdateInterval\) < SystemClockRate \/ 2  
  
The CurrentTimeOffset is measured in clock ticks, where 1ms \= 10,000 clock ticks on a Windows system.  
  
SystemClockRate and PhaseCorrectRate are also measured in clock ticks. To get the SystemClockRate, you can use the following command and convert it from seconds to clock ticks using the formula of seconds\*1000\*10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate is the rate of the clock on the system. Using 156000 seconds as an example, the SystemclockRate would be \= 0.0156000 \* 1000 \* 10000 \= 156000 clock ticks.  
  
MaxAllowedPhaseOffset is also in seconds. To convert it to clock ticks, multiply MaxAllowedPhaseOffset\*1000\*10000.  
  
The following two examples show how to apply  
  
Example 1: Time differs by 4 minutes \(For example, your time is 11:05 AM and the time sample received from a peer and believed to be correct is 11:09 AM\).  
  
phasecorrectRate \= 1  
  
UpdateInterval \= 30000 \(clock ticks\)  
  
systemclockRate \= 156000 \(clock ticks\)  
  
MaxAllowedPhaseOffset \= 10min \= 600 seconds \= 600\*1000\*10000\=6000000000 clock ticks  
  
|currentTimeOffset| \= 4mins \= 4\*60\*1000\*10000 \= 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 \= TRUE  
  
AND does it satisfy the above equation? \(|CurrentTimeOffset| \/ \(PhaseCorrectRate\*UpdateInterval\) < SystemClockRate \/ 2\)  
  
Is 2,400,000,000 \/ \(30000\*1\) < 156000\/2  
  
Is 100,000 < 78,000  
  
NO\/FALSE  
  
Therefore W32tm would set the clock back immediately.  
  
> [!NOTE]  
> In this case, if you want to set the clock back slowly, you would need to adjust the values of PhaseCorrectRate or updateInterval in the registry as well to ensure the equation results in TRUE.  
  
Example 2: Time differs by 3 minutes.  
  
phasecorrectRate \= 1  
  
UpdateInterval \= 30000 \(clock ticks\)  
  
systemclockRate \= 156000 \(clock ticks\)  
  
MaxAllowedPhaseOffset \= 10min \= 600 seconds \= 600\*1000\*10000\=6000000000 clock ticks  
  
currentTimeOffset \= 3mins \= 3\*60\*1000\*10000 \= 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 \= TRUE  
  
AND does it satisfy the above equation? \(|CurrentTimeOffset| \/ \(PhaseCorrectRate\*UpdateInterval\) < SystemClockRate \/ 2\)  
  
Is 3 mins \(1,800,000,000\) \/ \(30000\*1\) < 156000\/2  
  
Is 60,000 < 78,000  
  
YES\/TRUE  
  
In this case the clock will be set back slowly.  
  
#### MaxClockRate  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry is maintained by W32Time. It contains reserved data that is used by the Windows operating system, and any changes to this setting can cause unpredictable results. The default value for domain members is 155860. The default value for stand\-alone clients and servers is 155860.  
  
#### MaxNegPhaseCorrection  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the largest negative time correction in seconds that the service makes. If the service determines that a change larger than this is required, it logs an event instead. Special case: 0xFFFFFFFF means always make time correction. The default value for domain members is 0xFFFFFFFF. The default value for stand\-alone clients and servers is 54,000 \(15 hrs\).  
  
#### MaxPollInterval  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the largest interval, in log2 seconds, allowed for the system polling interval. Note that while a system must poll according to the scheduled interval, a provider can refuse to produce samples when requested to do so. The default value for domain controllers is 10. The default value for domain members is 15. The default value for stand\-alone clients and servers is 15.  
  
#### MaxPosPhaseCorrection  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2 .  
  
This entry specifies the largest positive time correction in seconds that the service makes. If the service determines that a change larger than this is required, it logs an event instead. Special case: 0xFFFFFFFF means always make time correction. The default value for domain members is 0xFFFFFFFF. The default value for stand\-alone clients and servers is 54,000 \(15 hrs\).  
  
#### MinClockRate  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry is maintained by W32Time. It contains reserved data that is used by the Windows operating system, and any changes to this setting can cause unpredictable results. The default value for domain members is 155860. The default value for stand\-alone clients and servers is 155860.  
  
#### MinPollInterval  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the smallest interval, in log2 seconds, allowed for the system polling interval. Note that while a system does not request samples more frequently than this, a provider can produce samples at times other than the scheduled interval. The default value for domain controllers is 6. The default value for domain members is 10. The default value for stand\-alone clients and servers is 10.  
  
#### NtpServer  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Parameters**  
  
###### Version  
Windows Server 2003 and  Windows Server 2008   
  
This entry specifies a space\-delimited list of peers from which a computer obtains time stamps, consisting of one or more DNS names or IP addresses per line. Each DNS name or IP address listed must be unique. Computers connected to a domain must synchronize with a more reliable time source, such as the official U.S. time clock.  
  
-   0x01 SpecialInterval  
  
-   0x02 UseAsFallbackOnly  
  
-   0x04 SymmetricActive  
  
    For more information about this mode, see [Windows Time Server: 3.3 Modes of Operation](http://go.microsoft.com/fwlink/?LinkId=208012) \(http:\/\/go.microsoft.com\/fwlink\/?LinkId\=208012\).  
  
-   0x08 Client  
  
There is no default value for this registry entry on domain members. The default value on stand\-alone clients and servers is time.windows.com,0x1.  
  
> [!NOTE]  
> For more information on available NTP Servers, see [Microsoft Knowledge Base article 262680](http://go.microsoft.com/fwlink/?LinkId=186067) \(http:\/\/go.microsoft.com\/fwlink\/?LinkId\=186067\)  
  
#### PhaseCorrectRate  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the rate at which the phase error is corrected. Specifying a small value corrects the phase error quickly, but might cause the clock to become unstable. If the value is too large, it takes a longer time to correct the phase error.  
  
The default value on domain members is 1. The default value on stand\-alone clients and servers is 7.  
  
> [!NOTE]  
> 0 is an invalid value for the PhaseCorrectRate registry entry. On Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2 computers, if the value is set to 0, the Windows Time service automatically changes it to 1.  
  
#### PollAdjustFactor  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the decision to increase or decrease the poll interval for the system. The larger the value, the smaller the amount of error that causes the poll interval to be decreased. The default value on domain members is 5. The default value on stand\-alone clients and servers is 5.  
  
#### ResolvePeerBackOffMaxTimes  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the maximum number of times to double the wait interval when repeated attempts to locate a peer to synchronize with fail. A value of zero means that the wait interval is always the minimum. The default value on domain members is 7. The default value on stand\-alone clients and servers is 7.  
  
#### ResolvePeerBackOffMinutes  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the initial interval to wait, in minutes, before attempting to locate a peer to synchronize with. The default value on domain members is 15. The default value on stand\-alone clients and servers is 15.  
  
#### ServiceDll  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Parameters**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry is maintained by W32Time. It contains reserved data that is used by the Windows operating system, and any changes to this setting can cause unpredictable results. The default location for this DLL on both domain members and stand\-alone clients and servers is %windir%\\System32\\W32Time.dll.  
  
#### ServiceMain  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Parameters**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry is maintained by W32Time. It contains reserved data that is used by the Windows operating system, and any changes to this setting can cause unpredictable results. The default value on domain members is SvchostEntry\_W32Time. The default value on stand\-alone clients and servers is SvchostEntry\_W32Time.  
  
#### SpecialPollInterval  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the special poll interval in seconds for manual peers. When the SpecialInterval 0x1 flag is enabled, W32Time uses this poll interval instead of a poll interval determine by the operating system. The default value on domain members is 3,600. The default value on stand\-alone clients and servers is 604,800.  
  
#### SpecialPollTimeRemaining  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\TimeProviders\\NtpClient**  
  
###### Version  
Windows Server 2003 and  Windows Server 2008   
  
This entry is maintained by W32Time. It contains reserved data that is used by the Windows operating system. It specifies the time in seconds before W32Time will resynchronize after the computer has restarted. Any changes to this setting can cause unpredictable results. The default value on both domain members and on stand\-alone clients and servers is left blank.  
  
#### SpikeWatchPeriod  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry specifies the amount of time that a suspicious offset must persist before it is accepted as correct \(in seconds\). The default value on domain members is 900. The default value on stand\-alone clients and workstations is 900.  
  
#### Type  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Parameters**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry Indicates which peers to accept synchronization from:  
  
-   **NoSync.** The time service does not synchronize with other sources.  
  
-   **NTP.** The time service synchronizes from the servers specified in the **NtpServer.** registry entry.  
  
-   **NT5DS.** The time service synchronizes from the domain hierarchy.  
  
-   **AllSync.** The time service uses all the available synchronization mechanisms.  
  
The default value on domain members is **NT5DS.** The default value on stand\-alone clients and servers is **NTP.**  
  
#### UpdateInterval  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows XP, Windows Vista,  Windows 7 , Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2 .  
  
This entry specifies the number of clock ticks between phase correction adjustments. The default value for domain controllers is 100. The default value for domain members is 30,000. The default value for stand\-alone clients and servers is 360,000.  
  
> [!NOTE]  
> 0 is an invalid value for the UpdateInterval registry entry. On computers running Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2 , if the value is set to 0 the Windows Time service automatically changes it to 1.  
  
The following three registry entries are not a part of the W32Time default configuration but can be added to the registry to obtain increased logging capabilities. The information logged to the System Event log can be modified by changing value for the EventLogFlags setting in the Group Policy Object Editor. By default, the time service creates a log in Event Viewer every time that it switches to a new time source.  
  
> [!WARNING]  
> Some of the preset values that are configured in the System Administrative template file \(System.adm\) for the Group Policy object \(GPO\) settings are different from the corresponding default registry entries. If you plan to use a GPO to configure any Windows Time setting, be sure that you review [Preset values for the Windows Time service Group Policy settings are different from the corresponding Windows Time service registry entries in Windows Server 2003](http://go.microsoft.com/fwlink/?LinkId=186066) \(http:\/\/go.microsoft.com\/fwlink\/?LinkId\=186066\). This issue applies to  Windows Server 2008 R2 ,  Windows Server 2008 , Windows Server 2003 R2, and Windows Server 2003.  
  
The following registry entries must be added in order to enable W32Time logging:  
  
#### FileLogEntries  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the amount of entries created in the Windows Time log file. The default value is none, which does not log any Windows Time activity. Valid values are 0 to 300. This value does not affect the event log entries normally created by Windows Time.  
  
#### FileLogName  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the location and file name of the Windows Time log. The default value is blank, and should not be changed unless **FileLogEntries** is changed. A valid value is a full path and file name that Windows Time will use to create the log file. This value does not affect the event log entries normally created by Windows Time.  
  
#### FileLogSize  
  
###### Registry path  
**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\W32Time\\Config**  
  
###### Version  
Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2   
  
This entry controls the circular logging behavior of Windows Time log files. When **FileLogEntries** and **FileLogName** are defined, this entry defines the size, in bytes, to allow the log file to reach before overwriting the oldest log entries with new entries. Any positive number is valid, and 3000000 is recommended. This value does not affect the event log entries normally created by Windows Time.  
  
## <a name="w2k3tr_times_tools_vwtt"></a>Windows Time Service Group Policy Settings  
You can configure most W32Time parameters by using the Group Policy Object Editor. This includes configuring a computer to be an NTPServer or NTPClient, configuring the time synchronization mechanism, and configuring a computer to be a reliable time source.  
  
> [!NOTE]  
> Group Policy settings for the Windows Time service can be configured on Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2  domain controllers and can be applied only to computers running Windows Server 2003, Windows Server 2003 R2,  Windows Server 2008 , and  Windows Server 2008 R2 .  
  
You can find the Group Policy settings used to configure W32Time in the Group Policy Object Editor snap\-in in the following locations:  
  
-   Computer Configuration\\Administrative Templates\\System\\Windows Time Service  
  
    Configure **Global Configuration Settings** here.  
  
-   Computer Configuration\\Administrative Templates\\System\\Windows Time Service\\Time Providers  
  
    Configure **Windows NTP Client** settings here.  
  
    Enable **Windows NTP Client** here.  
  
    Enable **Windows NTP Server** here.  
  
> [!WARNING]  
> Some of the preset values that are configured in the System Administrative template file \(System.adm\) for the Group Policy object \(GPO\) settings are different from the corresponding default registry entries. If you plan to use a GPO to configure any Windows Time setting, be sure that you review [Preset values for the Windows Time service Group Policy settings are different from the corresponding Windows Time service registry entries in Windows Server 2003](http://go.microsoft.com/fwlink/?LinkId=186066) \(http:\/\/go.microsoft.com\/fwlink\/?LinkId\=186066\). This issue applies to  Windows Server 2008 R2 ,  Windows Server 2008 , Windows Server 2003 R2, and Windows Server 2003.  
  
The following table lists the global Group Policy settings that are associated with the Windows Time service and the pre\-set value associated with each setting. For more information about each setting, see the corresponding registry entries in “[Windows Time Service Registry Entries](#w2k3tr_times_tools_uhlp)” earlier in this subject. The following settings are contained in a single GPO called **Global Configuration Settings**.  
  
**Global Group Policy Settings Associated with Windows Time**  
  
|Group Policy Setting|Pre\-Set Value|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54,000 \(15 hours\)|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54,000 \(15 hours\)|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
The following table lists the available settings for the **Configure Windows NTP Client** GPO and the pre\-set values that are associated with the Windows Time service. For more information about each setting, see the corresponding registry entries in “[Windows Time Service Registry Entries](#w2k3tr_times_tools_uhlp)” earlier in this subject.  
  
**NTP Client Group Policy Settings Associated with Windows Time**  
  
|Group Policy Setting|Default Value|  
|------------------------|-----------------|  
|NtpServer|time.windows.com,0x1|  
|Type|Default options:<br /><br />-   **NTP.** Use on computers that are not joined to a domain.<br />-   **NT5DS.** Use on computers that are joined to a domain.|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="w2k3tr_times_tools_suxb"></a>Network Ports Used by the Windows Time Service  
Windows Time follows the NTP specification, which requires the use of UDP port 123 for all time synchronization communication. This port is reserved by Windows Time and remains reserved at all times. Whenever the computer synchronizes its clock or provides time to another computer, that communication is performed on UDP port 123.  
  
> [!NOTE]  
> If you have a computer with multiple network adapters \(also called a multihomed computer\), you cannot selectively enable the Windows Time service based on the network adapter.  
  
## <a name="w2k3tr_times_tools_qoep"></a>Related Information  
The following resources contain additional information that is relevant to this section.  
  
-   RFC *1305* in the IETF RFC Database  
  
## See Also  
[Videos about the Windows Time Service](http://go.microsoft.com/fwlink/?LinkID=197277)  
  
