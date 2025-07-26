#  风险提示｜泛微OA e-cology XXE 漏洞   
长亭安全应急响应  黑伞安全   2023-07-13 08:14  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/FOh11C4BDicSKRI5jxj81R7Tn37JSxDxbvIZV0K2XwK1YU0J24doK3rVCuRnj8carnqCno11voZ7Cs6YY3Hhzjw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
泛微e-cology是一款由泛微网络科技开发的协同管理平台，支持人力资源、财务、行政等多功能管理和移动办公。近期，长亭科技监测到泛微官方发布了新补丁修复了一处XXE漏洞。长亭应急团队经过分析后发现该漏洞类型为XXE，仍有较多公网系统仍未修复漏洞。长亭应急响应实验室根据漏洞原理编写了X-POC远程检测工具和牧云本地检测工具，目前已向公众开放下载使用。  
**漏洞描述**  
  
   
Description   
  
  
  
**0****1**  
  
泛微e-cology某处功能点最初针对用户输入的过滤不太完善，导致在处理用户输入时可触发XXE。后续修复规则依旧可被绕过，本次漏洞即为之前修复规则的绕过。攻击者可利用该漏洞列目录、读取文件，甚至可能获取应用系统的管理员权限。长亭应急响应实验室经过深入分析发现，本次漏洞需要安装增量补丁10.58.1进行修复，全量补丁10.58.1中未包含相关修复代码。  
**检测工具**  
  
   
Detection   
  
  
  
**0****2**  
#   
#   
X-POC远程检测工具检测方法：```
xpoc -r 401 -t http://xpoc.org
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/FOh11C4BDicSKRI5jxj81R7Tn37JSxDxblXI06gia4gSfcbzeo90fkoow0pjJMdia8GsNyS1kzZ2wF89UQx9wULzQ/640?wx_fmt=png "")  
工具获取方式：https://github.com/chaitin/xpochttps://stack.chaitin.com/tool/detail?id=1036牧云本地检测工具检测方法：在本地主机上执行以下命令即可无害化扫描：weaver_ecology_xxe_vuln_scanner_windows_amd64.exe工具获取方式：https://stack.chaitin.com/tool/detail?id=1198  
**影响范围**  
  
   
Affects  
   
  
  
  
**0****3**  
e-cology 9.x 增量补丁版本 < 10.58.1```
```  
  
**解决方案**  
  
   
Solution   
  
  
  
**0****4**  
临时缓解方案限制访问来源地址，如非必要，不要将系统开放在互联网上。升级修复方案安装增量补丁10.58.1，使用10.58.1全量补丁修复无效。  
**产品支持**  
  
   
Support   
  
  
  
**0****5**  
云图：默认支持该产品的指纹识别，同时支持该漏洞的PoC原理检测。洞鉴：支持该漏洞的PoC原理检测雷池：默认支持该漏洞利用行为的检测。全悉：默认支持该漏洞利用行为的检测。牧云：使用管理平台 23.05.001 及以上版本的用户可通过升级平台下载应急漏洞情报库升级包（EMERVULN-23.07.012）“漏洞应急”功能支持该漏洞的检测；其它管理平台版本暂不支持该漏洞检测。  
  
**时间线**  
  
   
Timeline   
  
  
  
**0****6**  
7月11日 长亭科技获取漏洞情报7月12日 长亭应急响应实验室漏洞分析与复现7月12日 长亭安全应急响应中心发布通告  
参考资料：  
[1].https://www.weaver.com.cn/cs/securityDownload.html?src=cn  
  
  
**长亭应急响应服务**  
  
  
  
  
全力进行产品升级  
  
及时将风险提示预案发送给客户  
  
检测业务是否收到此次漏洞影响  
  
请联系长亭应急团队  
  
7*24小时，守护您的安全  
  
  
第一时间找到我们：  
  
邮箱：support@chaitin.com  
  
应急响应热线：4000-327-707  
  
  
收录于合集   
#  
2023漏洞风险提示  
  
 16  
个  
  
上一篇  
风险提示｜泛微 OA e-cology 前台SQL注入漏洞  
  
  
  
  
