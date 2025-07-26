#  盘点那些漏洞 POC 测试工具   
原创 xazlwiki  信安之路   2023-09-18 11:23  
  
大家常说的安全漏洞，根据漏洞类型划分，我们知道有 OWASP TOP 10，这类漏洞的测试有固定的测试 payload，需要借助网络爬虫的能力，发现网站接口，然后针对这些功能接口进行 payload 测试，从而发现漏洞。  
  
除此之外，还有一些隐藏比较深的漏洞也可以自动化发现，这种漏洞也就是我们常说的 Nday 漏洞，这类漏洞的发现方式比较多样，有通过黑盒测试发现的，也有通过白盒代码审计发现，这类漏洞所对应的系统是通用的，可以是商业的解决方案，也可以是开源的应用系统，通俗点说就是不只一家企业在用。  
  
nday 漏洞的效果与所对应的系统使用量有关，使用的企业越多，那么可能还存在相应漏洞的数量就越大，因为漏洞的修复率是与漏洞补丁的发布时间是有关的，时间越长，修复率越高，时间越短，修复率越低，存在的漏洞的系统就越多，所以就有 1day 和 nday 的说法，1day 通常来说存在漏洞的概率比较大。  
  
我们最喜欢的就是系统共用数量更多的 1day 漏洞，因为，分分钟可以发现很多存在漏洞的系统，今天来给大家判断一下常用的 nday 漏洞扫描工具有哪些。  
  
评价一个 nday 漏洞扫描工具的能力，一方面是它的扫描速度，另一方面就是 POC 的覆盖度和准确度，如果你有相关工具的使用经验，欢迎留言。  
### 0x01 Xpoc/Xray  
  
Xpoc 是长亭发布的 poc 扫描工具，其默认自带 406 个 POC 规则，覆盖国内常见的应用系统和历史危害比较高的漏洞，通过命令可以自动更新远程 POC 到本地，使用也比较方便，在公开重大漏洞的时候，更新还是比较快的，如图：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfflJHTF6Og32kC0qe0QW2cBuAhZ0rsZsMtYKUQ8JSarv7YanDRhnADtyyRGmOuvgXOPLGnI9KkCWw/640?wx_fmt=png "")  
  
项目地址：  
> https://stack.chaitin.com/tool/detail/1036  
  
  
Xpoc 可以说是从 Xray 中剥离出来专注做 POC 验证的工具，Xray 能力相对比较综合，具有 POC 验证，常规漏洞扫描，爬虫、子域名枚举等功能。  
### 0x02 Nuclei  
  
Nuclei 是国外的一款开源 POC 扫描工具，社区有大量的研究员为其提供 POC，更新频率比较快，对于国外的系统覆盖更好，国内常见的漏洞也有覆盖，目前已有 6000+ 的 POC，具体如图：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfflJHTF6Og32kC0qe0QW2cBqoAUicBia2RvSUEicMgGRtEmpAySr180SC1xtSaMqGYER0bDcEp3tepFQ/640?wx_fmt=png "")  
  
项目地址：  
> https://github.com/projectdiscovery/nuclei  
  
  
POC 模板库：  
> https://github.com/projectdiscovery/nuclei-templates  
  
### 0x03 afrog  
  
afrog 是国内安全研究员 zan8in 开发维护的开源项目，国内众多白帽子为其添砖加瓦，POC 更新还算及时，目前拥有的 POC 超过 1100，如图：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfflJHTF6Og32kC0qe0QW2cBVDR0s1KeYIiaZ1byrrN0U76WM5nqMC0vJ4ljX57zmstbbGsiatfHNtRA/640?wx_fmt=png "")  
  
项目地址：  
> https://github.com/zan8in/afrog  
  
### 0x04 yakit  
  
yakit 也叫集成化单兵安全能力平台，不仅仅是 POC 扫描，还包括常规漏洞的扫描，通过 GUI 操作，对于不想写代码的安全从业者是个不错的选择，通过插件的方式扩展其能力，比如可以将 nuclei 的模板库导入进去供其使用，如图：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfflJHTF6Og32kC0qe0QW2cBosEfqxGpIia5nGr0rLyuhvzrXdb70KYVRSUncyXXBF4YwibH3PvzKOLw/640?wx_fmt=png "")  
  
项目地址：  
> https://yaklang.io/products/intro/  
  
  
看着很有吸引力，目前还没有怎么使用过，后续有一些心得之后再做分享。  
### 0x05 Goby  
  
Goby 是国内企业白帽汇开发维护的商业系统，也可以免费使用，免费版自带 100+ POC，如图：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/sGfPWsuKAfflJHTF6Og32kC0qe0QW2cBrRw1DU29Ezicyyia48rcJLFPqXibicfvibXjtBt0KbPn1S2CJCAy5OiafDwg/640?wx_fmt=png "")  
  
官网：  
> https://gobysec.net/  
  
  
其可以通过插件来扩展其能力。  
### 0x06 总结  
  
以上工具覆盖肯定不全，还有其他专注 POC 检测的工具，比如知道创宇的 pocsuite3，只随软件携带来十几个案例 POC，目前我使用最多的就是 xray、xpoc、nuclei 这些工具，最近开始用 afrog，大家有好用的工具欢迎留言推荐，一起学习。  
  
文库目录：https://wiki.xazlsec.com/static/forder.html  
  
**欢迎注册文库，阅读更多内容**  
  
