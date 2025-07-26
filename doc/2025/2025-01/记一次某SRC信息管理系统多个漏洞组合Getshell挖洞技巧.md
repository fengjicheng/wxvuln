#  记一次某SRC信息管理系统多个漏洞组合Getshell|挖洞技巧   
漏洞挖掘  渗透安全HackTwo   2025-01-12 16:00  
  
0x01 前言   
       记一次某SRC信息管理系统多个漏洞组合Getshell，并已提交至 SRC 平台修复。过程包括发现未授权访问漏洞、利用 SQL 注入获取邮箱账号、爆破弱口令登录后台、利用目录遍历漏洞找到上传路径，并通过条件竞争成功绕过黑名单上传 webshell 实现 getshell。  
  
**末尾可领取挖洞资料文件**  
  
0x02 漏洞发现首先通过子域名收集到招录信息管理系统，打开登录页面https://zlxt.xxx.xxx通过谷歌hacking语法搜索网站，成功利用谷歌发现搜索引擎爬取到的历年数据页面并且可以未授权访问https://zlxt.xxx.xxx/school/recuit/index/#/historical-data?key=rrxgTw%3D%3D&amp;school_id=5374访问页面尝试点击功能点抓包测试注入点POST /school/recuit/config HTTP/1.1Host: zlxt.xxx.xxxCookie: CNZZDATA1278742331=458302413-1660035428-https%253A%252F%252Fzlxt.xxx.xxx%252F%7C1660035428; gr_user_id=6c79cc7c-c8d8-4fa8-b251-e952713f19b8; email=37192588@qq.com; uid=undefined; schoolname=%u4E91%u5357%u8D22%u7ECF%u5927%u5B66; schoolId=5374; province=53; provinceName=%u4E91%u5357; year=2022; years=%5B%222022%22%2C%222021%22%2C%222020%22%5D; active_key=undefined; hide_type=undefined; h5Logo=; campus_id=0; PHPSESSID=shchmtd9um97dlh2b4hqes4j65; ci_session=l60a1vrs7u0q0bl2epb7alef0ll5j4qqUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:105.0) Gecko/20100101 Firefox/105.0Accept: */*Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2Accept-Encoding: gzip, deflateReferer: https://zlxt.xxx.xxx/school/recuit/index/Content-Type: application/jsonContent-Length: 103Origin: https://zlxt.xxx.xxxSec-Fetch-Dest: emptySec-Fetch-Mode: corsSec-Fetch-Site: same-originTe: trailersConnection: close{"key":"rrxgTw%3D%3D","school_id":"5374","year":"注入点*","type":"2"}通过观察发现原语句为SELECT `province` FROM school_students_2024' WHERE `school_id` = '5374' GROUP BY `province` ORDER BY `id` DESC注入点发生在表名这里school_students_2024'，那么尝试闭合，经过大量测试发现;可以闭合后面的语句，那么语句变为SELECT `province` FROM school_students_2024 WHERE `school_id` = '5374';WHERE `school_id` = '5374' GROUP BY `province` ORDER BY `id` DESC闭合成功以后，那么我们的payload就应该在”5374“之后，“；”之前，payload 为and 1=1返回数据正常，payload 为and 1=2返回数据不正常，通过这里可以发现此处可以用布尔盲注直接上sqlmap一把梭通过测试发现该处注入点无法获取os-shell，因为mysql数据库版本>=5.6,默认不允许导入导出数据，故放弃注入getshell，通过注入成功拿到数据库中的邮箱账号3719xxxx@qq.com和密码hash，但是由于存在salt值，所以无法解密出来，故放弃。前面观察到登录名为邮箱，此时我的思路为通过获取到的email去爆破登录框，从后台入手先输入账号密码和验证码抓包，通过测试发现验证码在有效期内可重复使用，再观察发现密码为数字密码两种组成，可以尝试爆破。使用burp爆破，注意密码需要使用md5规则，字典选择数字加密码的组合，成功跑出超级管理员的密码成功在验证码有效期跑出密码，hash值解密出来为弱口令1111111a 使用3719xxxx@qq.com和密码111111a成功登录系统，且权限为超级管理员在其他功能点上发下文件上传通过抓包测试，并且输入后缀名php1成功上传，发现上传为黑名单限制。但是发现未返回准确路径，需要先尝试fuzz路径，ctrl+u查看源码图片路径发现一处图片路径/app/static/imgs/el_box_img.png尝试访问发现/app路径存在目录遍历经过翻找，发现/app/upload/img/xxx/5374/为最终上传目录点fuzz绕过后缀，但是经过查找burp未发现成功上传返回200状态码的后缀经过大量测试发现文件名和内容都未做限制，突破点就在发包频率上面，由于是黑盒测试，这里感觉像条件竞争的漏洞，因为只要在上传内容设置一个变量，并且通过burp爆破就能成功上传php文件，这里尝试打个phpinfo看看，先设置burp为如下：成功打出phpinfo()，查看禁用函数几乎没做任何限制，那么把phpinfo()换成一句话尝试phpinfo url ：https://zlxt.xxx.xxx/app/upload/img/2024/5374/63513d946b2e55_1666268564_4966.php使用蚁剑连接，Getshell成功，但是发现shell不稳定，最后上了一个哥斯拉马0x03 总结        最后总结，挖洞还是得细心分析功能点，fuzz也很重要，最终成功获得网站控制权限。喜欢的师傅可以点赞转发支持一下谢谢！0x04 内部星球VIP介绍-V1.3（福利）        如果你想学习更多渗透挖洞技术/技巧欢迎加入我们内部星球可获得内部工具字典和享受内部资源，每周一至五周更新1day/0day漏洞刷分上分，包含网上一些付费工具BurpSuite漏洞检测插件等等，Fofa高级会员，Ctfshow各种账号会员共享，fuzz字典/最新SRC挖掘/红队/代审视频资源等等。详情直接点击下方链接进入了解，觉得价格高的师傅可后台回复" 星球 "有优惠券名额有限先到先得！后续资源会更丰富在加入还是低价！（价格随资源人数进行上涨早加入早享受）👉点击了解加入-->>内部VIP知识星球福利介绍V1.3版本-星球介绍-1day/0day漏洞推送-含POC  
  
结尾  
  
# 免责声明  
  
  
# 获取方法  
  
  
回复“**app**  
" 获取  app渗透和app抓包教程  
  
回复“**渗透字典**" 获取 一些字典已重新划分处理**（需要内部专属字典可加入星球获取，内部字典多年积累整理好用！持续整理中！）**  
  
  
回复“**书籍**" 获取 网络安全相关经典书籍电子版pdf  
  
# 最后必看  
  
  
      
文章中的案例或工具仅面向合法授权的企业安全建设行为，如您需要测试内容的可用性，请自行搭建靶机环境，勿用于非法行为。如  
用于其他用途，由使用者承担全部法律及连带责任，与作者和本公众号无关。  
本项目所有收录的poc均为漏洞的理论判断，不存在漏洞利用过程，不会对目标发起真实攻击和漏洞利用。文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用。  
如您在使用本工具或阅读文章的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。本工具或文章或来源于网络，若有侵权请联系作者删除，请在24小时内删除，请勿用于商业行为，自行查验是否具有后门，切勿相信软件内的广告！  
  
  
  
> **彩蛋🌟昨日内部星球新增0day/1day漏洞及资源:(已连续更新600天+星球涵盖全网90%资源)**  
  
  
```
勤云科技 abstract 存在SQL注入漏洞
安科瑞-智能环保云平台 uploadworld 存在任意文件上传漏洞
以柔资讯-D-Security终端文件保护系统 logFileName 任意文件读取漏洞
移动应用getPicServlet存在任意文件的读取漏洞
Aviatrix Controller存在命令注入漏洞(CVE-2024-50603)
爱数AnyShare SMTP_GetConfig存在信息泄露漏洞
Netis路由器漏洞
CS_CounterStrike1.6.1最新版
CS/Webshell等免杀工具分享
全网最全的SRC资产表更新
独眼情报书签情报完整版
SRC漏洞挖掘培训视频更新
```  
  
  
  
# 往期推荐  
  
  
**1. 内部VIP知识星球福利介绍V1.2版本-元旦优惠**  
  
**2. 最新BurpSuite2023.12.1专业版中英文版下载**  
  
[3. 最新Nessus2023下载Windows/Linux](http://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247484713&idx=1&sn=0fdab59445d9e0849843077365607b18&chksm=cf16a399f8612a8f6feb8362b1d946ea15ce4ff8a4a4cf0ce2c21f433185c622136b3c5725f3&scene=21#wechat_redirect)  
  
  
[4. 最新xray1.9.11高级版下载Windows/Linux](http://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247483882&idx=1&sn=e1bf597eb73ee7881ae132cc99ac0c8e&chksm=cf16a75af8612e4c73eda9f52218ccfc6de72725eb37aff59e181435de095b71e653b446c521&scene=21#wechat_redirect)  
  
  
[5. 最新HCL AppScan Standard 10.2.128273破解版下载](http://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247483850&idx=1&sn=8fad4ed1e05443dce28f6ee6d89ab920&chksm=cf16a77af8612e6c688c55f7a899fe123b0f71735eb15988321d0bd4d14363690c96537bc1fb&scene=21#wechat_redirect)  
  
  
  
###### 渗透安全HackTwo  
  
  
微信号：关注公众号获取  
  
后台回复星球加入：  
知识星球  
  
扫码关注 了解更多  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6qFFAxdkV2tgPPqL76yNTw38UJ9vr5QJQE48ff1I4Gichw7adAcHQx8ePBPmwvouAhs4ArJFVdKkw/640?wx_fmt=png "二维码")  
  
  
  
喜欢的朋友可以点赞转发支持一下  
  
  
