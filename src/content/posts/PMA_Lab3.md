---
title: "Solving Lab 1 from Practical Malware Analysis "
date: 2026-05-07
tags: [Labs]
description: Solving solution from lab 1. 
---

# Lab 3.1

Analyze the malware found in the file Lab03-01.exe using basic dynamic analysis
tools.

Questions
### 1. What are this malware’s imports and strings?

The import in this malware is `ExitProcess` With strings 
```
Offset	Size	Type	String
01a8	05	A	.text
01cf	06	A	`.data
024e	0b	A	ExitProcess
025a	0c	A	kernel32.dll
0485	06	A	ws2_32
08f2	05	A	thj@h
122d	08	A	advapi32
1247	05	A	ntdll
125e	06	A	user32
14f7	07	A	advpack
169c	21	A	 www.practicalmalwareanalysis.com
1735	2d	A	SOFTWARE\Microsoft\Windows\CurrentVersion\Run
18e5	40	A	SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
1943	07	A	AppData
```

### 2. What are the malware’s host-based indicators?

Using procmon we can record the function and find the reg interference 
![Procomon](/images/Lab3.1procmon.png)

As well as see  56 changes in registry value.

----------------------------------
Total changes: 56
----------------------------------

```

----------------------------------
Keys deleted: 1
----------------------------------
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1\ApplicationViewManagement\W32:00000000001004C6

----------------------------------
Keys added: 3
----------------------------------
HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Control Panel\DeviceRegion
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1\ApplicationViewManagement\W32:00000000001F049A
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1\ApplicationViewManagement\W32:00000000003E03F2

----------------------------------
Values deleted: 1
----------------------------------
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1\ApplicationViewManagement\W32:00000000001004C6\VirtualDesktop: 10 00 00 00 30 30 44 56 CB D7 0D 9B 00 4C BE 40 8D 72 73 7E 8A 6D 4B 82

----------------------------------
Values added: 13
----------------------------------
HKLM\SOFTWARE\Microsoft\FTH\CheckPointTime: 0x0D55976B
HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Control Panel\DeviceRegion\DeviceRegion: 0x000000F4
HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\AmiHivePermissionsCorrect: 0x00000001
HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\AmiHiveOwnerCorrect: 0x00000001
HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\AmiOverridePath: "C:\Windows\AppCompat\Programs\Amcache.hve.tmp"
HKLM\SYSTEM\ControlSet001\Control\Session Manager\PendingFileRenameOperations: "\??\C:\Windows\AppCompat\Programs\Amcache.hve.tmp", "!\??\C:\Windows\AppCompat\Programs\Amcache.hve"
HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\PendingFileRenameOperations: "\??\C:\Windows\AppCompat\Programs\Amcache.hve.tmp", "!\??\C:\Windows\AppCompat\Programs\Amcache.hve"
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FeatureUsage\AppSwitched\C:\Users\flare\AppData\Local\Temp\procexp64.exe: 0x00000008
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FeatureUsage\AppSwitched\C:\Tools\Regshot-x64-Unicode\Regshot-x64-Unicode.exe: 0x00000006
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1\ApplicationViewManagement\W32:00000000001F049A\VirtualDesktop: 10 00 00 00 30 30 44 56 CB D7 0D 9B 00 4C BE 40 8D 72 73 7E 8A 6D 4B 82
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\SessionInfo\1\ApplicationViewManagement\W32:00000000003E03F2\VirtualDesktop: 10 00 00 00 30 30 44 56 CB D7 0D 9B 00 4C BE 40 8D 72 73 7E 8A 6D 4B 82
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Store\C:\Users\flare\Documents\Malware Samples\PracticalMalwareAnalysis-Labs-master\PracticalMalwareAnalysis-Labs\Practical Malware Analysis Labs\BinaryCollection\Chapter_3L\Lab03-01.exe: 53 41 43 50 01 00 00 00 00 00 00 00 07 00 00 00 28 00 00 00 00 1C 00 00 E6 84 00 00 01 00 00 00 00 00 00 00 00 00 00 06 71 00 00 00 50 BB 64 ED DD AC D5 01 00 00 00 00 00 00 00 00 05 00 00 00 10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 06 00 00 00 08 00 00 00 10 10 00 00 00 00 00 00 02 00 00 00 50 00 00 00 00 00 00 00 00 00 00 00 10 10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 13 02 00 00 00 00 00 00 04 00 00 00 01 00 00 00 00 00 00 00 00 00 00 40 10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0A 01 00 00 00 00 00 00 01 00 00 00 00 00 00 00
HKU\S-1-5-21-643046334-913388524-3226014734-1000\SOFTWARE\Sysinternals\Process Monitor\PropertySheetDialog: 2C 00 00 00 00 00 00 00 01 00 00 00 FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF E3 02 00 00 B4 00 00 00 EC 05 00 00 4F 03 00 00

----------------------------------

"Edit: After reviewing, many registry changes were from my analysis tools, not the malware itself. Next time I'll take a clean baseline before execution."

```


### 3. Are there any useful network-based signatures for this malware? If so, what are they?

No using Wireshark to intercept the data there is no any network-based signature as currently while doing dynamic analysis. Static anaylysis showed up network info about `www.practicalmalwareanalysis.com`
This conclusion was on basis that `InetSim` was running on another VM as `DNS server`.

