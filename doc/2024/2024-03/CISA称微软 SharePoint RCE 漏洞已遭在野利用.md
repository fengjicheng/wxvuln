#  CISA称微软 SharePoint RCE 漏洞已遭在野利用   
Sergiu Gatlan  代码卫士   2024-03-28 17:29  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
  
   
聚焦源代码安全，网罗国内外最新资讯！  
  
**编译：代码卫士**  
  
**CISA 提醒称攻击者正在利用微软 SharePoint 代码注入漏洞发动攻击。而组合一个严重的提权漏洞，即可执行预认证RCE攻击。第一个漏洞的编号是CVE-2023-24955，可导致具有Site Owner权限的认证攻击者在易受攻击服务器上远程执行代码。第二个漏洞CVE-2023-29357可导致远程攻击者通过受欺骗的 JWT 认证令牌绕过认证，获得易受攻击 SharePoint 服务器上的管理员权限。**  
  
  
STAR Labs 研究员 Janggggg 在2023年Pwn2Own 温哥华大赛上演示表明，未认证攻击者可组合利用这两个漏洞在未修复服务器上获得RCE权限。就在该研究员发布技术详情说明利用过程后的第二天即9月25日，GitHub 上就出现了CVE-2023-29357的PoC利用。  
  
尽管该 PoC 利用无法使攻击者获得目标系统的RCE权限，但威胁人员仍然可进行修改，通过CVE-2023-24955利用能力完成利用链，发动 RCE 攻击。之后，针对该利用链的多个 PoC 利用出现在网络，使得技能较低的攻击者也可使用。一个月之后，CISA 将 CVE-2023-29357增加到必修清单并要求美国联邦机构在1月31日前修复。  
  
本周二，CISA将CVE-2023-24955代码注入漏洞也纳入已遭利用漏洞清单。根据要求，联邦机构必须在4月16日前保护SharePoint服务器的安全。虽然CISA并未说明组合利用这两个Sharepoint漏洞的共计详情，但表示并未用于勒索攻击中。  
  
CISA 提到，“这类漏洞常常是恶意网络人员的攻击向量，并对联邦企业带来重大风险。”虽然CISA的必修清单主要关注的是联邦机构尽快修复漏洞，但建议私营组织机构也优先修复该利用链。  
  
  
  
代码卫士试用地址：  
https://codesafe.qianxin.com  
  
开源卫士试用地址：https://oss.qianxin.com  
  
  
  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[新瓶装旧酒：微软修复3个月后，SharePoint RCE 漏洞重现](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247511813&idx=2&sn=88dc3853240665eb7fc5ad56a21a50ea&chksm=ea949e6fdde317799af44ba2161215a6021d98607b5a9de999db326342e3b7c84125d847801f&scene=21#wechat_redirect)  
  
  
[微软将本地版Exchange、SharePoint和Skype 纳入漏洞奖励计划](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247511275&idx=4&sn=efe2927c135f8cf5c5c2663330e1a4f3&chksm=ea949d81dde31497c5ade51ddb1de4709626af7f0d773fe10c928c1eb7a230fd5b290868d5ff&scene=21#wechat_redirect)  
  
  
[比 Windows DNS 蠕虫漏洞更严重！SharePoint 反序列化RCE漏洞详情已发布，速修复](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247494226&idx=1&sn=53966f7edd1a0b5bdec134da241a8ff9&chksm=ea94db38dde3522e0d1cb2eae0ecad083af835002e18ce768903161a44d76995eda6387e65c9&scene=21#wechat_redirect)  
  
  
[多个黑客组织正在攻击微软 SharePoint 服务器](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247489933&idx=2&sn=ba11edbd165005c6383a671b30670e8d&chksm=ea9728e7dde0a1f11f5ac3d441fdc41b8f67589a5c0f4dfede8f58f548e665f2babc357da100&scene=21#wechat_redirect)  
  
  
[微软修复SharePoint 2013中的XSS漏洞](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247485881&idx=3&sn=75aece8bc928931b58e5c27ace701fcd&chksm=ea9738d3dde0b1c5964f135eb636af076c88ad9ce2c2511d44b1898b43f70c251680f2f7122f&scene=21#wechat_redirect)  
  
  
  
  
**原文链接**  
  
  
https://www.bleepingcomputer.com/news/security/cisa-tags-microsoft-sharepoint-rce-bug-as-actively-exploited/  
  
  
题图：  
Pixabay  
 License  
  
****  
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
  
