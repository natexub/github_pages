---
title: Windows 10 下常用的环境变量 
date: 2022-04-24 17:07:36
updated: 2022-04-24 17:07:36
categories:
tags:
---

| VARIABLE                    | WINDOWS 10                                                   |
| :-------------------------- | :----------------------------------------------------------- |
| %ALLUSERSPROFILE%           | C:\ProgramData                                               |
| %APPDATA%                   | C:\Users\{username}\AppData\Roaming                          |
| %COMMONPROGRAMFILES%        | C:\Program Files\Common Files                                |
| %COMMONPROGRAMFILES(x86)%   | C:\Program Files (x86)\Common Files                          |
| %CommonProgramW6432%        | C:\Program Files\Common Files                                |
| %COMSPEC%                   | C:\Windows\System32\cmd.exe                                  |
| %HOMEDRIVE%                 | C:\                                                          |
| %HOMEPATH%                  | C:\Users\{username}                                          |
| %LOCALAPPDATA%              | C:\Users\{username}\AppData\Local                            |
| %LOGONSERVER%               | \\{domain_logon_server}                                      |
| %PATH%                      | C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem      |
| %PathExt%                   | .com;.exe;.bat;.cmd;.vbs;.vbe;.js;.jse;.wsf;.wsh;.msc        |
| %PROGRAMDATA%               | C:\ProgramData                                               |
| %PROGRAMFILES%              | C:\Program Files                                             |
| %ProgramW6432%              | C:\Program Files                                             |
| %PROGRAMFILES(X86)%         | C:\Program Files (x86)                                       |
| %PROMPT%                    | $P$G                                                         |
| %SystemDrive%               | C:                                                           |
| %SystemRoot%                | C:\Windows                                                   |
| %TEMP%                      | C:\Users\{username}\AppData\Local\Temp                       |
| %TMP%                       | C:\Users\{username}\AppData\Local\Temp                       |
| %USERDOMAIN%                | Userdomain associated with current user.                     |
| %USERDOMAIN_ROAMINGPROFILE% | Userdomain associated with roaming profile.                  |
| %USERNAME%                  | {username}                                                   |
| %USERPROFILE%               | C:\Users\{username}                                          |
| %WINDIR%                    | C:\Windows                                                   |
| %PUBLIC%                    | C:\Users\Public                                              |
| %PSModulePath%              | %SystemRoot%\system32\WindowsPowerShell\v1.0\Modules\        |
| %OneDrive%                  | C:\Users\{username}\OneDrive                                 |
| %DriverData%                | C:\Windows\System32\Drivers\DriverData                       |
| %CD%                        | Outputs current directory path. (Command Prompt.)            |
| %CMDCMDLINE%                | Outputs command line used to launch current Command Prompt session. (Command Prompt.) |
| %CMDEXTVERSION%             | Outputs the number of current command processor extensions. (Command Prompt.) |
| %COMPUTERNAME%              | Outputs the system name.                                     |
| %DATE%                      | Outputs current date. (Command Prompt.)                      |
| %TIME%                      | Outputs time. (Command Prompt.)                              |
| %ERRORLEVEL%                | Outputs the number of defining exit status of previous command. (Command Prompt.) |
| %PROCESSOR_IDENTIFIER%      | Outputs processor identifier.                                |
| %PROCESSOR_LEVEL%           | Outputs processor level.                                     |
| %PROCESSOR_REVISION%        | Outputs processor revision.                                  |
| %NUMBER_OF_PROCESSORS%      | Outputs the number of physical and virtual cores.            |
| %RANDOM%                    | Outputs random number from 0 through 32767.                  |
| %OS%                        | Windows_NT                                                   |

一些变量不是特定于位置的，包括%COMPUTERNAME%, %PATHEXT%, %PROMPT%, %USERDOMAIN%, %USERNAME%。

## 使用 Powershell 批量设置

示例：

设置新环境变量

```powershell
[System.Environment]::SetEnvironmentVariable('PYENV',$env:USERPROFILE + "\.pyenv\pyenv-win\","User")

[System.Environment]::SetEnvironmentVariable('PYENV_ROOT',$env:USERPROFILE + "\.pyenv\pyenv-win\","User")

[System.Environment]::SetEnvironmentVariable('PYENV_HOME',$env:USERPROFILE + "\.pyenv\pyenv-win\","User")

```

修改`PATH`

```powershell
[System.Environment]::SetEnvironmentVariable('path', $env:USERPROFILE + "\.pyenv\pyenv-win\bin;" + $env:USERPROFILE + "\.pyenv\pyenv-win\shims;" + [System.Environment]::GetEnvironmentVariable('path', "User"),"User")
```

### Windows 路径优先级

Windows 从`PATH`环境变量中检测到当前（活动的）Python 解释器。当您输入`python`您最喜欢的 shell（终端、CMD、PowerShell 等）时，操作系统`python.exe`从顶部（开始）开始搜索 PATH 记录。一旦找到解释器 - 操作系统就会停止搜索并运行可执行文件。这就是 PATH 中的顺序很重要的原因。

Windows 具有三个环境变量范围：系统（或机器）、用户和进程范围。解释器搜索遵循相同的顺序：**System PATH -> User PATH -> Process (session) PATH**
## 参考

[Complete list of environment variables on Windows 10](https://pureinfotech.com/list-environment-variables-windows-10/)