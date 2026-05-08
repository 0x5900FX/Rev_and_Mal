---
title: "Static Analysis of Keylogger "
date: 2026-05-08
tags: [Malware_Analysis , Learning]
description:  Comprehensive notes and hands-on analysis from the Practical Malware Analysis book, covering malware behavior, reverse engineering techniques, debugging, static and dynamic analysis, and real-world malware investigation workflows.
---


### Basic Static Techniques
- Antivirus scanning
- Hashing
- String Analysis



`Packed & Obfuscated Malware`

Obfuscated programs are ones whose executionthe malware author has attempted to hide. 
Packed programs are a subset of obfuscated programs in which the malicious program is compressed and cannot be analyzed.


Static Linking 
When a library is statically linked to an executable, all code from that library is copied into the executable, which
makes the executable grow in size. When analyzing code, it’s difficult to differentiate between statically linked code.

Runtime Linking
Executables that use runtime linking connect to libraries only when that function is needed, not at program start, as with dynamically linked programs.

Commonly used library:
LoadLibrary  
GetProcAddress    
usage : allow a program to access any function in any library on the system


---

### Exploring Dynamically liked program. 

Dependency Walker:
Dependency Walker is a free utility that scans any 32-bit or 64-bit Windows module (exe, dll, ocx, sys, etc.) and builds a hierarchical tree diagram of all dependent modules. For each module found, it lists all the functions that are exported by that module, and which of those functions are actually being called by other modules. 

![Dependency Walker](/images/depn_walker.png)

#### Common Dlls

| DLL | Description |
|---|---|
| `Kernel32.dll` | This is a very common DLL that contains core functionality, such as access and manipulation of memory, files, and hardware. |
| `Advapi32.dll` | This DLL provides access to advanced core Windows components such as the Service Manager and Registry. |
| `User32.dll` | This DLL contains all the user-interface components, such as buttons, scroll bars, and components for controlling and responding to user actions. |
| `Gdi32.dll` | This DLL contains functions for displaying and manipulating graphics. |
| `Ntdll.dll` | This DLL is the interface to the Windows kernel. Executables generally do not import this file directly, although it is always imported indirectly by `Kernel32.dll`. If an executable imports this file, it means that the author intended to use functionality not normally available to Windows programs. Some tasks, such as hiding functionality or manipulating processes, will use this interface. |
| `WSock32.dll` / `Ws2_32.dll` | These are networking DLLs. A program that accesses either of these most likely connects to a network or performs network-related tasks. |
| `Wininet.dll` | This DLL contains higher-level networking functions that implement protocols such as FTP, HTTP, and NTP. |


#### Static Analysis in Practice

Analyzing a keylogger program 
SHA256: d53c05389816a8e7280281873cbb49d2940ab0861d8052b0100f0d4dee9568aa
Origin country : RU

![Dependency Walker](/images/depn_keylogger.png)


List of Imported Dlls:
MSCORE.DLL
KERNEL32.DLL

The imported functionality of KERNEl32.DLL

| Functions | functions |
|---|---|
| AcquireSRWLockExclusive | GetTickCount |
| CloseHandle | GetTimeFormatW |
| CreateEventW | GetVersionExW |
| CreateFileMappingW | GetWindowsDirectoryW |
| CreateFileW | HeapAlloc |
| CreateMutexW | HeapCreate |
| CreateSemaphoreW | HeapDestroy |
| CreateToolhelp32Snapshot | HeapFree |
| DebugBreak | HeapReAlloc |
| DecodePointer | HeapSetInformation |
| DeleteCriticalSection | HeapSize |
| DisableThreadLibraryCalls | HeapValidate |
| EncodePointer | InitializeCriticalSection |
| EnterCriticalSection | InitializeCriticalSectionAndSpinCount |
| ExitProcess | IsDBCSLeadByte |
| FindClose | IsDBCSLeadByteEx |
| FindFirstFileW | IsValidCodePage |
| FlsAlloc | LCMapStringW |
| FlsFree | LeaveCriticalSection |
| FlsGetValue | LoadLibraryExA |
| FlsSetValue | LoadLibraryExW |
| FlushFileBuffers | LoadLibraryW |
| FormatMessageW | LocalFree |
| FreeEnvironmentStringsW | lstrlenW |
| FreeLibrary | MapViewOfFile |
| GetACP | MultiByteToWideChar |
| GetCommandLineA | OutputDebugStringA |
| GetConsoleCP | OutputDebugStringW |
| GetConsoleMode | QueryActCtxW |
| GetCPInfo | QueryPerformanceCounter |
| GetCurrentProcess | RaiseException |
| GetCurrentProcessId | ReadConsoleW |
| GetCurrentThreadId | ReadFile |
| GetDateFormatW | ReadProcessMemory |
| GetEnvironmentStringsW | ReleaseMutex |
| GetEnvironmentVariableW | ReleaseSemaphore |
| GetFileAttributesExW | ReleaseSRWLockExclusive |
| GetFileAttributesW | ResetEvent |
| GetFileSize | RtlCaptureContext |
| GetFileType | RtlLookupFunctionEntry |
| GetFullPathNameW | RtlPcToFileHeader |
| GetLastError | RtlUnwindEx |
| GetLocalTime | RtlVirtualUnwind |
| GetModuleFileNameA | SetErrorMode |
| GetModuleFileNameW | SetEvent |
| GetModuleHandleExW | SetFilePointer |
| GetModuleHandleW | SetLastError |
| GetOEMCP | SetStdHandle |
| GetProcAddress | SetThreadStackGuarantee |
| GetProcessHeap | SetUnhandledExceptionFilter |
| GetStartupInfoW | Sleep |
| GetStdHandle | SleepEx |
| GetStringTypeW | TerminateProcess |
| GetSystemDefaultLCID | TlsAlloc |
| GetSystemDirectoryW | TlsFree |
| GetSystemInfo | TlsGetValue |
| GetSystemTimeAsFileTime | TlsSetValue |
| UnhandledExceptionFilter | UnmapViewOfFile |
| VirtualAlloc | VirtualFree |
| VirtualProtect |  VirtualQuery |
| WaitForSingleObject | WaitForSingleObjectEx |
| WideCharToMultiByte | WriteConsoleW |
| WriteFile |

The import of KERNEL32.DLL can highlight the capablity of this keylogger like 
- `Keylogging functionality (console)`  using<i><b> Readfile, Writefile, ReadConsoleW, WriteConsoleW </i></b>
- `Memory & process manipulation` using <i><b>ReadProcessMemory,TerminateProcess,GetCurrentProcess, 
GetCurrentProcessId , ExitProcess</i></b>
- `Data Stealing` using <i><b> CreateToolhelp32Snapshot </i></b> to Enumerate process, <i><b> ReadProcessMemory </i></b> to read memory of other process.



### Analysis of Keylogger

| Capability | Evidence | Confidence |
| -------------------  | ------------------ | ---------------- |
| Console Keylogging | ReadConsoleW , ReadFile | High |
| Process Theft | ReadProcessMemory,TerminateProcess,GetCurrentProcess | High |
| Data Stealing | CreateToolhelp32Snapshot , ReadProcessMemory | High |
| File Logging  | WriteFile , CreateFileW | Moderate |
| Network Exfiltration | - | None |


This is just a static analysis. It maybe or maynot be correct but a learning experince.
