#test
###目录说明:
framework			程序运行框架
framework/database	程序数据库目录
framework/logs		日志文件目录
modules				常用模块的封装
scripts				扫描脚本目录
####2016-01-21:
启动项目
####2016-01-22
增加Gemfile
使用icmp4em包实现了初期的主机存活性检测
对于检测中存在的误报，暂时不予考虑。
初期实现接口即可。
后期可以考虑使用nmap作为主机存活性和端口检测引擎。
####2016-01-25
开始写端口检测模块，开启85个线程探测65535个主机端口，使用socket连接并返回数据。
####2016-01-26
写到端口信息探测和指纹识别模块，不知道该怎么写了，很难啊。
####2016-01-27
暂时什么也没写，在忙别的。晚上有空先测试一下socket通信。
####2016-01-28
貌似是时候写服务识别模块了。这个顺序有点坑，是先判断端口是否开放再决定是否去探测。
恩，这样应该没问题。
####2016-01-29
加入了web目录，看了一下webrick的文档，基本上可以满足制作扫描器的界面需求。
所以web服务采用webrick作为服务器，通过web界面调用后台扫描程序的运行。
####2016-02-02
开始考虑加入数据库，主要的数据库如下：
程序数据库
1).config表
2).端口指纹表
3).扫描任务表
4).漏洞数据表
妈的下班了，不写了回头再说
####2016-02-03
开始写参数处理这部分，还是比较简单的
今天设计了一些程序流程图
####2016-02-17
得赶快开工啊。。。
####2016-02-20
好几天没写代码了，惭愧啊。。。
####2016-02-22
今天开始继续把框架向后推进，现在已经进展到任务创建和poc框架建设
这里有两个想法
一是关于任务创建的，在创建扫描线程的时候，有两个因素要考虑，即待扫描的脚本数量和待扫描的主机数量
一种创建方法是根据扫描目标的数量来创建多线程，另一种是根据扫描脚本的队列来创建线程
我这里分两种方式来创建，当扫描类型为单个脚本时，根据目标的数量来创建
当扫描类型为多个脚本(即规则扫描)时，根据脚本的数量来创建
当扫描类型为task时，可以根据task的类型归类到上述两种类型当中
二是关于poc框架的，扫描器其实就是对可能被攻击的漏洞的验证，所以应当从攻击的角度来划分漏洞的类型
我认为针对计算机(及其他类计算机设备)的远程主机类型攻击的主要参数就是ip/域名+端口+数据这种形式，
而web漏洞可以理解成一种特殊的主机类型攻击，在这种攻击类型当中除了有ip/域名+端口之外，还存在url，http参数等等
其中url，http参数这些可以归为数据当中，所以在poc的体现中仍然属于针对远程主机类型的攻击
所以在创建poc的时候也许会考虑为web类型的poc增加一条属性，来表明其为web，同样的也会针对此种类型创建规则
####2016-02-23
今天开始设计数据库
##一些参考
nmap源码分析-服务与版本扫描
http://qiusuoge.com/11442.html
wyportmap目标端口和指纹识别组件
http://www.hx99.net/Article/Tools/201507/36861.html
操作系统指纹识别概述
http://www.freebuf.com/articles/system/30037.html
###项目笔记
模块
1.界面
2.扫描进程调度模块
3.分布式服务器管理模块（C/S模式）
4.漏洞扫描模块
5.爬虫模块
6.数据库模块
任务的生命周期
创建->运行<->挂起->结束
        |
      结束
C create
R run
P pause
D destroy
任务在数据库中的形态
task_id
task_name
task_attribute
task_type
task_target
task_payload
task_status
task_

分布式扫描：
