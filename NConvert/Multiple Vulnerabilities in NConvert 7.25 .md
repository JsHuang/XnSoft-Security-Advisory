# Description

Multiple vulnerabilities was found in XnSoft Software NConvert(2 OOB read and 1infinite loop). 

## Affected version
NConvert 7.25 for Windows 

## Affected platform
All the vulnerabilities are tested with Windows versions ,but may exists in other platform versions.

# Vulnerability Details
The poc files loated in  <https://github.com/JsHuang/XnSoft-Security-Advisory/tree/master/NConvert/poc>

Details about the vulnerabilities are as below. 

## 1. OOB-Read (nconvert+19b1ab)

windbg result:

```bash
FAULTING_IP: 
nconvert+19b1ab
0059b1ab f3a5            rep movs dword ptr es:[edi],dword ptr [esi]

EXCEPTION_RECORD:  ffffffff -- (.exr 0xffffffffffffffff)
ExceptionAddress: 0059b1ab (nconvert+0x0019b1ab)
   ExceptionCode: c0000005 (Access violation)
  ExceptionFlags: 00000000
NumberParameters: 2
   Parameter[0]: 00000000
   Parameter[1]: 08f69fff
Attempt to read from address 08f69fff

FAULTING_THREAD:  0000007c

PROCESS_NAME:  nconvert.exe

ADDITIONAL_DEBUG_TEXT:  
Use '!findthebuild' command to search for the target build information.
If the build information is available, run '!findthebuild -s ; .reload' to set symbol path and load symbols.

FAULTING_MODULE: 76ac0000 kernel32

DEBUG_FLR_IMAGE_TIMESTAMP:  5c3dbe14

MODULE_NAME: nconvert

ERROR_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_PARAMETER1:  00000000

EXCEPTION_PARAMETER2:  08f69fff

READ_ADDRESS:  08f69fff 

FOLLOWUP_IP: 
nconvert+19b1ab
0059b1ab f3a5            rep movs dword ptr es:[edi],dword ptr [esi]

MOD_LIST: <ANALYSIS/>

LAST_CONTROL_TRANSFER:  from 00523dca to 774b15ee

BUGCHECK_STR:  APPLICATION_FAULT_INVALID_POINTER_READ_WRONG_SYMBOLS

PRIMARY_PROBLEM_CLASS:  INVALID_POINTER_READ

DEFAULT_BUCKET_ID:  INVALID_POINTER_READ

STACK_TEXT:  
WARNING: Stack unwind information not available. Following frames may be wrong.
0018b940 00523dca 08f68bfb 08f6a2d8 00002d23 ntdll!ZwRaiseException+0x12
0018b95c 004584ac 08f68bfb 08f6e2d8 00002d23 nconvert+0x123dca
0018b984 0050e26c 08f68bfb 00000000 00000082 nconvert+0x584ac
0018b9c0 00513757 00000000 08f65ed8 ffffffff nconvert+0x10e26c
0018bd1c 00512039 08dd8f60 08f65ed8 0018beb4 nconvert+0x113757
0018bfa4 00511810 08dd8f60 075bdca8 00000000 nconvert+0x112039
0018bfb8 00457d25 08dd8f60 075bdca8 075bdca8 nconvert+0x111810
0018bfd4 00457bc2 08dd8f60 075bdca8 00000000 nconvert+0x57d25
0018c0fc 0045bc45 08dd8f60 075bdca8 ffffffff nconvert+0x57bc2
0018c130 0045ba2c 054eaed0 0018c26c 0018c17c nconvert+0x5bc45
0018c158 00440ab8 054eaed0 0018c26c 0018c17c nconvert+0x5ba2c
0018dad4 004422d8 054eaed0 00000000 0018db0c nconvert+0x40ab8
0018e32c 0044b04e 054eaed0 00000000 0018f4e4 nconvert+0x422d8
0018f9ec 0044a297 06eedfe8 00000004 00000000 nconvert+0x4b04e
0018ff40 005a0998 00000004 06eedfe8 05107f10 nconvert+0x4a297
0018ff88 76ad33ca 7efde000 0018ffd4 774c9ed2 nconvert+0x1a0998
0018ff94 774c9ed2 7efde000 767854ac 00000000 kernel32!BaseThreadInitThunk+0x12
0018ffd4 774c9ea5 005a0a15 7efde000 00000000 ntdll!RtlInitializeExceptionChain+0x63
0018ffec 00000000 005a0a15 7efde000 00000000 ntdll!RtlInitializeExceptionChain+0x36
```
poc file <https://github.com/JsHuang/XnSoft-Security-Advisory/blob/master/NConvert/poc/oob-read-nconvert%2B19b1ab.tif>




## 2. OOB-Read (nconvert+10e3d0)

windbg result:

```bash
FAULTING_IP: 
nconvert+10e3d0
0050e3d0 0fb707          movzx   eax,word ptr [edi]

EXCEPTION_RECORD:  ffffffff -- (.exr 0xffffffffffffffff)
ExceptionAddress: 0050e3d0 (nconvert+0x0010e3d0)
   ExceptionCode: c0000005 (Access violation)
  ExceptionFlags: 00000000
NumberParameters: 2
   Parameter[0]: 00000000
   Parameter[1]: 0a3d1000
Attempt to read from address 0a3d1000

FAULTING_THREAD:  000005fc

PROCESS_NAME:  nconvert.exe

ADDITIONAL_DEBUG_TEXT:  
Use '!findthebuild' command to search for the target build information.
If the build information is available, run '!findthebuild -s ; .reload' to set symbol path and load symbols.

FAULTING_MODULE: 76ac0000 kernel32

DEBUG_FLR_IMAGE_TIMESTAMP:  5c3dbe14

MODULE_NAME: nconvert

ERROR_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_CODE: (NTSTATUS) 0xc0000005 - 0x%08lx

EXCEPTION_PARAMETER1:  00000000

EXCEPTION_PARAMETER2:  0a3d1000

READ_ADDRESS:  0a3d1000 

FOLLOWUP_IP: 
nconvert+10e3d0
0050e3d0 0fb707          movzx   eax,word ptr [edi]

MOD_LIST: <ANALYSIS/>

LAST_CONTROL_TRANSFER:  from 00513757 to 774b15ee

BUGCHECK_STR:  APPLICATION_FAULT_INVALID_POINTER_READ_WRONG_SYMBOLS

PRIMARY_PROBLEM_CLASS:  INVALID_POINTER_READ

DEFAULT_BUCKET_ID:  INVALID_POINTER_READ

STACK_TEXT:  
WARNING: Stack unwind information not available. Following frames may be wrong.
0018b9c0 00513757 0a3c8bc0 09aa0800 ffffffff ntdll!ZwRaiseException+0x12
0018bd1c 00512039 090a8f60 09aa0800 0018beb4 nconvert+0x113757
0018bfa4 00511810 090a8f60 077bdca8 00000000 nconvert+0x112039
0018bfb8 00457d25 090a8f60 077bdca8 077bdca8 nconvert+0x111810
0018bfd4 00457bc2 090a8f60 077bdca8 00000000 nconvert+0x57d25
0018c0fc 0045bc45 090a8f60 077bdca8 ffffffff nconvert+0x57bc2
0018c130 0045ba2c 06d8aed0 0018c26c 0018c17c nconvert+0x5bc45
0018c158 00440ab8 06d8aed0 0018c26c 0018c17c nconvert+0x5ba2c
0018dad4 004422d8 06d8aed0 00000000 0018db0c nconvert+0x40ab8
0018e32c 0044b04e 06d8aed0 00000000 0018f4e4 nconvert+0x422d8
0018f9ec 0044a297 06fddfe8 00000004 00000000 nconvert+0x4b04e
0018ff40 005a0998 00000004 06fddfe8 05317f10 nconvert+0x4a297
0018ff88 76ad33ca 7efde000 0018ffd4 774c9ed2 nconvert+0x1a0998
0018ff94 774c9ed2 7efde000 767d4604 00000000 kernel32!BaseThreadInitThunk+0x12
0018ffd4 774c9ea5 005a0a15 7efde000 00000000 ntdll!RtlInitializeExceptionChain+0x63
0018ffec 00000000 005a0a15 7efde000 00000000 ntdll!RtlInitializeExceptionChain+0x36


SYMBOL_STACK_INDEX:  0

SYMBOL_NAME:  nconvert+10e3d0

FOLLOWUP_NAME:  MachineOwner

STACK_COMMAND:  ~0s ; kb

BUCKET_ID:  WRONG_SYMBOLS

IMAGE_NAME:  C:\Program Files (x86)\XnView\nconvert.exe

FAILURE_BUCKET_ID:  INVALID_POINTER_READ_c0000005_C:_Program_Files_(x86)_XnView_nconvert.exe!Unknown

Followup: MachineOwner

```
poc file  <https://github.com/JsHuang/XnSoft-Security-Advisory/blob/master/NConvert/poc/oob-read-nconvert%2B10e3d0.tif>




## 3. Infinite Loop

See poc file

<https://github.com/JsHuang/XnSoft-Security-Advisory/blob/master/NConvert/poc/NConvert-infinite-loop.tif>



# Reproduce

Reproduce it with the following nconvert command

`nconvert.exe -out png poc.tif`



# Timeline

2019-02-28 	: Reported these vulnerabilities to the XnSoft developer    
2019-06-04 	: Vulnerabilities were not fixed yet and become public

# Credit 

ADLab of Venustech
