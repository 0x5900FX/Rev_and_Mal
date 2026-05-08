---
title: "Solving Lab 1.1 from Practical Malware Analysis "
date: 2026-05-07
tags: [Labs]
description: Solving solution from lab 1. 
---



## Lab 1-1 
This lab uses the files Lab01-01.exe and Lab01-01.dll.
Labs are available at https://github.com/mikesiko/PracticalMalwareAnalysis-Labs/

1Upload the files to http://www.VirusTotal.com/ and view the reports. Does either file match any existing antivirus signatures?<br>

Uploading the file to VirusTotal wecan see that file `Lab01-01.exe` & `Lab01-01.dll` We can see that it's detected as a `trojan`.
Where lab1-01.exe is labled as `trojan.ulise/aenjaris` <br>
And as Lab01-01.dll `trojan.skeeyah/r002c0phf20`


2. When were these files compiled?

Lab01-01.exe was compiled at 2010-12-19 16:16:19 UTC
Lab01-01.dll was compiled at 2010-12-19 16:16:38 UTC



3. Are there any indications that either of these files is packed or obfuscated?
If so, what are these indicators?

Using the file `Lab01-01.exe` & `Lab01-01.dll` on `PEID` tools.  We can say that it's not obfuscated and it's a standard dll compiled using `Microsoft Visual C++ 6.0 into DLL`


4. Do any imports hint at what this malware does? If so, which imports
are they?

For lab01-01.exe it doesn't do anything particular
for import of Lab01-01.dll it can be a trojan that has network capability , launch & exit processes, memory management for program. Maybe C2 Communication.


5. Are there any other files or host-based indicators that you could look for
on infected systems?

Using strings on `Lab01-01.exe` we can see that  C:\windows\system32\kerne132.dll to make it seems like legit dll. but its `1` insted of `l`. Thus making it a host-based indicator for infected system.


6. What network-based indicators could be used to find this malware on
infected machines?

Where as string on `Lab01-01.dll` we can see communication to 127.26.152.13 indicating it's compromised and act as network-based indicator for infected systems.


7. What would you guess is the purpose of these files?

We can assume `kerne132.dll` is a backdoor that can be executed with an .exe program prob the lab01-01.exe that can run or install the content of dll.