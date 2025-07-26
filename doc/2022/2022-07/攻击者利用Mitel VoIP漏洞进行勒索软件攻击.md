#  攻击者利用Mitel VoIP漏洞进行勒索软件攻击   
~阳光~  嘶吼专业版   2022-07-08 12:00  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/wpkib3J60o297rwgIksvLibPOwR24tqI8dGRUah80YoBLjTBJgws2n0ibdvfvv3CCm0MIOHTAgKicmOB4UHUJ1hH5g/640?wx_fmt=gif "")  
  
勒索软件集团正在滥用未打补丁的基于Linux的Mitel VoIP（网络电话）应用程序，并将其作为跳板在目标系统上植入恶意软件。这个关键的远程代码执行（RCE）漏洞被追踪为CVE-2022-29499，该漏洞是Crowdstrike在4月首次报告的零日漏洞，并且现在已经打了补丁。  
  
Mitel由于向各种组织提供商业电话系统和统一通信服务（UCaaS）而广为人知。Mitel专注于VoIP技术，允许用户使用互联网连接而不是普通的电话线来拨打电话。  
  
据Crowdstrike称，该漏洞影响了Mitel MiVoice设备SA 100、SA 400和虚拟SA。MiVoice提供了一个简单的界面，并将所有的通信和工具结合了起来。  
# 该漏洞被用来植入勒索软件   
  
Crowdstrike的研究人员最近调查了一次疑似勒索软件攻击。研究团队迅速处理了该入侵事件，并认为勒索软件攻击利用了该漏洞（CVE-2022-29499）。  
  
Crowdstrike发现恶意活动的源头与一个基于Linux的Mitel VoIP设备的IP地址有关。进一步分析发现这是一个最新的远程代码执行漏洞。  
  
Patrick Bennet在一篇博文中写道，该设备已经被下线并进行了特殊处理，以对其做进一步分析，从而发现了一个新型的远程代码执行漏洞，该漏洞可以被威胁者用来获得对环境的初始访问权限。  
  
该漏洞的利用主要是使用了两个GET请求。第一个是针对一个PHP文件的 "get_url "参数，第二个是来自于设备本身。  
  
研究员解释说，这第一个请求是必要的，因为含有漏洞的URL被限制接收来自外部IP地址的请求。  
  
第二个请求通过对攻击者控制的基础设施进行HTTP GET请求来执行命令注入，并在攻击者的服务器上运行存储的命令。  
  
据研究人员说，攻击者利用这个漏洞，通过 "mkfifo "命令和 "openssl_client "创建一个支持SSL的反向shell，并从被攻击的网络中向外发送请求。mkfifo 命令用于创建一个由文件参数指定的特殊文件，并可由多个进程打开并用于读写。  
  
一旦反向shell建立了起来，攻击者就会创建一个名为 "pdf_import.php "的网络连接。网络连接的原始内容并没有被恢复，但研究人员发现了一个日志文件，其中包括一个向利用该漏洞来源的IP地址发送的POST请求。攻击者还在VoIP设备上下载了一个名为 "Chisel "的隧道工具，这样可以进一步深入到网络内部而不被发现。  
  
Crowdstrike还发现了威胁者为掩盖攻击活动而实施的反取证技术。  
  
Bennett说，尽管威胁者删除了VoIP设备文件系统中的所有文件，但CrowdStrike还是能够从该设备中恢复取证数据。这包括最初用于破坏设备的漏洞，威胁者随后下载到设备上的工具，甚至包括威胁者采取的特定反取证措施的证据。  
  
Mitel于2022年4月19日发布了针对MiVoice Connect 19.2 SP3及以前版本的安全公告，并且目前还没有发布官方补丁。  
# Shodan上搜索有漏洞的Mitel设备  
  
安全研究员Kevin Beaumont在Twitter上分享了一个字符串 "http.html_hash:-1971546278"，该字符串可以在Shodan搜索引擎上搜索有漏洞的Mitel设备。据Kevin称，全球大约有21,000台可公开访问的Mitel设备，其中大部分位于美国，继而是英国。  
# Mitel公司的缓解建议  
  
Crowdstrike建议企业通过使用威胁建模和识别恶意活动来加强防御机制。该研究人员还建议隔离关键资产和周边设备，限制访问控制，以防周边设备遭到破坏。  
  
研究人员解释说，及时打补丁是保护周边设备的关键。然而，当攻击者利用那些未记录的漏洞时，及时打补丁就已经变得不重要了。  
  
参考及来源：  
https://threatpost.com/mitel-voip-bug-exploited/180079/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icGh5SfpLbxXAW4ZUoCCLo9f16W87DFLibspaRcFuR9eic8tOwd44OTySUGOPIJ0MaLiahEmRgogvpPw/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icGh5SfpLbxXAW4ZUoCCLo9kG8sNRtw4XD7icHbKYSn4ELDniasjt61KDvxGibNibuAqTUbBHZiaVGhXng/640?wx_fmt=png "")  
  
