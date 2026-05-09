---
title: "Dynamic analysis of a keylogger"
date: 2026-05-02
tags: [Learning]
description: Analyzing a keylogger dynamically.
---


Sandboxes: The Quick-and-Dirty Approach:
Easiest way to perform basic dynamic analysis.

Running Malware :
`C:\>rundll32.exe DLLname, Export arguments`

The file rip.dll has the following exports:
Install
Uninstall

Install appears to be a likely way to launch rip.dll, so let’s launch the malware
as follows:
`C:\>rundll32.exe rip.dll, Install`


DLL malware may also need to be installed as a service, sometimes with a
convenient export such as InstallService, as listed in ipr32x.dll:
`C:\>rundll32 ipr32x.dll,InstallService ServiceName`

`C:\>net start ServiceName`

---


# Basic Dynamic Malware analysis process.

1. Running procmon and setting a filter on the malware executable name
and clearing out all events just before running.
2. Starting Process Explorer.
3. Gathering a first snapshot of the registry using Regshot.
Basic Dynamic Analysis 57
4. Setting up your virtual network to your liking using INetSim and
ApateDNS.
5. Setting up network traffic logging using Wireshark.