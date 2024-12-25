# WindowsLog_Check V3.3
V3.3 主要更新以下内容：
1. 将此前命令行界面换成GUI进行操作
2. 增加内存字符串检索、文件联动微步分析、MD5值检索和样本企微同步功能；
3. 改变日志存储和查询逻辑，将日志文件存于SQLite数据库中，使用SQL语句进行检索；
4. 增加部分功能输出内容
5. 考虑到兼容性（主要是适配win7、win2008和winserver 2012），暂时删除部分功能，例如：浏览器历史记录、向日葵todesk被控日志记录，对应功能后续应该会逐步加上；

免责声明：此工具仅限于安全研究，用户承担因使用此工具而导致的所有法律和相关责任！作者不承担任何法律责任

## 工具原理介绍
简单说下：利用Go调用Windows API获取原始日志信息，根据原始日志xml格式，定义对应事件ID结构体，从结构体中映射关键字段信息。将提取出来的日志信息存在SQLLite数据库不同表中，使用SQL语句对日志进行检索，大致就是这样。后续在分享具体Windows API使用、GO GUI编程和一些细节问题，喜欢的同学可以持续关注；

## V3.3工具运行界面
需要注意两点：

1、需要右键管理员运行，加载完成后 会弹出GUI界面

2、GUI弹出后，先将GUI最大化，在点击具体功能点，要不然列表会出现显示BUG，目前还没解决好
![image](https://github.com/user-attachments/assets/45fe7324-f945-4889-adc2-ab0e547a97c7)

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

开启所有审核日志

管理员运行powershell 执行：

Get-Command -CommandType Cmdlet

auditpol /set /category:* /success:enable /failure:enable

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

补充下：

因为日志数据解析完是存入SQLLite数据库中，因此也可直接使用Navicat之类数据库连接工具进行查看日志
![image](https://github.com/user-attachments/assets/733df5ba-0662-428f-bb9c-659a74a3cbcc)

# 欢迎关注作者本人公众号
![qrcode_for_gh_39a7ba3bcbf8_430](https://github.com/user-attachments/assets/3c97dd24-67cb-4e87-b395-8729dcad6860)

# 战友广告
![image](https://github.com/user-attachments/assets/8f00dff5-0b86-43b2-b1aa-488b28609009)

## Stargazers over time
[![Stargazers over time](https://starchart.cc/mifine666/miscan.svg?variant=adaptive)](https://starchart.cc/mifine666/miscan)


