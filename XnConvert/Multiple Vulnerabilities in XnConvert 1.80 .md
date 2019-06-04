# Description

Multiple vulnerabilities was found in XnSoft Software XnConvert(3 OOB read and 1infinite loop). 

## Affected version
 XnConvert 1.80

## Affected platform
All the vulnerabilities are tested with Windows versions ,but may exists in other platform versions.

# Vulnerability Details
The poc files loated in <https://github.com/JsHuang/XnSoft-Security-Advisory/tree/master/XnConvert/poc>

Details about the vulnerabilities are as below. 

## 1. OOB-Read 

windbg result:

```bash
FAULTING_IP: 
ntdll!RtlpNtEnumerateSubKey+1b25
7755e653 eb12            jmp     ntdll!RtlpNtEnumerateSubKey+0x1b39 (7755e667)

EXCEPTION_RECORD:  ffffffff -- (.exr 0xffffffffffffffff)
ExceptionAddress: 7755e653 (ntdll!RtlpNtEnumerateSubKey+0x00001b25)
   ExceptionCode: c0000374
  ExceptionFlags: 00000001
NumberParameters: 1
   Parameter[0]: 77594268

FAULTING_THREAD:  0000090c

PROCESS_NAME:  xnconvert.exe

ADDITIONAL_DEBUG_TEXT:  
Use '!findthebuild' command to search for the target build information.
If the build information is available, run '!findthebuild -s ; .reload' to set symbol path and load symbols.

FAULTING_MODULE: 76ac0000 kernel32

DEBUG_FLR_IMAGE_TIMESTAMP:  5c177ca7

MODULE_NAME: Qt5Gui

ERROR_CODE: (NTSTATUS) 0xc0000374 - <Unable to get error code text>

EXCEPTION_CODE: (NTSTATUS) 0xc0000374 - <Unable to get error code text>

EXCEPTION_PARAMETER1:  77594268

MOD_LIST: <ANALYSIS/>

BUGCHECK_STR:  APPLICATION_FAULT_WRONG_SYMBOLS_EXPLOITABLE

PRIMARY_PROBLEM_CLASS:  WRONG_SYMBOLS_EXPLOITABLE

DEFAULT_BUCKET_ID:  WRONG_SYMBOLS_EXPLOITABLE

LAST_CONTROL_TRANSFER:  from 7755e5e6 to 774b15ee

STACK_TEXT:  
WARNING: Stack unwind information not available. Following frames may be wrong.
001fb5ac 7755e5e6 001fb6f4 001fb744 00000000 ntdll!ZwRaiseException+0x12
001fb5c0 7755e663 c0000374 001fb5f4 775020b4 ntdll!RtlpNtEnumerateSubKey+0x1ab8
001fbc20 7755f559 c0000374 77594268 001fbc64 ntdll!RtlpNtEnumerateSubKey+0x1b35
001fbc30 7755f639 00000002 7630ddc7 00360000 ntdll!RtlpNtEnumerateSubKey+0x2a2b
001fbc64 7755f8a2 00000003 00360000 0457bc40 ntdll!RtlpNtEnumerateSubKey+0x2b0b
001fbcbc 7751aebb 00360000 0457bc40 00000000 ntdll!RtlpNtEnumerateSubKey+0x2d74
001fbda0 774c3cce 00000f64 00000f70 003600c4 ntdll!RtlUlonglongByteSwap+0xdacb
001fbe24 6da2fd36 00360000 00000000 00000f64 ntdll!RtlImageNtHeader+0xb6a
001fbe40 6ddbc9ab 00000f64 6ddc0966 04352418 ucrtbase!malloc_base+0x26
001fbe48 6ddc0966 04352418 00000f64 04298ee0 Qt5Gui!QWheelEvent::y+0xf74b
001fbe78 6ddbc9ea 00000310 6ddef367 6ddaf117 Qt5Gui!QWheelEvent::y+0x13706
001fbfc0 774c3c74 774c3ca3 7630a13f 04352418 Qt5Gui!QWheelEvent::y+0xf78a
00000000 00000000 00000000 00000000 00000000 ntdll!RtlImageNtHeader+0xb10

```
poc file <https://github.com/JsHuang/XnSoft-Security-Advisory/blob/master/XnConvert/poc/crash1.jpg>




## 2. OOB-Read

windbg result:

```bash
FAULTING_IP: 
ntdll!RtlInitUnicodeString+196
774be39e 8b4604          mov     eax,dword ptr [esi+4]

EXCEPTION_RECORD:  ffffffff -- (.exr 0xffffffffffffffff)
ExceptionAddress: 774be39e (ntdll!RtlInitUnicodeString+0x00000196)
   ExceptionCode: c0000005 (Access violation)
  ExceptionFlags: 00000000
NumberParameters: 2
   Parameter[0]: 00000000
   Parameter[1]: 9efef507
Attempt to read from address 9efef507

FAULTING_THREAD:  00000bf8

PROCESS_NAME:  xnconvert.exe

FAULTING_MODULE: 76ac0000 kernel32

DEBUG_FLR_IMAGE_TIMESTAMP:  0

MODULE_NAME: heap_corruption

ERROR_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_PARAMETER1:  00000000

EXCEPTION_PARAMETER2:  9efef507

READ_ADDRESS:  9efef507 

FOLLOWUP_IP: 
ntdll!RtlInitUnicodeString+196
774be39e 8b4604          mov     eax,dword ptr [esi+4]

MOD_LIST: <ANALYSIS/>

ADDITIONAL_DEBUG_TEXT:  
Use '!findthebuild' command to search for the target build information.
If the build information is available, run '!findthebuild -s ; .reload' to set symbol path and load symbols. ; Enable Pageheap/AutoVerifer

DEFAULT_BUCKET_ID:  HEAP_CORRUPTION

PRIMARY_PROBLEM_CLASS:  HEAP_CORRUPTION

BUGCHECK_STR:  APPLICATION_FAULT_HEAP_CORRUPTION_INVALID_POINTER_READ_WRONG_SYMBOLS

LAST_CONTROL_TRANSFER:  from 774be003 to 774b15ee

STACK_TEXT:  
WARNING: Stack unwind information not available. Following frames may be wrong.
037ce7e4 774be003 037cecf4 6da2dd40 03effd78 ntdll!ZwRaiseException+0x12
037ce7fc 6da2dd8b 006a0000 00000000 03effdc8 ntdll!RtlFreeHeap+0x7e
037ce810 6da2dd58 03effdc8 00000000 037ceb5c ucrtbase!free_base+0x1b
037ce820 00d6cd31 03effdc8 00000000 04471ad8 ucrtbase!free+0x18
037ceb5c 00d6afa6 041b0ab8 040dff50 037cecf4 xnconvert+0x2bcd31
037cede4 00d6a790 041b0ab8 04471ad8 00000000 xnconvert+0x2bafa6
037cedf8 00d26d87 041b0ab8 04471ad8 04471ad8 xnconvert+0x2ba790
037cee14 00d26c22 041b0ab8 04471ad8 00000000 xnconvert+0x276d87
037cf13c 00d2ad75 041b0ab8 04471ad8 ffffffff xnconvert+0x276c22
037cf170 00d2acdc 04047da8 037cf614 037cf1c0 xnconvert+0x27ad75
037cf198 00c5287c 04047da8 037cf614 037cf1c0 xnconvert+0x27acdc
037cf3bc 00c52526 037cf4ec 037cf614 00000060 xnconvert+0x1a287c
037cf454 00cbccd8 037cf4ec 037cf614 00000060 xnconvert+0x1a2526
037cf504 00cbd26c 037cf65c 037cf614 00000060 xnconvert+0x20ccd8
037cfadc 00cbe0a9 00000000 0432a388 00000060 xnconvert+0x20d26c
037cfd70 6e06e09c 0989e9fc 00000000 00000000 xnconvert+0x20e0a9
037cfd98 76ad33ca 00775560 037cfde4 774c9ed2 Qt5Core!QThread::start+0x2fc
037cfda4 774c9ed2 006f6680 755d85fe 00000000 kernel32!BaseThreadInitThunk+0x12
037cfde4 774c9ea5 6e06df60 006f6680 00000000 ntdll!RtlInitializeExceptionChain+0x63
037cfdfc 00000000 6e06df60 006f6680 00000000 ntdll!RtlInitializeExceptionChain+0x36
```
poc file   <https://github.com/JsHuang/XnSoft-Security-Advisory/blob/master/XnConvert/poc/crash2.tif>



## 3.OOB-Read

windbg result

```bash
FAULTING_IP: 
xnconvert+2b7618
00f77618 0fb702          movzx   eax,word ptr [edx]

EXCEPTION_RECORD:  ffffffff -- (.exr 0xffffffffffffffff)
ExceptionAddress: 00f77618 (xnconvert+0x002b7618)
   ExceptionCode: c0000005 (Access violation)
  ExceptionFlags: 00000000
NumberParameters: 2
   Parameter[0]: 00000000
   Parameter[1]: 069e1000
Attempt to read from address 069e1000

FAULTING_THREAD:  00000524

PROCESS_NAME:  xnconvert.exe

ADDITIONAL_DEBUG_TEXT:  
Use '!findthebuild' command to search for the target build information.
If the build information is available, run '!findthebuild -s ; .reload' to set symbol path and load symbols.

FAULTING_MODULE: 76ac0000 kernel32

DEBUG_FLR_IMAGE_TIMESTAMP:  5c7a6875

MODULE_NAME: xnconvert

ERROR_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_PARAMETER1:  00000000

EXCEPTION_PARAMETER2:  069e1000

READ_ADDRESS:  069e1000 

FOLLOWUP_IP: 
xnconvert+2b7618
00f77618 0fb702          movzx   eax,word ptr [edx]

MOD_LIST: <ANALYSIS/>

LAST_CONTROL_TRANSFER:  from 00f7c8b3 to 774b15ee

BUGCHECK_STR:  APPLICATION_FAULT_INVALID_POINTER_READ_WRONG_SYMBOLS

PRIMARY_PROBLEM_CLASS:  INVALID_POINTER_READ

DEFAULT_BUCKET_ID:  INVALID_POINTER_READ

STACK_TEXT:  
WARNING: Stack unwind information not available. Following frames may be wrong.
0386e890 00f7c8b3 069d87e0 06330020 ffffffff ntdll!ZwRaiseException+0x12
0386ebf0 00f7afa6 042390f8 06330020 0386ed88 xnconvert+0x2bc8b3
0386ee78 00f7a790 042390f8 044973e0 00000000 xnconvert+0x2bafa6
0386ee8c 00f36d87 042390f8 044973e0 044973e0 xnconvert+0x2ba790
0386eea8 00f36c22 042390f8 044973e0 00000000 xnconvert+0x276d87
0386f1d0 00f3ad75 042390f8 044973e0 ffffffff xnconvert+0x276c22
0386f204 00f3acdc 04090f68 0386f6a8 0386f254 xnconvert+0x27ad75
0386f22c 00e6287c 04090f68 0386f6a8 0386f254 xnconvert+0x27acdc
0386f450 00e62526 0386f580 0386f6a8 00000060 xnconvert+0x1a287c
0386f4e8 00ecccd8 0386f580 0386f6a8 00000060 xnconvert+0x1a2526
0386f598 00ecd26c 0386f6f0 0386f6a8 00000060 xnconvert+0x20ccd8
0386fb70 00ece0a9 00000000 043ef098 00000060 xnconvert+0x20d26c
0386fe04 6f06e09c 1dc63f0f 00000000 00000000 xnconvert+0x20e0a9
0386fe2c 76ad33ca 008b0068 0386fe78 774c9ed2 Qt5Core!QThread::start+0x2fc
0386fe38 774c9ed2 00836680 75a23163 00000000 kernel32!BaseThreadInitThunk+0x12
0386fe78 774c9ea5 6f06df60 00836680 00000000 ntdll!RtlInitializeExceptionChain+0x63
0386fe90 00000000 6f06df60 00836680 00000000 ntdll!RtlInitializeExceptionChain+0x36
```

poc file <https://github.com/JsHuang/XnSoft-Security-Advisory/blob/master/XnConvert/poc/crash3.tif>






## 4. Infinite Loop

See poc file  <https://github.com/JsHuang/XnSoft-Security-Advisory/blob/master/XnConvert/poc/XnConvert-infinite-loop.tif>





# Reproduce

Reproduce it as below :

- step1: Install and open XnConvert 1.80.

- step2: Click "Add files".

- step3: Choose the poc image file.



# Timeline

2019-02-28 	: Reported these vulnerabilities to the XnSoft developer    
2019-06-04 	: Vulnerabilities were not fixed yet and become public

# Credit 

ADLab of Venustech
