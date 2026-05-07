---
title: "Setting up lab for malware analysis & Rev Eng"
date: 2026-05-02
tags: [Learning]
description: A quick note on setting up lab for Malware analysis.
---


#### Lab Supplies & Required Software

##### VMware `( Virtualization )` :
Purpose: The main hypervisor software used to create and manage your virtual machines.

##### ISO: `Windows(Flare-VM) , Linux (Remnux)`
Purpose: FLARE-VM for PE analysis, debugging, and unpacking on Windows; REMnux for network traffic inspection, malicious document triage, and script deobfuscation on Linux.

`Host Machine Purposes`:
- Runs VMs
- Store snapshot
- Main internet access

#### Vm architechture

```
    [Windows 11]
        |
        |
      Vmware
    /       \
   /         \
Remnux      Flare 

```
Flare VM:
- Dynamic Malware analysis
- Debugging
- Reverse Eng
- Malware execution

Remnux:
- Static analysis
- Network simulation
- Packet capture
- Malware unpacking
- IOC extraction


---


#### Networking

Use `Custom Host only network`
- `VMnet9` Isolating malware from real internet.

```
Windows:
Ip: 10.10.0.9
DNS: 10.10.0.1
Gateway: 10.10.0.1



Remnux:
Ip: 10.10.0.1

Acting as fake DNS Server
Traffic Monitor
Fake internet


```


### Creating Snapshot

Before running a malware 

- `Create a snapshot `

- `Disable shared-folder`

- `Printer sharing`


---

Now we can use the tools for analysis. This is just a basic setup. You can lookup resources to find the detailed explanation.
It's just an overview on how my lab is configured.

