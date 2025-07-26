#  IBM Cognos Analytics 漏洞允许攻击者上传恶意文件   
邑安科技  邑安全   2025-05-09 05:33  
  
更多全球网络安全资讯尽在邑安全  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1N39PtINn8sQyd4ibGZtpCJ9ByZ5icq3nxHfWBsFYfcOcP9gImrqB821ZFT6LU4ibBUseT98E8voFucQTss27IfEQ/640?wx_fmt=png&from=appmsg "")  
  
IBM 发布了一个重要的安全建议警告，指出两个影响其 Cognos Analytics 平台的高严重性漏洞，这些漏洞可能允许攻击者上传恶意文件并在受影响的系统上执行代码。  
  
这些漏洞被确定为 CVE-2024-40695 和 CVE-2024-51466，影响了流行的商业智能解决方案的多个版本，需要立即修补。  
  
IBM Cognos Analytics 中的重大漏洞  
  
第一个漏洞 （CVE-2024-40695） 涉及一个恶意文件上传漏洞，CVSS 基本分数为 8.0，归类为 CWE-434（无限制上传危险类型的文件）。  
  
此安全问题的根源是 IBM Cognos Analytics 未能正确验证通过其 Web 界面上传的文件内容  
  
特权用户可以利用此漏洞上传恶意可执行文件，这些文件可以在产品中自动处理，从而在发送给受害者时可能导致进一步的攻击。  
  
“攻击者可以利用这个弱点并将恶意可执行文件上传到系统中，并且可以将其发送给受害者以执行进一步的攻击，”IBM 在其安全公告中表示。  
  
漏洞向量分类为 CVSS：3.0/AV：N/AC：L/PR：L/UI：R/S：U/C：H/I：H/A：H，表示基于网络的攻击向量，具有潜在的严重后果。  
  
第二个漏洞 （CVE-2024-51466） 是表达式语言 （EL） 注入漏洞，CVSS 基本分数更高，为 9.0，分类为 CWE-917。  
  
此漏洞允许远程攻击者利用 Expression Language 语句中未正确中和的特殊元素，而无需用户交互或事先进行身份验证。  
  
“远程攻击者可以利用此漏洞来暴露敏感信息、消耗内存资源和/或在使用特制 EL 语句时导致服务器崩溃，”IBM 公告称。  
  
表达式语言注入漏洞 （CVE-2024-51466） 由 eClinicalWorks 应用程序安全团队的 Vivek Singh 报告给 IBM。  
<table><tbody><tr style="box-sizing: border-box;background-color: rgb(240, 240, 240);"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong msttexthash="7544134" msthash="72" style="box-sizing: border-box;font-weight: bold;"><span leaf=""><span textstyle="" style="font-size: 15px;">CVE 证书</span></span></strong></td><td data-colwidth="131" style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong msttexthash="17242355" msthash="73" style="box-sizing: border-box;font-weight: bold;"><span leaf=""><span textstyle="" style="font-size: 15px;">受影响的产品</span></span></strong></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong msttexthash="4085822" msthash="74" style="box-sizing: border-box;font-weight: bold;"><span leaf=""><span textstyle="" style="font-size: 15px;">冲击</span></span></strong></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong msttexthash="17124536" msthash="75" style="box-sizing: border-box;font-weight: bold;"><span leaf=""><span textstyle="" style="font-size: 15px;">利用先决条件</span></span></strong></td><td data-colwidth="115" style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong msttexthash="8943688" msthash="76" style="box-sizing: border-box;font-weight: bold;"><span leaf=""><span textstyle="" style="font-size: 15px;">CVSS 3.1 分数</span></span></strong></td></tr><tr style="box-sizing: border-box;"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">CVE-2024-40695漏洞</span></span></section></td><td data-colwidth="131" style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">IBM Cognos Analytics 12.0.0–12.0.4、11.2.0–11.2.4 FP4</span></span></section></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">恶意文件上传，允许代码执行或进一步攻击</span></span></section></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">特权用户访问和用户交互</span></span></section></td><td data-colwidth="115" style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">8.0（高）</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(240, 240, 240);"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">CVE-2024-51466漏洞</span></span></section></td><td data-colwidth="131" style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">IBM Cognos Analytics 12.0.0–12.0.4、11.2.0–11.2.4 FP4</span></span></section></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">EL 注入导致数据泄露、资源耗尽或服务器崩溃</span></span></section></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">未经身份验证的远程攻击者</span></span></section></td><td data-colwidth="115" style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><section style="margin-top: 8px;margin-bottom: 8px;"><span leaf=""><span textstyle="" style="font-size: 15px;">9.0（严重）</span></span></section></td></tr></tbody></table>  
受影响的版本和补救措施  
  
这些漏洞会影响 IBM Cognos Analytics 版本 11.2.0 到 11.2.4 FP4 以及 12.0.0 到 12.0.4。  
  
IBM 强烈建议组织立即升级以解决这些漏洞，因为没有可用的解决方法或其他缓解措施。建议的补救路径包括：  
- 对于 IBM Cognos Analytics 12.0.0-12.0.4：升级到 12.0.4 Interim Fix 1。  
  
- 对于 IBM Cognos Analytics 11.2.0-11.2.4 FP4：升级到 11.2.4 FP5。  
  
漏洞扫描程序 Nessus 提供了一个插件 （ID 213474） 来帮助安全团队确定其环境是否受到影响。  
  
这一发现凸显了处理和分析敏感企业数据的企业商业智能平台面临的持续安全挑战。  
  
使用 IBM Cognos Analytics 的组织应将这些补丁作为其安全维护协议的一部分进行优先级排序，以防止这些高严重性缺陷导致潜在的数据泄露和系统危害。  
  
原文来自: cybersecuritynews.com  
  
原文链接:   
https://cybersecuritynews.com/ibm-cognos-analytics-vulnerability-2/  
  
欢迎收藏并分享朋友圈，让五邑人网络更安全  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/1N39PtINn8tD9ic928O6vIrMg4fuib48e1TsRj9K9Cz7RZBD2jjVZcKm1N4QrZ4bwBKZic5crOdItOcdDicPd3yBSg/640?wx_fmt=jpeg "")  
  
欢迎扫描关注我们，及时了解最新安全动态、学习最潮流的安全姿势！  
  
推荐文章  
  
1  
  
[新永恒之蓝？微软SMBv3高危漏洞（CVE-2020-0796）分析复现](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247488913&idx=1&sn=acbf595a4a80dcaba647c7a32fe5e06b&chksm=fa39554bcd4edc5dc90019f33746404ab7593dd9d90109b1076a4a73f2be0cb6fa90e8743b50&scene=21#wechat_redirect)  
  
  
2  
  
[重大漏洞预警：ubuntu最新版本存在本地提权漏洞（已有EXP）　](http://mp.weixin.qq.com/s?__biz=MzUyMzczNzUyNQ==&mid=2247483652&idx=1&sn=b2f2ec90db499e23cfa252e9ee743265&chksm=fa3941decd4ec8c83a268c3480c354a621d515262bcbb5f35e1a2dde8c828bdc7b9011cb5072&scene=21#wechat_redirect)  
  
  
  
