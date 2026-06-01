---
tags:
  - seed
created: 2026-05-30
updated: 2026-05-30
topic: tech
---

**命令提示符窗口中（cmd）:windbg-y** _SymbolPath_ **-i** _ImagePath_ **-z** _DumpFileName_

_符号路径  应用程序路径  崩溃文件路径_


**windbg** -y symbolpath **-i** _ImagePath _-z dumpfile -c command -logo logfilepath

如果你想在-c 命令中使用多个命令，使用 ; 分割，或者使用 &< 脚本模式，参考巨硬的官方文档即可。

-c "&<xxx.txt"


E:\Software\windbg>windbg -y D:\code\products\rail_transport\monitor\pcds\1.x\software\branches\iteration_1.0.1\applications\detection_server\bin\win64_vc141_release -i D:\code\products\rail_transport\monitor\pcds\1.x\software\branches\iteration_1.0.1\applications\detection_server\bin\win64_vc141_release -z D:\code\products\rail_transport\monitor\pcds\1.x\software\branches\iteration_1.0.1\applications\detection_server\bin\win64_vc141_release\DetectionServer.exe_210914_162120.dmp -c "!analyze -v; ~*kbn; q" -logo logfile.txt


//定时检查崩溃文件的脚本


rem pdb路径

set symbolPath=D:/SpecularVision/

rem 应用程序路径

set exePath=D:/SpecularVision/

rem 崩溃文件路径

set dumpDir=D:/dumps

rem 崩溃文件前缀

set dumpFileSuffix=inspect_server

rem 崩溃分析文件路径

set analyzeFileDir=D:/dumps/analyze


cd %~dp0

for %%i in (*) do ( echo %%i )


