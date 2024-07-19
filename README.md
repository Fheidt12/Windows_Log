# WindowsLog_Check
此工具主要实现对windows日志进行分析，主要分为以下功能，
1. 针对常规 登录成功、登录失败、RDP登录、用户创建、服务创建等日志进行分析，判断是否存在爆破、是否有代理登录行为；
2. 针对SQLServer数据库，可分析是否存在爆破，以及开启Xp_cmdshell等记录；
3. 针对域控主机日志分析，是否存在漏洞利用，密码抓取，后门用户等情况，前提为域控主机开启高级策略配置，否则主机不会有日志记录；
4. 分析主机进程，外联，文件是否存在已知恶意的文件或者IP；
5. 分析常用远控软件被控记录，可定位IP和动作；

## 工具运行界面
![image](https://github.com/user-attachments/assets/32a5251c-c33b-46fc-972c-3a038fb33a65)

## 功能1&2：登录成功和登录失败日志分析
（1）不输入登录类型和IP时，默认查看所有4624登录日志
![image](https://github.com/user-attachments/assets/77880066-bd91-4581-ba0c-26819e9e8659)

（2）可指定查看登录类型，例如查看登录类型：3
![image](https://github.com/user-attachments/assets/32d43361-7d0c-4dc1-aeeb-cf2bf45f2b5e)

(3)可指定登录源地址查看，例如指定源地址为：192.168.1.4
![image](https://github.com/user-attachments/assets/8cc0b664-284c-4780-8e17-ea536b26ec07)

（4）可同时指定登录类型和源地址
![image](https://github.com/user-attachments/assets/3131971e-0d1e-40db-bab6-ac128a5ee4f8)

## 功能3：RDP登录记录
查看rdp登录记录，并且会判断是否通过代理软件登录的情况，
日志ID说明：
1149：远程桌面认证成功
21：RDP连接成功
25：RDP重连成功
![image](https://github.com/user-attachments/assets/b9d1fc80-9c75-4472-a67c-4d774b3e9e5a)

## 功能4：RDP连接记录
RDP连接记录，查看当前主机，rdp登录过哪些机器
1102：当前主机使用RDP登录过哪些机器
1024：当前主机使用RDP尝试登录过哪些机器（不一定成功）
![image](https://github.com/user-attachments/assets/2211b626-7ccd-46cd-b44e-4363815f1067)

## 功能5：服务创建记录
查看服务创建记录，在横向渗透过程中或者病毒横向传播过程中均可能会创建恶意服务项，目前程序对sys相关的驱动服务进行了过滤
![image](https://github.com/user-attachments/assets/ab9c1abd-d4c8-4c00-ab8a-1d30708e38bc)

## 功能6：SQLServer 数据库爆破和配置修改
查看SQLServer数据库，爆破记录和配置修改记录支持筛选IP
![image](https://github.com/user-attachments/assets/ca70a33f-9832-4bed-a563-2bade97e2f19)

## 功能7：用户相关日志
查看用户，主要分为两个方面：
系统日志方面：创建用户记录i、启动用户记录、修改密码记录、修改所属组记录
主机信息方面：当前用户信息，包括：存在的所有用户、所在域、用户SID、是否禁用、用户信息描述
![image](https://github.com/user-attachments/assets/8408a34e-3099-4152-af3b-7aa33944cf70)

## 功能8：计划任务
查看计划任务 排版有点问题 后续调整  暂不展示
## 功能9：主机进程分析
查看进程信息，目前支持查看进程外联地址，启动时间，执行路径，执行用户，PID
![image](https://github.com/user-attachments/assets/f6487956-db79-4032-9088-7a9df5e3010c)


## 功能10：主机外联地址研判
查看当前主机外联地址是否存在恶意IP，需要微步有对应接口权限，我现在没有有 有的大哥可以自己测试下，之前测试是没有问题的
![image](https://github.com/user-attachments/assets/e4432b16-d8d6-4e1e-9667-0432094c37a3)

## 功能11：向日葵和todesk被控日志分析
向日葵被控记录，可查看远程连接时间和 对方内外网IP地址，以及远控时上传的文件
![image](https://github.com/user-attachments/assets/1927033c-66df-4164-9e0f-9db93a71804f)

todesk被控记录
![image](https://github.com/user-attachments/assets/52b7c7f0-1c8b-40d5-a318-34ed03365342)

## 功能12：批量分析文件MD5
批量计算指定文件目录所有文件MD5值，并判断是否为恶意
![image](https://github.com/user-attachments/assets/76a6e545-83f7-4b87-9cda-6f458c36d362)

## 功能13：kerberos协议分析
kerberos协议分析，判断是否存在利用kerberos协议进行用户名枚举和爆破，可筛选源地址和登录状态
默认查看所有
![image](https://github.com/user-attachments/assets/63ab2d27-cfee-4a98-bbd8-426236f4adce)

筛选IP和登录失败的记录
![image](https://github.com/user-attachments/assets/08466ae2-cf9f-449e-b52a-cafa2da75f81)

## 功能14：DCSync 攻击分析
查看是否存在利用某个用户进行DCSync攻击的记录
注意：机器账号的日志 情况为正常
![image](https://github.com/user-attachments/assets/a45f4c6a-09ff-41a6-8c4e-ceed84b23381)

## 功能15：SID后门分析
通过SID历史记录，判断是否存在SID后门
![image](https://github.com/user-attachments/assets/676c586e-9ade-4d56-aee3-cd328f99eabf)

## 功能16：Lsass内存读取记录
![image](https://github.com/user-attachments/assets/5efede14-a844-406c-9bfb-6aa7142d6bf4)

## 功能17：域内信息收集记录
攻击者查看所有域用户时会触发该记录
![image](https://github.com/user-attachments/assets/ae3a1a7b-7776-4176-a56d-3324d22c4115)
## 功能18：ZeroLogon漏洞利用记录
![image](https://github.com/user-attachments/assets/3012b73b-3b6e-40eb-90e8-a2f2356d79fc)

