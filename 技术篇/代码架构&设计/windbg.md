**<font style="color:rgb(34, 34, 34);">命令提示符窗口中（cmd）:windbg-y</font>**<font style="color:rgb(34, 34, 34);"> </font>_<font style="color:rgb(34, 34, 34);">SymbolPath</font>_<font style="color:rgb(34, 34, 34);"> </font>**<font style="color:rgb(34, 34, 34);">-i</font>**<font style="color:rgb(34, 34, 34);"> </font>_<font style="color:rgb(34, 34, 34);">ImagePath</font>_<font style="color:rgb(34, 34, 34);"> </font>**<font style="color:rgb(34, 34, 34);">-z</font>**<font style="color:rgb(34, 34, 34);"> </font>_<font style="color:rgb(34, 34, 34);">DumpFileName</font>_

_<font style="color:rgb(34, 34, 34);">符号路径  应用程序路径  崩溃文件路径</font>_



**<font style="color:rgb(34, 34, 34);">windbg</font>**<font style="color:rgb(0, 0, 0);background-color:rgb(245, 245, 245);"> -y symbolpath </font>**<font style="color:rgb(34, 34, 34);">-i</font>**<font style="color:rgb(34, 34, 34);"> </font>_<font style="color:rgb(34, 34, 34);">ImagePath </font>_<font style="color:rgb(0, 0, 0);background-color:rgb(245, 245, 245);">-z dumpfile -c command -logo logfilepath</font>

<font style="color:rgb(204, 204, 204);background-color:rgb(51, 51, 51);">如果你想在-c 命令中使用多个命令，使用 ; 分割，或者使用 &< 脚本模式，参考巨硬的官方文档即可。</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(245, 245, 245);">-c "&<xxx.txt"</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(245, 245, 245);"></font>

E:\Software\windbg>windbg -y D:\code\products\rail_transport\monitor\pcds\1.x\software\branches\iteration_1.0.1\applications\detection_server\bin\win64_vc141_release -i D:\code\products\rail_transport\monitor\pcds\1.x\software\branches\iteration_1.0.1\applications\detection_server\bin\win64_vc141_release -z D:\code\products\rail_transport\monitor\pcds\1.x\software\branches\iteration_1.0.1\applications\detection_server\bin\win64_vc141_release\DetectionServer.exe_210914_162120.dmp -c "!analyze -v; ~*kbn; q" -logo logfile.txt





//定时检查崩溃文件的脚本





rem pdb路径

set <font style="color:rgb(0, 0, 0);background-color:rgb(245, 245, 245);">symbolPath=D:/SpecularVision/</font>

rem 应用程序路径

set <font style="color:rgb(0, 0, 0);background-color:rgb(245, 245, 245);">exePath=D:/SpecularVision/</font>

rem 崩溃文件路径

set dumpDir=D:/dumps

rem 崩溃文件前缀

set dumpFile<font style="color:rgb(68, 68, 68);">Suffix</font>=inspect_server

rem 崩溃分析文件路径

set analyzeFileDir=D:/dumps/analyze



cd %~dp0

for %%i in (*) do ( echo %%i )





