#  热门开源PDF库 Ghostscript中存在严重的RCE漏洞   
Bill Toulas  代码卫士   2023-07-13 18:22  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
  
   
聚焦源代码安全，网罗国内外最新资讯！****  
  
**编译：代码卫士**  
  
**广泛用于 Linux 中的PostScript 语言和 PDF 文件的开源解释器 Ghostscript 易受一个严重远程代码执行 (RCE) 漏洞影响。该漏洞编号是CVE-2023-3664，CVSS评分为9.8，影响所有早于10.01.2的 Ghostscript 版本。**  
  
  
Kroll 公司的分析师 G.Glass 和 D.Truman 开发了相关 PoC 利用，他们指出，只需打开恶意的特殊构造的文件即可触发代码执行。鉴于 Ghostscript 默认安装在无数 Linux 发行版本中且广泛用于多种软件中如 LibreOffice、GIMP、Inkscape、Scribus、ImageMagick和 CUPS 打印系统，因此在多数情况下存在很多触发该漏洞的机会。  
  
Kroll 公司认为如果 Windows 上的开源应用使用了 Ghostscript 端口，则也受影响。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/oBANLWYScMRIx5ocqdpLnMUokS9Iy8IJreRrxRpIhEOWEFoNG71N7z86AUXdgGhFnX9zqv8EUAU5vlvY6rwPkA/640?wx_fmt=png "")  
  
**漏洞简析**  
  
  
CVE-2023-3664和操作系统管道有关，可导致不同应用通过将输出作为输入传递给另外一款数据而进行数据交换。该漏洞源自 Ghostscript 中的 “gp_file_name_reduce()”函数。该函数提取多个路径并进行组合，且通过删除相对路径引用进行简化以提高效率。然而，如果受影响函数收到特殊构造的路径，则可返回异常结果，从而导致验证机制遭覆盖以及引发潜在利用。另外，当 Ghostscript 试图打开文件时，它会使用另外一个函数 “gp_validate_path”检查位置是否安全。  
  
然而，由于该易受攻击的函数在第二个函数检查前会更改位置详情，则攻击者可轻易利用该漏洞并强制 Ghostscript 在应该被限制的位置处理文件。  
  
研究人员提到通过在使用 Ghostscript 的任何应用上打开一个 EPS 文件就能触发 PoC。  
  
建议 Linux 用户升级至 Ghostscript 最新版本10.01.2。如所用发行版本的软件无法获取最新版本，则建议从源代码对其进行编译。遗憾的是，使用 Ghostscript 端口的 Windows 上的开源软件会要求更多时间迁移到最新版本，因此建议尤其注意 Windows 中的安装。  
  
研究人员还在 GitHub 仓库中分享了 Sigma 规则用于漏洞检测：https://github.com/KrollCYB/Kroll-CYB/tree/main/CVE-2023-36664。  
  
  
  
  
代码卫士试用地址：  
https://codesafe.qianxin.com  
  
开源卫士试用地址：https://oss.qianxin.com  
  
  
  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[奇安信入选全球《静态应用安全测试全景图》代表厂商](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247516678&idx=1&sn=5b9e480c386161b1e105f9818b2a5a3d&chksm=ea94b36cdde33a7a05cafa9918733669252a02611c222b02bc6e66cbb508ee3fbf748453ee7a&scene=21#wechat_redirect)  
  
  
[奇安信入选全球《软件成分分析全景图》代表厂商](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247515374&idx=1&sn=8b491039bc40f1e5d4e1b29d8c95f9e7&chksm=ea948d84dde30492f8a6c9953f69dbed1f483b6bc9b4480cab641fbc69459d46bab41cdc4859&scene=21#wechat_redirect)  
  
  
[热门开源Dompdf PHP 库中存在严重漏洞](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247515460&idx=2&sn=6ff90ed5a1a5cfe857a4aa75a16def08&chksm=ea948c2edde305386563b822262353daa67aecbbe719fdcbf7b97f402220ee247091ea7aeac0&scene=21#wechat_redirect)  
  
  
[Foxit 修复PDF阅读器中的多个代码执行漏洞](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247514639&idx=2&sn=1210ae31e627c0bc1cd7e277d8bf708b&chksm=ea948b65dde30273ce56eac981bddd7373782fe525d9d5d09c5e67e0eecdb9233b8c72344b20&scene=21#wechat_redirect)  
  
  
[安全机构未清理 PDF 文件，暴露敏感信息](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247502446&idx=3&sn=97ea655f92d879cf531d282679c5d206&chksm=ea94fb04dde372128c987ab2e7f305685dd817329fb0e76a5c0cf424da4f00b55689bd8dba34&scene=21#wechat_redirect)  
  
  
[Nitro PDF 数据遭大规模泄露，波及微软、谷歌、苹果等](http://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247496115&idx=2&sn=848d81302fe7613a3efa4c744c110676&chksm=ea94c0d9dde349cf8359c0107b8a772b037161b181af32d8139b5d63a24ee9c8e9ad430cf35f&scene=21#wechat_redirect)  
  
  
  
  
**原文链接**  
  
https://www.bleepingcomputer.com/news/security/critical-rce-found-in-popular-ghostscript-open-source-pdf-library/  
  
  
题图：Pexels License  
  
  
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
  
