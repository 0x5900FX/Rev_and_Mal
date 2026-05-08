---
title: "Analysis of A PE header"
date: 2026-05-07
tags: [Learning , Malware_Analysis]
description: Analyzing PE sections 
---

### Introduction to PE structure
The PE format is like a blueprint for Windows programs – it tells the system how to load and run an app, from where to start to what to use.

PE file format is one such COFF format available today for executable, object code, DLLs, FON font files, and core dumps in 32-bit and 64-bit versions of Windows operating systems. Several examples of PE file extensions include.exe,.dll,.scr, and.sys, to name just a few.
And if you ask me what’s on the plate for Linux then? We have an Executable Link File (ELF) format for Linux.

The PE file format contains a header followed by a series of sections.
The header contain metadata about the file itself. After the header actual code section  are included. 

Most common section of a PE file.
- `Dos Header` :  A 64-byte structure located at the very beginning of a Portable Executable (PE) file, designed for backward compatibility to allow Windows applications to run in MS-DOS

- `NT Header` : This is where things get serious. The NT header (starts with ‘PE’) tells Windows, ’Here comes the real program structure – pay attention!



- `.text` : The `.text` section contains the instructions that the CPU executes.
All other sections store data and supporting information. Generally, this is the only section that can execute, and it should be the only section that includes code.

- `.rdata` : Contains both import & export information available to view from PEview , DependencyWalker. This section can also store other read-only data used by the program. Sometimes a file will contain an .idata and .edata section, which store the import and export information

- `.data` : Contain program's global data which is accessible from every part of a program. Local data is not stored in
this section, or anywhere else in the PE file.

- `.rsrc` : This .rsrc section includes the resources used by the executable that are not considered part of the executable, such as icons, images, menus, and strings. 
Strings can be stored either in the .rsrc section or in the main program, but they are often stored in the .rsrc section for
multilanguage support.

![PE section](/images/PE_Section.png)


### Analyzing PE section of Keylogger.
Tool used - PE-Bear

![PE section of keylogger](/images/PE_keylogger.png)

Here the image tells us that it was compiled at `Friday, 05.05.2023 21:31:31 UTC`  Indicating it was an old keylogger and antivirus programs might contain signatures for the malware

Section information for potential Keylogger


| .section | Virtual size  | Raw size |
| ------- | --------- | -------|
| .text    | 1E6DE4 | 1E6E00 | 
| .rsrc    | 56E | 600 | 
| .reloc    | c | 200 | 



