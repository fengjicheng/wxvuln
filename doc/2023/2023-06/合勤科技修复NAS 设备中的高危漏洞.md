#  合勤科技修复NAS 设备中的高危漏洞   
Helga Labus  代码卫士   2023-06-02 17:37  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
  
   
聚焦源代码安全，网罗国内外最新资讯！****  
  
**编译：代码卫士**  
  
****  
**合勤科技 (Zyxel) 修复了家庭用户所使用的 NAS 设备中的一个高危认证命令注入漏洞 (CVE-2023-27988)。**  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/oBANLWYScMRXJlqiaBqkj0pz1iau7yfTAONN3u5ibjNoLzfY8VR7YmyeySsWkg7NmKlaQZenL6b7mGI3FXLphOVJA/640?wx_fmt=png "")  
  
  
**漏洞简述**  
  
  
该漏洞位于设备的 web 管理接口中。  
  
合勤科技证实称，“具有管理员权限的认证攻击者可利用该漏洞，远程在受影响设备上执行某些操作系统命令。”如下设备受影响：  
  
- NAS326 版本5.2.1 (AAZF.12) C0及以下版本  
  
- NAS540版本5.21 (AAZF.9) C0及以下版本  
  
- NAS542版本5.21 (AAZF.9) C0及以下版本  
  
  
  
该漏洞由 Sternum 公司的研究员报告，研究员发布了关于该漏洞的根因分析并说明了如何使目标设备做出非预期内行为。  
  
研究人员解释称，“这些测试已经证实，认证攻击者可通过设备上的根权限执行任意系统命令。最终，这些漏洞可用于执行更加恶意的操作如远程恶意软件注入。”  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/oBANLWYScMRXJlqiaBqkj0pz1iau7yfTAONN3u5ibjNoLzfY8VR7YmyeySsWkg7NmKlaQZenL6b7mGI3FXLphOVJA/640?wx_fmt=png "")  
  
  
**迅速修复**  
  
  
合勤科技在本周二发布固件更新，用户应尽快执行这些更新。该公司并未提到可能的应变措施。虽然目前并不存在漏洞遭利用的证据，但 NAS 设备一般是受网络犯罪分子喜欢的目标，之前 QNAP NAS 设备多次遭勒索攻击。  
  
  
  
代码卫士试用地址：  
https://codesafe.qianxin.com  
  
开源卫士试用地址：https://oss.qianxin.com  
  
  
  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247511052&idx=3&sn=fb116392e405ae62e6c339117fffdb59&chksm=ea949d66dde31470758b6ee8f9dbecdb67ef6c0c8af277f26b83b60dbac95748d28db787a4b4&scene=21#wechat_redirect)  
[奇安信入选全球《软件成分分析全景图》代表厂商](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247515374&idx=1&sn=8b491039bc40f1e5d4e1b29d8c95f9e7&chksm=ea948d84dde30492f8a6c9953f69dbed1f483b6bc9b4480cab641fbc69459d46bab41cdc4859&scene=21#wechat_redirect)  
  
  
[合勤科技防火墙和VPN设备中存在多个严重漏洞](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247516588&idx=2&sn=9de12fd66aca0db1dd5cf6843cf5172b&chksm=ea94b0c6dde339d067936cdf6ad3806f1669be3d65022c613d8544726872d4dca7d3377e4a9f&scene=21#wechat_redirect)  
  
  
[刚刚，合勤科技发布NAS新固件，修复严重的RCE漏洞](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247513843&idx=2&sn=8542b0597bb31128891e9f651a8afc17&chksm=ea948799dde30e8fec2983d236de83edbc94c48dc8271c57d4c59d10286c37ff6d303542a054&scene=21#wechat_redirect)  
  
  
[合勤科技修复四个高危漏洞，影响AP、API控制器和防火墙设备](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247512001&idx=3&sn=e25d8213ca24152e4fe49ee900f53295&chksm=ea949eabdde317bdbdb50c88bc48a6238c3d4eb1a57347b38b9cfba3db4a24295bf78c1d8951&scene=21#wechat_redirect)  
  
  
[合勤科技称企业防火墙和VPN设备遭复杂攻击](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247506015&idx=2&sn=0f9526d0ee1779ec004d5b2eeabd6a1d&chksm=ea94e935dde360235b0955a9b0143d20560258e44f96dc4a2023d86ba3a34b836e6068a3e766&scene=21#wechat_redirect)  
  
  
[10万+合勤科技防火墙和 VPN 网关被曝秘密后门](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247499934&idx=2&sn=a370ec84c6da47a9b94ccab3a8e78b50&chksm=ea94f1f4dde378e22118864dc9ffe43574f156308485b517eef58b5e7514a53f5c44b11e45c1&scene=21#wechat_redirect)  
  
  
  
  
**原文链接**  
  
https://www.helpnetsecurity.com/2023/05/31/cve-2023-27988/  
  
  
题图：Pexels License  
  
  
**本文由奇安信编译，不代表奇安信观点。转载请注明“转自奇安信代码卫士 https://codesafe.qianxin.com”。**  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/oBANLWYScMSf7nNLWrJL6dkJp7RB8Kl4zxU9ibnQjuvo4VoZ5ic9Q91K3WshWzqEybcroVEOQpgYfx1uYgwJhlFQ/640?wx_fmt=jpeg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/oBANLWYScMSN5sfviaCuvYQccJZlrr64sRlvcbdWjDic9mPQ8mBBFDCKP6VibiaNE1kDVuoIOiaIVRoTjSsSftGC8gw/640?wx_fmt=jpeg "")  
  
**奇安信代码卫士 (codesafe)**  
  
国内首个专注于软件开发安全的产品线。  
  
   ![](https://mmbiz.qpic.cn/mmbiz_gif/oBANLWYScMQ5iciaeKS21icDIWSVd0M9zEhicFK0rbCJOrgpc09iaH6nvqvsIdckDfxH2K4tu9CvPJgSf7XhGHJwVyQ/640?wx_fmt=gif "")  
  
   
觉得不错，就点个 “  
在看  
” 或 "  
赞  
” 吧~  
  
