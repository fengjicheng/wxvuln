#  FastJson全版本Docker漏洞环境   
lemono0  WIN哥学安全   2024-07-11 20:36  
  
## 免责声明：  
  
由于传播、利用本公众号所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号及作者不为此承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
# FastJsonParty  
  
### 介绍  
  
FastJson全版本Docker漏洞环境(涵盖1.2.47/1.2.68/1.2.80等版本)，主要包括JNDI注入、waf绕过、文件读写、原生反序列化、利用链探测绕过、不出网利用等。设定场景为黑盒利用，从黑盒的角度覆盖FastJson深入利用全过程，部分环境需要给到jar包反编译分析。  
  
从黑盒的角度覆盖FastJson深入利用  
  
Docker环境，开箱即用。  
  
### 环境启动：  
  
```
docker compose up -d
```  
  
若docker拉取环境缓慢，请尝试使用国内镜像  
  
https://www.runoob.com/docker/docker-mirror-acceleration.html  
  
  
环境启动后，访问对应ip的80端口：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/1mtwZURvGTlVXiaRwXjWE3bHtkGcpRD60Y8FAxBXDC3iaib7niaz7E7kCicB6rdX3aX9U3edibdzrwxBzBIwtUxGOT8w/640?wx_fmt=png&from=appmsg "")  
  
  
总结了一些关于FastJson全版本常用漏洞利用Poc,可搭配食用：Fastjson全版本检测及利用-Poc  
```
https://github.com/lemono0/FastJsonParty/blob/main/Fastjson%E5%85%A8%E7%89%88%E6%9C%AC%E6%A3%80%E6%B5%8B%E5%8F%8A%E5%88%A9%E7%94%A8-Poc.md
```  
  
环境使用后请销毁,否则可能会冲突：docker compose down  
  
整理一下靶场顺序：(根据利用特点分成三个大类)  
  
  
## FastJson 1.2.47  
  
  
1247-jndi  
  
1247-jndi-waf  
  
1247-waf-c3p0  
  
1245-jdk8u342  
## FastJson 1.2.68  
  
  
1268-readfile  
  
1268-jkd11-writefile  
  
1268-jdk8-writefile  
  
1268-writefile-jsp  
  
1268-writefile-no-network  
  
1268-jdbc  
  
1268写文件利用另外写了一篇，可搭配使用：FastJson1268写文件RCE探究  
## FastJson 1.2.80  
  
  
1280-groovy  
  
1283-serialize  
  
每个机器根目录下都藏有flag文件，去尝试获取吧！  
  
部分环境wp目前还未给出,打算过段时间放出，也欢迎提交你的wp和建议。  
  
And Enjoy FastJson Party!  
```
```  
  
##   
  
  
Tips:  
   
  
  
项目地址：  
  
```
https://github.com/lemono0/FastJsonParty
```  
  
‍  
‍  
HVV常态化招聘  
：  
投递到-->  
  
```
https://send2me.cn/s4blZRtp/RSm7tIGEY-eiTg
```  
  
  
考证咨询  
：  
全网最低最优惠报考NISP/CISP/CISSP/PTE/PTS/IRE/IRS  
等证书，后台回复“好友”加V私聊。  
  
  
往期推荐  
  
[记渗透某cms到代码审计拿权限](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247500369&idx=1&sn=3e2d9fba710d06e1550755a4e8cbea18&chksm=c0c863a5f7bfeab3fb0f7dabcfbe5be6c76fe16ee76f485628b2701ea8f6e149a59f1e5016a5&scene=21#wechat_redirect)  
  
  
[实战｜对一个随身WIFI设备的漏洞挖掘](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247500351&idx=1&sn=595b68a89697cc5f32af381a57aefc49&chksm=c0c863cbf7bfeadd7537f52fd90d1685533a5503fbb276fd5eb412bd20744fe172c4d9c4c1bb&scene=21#wechat_redirect)  
  
  
[速看、紧急、震惊……谁在制造网安圈“标题党”文章？——网安从业者和"紧急,震惊"的关系](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247500361&idx=1&sn=06ad9d55637c2bf187a62a4ba6413e99&chksm=c0c863bdf7bfeaabff0412662cdada65227b908e54f6fa504f93aa2df1dff7df122199034c32&scene=21#wechat_redirect)  
  
  
[一个安服仔的自白——我还会留下来吗？](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247500340&idx=1&sn=82a2640f2372699a469ed12e17bd8c5a&chksm=c0c863c0f7bfead6f6ae1081637b7ce097c0f0e61b76e5f201f83a8fedd2af1264a18f38b1d9&scene=21#wechat_redirect)  
  
  
[【2024HW招聘】最快当天面试，冲啊](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247500340&idx=2&sn=5062138a0f4c098ca6d3fe105b419700&chksm=c0c863c0f7bfead680dd577c45830e22161a742d50f1fc1855dc314660ced4788008ba5ff4ff&scene=21#wechat_redirect)  
  
  
[记一次梦游渗透从jmx到rce](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499984&idx=1&sn=00c302e1dccd5ce488d02d5c543dc6b1&chksm=c0c86124f7bfe8323351025e4e53410b411633b9d9abce30a1b8b4c54277eb86a017ca9010a5&scene=21#wechat_redirect)  
  
  
[记某高校渗透测试24-0506](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499949&idx=1&sn=0594a76a0d617ba4d759d00745fa9e82&chksm=c0c86159f7bfe84f26321989ebb8df832038a71acb0c296dbad4deb477d0a210953ee7e84e26&scene=21#wechat_redirect)  
  
  
[一键解密，网络安全神器现已问世！](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499937&idx=1&sn=0d3e8470745296f9662b20db1ae34ff5&chksm=c0c86155f7bfe843570e757b5e9e155af24cffc9e4b4200f1133990a82c1379ae3ecf4b031a1&scene=21#wechat_redirect)  
  
  
[一个IP Getshell续篇](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499918&idx=1&sn=5de7c447aaf6dc1a77988c2998e01de5&chksm=c0c8617af7bfe86c11ce2549f7b1bf851a09d61399d95392ea29d185154d86907b2b47bbc84d&scene=21#wechat_redirect)  
  
  
[实战|记一次女朋友学校漏洞挖掘](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499797&idx=1&sn=1d29cb3af3354861ebca4938c4655d46&chksm=c0c861e1f7bfe8f7a9dd4d13554174b2cf683d4cf8238a3d144695425ce5f223e822e070bab2&scene=21#wechat_redirect)  
  
  
[记一次对某学校APP渗透](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499788&idx=1&sn=32e40ad903289e36a826f43bbcbf0f44&chksm=c0c861f8f7bfe8eef5cc64f1072affe50f144f9fc199a1bcc604fd5570d89af03005f08c8631&scene=21#wechat_redirect)  
  
  
[红蓝对抗快速搭建基础设施平台](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499479&idx=1&sn=49ec494a4d031faab8bab164412c2cab&chksm=c0c85f23f7bfd6350db52c2128231a46ebaf65db80749d38f3749d3afc31d6d4287f7bb2feb5&scene=21#wechat_redirect)  
  
  
[【工具更新】最强渗透集成环境 V 5.0](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247499447&idx=1&sn=f21e9aeea886f9cb4d7310f878ecb5c6&chksm=c0c85f43f7bfd65543de11232050790765df79a6c980ca786ba8adbdc64a1b66a9fe2bacb1cb&scene=21#wechat_redirect)  
  
  
  
