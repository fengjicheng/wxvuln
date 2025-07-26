#  Splunk 高危漏洞：攻击者可通过文件上传执行任意代码   
 信息安全大事件   2025-03-28 18:02  
  
Splunk 近日发布补丁，修复了影响 Splunk Enterprise 和 Splunk Cloud Platform 的高危远程代码执行（RCE）漏洞。该漏洞编号为 CVE-2025-20229，可能允许低权限用户通过上传恶意文件执行任意代码。  
  
漏洞影响范围  
  
该漏洞存在于以下版本中：  
- Splunk Enterprise：9.3.3、9.2.5 和 9.1.8 之前的版本  
  
- Splunk Cloud Platform：9.3.2408.104、9.2.2406.108、9.2.2403.114 和 9.1.2312.208 之前的版本  
  
根据  
 Splunk 的安全公告，即使没有"admin"或"power"权限的低权限用户也可利用此漏洞。攻击者通过向"$SPLUNK_HOME/var/run/splunk/apptemp"目录上传文件，即可绕过必要的授权检查。  
  
Splunk 为该漏洞评定的 CVSSv3.1 分数为 8.0（高危），攻击向量为 CVSS:3.1/AV:N/AC:L/PR:L/UI:R/S:U/C:H/I:H/A:H。  
  
修复建议  
  
Splunk 建议用户采取以下措施修复漏洞：  
- Splunk Enterprise 用户：升级至 9.4.0、9.3.3、9.2.5、9.1.8 或更高版本  
  
- Splunk Cloud Platform 用户：Splunk 正在主动监控和修补实例  
  
Splunk Secure Gateway 应用漏洞  
  
除上述  
 RCE 漏洞外，Splunk 还披露了影响 Splunk Secure Gateway 应用的另一个高危漏洞（CVE-2025-20231）。该漏洞可能允许低权限用户以高权限用户的权限进行搜索，导致敏感信息泄露。  
<table><tbody><tr><td data-colwidth="141" style="border-color:#000000;"><section style="text-align: center;"><span leaf=""><span textstyle="" style="font-weight: bold;">产品</span></span></section></td><td data-colwidth="264" style="border-color:#000000;"><section style="text-align: center;"><span leaf=""><span textstyle="" style="font-weight: bold;">受影响版本</span></span></section></td><td data-colwidth="169" style="border-color:#000000;"><section style="text-align: center;"><span leaf=""><span textstyle="" style="font-weight: bold;">修复版本</span></span></section></td></tr><tr><td data-colwidth="141" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">Splunk Enterprise</span></span></section></td><td data-colwidth="264" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">9.3.0-9.3.2, 9.2.0-9.2.4, 9.1.0-9.1.7</span></span></section></td><td data-colwidth="169" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">9.3.3, 9.2.5, 9.1.8, 9.4.0</span></span></section></td></tr><tr><td data-colwidth="141" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">Splunk Cloud Platform</span></span></section></td><td data-colwidth="264" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">9.3.2408.100-9.3.2408.103, 9.2.2406.100-9.2.2406.107, 低于 9.2.2403.113, 低于 9.1.2312.207</span></span></section></td><td data-colwidth="169" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">9.3.2408.104, 9.2.2406.108, 9.2.2403.114, 9.1.2312.208</span></span></section></td></tr><tr><td data-colwidth="141" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">Splunk Secure Gateway App</span></span></section></td><td data-colwidth="264" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">低于 3.8.38, 低于 3.7.23</span></span></section></td><td data-colwidth="169" style="border-color:#000000;"><section><span leaf=""><span textstyle="" style="font-size: 15px;">3.8.38, 3.7.23</span></span></section></td></tr></tbody></table>  
  
  
漏洞详情  
  
当调用  
/services/ssg/secretsREST 端点时，Splunk Secure Gateway 会在 splunk_secure_gateway.log 文件中以明文形式暴露用户会话和授权令牌。成功利用此漏洞需要攻击者诱骗受害者在浏览器中发起请求。  
  
Splunk 将该漏洞评为高危，CVSSv3.1 分数为 7.1，攻击向量为 CVSS:3.1/AV:N/AC:H/PR:L/UI:R/S:U/C:H/I:H/A:H。  
  
解决方案  
  
Splunk 建议：  
- 升级  
 Splunk Enterprise 至 9.4.1、9.3.3、9.2.5 和 9.1.8 或更高版本  
  
- Splunk Cloud Platform 实例正在主动修补中  
  
用户可临时禁用  
 Splunk Secure Gateway 应用作为缓解措施，但这可能影响 Splunk Mobile、Spacebridge 和 Mission Control 用户的功能。Splunk 建议客户及时关注安全更新并尽快应用补丁，以保护系统免受潜在攻击。  
  
来源： FreeBuf  
  
<table><tbody><tr class="ue-table-interlace-color-single js_darkmode__3" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;background-color: rgb(28, 28, 28);visibility: visible;color: rgb(205, 205, 205) !important;"><td data-colwidth="557" width="557" valign="top" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(76, 76, 76);max-width: 100%;box-sizing: border-box !important;background-color: rgb(255, 218, 169);visibility: visible;color: rgb(25, 25, 25) !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;line-height: normal;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;font-size: 12px;visibility: visible;color: rgb(0, 0, 0);"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">尊敬的读者：</span><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><br/></span><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">感谢您花时间阅读我们提供的这篇文章。我们非常重视您的时间和精力，并深知信息对您的重要性。</span><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><br/></span><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">我们希望了解您对这篇文章的看法和感受。我们真诚地想知道您是否认为这篇文章为您带来了有价值的资讯和启示，是否有助于您的个人或职业发展。</span><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><br/></span><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">如果您认为这篇文章对您非常有价值，并且希望获得更多的相关资讯和服务，我们愿意为您提供进一步的定制化服务。请通过填写我们提供的在线表单，与我们联系并提供您的邮箱地址或其他联系方式。我们将定期向您发送相关资讯和更新，以帮助您更好地了解我们的服务和文章内容。</span></span></section><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;line-height: normal;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><br/></span></section><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;line-height: normal;text-indent: 0em;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;color: rgb(0, 0, 0);"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">                   </span></span><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><img data-src="https://mmbiz.qpic.cn/sz_mmbiz_png/JqliagemfTA5N8G6ZVujodYTTD7NSaxFG5suXlkibicfoGRzCk6vHhCUBx7ST8b4AxdsFVNNAH4ltePBWX4AxKY0A/640?wx_fmt=other&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1&amp;tp=webp" alt="图片" class="rich_pages wxw-img" data-ratio="1" data-s="300,640" data-type="png" data-w="1000" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;vertical-align: bottom;height: auto !important;font-family: 宋体;font-size: 14px;letter-spacing: 0.578px;text-align: center;visibility: visible !important;width: 119px !important;" data-cropselx1="0" data-cropselx2="119" data-cropsely1="0" data-cropsely2="119" data-backw="106" data-backh="106" data-imgfileid="100006629"/></span></section><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;line-height: normal;text-indent: 0em;"><span style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;font-family: 宋体;font-size: 12px;letter-spacing: 0.578px;text-align: center;color: rgb(0, 0, 0);"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">                               扫描二维码，参与调查</span></span></section><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;line-height: normal;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><br/></span></section></td></tr></tbody></table>  
  
  
**END**  
  
  
  
点击下方，关注公众号  
  
获取免费咨询和安全服务  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/JqliagemfTA5OxIlGh6IbpxrTJHkcY5DZ4O80nevX4Ev7IHvjZfPZDDMxibSVWk4IdYfaYpuhBgz2iaWS5tzXZLJw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
  
  
安全咨询/安全集成/安全运营  
  
专业可信的信息安全应用服务商！  
  
http://www.jsgjxx.com  
  
  
  
