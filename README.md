# WindowsLog_Check V3.3
V3.3 主要更新以下内容：
1. 将此前命令行界面换成GUI进行操作
2. 增加内存字符串检索、文件联动微步分析、MD5值检索和样本企微同步功能；
3. 改变日志存储和查询逻辑，将日志文件存于SQLite数据库中，使用SQL语句进行检索；
4. 增加部分功能输出内容
5. 考虑到兼容性（主要是适配win7、win2008和winserver 2012），暂时删除部分功能，例如：浏览器历史记录、向日葵todesk被控日志记录，对应功能后续应该会逐步加上；

免责声明：此工具仅限于安全研究，用户承担因使用此工具而导致的所有法律和相关责任！作者不承担任何法律责任

## V3.3工具运行界面
![image](https://github.com/user-attachments/assets/0f5bc79a-3fb4-4ea6-998b-b3c66d3d6a30)

## V3.3功能介绍

### 日志分析
重点说下，登录成功和登录失败日志，其他日志大家自己点着看就行了（前面的版本也有说明不在赘述）

登录成功和登录失败日志，显示详细的登录信息包括：源目地址、目标用户名、登录进程、认证协议
![image](https://github.com/user-attachments/assets/0b4487a6-fdf0-4af6-8eb3-cd88f393be36)

可以对所有登录成功和登录失败日志按照：源地址、源用户名、目的用户名、登录类型进行筛选,举例筛选登录类型：5
![image](https://github.com/user-attachments/assets/3a70ce84-98e8-4bec-89bc-aaf78fd06d29)

默认显示前1000条日志，可点击下一页查看更多的日志
![image](https://github.com/user-attachments/assets/2f286b5e-642c-4cf5-8ae1-3d2d6d33eb1a)

目前仍保留对离线日志的分析，将目标机器上面C:\Windows\System32\winevt\Logs 路径下全部打包放到工具同步即可，不要修改文件名
![image](https://github.com/user-attachments/assets/ddb90142-8314-40ee-ad93-356f51fcbf80)

提示下：有些没有日志记录的，大概率是主机没有开启日志审核策略导致主机没有记录，具体怎么开百度就行 

### 威胁检索
1、内存检索，将https://github.com/Fheidt12/Windows_Memory_Search 集成过来

使用场景：根据外联域名或者IP定位恶意进程（很推荐）
![image](https://github.com/user-attachments/assets/74349e74-2751-496a-a3ca-c0b968551043)


2、微步MD5分析，此功能不会直接上文件上传微步，而是计算文件的MD5，将MD5进行检索，返回文件是否为已知恶意文件

使用场景：批量快速确认文件是否为已知恶意文件
![image](https://github.com/user-attachments/assets/9c9cedfb-3356-425c-9b39-ba40893cbad2)

3、检索相同MD5文件

使用场景，当发现恶意文件后，可以检索指定目录先是否存在相同文件
![image](https://github.com/user-attachments/assets/431619b4-a58e-4a2d-963c-ed022059a514)

4、文件同步

使用场景：发现未知文件后，可以快速同步样本分析同事，协同进行分析；也可以将客户日志同步到本地，进行分析；
![image](https://github.com/user-attachments/assets/1cae6ccb-3c88-4aa7-ae7d-7fd4d51dc274)

![image](https://github.com/user-attachments/assets/0e87ec4a-5cc0-4d05-b759-b16f836cacee)

说明：这里微步和企业微信机器人的key 可以保存到相同目录下config.ini文件，也可以每次手动输入，如果手动输入，程序会以手动输入为准


### 主机信息
1、进程列表，输出：PID、进程名、PPID、父进程名、所属用户、所属服务、外联地址、创建时间、执行路径及参数；

![image](https://github.com/user-attachments/assets/3135e41b-feea-48e9-b780-ad4cca46311d)
说明：输出的信息并非实时更新，若需要更新，需手动右键点击进行列表进行手动刷新
![image](https://github.com/user-attachments/assets/96f86239-f5d7-4b50-9f76-c39bf7fe0ba7)

2、用户信息
![image](https://github.com/user-attachments/assets/86f88cb5-d0a1-4446-a88e-ecb3d4a71c0c)

3、任务计划
![image](https://github.com/user-attachments/assets/08e3b973-4cfd-463f-a2d4-e33ef0c3c147)

4、服务信息
![image](https://github.com/user-attachments/assets/66054c22-1965-4ea7-a1f1-34e819736228)

5、启动项
![image](https://github.com/user-attachments/assets/a319510c-ec06-4592-a923-b7a74e8e3f37)


# WindowsLog_Check V3.1&3.2
此工具主要实现对windows日志进行分析，主要分为以下功能，
1. 针对常规 登录成功、登录失败、RDP登录、用户创建、服务创建等日志进行分析，判断是否存在爆破、是否有代理登录行为；
2. 针对SQLServer数据库，可分析是否存在爆破，以及开启Xp_cmdshell等记录；
3. 针对域控主机日志分析，是否存在漏洞利用，密码抓取，后门用户等情况，前提为域控主机开启高级策略配置，否则主机不会有日志记录；
4. 分析主机进程，外联，文件是否存在已知恶意的文件或者IP；
5. 分析常用远控软件被控记录，可定位IP和动作；
6. 目前支持离线分析，若针对win2008和win7版本操作系统，只能使用离线分析，因为低版本系统无法运行，winserver 2012也建议使用离线分析，便于查看日志信息，高版本 winserver2016 2019 win10 win11 均可以在主机直接进行分析；

免责声明：此工具仅限于安全研究，用户承担因使用此工具而导致的所有法律和相关责任！作者不承担任何法律责任
## V3.2工具运行界面
![image](https://github.com/user-attachments/assets/53289a01-b1ee-4a0a-a15c-9ba3b61d3d8b)

### 功能1&2：登录成功和登录失败日志分析
（1）不输入登录类型和IP时，默认查看所有4624登录日志
![image](https://github.com/user-attachments/assets/77880066-bd91-4581-ba0c-26819e9e8659)

（2）可指定查看登录类型，例如查看登录类型：3
![image](https://github.com/user-attachments/assets/32d43361-7d0c-4dc1-aeeb-cf2bf45f2b5e)

(3)可指定登录源地址查看，例如指定源地址为：192.168.1.4
![image](https://github.com/user-attachments/assets/8cc0b664-284c-4780-8e17-ea536b26ec07)

（4）可同时指定登录类型和源地址
![image](https://github.com/user-attachments/assets/3131971e-0d1e-40db-bab6-ac128a5ee4f8)

### 功能3：RDP登录记录
查看rdp登录记录，并且会判断是否通过代理软件登录的情况，
日志ID说明：
1149：远程桌面认证成功
21：RDP连接成功
25：RDP重连成功
![image](https://github.com/user-attachments/assets/b9d1fc80-9c75-4472-a67c-4d774b3e9e5a)

### 功能4：RDP连接记录
RDP连接记录，查看当前主机，rdp登录过哪些机器
1102：当前主机使用RDP登录过哪些机器
1024：当前主机使用RDP尝试登录过哪些机器（不一定成功）
![image](https://github.com/user-attachments/assets/2211b626-7ccd-46cd-b44e-4363815f1067)

### 功能5：服务创建记录
查看服务创建记录，在横向渗透过程中或者病毒横向传播过程中均可能会创建恶意服务项，目前程序对sys相关的驱动服务进行了过滤
![image](https://github.com/user-attachments/assets/ab9c1abd-d4c8-4c00-ab8a-1d30708e38bc)

### 功能6：SQLServer 数据库爆破和配置修改
查看SQLServer数据库，爆破记录和配置修改记录支持筛选IP
![image](https://github.com/user-attachments/assets/ca70a33f-9832-4bed-a563-2bade97e2f19)

### 功能7：用户相关日志
查看用户，主要分为两个方面：
系统日志方面：创建用户记录i、启动用户记录、修改密码记录、修改所属组记录
主机信息方面：当前用户信息，包括：存在的所有用户、所在域、用户SID、是否禁用、用户信息描述
![image](https://github.com/user-attachments/assets/8408a34e-3099-4152-af3b-7aa33944cf70)

### 功能8：计划任务
查看全部计划任务，显示计划任务名称、创建人、执行动作、是否启动、创建时间、描述
![image](https://github.com/user-attachments/assets/88dca751-6fc8-4ee7-b0d8-16caf6e2e244)
根据文件名称检索计划任务

<img width="808" alt="9aa8912a7a6a4d1cd6fa6c354067bc6" src="https://github.com/user-attachments/assets/9af2d8ee-1fb6-4e8e-a121-83dad957d081">

### 功能9：主机进程分析
查看进程信息，目前支持查看进程外联地址，启动时间，执行路径，执行用户，PID
![image](https://github.com/user-attachments/assets/f6487956-db79-4032-9088-7a9df5e3010c)


### 功能10：主机外联地址研判
查看当前主机外联地址是否存在恶意IP，需要微步有对应接口权限，我现在没有有 有的大哥可以自己测试下，之前测试是没有问题的
![image](https://github.com/user-attachments/assets/e4432b16-d8d6-4e1e-9667-0432094c37a3)

### 功能11：向日葵和todesk被控日志分析
向日葵被控记录，可查看远程连接时间和 对方内外网IP地址，以及远控时上传的文件
![image](https://github.com/user-attachments/assets/1927033c-66df-4164-9e0f-9db93a71804f)

todesk被控记录
![image](https://github.com/user-attachments/assets/52b7c7f0-1c8b-40d5-a318-34ed03365342)

### 功能12：批量分析文件MD5
批量计算指定文件目录所有文件MD5值，并判断是否为恶意
![image](https://github.com/user-attachments/assets/76a6e545-83f7-4b87-9cda-6f458c36d362)

### 功能13：kerberos协议分析
kerberos协议分析，判断是否存在利用kerberos协议进行用户名枚举和爆破，可筛选源地址和登录状态
默认查看所有
![image](https://github.com/user-attachments/assets/63ab2d27-cfee-4a98-bbd8-426236f4adce)

筛选IP和登录失败的记录
![image](https://github.com/user-attachments/assets/08466ae2-cf9f-449e-b52a-cafa2da75f81)

### 功能14：DCSync 攻击分析
查看是否存在利用某个用户进行DCSync攻击的记录
注意：机器账号的日志 情况为正常
![image](https://github.com/user-attachments/assets/a45f4c6a-09ff-41a6-8c4e-ceed84b23381)

### 功能15：SID后门分析
通过SID历史记录，判断是否存在SID后门
![image](https://github.com/user-attachments/assets/676c586e-9ade-4d56-aee3-cd328f99eabf)

### 功能16：Lsass内存读取记录
![image](https://github.com/user-attachments/assets/5efede14-a844-406c-9bfb-6aa7142d6bf4)

### 功能17：域内信息收集记录
攻击者查看所有域用户时会触发该记录
![image](https://github.com/user-attachments/assets/ae3a1a7b-7776-4176-a56d-3324d22c4115)
### 功能18：ZeroLogon漏洞利用记录
![image](https://github.com/user-attachments/assets/3012b73b-3b6e-40eb-90e8-a2f2356d79fc)
# 欢迎关注
![image](https://github.com/user-attachments/assets/8f00dff5-0b86-43b2-b1aa-488b28609009)


