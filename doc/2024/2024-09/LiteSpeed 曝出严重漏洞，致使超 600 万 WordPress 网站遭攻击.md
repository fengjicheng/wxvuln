#  LiteSpeed 曝出严重漏洞，致使超 600 万 WordPress 网站遭攻击   
FreeBuf  商密君   2024-09-08 15:40  
  
近日，Patchstack 的 Rafie Muhammad 在 LiteSpeed Cache 插件中发现了一个严重漏洞，该插件主要用于加快超 600 万个 WordPress 网站的用户浏览速度。该漏洞被追踪为 CVE-2024-44000，并被归类为未经身份验证的帐户接管问题 。随着 LiteSpeed Cache 6.5.0.1 版本的发布，修复程序也于昨天（9月4日）发布。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3iciaZHv8e3qoz13B2w7w92fvMVkwibtKs5407YpE6zFkdmXHic3KOv2ekGTUXGl3lTME0SNU17UibD7og/640?wx_fmt=jpeg&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
##   
  
**调试功能将 cookie 写入文件**  
  
  
## 该漏洞与插件的调试日志功能有关，当启用该功能时，它会将所有 HTTP 响应头（包括 “Set-Cookie ”头）记录到文件中。  
  
  
这些标头包含用于验证用户身份的会话 cookie，一旦攻击者成功窃取这些 cookie，就可以冒充管理员用户完全控制网站。  
  
  
要利用该漏洞，攻击者必须能够访问“/wp-content/debug.log ”中的调试日志文件。在未实施文件访问限制（如 .htaccess 规则）的情况下，只需输入正确的 URL 即可。  
  
  
当然，攻击者只能窃取在调试功能激活时登录网站的用户的会话 cookie，但如果日志被无限期保存而不是定期清除，这甚至包括过去的登录事件。  
  
  
该插件的供应商 LiteSpeed Technologies 通过将调试日志移至专用文件夹（'/wp-content/litespeed/debug/'）、随机化日志文件名、移除记录 Cookie 的选项，以及添加一个虚假索引文件以提供额外保护，解决了这一问题。  
  
  
建议 LiteSpeed Cache 用户清除其服务器上的所有 “debug.log ”文件，以删除可能被威胁行为者窃取的潜在有效会话 cookie。  
  
  
此外，还应设置 .htaccess 规则，拒绝直接访问日志文件，因为新系统上的随机名称仍可能通过暴力破解来猜测。  
  
  
WordPress.org报告称，昨天，也就是v6.5.0.1发布的当天，下载LiteSpeed Cache的用户刚刚超过37.5万，因此易受这些攻击影响的网站数量可能超过560万。  
##   
  
**受到攻击的 LiteSpeed Cache**  
  
  
## LiteSpeed Cache 插件漏洞因其广泛的影响力成为了近期安全研究人员的重点研究对象。与此同时，黑客们一直在寻找机会通过利用该漏洞对网站发起攻击。  
  
  
2024 年 5 月，有人发现黑客利用该插件的一个过时版本（受跟踪为 CVE-2023-40000 的未验证跨站脚本缺陷影响）创建管理员用户并控制网站。  
  
  
今年 8 月 21 日，研究人员又发现了一个关键的未经身份验证的权限升级漏洞，该漏洞被追踪为 CVE-2024-28000，研究人员对利用该漏洞的难度敲响了警钟。  
  
  
该漏洞披露后仅几个小时，威胁者就开始大规模攻击网站，Wordfence 报告称阻止了近 5万次攻击。  
  
  
据统计，在过去的 24 小时内，因其漏洞导致的攻击次数达到了 34 万次。  
  
  
编辑：陈十九  
  
审核：商密君  
  
**征文启事**  
  
大家好，为了更好地促进同业间学术交流，商密君现开启征文活动，只要你对商用密码、网络安全、数据加密等有自己的独到见解和想法，都可以积极向商密君投稿，商密君一定将您的声音传递给更多的人。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/1HyKzSU2XXNcXmbiaiaCljdXpwzOEQ9QTBXMibM6rZTOnbTSwTmCXncQLria2vuLGxn8QPtznzBc0as8vBxWIjrWxQ/640?wx_fmt=jpeg "")  
  
来源：FreeBuf  
  
注：内容均来源于互联网，版权归作者所有，如有侵权，请联系告知，我们将尽快处理。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/1HyKzSU2XXOdeQx0thlyozF2swQTEN9iaaBNDG0jTKfAgqgdesve8x5IEWNvYxjF6sAWjO1TPCZVsWd0oiaDn3uw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1HyKzSU2XXMyyClGk1cttkSBbJicAn5drpXEbFIeChG9IkrslYEylRF4Z6KNaxNafDwr5ibcYaZXdnveQCNIr5kw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1HyKzSU2XXMZPiaDBD8yxbIHiciauWK4tuiaMcJkA69QYZ9T4jmc3fdN6EA7Qq9A8E3RWcTKhxVEU1QjqOgrJMu2Qg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
点分享  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1HyKzSU2XXMZPiaDBD8yxbIHiciauWK4tuiaiaRXdw4BFsc7MxzkVZaKGgtjWA5GKtUfm3hlgzsBtjJ0mnh9QibeFOGQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
点点赞  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1HyKzSU2XXMZPiaDBD8yxbIHiciauWK4tuiaeiaNlRO9954g4VS87icD7KQdxzokTGDIjmCJA563IwfStoFzPUaliauXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
点在看  
  
