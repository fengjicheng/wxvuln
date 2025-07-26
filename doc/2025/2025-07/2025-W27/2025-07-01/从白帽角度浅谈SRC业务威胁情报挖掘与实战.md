> **原文链接**: https://mp.weixin.qq.com/s?__biz=Mzg2ODYxMzY3OQ==&mid=2247519511&idx=1&sn=37834d7e1f92b6d8120d045afe96e57c

#  从白帽角度浅谈SRC业务威胁情报挖掘与实战  
 Z2O安全攻防   2025-06-30 13:12  
  
#   
# 1 前⾔  
  
  
已是凌晨三点，想着写点什么，发现互联⽹上针对漏洞相关⽂章资源较多，⽽威胁情报相关内容却是少之⼜少。便创作此⽂。希望能够借助先知社区这个优秀的平台，和各位师傅⼀起分享我在情报 ⽅向的⼀些思考和个⼈⻅解。⽂笔不好，敬请斧正！  
  
# 2 威胁情报相关的规则  
  
  
我们这里以某厂商举例，可以发现对于这家厂商比较核心的内容有两个点，第一个是“  
业务  
”，第二个是“  
线索  
”。所以我们的挖掘方向一定是跟着业务走，此时需要思考两个方面的问题  
  
1. 如果我是企业的运营人员，我觉得这个情报是否重要？  
  
1. 如果我是企业的运营人员，这个漏洞会对我的公司产生多大的威胁？  
  
      弄清楚这两个问题，以自身带入企业人员相关角色，便可更好的去挖掘威胁情报。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYtG5Db2oLSmDFA9a9daicUoS69ZoribcibDwiajcibE21mzichTm2uiaDftuTQ/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYJpW7uHgOdibK1kbbT9NQg7ibehGRpjGclRFSJ1Eb9ZsA5Oz6oDZ2MGnA/640?wx_fmt=png&from=appmsg "")  
  
  
可以发现，不同的厂商对于核心的侧重点并不同。电商为营收核心的厂商更加关注   
用户信息、订单泄漏  
等方面的问题，其次是  
合规风险、内容安全  
等等。而以游戏为营收核心的厂商则更加关注   
游戏外挂、盗号木马、游戏代练工作室、低价黑市   
这些问题。首先第一步就是要搞清楚我们需要挖掘的SRC，他的核心侧重点到底在哪一方面，才能更好的去挖掘（捡钱）。  
  
  
下面，我会以一些简单的威胁情报案例，来给各位读者开阔思路  
  
# 3 案例一：OSS存储桶刷量问题  
  
## 3.1 按量计费与按时付费  
  
  
我们这里以某厂商的OSS举例，它的计费模式有两种，一种是你先用，用完之后我再给你算钱，还有一种是像视频会员一样。包月付费，付了一次钱，在这个有效期内再去使用。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYUr0OP8VJA7buzANYT3M4NfynnJKteLJ7D6kJn8nbjaUeFNoCe8ibNcQ/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
我们先来看第一种模式，按量计费  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHY89Kic1gYic6623dTBL2Xcxv8jxOtcGaSRtlKib7JSxsLRsSL0icvufPaPQ/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
## 3.2 按量计费相关问题   
  
  
     按量计费的相关模式是默认开通的，我们先进行使用，在一个结账期内，云产品官方会将账号下   
网络流量+存储流量   
按照固定的比例计算好后，推送至账单中。但是这样就存在一个问题，也就是OSS存储桶刷量问题。  
  
  
例如在我的账号存储桶下配备了两个视频资源文件，分别是  
  
  
演唱会.mp4和视频资源.mp4  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHY7ggvMMjuicp7xBPCrhFhVXInGNVBsTY6AElKaShC09Fzt7e3cMhowSA/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
当我们给存储桶配置的权限为 公有可读写时。则可以在浏览器中打开对应的视频资源进行播放，也可直接进行下载。而在对视频进行观看和下载的过程中所消耗的流量，都是要消耗资金的。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYoFEV0GRticEBkFOR7iaAvlmwuC6EbAqp7Ep3icKySYc5tFrQjwsiacp7Vw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
那么我们的思路就可以在消耗资源这个方向上去走，例如某业务引入了第三方的OSS资源桶，命名为target.oss.beijing.com 而在这个桶下有一个较大的资源文件。我们就可以利用某些工具来对资源文件进行解析，达到恶意消耗资金的目的。相关的报道网上也有很多。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYCTEVgsGdUDzZX6E5VVWDnuicKBzNlicJObyc3nN8koian0S94IY4T7GDw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
  
而在其中最火的莫过于这一篇  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYbY4wMEmdFyGvYV5bjrwQf4bvWCqp3q5SLWcRFLKVicpibr4jFeod8Qqw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
由此可见，刷流量费用给用户带来的资金损失是巨大的。而厂商这块也有非常好的解法。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYPiaUrAhsjqicTpIQKlZiaGtTF5f0wL5vSNjKJKUALD0kZwQJVhpSmYeRg/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
将Bucket ACL设置为私有属性和采取OSS高防，其中使用较多的是配置权限  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYibffa6dFxFOvF4PWa3UXvyhS1uUwlyiaknK1xF82iaO5wKpU3HQA1Xcmw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
当我们把Bucket ACL设置为私有属性时，原来的资源文件链接会变成：  
  
  
target.oss.com -> target.oss.com?sign=xxx&timep=xxx  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYSEc5Iv2t3GnkNyic0ol9vIpIxmiaOaB2wavTvXHX6zWk9gXADUgamxtQ/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
  
而在真实业务场景中，更多的会去动态生成sign，确保每个资源文件的有效期。  
  
## 实际案例  
  
  
相信随着刚才前提的引入，各位师傅已经对刷量相关的知识有了一个初步的认知。那么在真实情况下，我们肯定不能通过刷量-》消耗资源再去提交漏洞来验证危害。这样就已经对公司造成了资金损失。所以接下来更多的还是放在攻击面上。  
  
  
这里以某个盗版电影网站举例，首先我们都知道，一些盗版电影、电视片网站的视频资源，如果存放在本地，或者使用CDN或者OSS话，价格是十分昂贵的，而且买了存储包之后还要购买流量包，当用户每次去访问一个视频资源的时候，所消耗的流量费用是巨大的。并且由于盗版网站经常被打击，使用国内的云产品存在消费高、被溯源、不易管理的特点。  
  
  
所以从20年开始，国内的黑产视频网站开始盯上了互联网大厂的存储桶业务，黑产人员先去XX、XX等集团公司寻找任意文件上传漏洞，把mp4格式的盗版视频转换成m3u8，ts格式的视频，同时在前端使用dplayer播放器，这样所有使用的存储资源，和流量费用，都会从对应的大厂账单里走。平常一个月消耗可能就达几十万的流量费用。  
  
  
首先我们找到一个网站，target.com  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYs9e9bOZChA3D7LEiakloUSb0qyXIBkL5GsTSCiaI9PEHpOQEL7eFnib4w/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
可以看到前端采用了  
dplayer播放器，他的特点是扩展性高，且支持的格式如下  
  
  
流媒体格式：  
  
- HLS (HTTP Live Streaming)  
- FLV (Flash Video)  
- MPEG DASH  
- WebTorrent  
- MP4 (H.264)  
  
- WebM  
- Ogg (Theora Vorbis)  
在这里  
我们看到了一个很有意思的点，也就是这个盗版资源并不是从源站过来的，而是从某大厂旗下的一个存储桶过来的。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYlnF3oewSIvu1XwxSRQxk1za7NribxTvsCDaYmfXfqoKzKWesD3cMc9A/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
  
当我们打开这个存储桶时，发现他的首页写了一行，xx业务流媒体资源中心。  
  
  
于是乎猜测这里的地址肯定是黑产人员利用了文件上传漏洞，把原有的上传功能点，当做了免费的oss视频资源加速站，那么我们来看看接下来这个文件到底包含了什么。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYgQv1TpXPwYSxrYW9lUGrMbVXibIg1uOuicqRhQTdPaywlQicrM2LRawAg/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
使用010Editor打开文件后，首先我的疑惑就在于，这是一个  
PNG格式的文件头  
，是如何做到当成视频流进行解析的？  
  
  
  但是真实情况真的如此吗？  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYgXKH6s4b0FN5aqYvzrPVIbXK52ANNyGanuic9l810ibbNeRNjfLicykkw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
而在文件的下方多了一个MPEG_TS，这是一个标准的视频文件流  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYl17KNWwQicMGrfqHFWZ7CdyiaUeibzU6aeB9XTicCNy8DjYVdMXoYBymCQ/640?wx_fmt=png&from=appmsg "")  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYhHvjrMXicica6z4VAqrpRstpGVB49unqgPC3rWmEY5ichOlQCXZjsBJUw/640?wx_fmt=png&from=appmsg "")  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYENazP3ibr1A9ia6R9NARibCyBumTResgfCecPXCWKXKAMCEMqms0RojKA/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
  
  
所以攻击路径：黑产人员把盗版资源分块成多个ts格式文件——在ts格式文件前加入一张看似很普通的图片——把内容通过文件上传漏洞上传到此公司某个OSS存储桶上并获取返回值（文件存储地址）——通过在盗版网站中内嵌Dplayer播放器的形式，把文件流内容读入播放器（从MPEG—ts）位置处开始读取，从而达到免费流量的手法，这样做也可以绕过安全设备的检测。  
  
  
     这只是其中一个案例，发散一下思维。如果这个OSS已做了绿标处理。那么是否可以将黄色视频上传到存储桶下进行刷量？一些发帖处、如果对用户上传的链接地址做了强校验，导致SSRF或者CSRF漏洞无法产生，是否可以通过上传黄色视频造成合规问题（图一乐，真别这么想嗷）  
  
  
  资金问题 > 法律问题 > 合规问题  
  
# 4 案例二：网店运营那些事儿  
  
  
小明是一个黑心商家，在某平台上开了一家店，开店的时候需要缴纳1000元保证金，小明为了能够卖假鞋。一咬牙一狠心交了，然而没有一个月，店铺被查封，冻结了1000元保证金。此时他找到了《资深网络安全专家》，也就是屏幕前的你，请选择你的做法：  
  
  
A：逻辑漏洞YYDS，我看看有没有逻辑漏洞  
  
  
B：事不关己高高挂起，赚钱的时候不想着哥们，有事的时候想起来了  
  
  
C：看看有没有其他的办法能够把保证金拿出来  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYNFEmyQ5cicgko7Oox2iaDficL8oKzESFTY8PpuVxicvMiawvE7mJeT6xyDg/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
相信大多数的人都会选择A和C  
  
  
其实在某些平台都是有延迟不发货补偿的。假设被冻结了保证金，我们在商家大号下挂一件5000元的商品，小号去拍下。之后大号不发货，小号在三天后就会自动收到扣除了商家百分之30保证金的心意红包。  
  
  
请问这个算漏洞吗？不算，但是他有危害吗？有  
  
  
为什么有危害，以此类推，制假售假，且小号得到的红包是不是还可以在某鱼、二级市场继续以八折、九折的方式出售？  
  
  
黑心商家提现了原本冻结的保证金、身为朋友的你帮助了朋友、购买了红包的消费者省了钱，而遭到骂名的却只有平台  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYu97f3D79JP7XTSwUGc2hALYLX6IhQUQYmzUeGu8Pf47eh2f6AEKUicQ/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
二级市场  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYx0rhZNF8Bngw2WNgl86g4Eo3Upm2fCjREZ8vvyoq24Ff6dd8tuicBBQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
而这个案例还可以引申出来更多的案例  
  
  
我们已经知道不发货有红包，那么是否可以找一些别人缴纳过保证金，但是一直没有运营的店铺呢？恭喜你，很刑。但是已经被别人捷足先登了。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYey2pialtQMABCMOWSAAxeibNNsVialfCLKvFKFw7HYTKico1PLNckzPPibw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
  
由此可见，一些常见的电商类场景，我们并不能单单把目光放在逻辑漏洞或者Top10漏洞的挖掘上，更多的要做到   
比业务更懂它  
  
  
许许多多的普通规则，在经过不同的场景中时，就会引发不一样的效果。就像笔者最近在学的反序列化链一样，正是多个不同场景的精心构造，才最终引发了安全问题  
  
# 5 案例三：营销策略不合理导致的风险  
  
  
在企业运营中，通常会引入第三方公司的产品对顾客人群进行画像，而如果画像发放的权益不对等，也有可能造成安全风险。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYHe1oJPicDr11lGpLEXL0LTZO6ibTZdkwUG1mpeBdcTD7KCicdZFTx0gsA/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
  
为了提高商品的复购率，大多数企业会创建企业微信群、来发放一些优惠券、或者活动等  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYS0iblib6Faib5ARU1TkTDlNE5En3he7QqzvRtHIBXr0PgdrrfjG5RPLdA/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
而在私域运营的过程中，会把用户分为几大块。例如核心用户，这些用户往往能够第一时间参加活动，了解第一手信息等。但是在运营的过程中，其实并不好做严格的权限划分，如下面这个例子。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYGHFp3KhFTVBWoQrKfc9Fd9aAJafXpIcBtl6p27ia1dkDXGeKNicVztZw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
在某个私域内，运营人员给核心用户发放了一个问卷调查的链接，当打开链接核心用户填写完毕后，就可以获取一张免单券。但是由于没有对权限做合理的划分。导致该链接在某音、某红书上被泛滥，任何人进入到此页面填写完毕，都可以免费得到一张免单券。  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHY9nVmLGbmjauByQnbX9xApsykrkBBADjIHn0Xl5E7PFnJ70GhCfliaHw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
如果是会员类业务、那么还可以采取数据库回溯、权益回收的方式避免损失，而对于实物类产品，对企业的损失往往是巨大的。  
  
  
  
例如某些企业喜欢在某音、某手上做一些营销活动，当用户支付完成后，给用户所在的账号里发一张券。但是由于没办法试试监测用户在某音的订单状态，导致用户退款后券还在，此时就可以造成安全问题。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYg39wxbpyes6myjuyicL7xSQxBFyvnnaQ0nQaRmXIcclTdnSO4lcqQwA/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
也不单单是个例  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYIVaAJkoiaht36z89tAljyfZQP6BKC7DBvgGnLYtGbtoZJSmVFH4FRNw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
亦  
  
  
  
或是知名的某事件  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYWRuIPVbz3WdoE1ShQ7XjQ43cEyNnT2z1DuzUZBic2ViampQJxHIAREhA/640?wx_fmt=png&from=appmsg "")  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYIE3HXFPRoElso6sgLQjSrQLhrEvW5zYCszn7mvddbs2JJzGXOIUlxA/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
  
从大的角度来看，是因为营销策略不合理，导致出现的安全风险。  
  
# 6 案例四：从注销漏洞角度看风险  
  
  
在SRC挖掘中，相信各位白帽师傅在遇到“新人1元购”，“新人低价开会员”这样的时候，第一时间想到的都是支付漏洞，例如《万物皆可并发》、《当我拿出10台手机同时开会员，阁下又该如何应对？》，其实像一些企业的账号体系不完善，也是可以造成这种支付漏洞的。  
  
  
这里以一个真实的情报举例子  
![](https://mmbiz.qpic.cn/mmbiz_png/iar31WKQlTTowjr1oMc3nSEtM7FVuuwHYjgvc5VPuoegTfcqPicLPOjLjicXVia4EHlc3dGxHuciaicxfFPXtvzXyiaYw/640?wx_fmt=png&from=appmsg "")  
![]( "")  
  
  
  
在某SRC的核心APP上，由于没有对账号体系做校验，比如我今天新注册的手机号是A，得到了一个8元的优惠券，那么我就可以以2元的价格去购买一张10元的E卡。拿到账号卡密后，我再把这个账号注销掉。等明天再次注册。这样一个号一天就可以获得10元的E卡。  
  
  
这个看似是漏洞，其实在提交的时候可能会遇到这样的反馈。  
  
###### 审核：你认吗？   
  
###### 业务：不认，我看你是憋佬仔吧？  
  
######       审核：漏洞已拒绝，有风控限制  
  
###### 白帽子：日你MMP，写小作文去了，都别挖这家xxxx  
  
  
其实像这种时候有个小技巧，就是你可以不去提交漏洞，而去提交情报。然后潜入到黑产群内看他们的发言，比如他们如果多开账号、或者30天内都可以这么用，那说明风控体系肯定是完全没有作用的，这时候你再去自评严重，很大概率是不会降级的。如果不好找聊天记录，其实自己去测试一下也是完全可以的。  
  
  
注销漏洞从18年开始，我至少挖了有10个了，每个基本都是高危，所以我觉得情报和漏洞其实是一个相辅相成的东西，希望以后可以跟大家共同探讨这些业务安全情报风险和自动化的反欺诈报告。  
  
  
真实的案例更加新奇有趣，有空此系列会长期更新哦～  
  
# 原文首发在:先知社区  
  
https://xz.aliyun.com/news/18322  
  
  
建立了一个  
src专项圈子  
，内容包含**src漏洞知识库**  
、**src挖掘技巧**  
、**src视频教程**  
等，一起学习赚赏金技巧，以及专属微信群一起挖洞  
  
圈子专注于更新src相关：  
  

```
1、维护更新src专项漏洞知识库，包含原理、挖掘技巧、实战案例
2、分享src优质视频课程
3、分享src挖掘技巧tips
4、小群一起挖洞
```

  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuaRqDOYRFjU73rIsVy2ISg41LkR0ezBlmjJY4Lwgg8mr1A5efwqe0yGE9KTQwLPJTe9zyv3wgYnhA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuY813zmiaXibeTuHFXd8WtJAOXg868PqXyjsACp9LhuEeyfB2kTZVOt5Pz48txg7ueRUvDdeefTNKdg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/h8P1KUHOKuZDDDv3NsbJDuSicLzBbwVDCPFgbmiaJ4ibf4LRgafQDdYodOgakdpbU1H6XfFQCL81VTudGBv2WniaDA/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "null")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/h8P1KUHOKuYY8q8CZqffeicCspiaq3u0y7lQ2ia1ZLXbhqIbjvnBdsrlTFOJjSXbu8fmmliaLticl6P0iaXjbnxokM0g/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp "")  
  
图片  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuaRqDOYRFjU73rIsVy2ISg4Bd1oBmTkA5xlNwZM5fLghYeibMBttWrf57h8sU7xDyTe5udCNicuHo8w/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuYrUoo5XZpxN9Inq87ic71D6aUeMdaWrKXgYYia2On8nMA7bqWDySa8odAq1a0kkp3WFgf0Zp0Eut0A/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
图片  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuaRqDOYRFjU73rIsVy2ISg4KKlic4yiafWTpLdejicQe3MllEQc24ypeI3anaK7IjJDVyq1WVQN2yKBA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
图片  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuY813zmiaXibeTuHFXd8WtJAOHgjJxnq1ibibJgVUx3LwCjZj62vygx8w6rxia1icmIWiax2YlP6S6LmlmlQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
图片  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuY813zmiaXibeTuHFXd8WtJAOApVm8H605qOibxia5DqPHfbWD6lmcweDjGv4DLl45waD068ugw2Iv2vg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
图片  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuY813zmiaXibeTuHFXd8WtJAOwldaSATYOh1WQpk1qz15rLxehOAn4aK7tdbSyNEuHDZpIISCtl6Q8w/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
图片  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuaRqDOYRFjU73rIsVy2ISg4jFsKRMMNDKbsAZhscCiagnyJScMVmFUqMtae5omlLRdu095mywWszjQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
图片  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/h8P1KUHOKuaRqDOYRFjU73rIsVy2ISg4uGJ2SA5BhZ3UyibZvVmcP3sozQEOfVr0jftWpC3YkpDiaAicS1ib3EgXHA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
  
