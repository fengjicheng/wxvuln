#  Google 修复了在野外积极利用的高严重性 Chrome 漏洞   
 信息安全大事件   2024-08-22 20:34  
  
谷歌已经推出了安全修复程序，以解决其Chrome浏览器中一个严重性高的安全漏洞，它表示该漏洞已在野外受到积极利用。  
  
该漏洞被跟踪为   
CVE-2024-7971，被描述为 V8 JavaScript 和 WebAssembly 引擎中的类型混淆错误。  
  
根据   
NIST 国家漏洞数据库 （NVD） 中对该漏洞的描述，“在 128.0.6613.84 之前的 Google Chrome 中，V8 中的类型混淆允许远程攻击者通过构建的 HTML 页面利用堆损坏。  
  
Microsoft 威胁情报中心 （MSTIC） 和 Microsoft 安全响应中心 （MSRC） 于 2024 年 8 月 19 日发现并报告了该漏洞。  
  
没有发布有关利用该漏洞的攻击性质或可能将其武器化的威胁行为者身份的更多详细信息，主要是为了确保大多数用户都更新了修复程序。  
  
然而，这家科技巨头在一份简短的声明中承认，它“意识到 CVE-2024-7971 的漏洞存在于野外”。值得一提的是，CVE-2024-7971 是继 CVE-2024-4947 和 CVE-2024-5274 之后，今年在 V8 中修补的第三类混淆 bug。  
  
自   
2024 年初以来，谷歌迄今为止已经解决了 Chrome 中的 9 个零日问题，其中包括在 Pwn2Own 2024 上展示的三个 -  
- CVE-2024-0519 - V8 中的越界内存访问  
  
- CVE-2024-2886 - WebCodecs 中的释放后使用（在 Pwn2Own 2024 上演示）  
  
- CVE-2024-2887 - WebAssembly 中的类型混淆（在 Pwn2Own 2024 上演示）  
  
- CVE-2024-3159 - V8 中的越界内存访问（在 Pwn2Own 2024 上演示）  
  
- CVE-2024-4671 - 视觉对象中的释放后使用  
  
- CVE-2024-4761 - V8 中的越界写入  
  
- CVE-2024-4947 - V8 中的类型混淆  
  
- CVE-2024-5274 - V8 中的类型混淆  
  
建议用户升级到  
   
Windows 和 macOS 的 Chrome 版本 128.0.6613.84/.85，以及 Linux 的 128.0.6613.84 版本，以缓解潜在威胁。  
  
还建议使用   
Microsoft Edge、Brave、Opera 和 Vivaldi 等基于 Chromium 的浏览器的用户在修复程序可用时应用它们。  
  
<table><tbody style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><tr class="ue-table-interlace-color-single js_darkmode__0" data-style="-webkit-tap-highlight-color: transparent; outline: 0px; background-color: rgb(28, 28, 28); visibility: visible; color: rgb(205, 205, 205) !important;" style="-webkit-tap-highlight-color: transparent;outline: 0px;background-color: rgb(28, 28, 28);visibility: visible;color: rgb(205, 205, 205) !important;"><td width="557" valign="top" data-style="-webkit-tap-highlight-color: transparent; outline: 0px; word-break: break-all; hyphens: auto; border-color: rgb(76, 76, 76); background-color: rgb(255, 218, 169); visibility: visible; color: rgb(25, 25, 25) !important;" class="js_darkmode__1" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-color: rgb(76, 76, 76);background-color: rgb(255, 218, 169);visibility: visible;color: rgb(25, 25, 25) !important;"><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-size: 12px;visibility: visible;color: rgb(0, 0, 0);">尊敬的读者：<br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/>感谢您花时间阅读我们提供的这篇文章。我们非常重视您的时间和精力，并深知信息对您的重要性。<br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/>我们希望了解您对这篇文章的看法和感受。我们真诚地想知道您是否认为这篇文章为您带来了有价值的资讯和启示，是否有助于您的个人或职业发展。<br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/>如果您认为这篇文章对您非常有价值，并且希望获得更多的相关资讯和服务，我们愿意为您提供进一步的定制化服务。请通过填写我们提供的在线表单，与我们联系并提供您的邮箱地址或其他联系方式。我们将定期向您发送相关资讯和更新，以帮助您更好地了解我们的服务和文章内容。</span></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;visibility: visible;"><br style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"/></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;text-indent: 0em;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(0, 0, 0);">                       </span><img class="rich_pages wxw-img" data-backh="106" data-backw="106" data-cropselx1="0" data-cropselx2="119" data-cropsely1="0" data-cropsely2="119" data-galleryid="" data-imgfileid="100006010" data-ratio="1" data-s="300,640" data-type="png" data-w="1000" data-src="https://mmbiz.qpic.cn/sz_mmbiz_png/JqliagemfTA5N8G6ZVujodYTTD7NSaxFG5suXlkibicfoGRzCk6vHhCUBx7ST8b4AxdsFVNNAH4ltePBWX4AxKY0A/640?wx_fmt=other&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1&amp;tp=webp" style="-webkit-tap-highlight-color: transparent;outline: 0px;font-family: 宋体;font-size: 14px;letter-spacing: 0.578px;text-align: center;visibility: visible !important;width: 119px !important;"/></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;text-indent: 0em;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-family: 宋体;font-size: 12px;letter-spacing: 0.578px;text-align: center;color: rgb(0, 0, 0);">                               扫描二维码，参与调查</span></section><section style="-webkit-tap-highlight-color: transparent;outline: 0px;line-height: normal;"><br style="-webkit-tap-highlight-color: transparent;outline: 0px;letter-spacing: 0.544px;"/></section></td></tr></tbody></table>  
  
**END**  
  
  
  
点击下方，关注公众号  
  
获取免费咨询和安全服务  
  
![](https://mmbiz.qpic.cn/mmbiz_png/JqliagemfTA5OxIlGh6IbpxrTJHkcY5DZ4O80nevX4Ev7IHvjZfPZDDMxibSVWk4IdYfaYpuhBgz2iaWS5tzXZLJw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
  
  
安全咨询/安全集成/安全运营  
  
专业可信的信息安全应用服务商！  
  
http://www.jsgjxx.com  
  
