#  CFS三层靶场复现   
 白帽子   2023-11-24 00:51  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/ofvnGicEPbfSWbzciazXPZD4HPXQFmKvLVI0CZzFarPUDeqxbQuCaIw70NQnBSibk30sfZOgTCEzrMNes9b3pet1U89fdYr2aVh/640?wx_fmt=svg "")  
  
  
  
  
**STATEMENT**  
  
**声明**  
  
由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，雷神众测及文章作者不为此承担任何责任。  
  
雷神众测拥有对此文章的修改和解释权。如欲转载或传播此文章，必须保证此文章的完整性，包括版权声明等全部内容。未经雷神众测允许，不得任意修改或者增减此文章内容，不得以任何方式将其用于商业目的。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/ofvnGicEPbfSWbzciazXPZD4HPXQFmKvLVI0CZzFarPUDeqxbQuCaIw70NQnBSibk30sfZOgTCEzrMNes9b3pet1U89fdYr2aVh/640?wx_fmt=svg "")  
  
  
  
  
复现靶场使用工具脚本  
  
1. Nmap  
  
1. 蚁剑  
  
1. 哥斯拉  
  
1. Viper  
  
1. Nps  
  
1. ThinkphpGui  
  
1. Fscan  
  
1. Msfconsole  
  
1. Sqlmap  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/ofvnGicEPbfSWbzciazXPZD4HPXQFmKvLVI0CZzFarPUDeqxbQuCaIw70NQnBSibk30sfZOgTCEzrMNes9b3pet1U89fdYr2aVh/640?wx_fmt=svg "")  
  
  
  
  
**操作步骤**  
  
**靶场搭建**  
  
1.网卡设置  
  
CentOS 7第一张网卡配置NAT模式，第二章网卡配置5网卡，仅主机模式，断开和本机的连接。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwagzNeIFhSf5uuBnbWREfbAzEia2UtN6OkSkJU0oonuYD5A8krhx1DpKQ/640?wx_fmt=png "")  
  
Ubuntu配置5,6网卡，都是仅主机模式，断开和本机的连接。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaeniadRKsLm4t4KXwlf8o3iaGDAt6Fb7Ob2wLn1IJx0GZ9w3jiacf2PELQ/640?wx_fmt=png "")  
  
Windos 7配置6网卡，仅主机模式，断开和本机的连接。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaogF1v0G6uib3Tz30u3vE2Y5brMicCL5n4AiaiaQl3JHxBN67luG5Tj29fQ/640?wx_fmt=png "")  
  
2.宝塔设置  
  
用kali查看ip：192.168.230.129  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwa2UEpdqbkh455gdQyS93JxVPlicLtQWpAib53IibleVcGeDW7wkUXMic5UQ/640?wx_fmt=png "")  
  
Nmap扫描192.168.230.0/24段的ip，看看能不能扫到centos7  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwahQN1gzZ3yf8RKxjdETu3WVTqoRuqAMYaW0zlsteXP7wdg9lmO4Hqpg/640?wx_fmt=png "")  
  
扫到了，192.168.230.128为centos7的ip地址，虚拟机的描述给了宝塔的登录地址，如下：http://192.168.230.128:8888/a768f109/  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaIftLib2B6LFND4p3SZ875gdK7bCibpqE7yLnJRjVV6r5jHEeHUDqkp1A/640?wx_fmt=png "")  
  
账号：eaj3yhsl密码：41bb8fee登录上去之后添加自己的ip，win7直接开机就可以  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwazVwnuicEafLLSaicEud72cXrCMMhZyZXlwUcEjaMzfrp8d7ics0ff4qtg/640?wx_fmt=png "")  
  
同样的方式，上unbuntu的宝塔，配unbuntu的ip:192.168.155.129  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaXkN1JKhLdZskZWcYNua9krwomz4C9yoRjg5rCjJTA7rOIAOxQpKBqQ/640?wx_fmt=png "")  
  
**渗透靶机**  
  
渗透CentOS7靶机  
  
Nmap针对性扫描192.168.230.128  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaLEmXlZ7dgicSuJ3fBdJ4b8R9M3oO1eIiaicUrwnmib3FXSX8CPB7HjeoaQ/640?wx_fmt=png "")  
  
开放了许多端口，http端口有80，888，8888，22ssh端口等，扫个目录：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaPcJjeMButKVIib193j3C0C7ck3cTaxiagiaic9DqSOc8rpicpCXD1dq25oQ/640?wx_fmt=png "")  
  
进robots.txt下，拿到第一个flag为:flag{QeaRqaw12fs}  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaSbDtBPCAaibBh4zPDt3gaVTwYqRNlM1A8dvQJNic9iaoOxdVDgIH9WLDg/640?wx_fmt=png "")  
  
上http端口看看：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwan1I1T07j88jb6NhHRbo9MKemKJJOt4EiaBo0RJibicUKiclGA52N6Lvh4w/640?wx_fmt=png "")  
  
Thinkphp5框架，直接上tp5通用漏洞利用脚本扫  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaPuFkM4XgVhwK068r2YoZNSTURX1I0tia9n0XgsvKasFicNMJq8Hwc7Fg/640?wx_fmt=png "")  
  
存在RCE漏洞，直接getshell开蚁剑连接  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwawoKZfaicGtT6FicKeH4HwrDXuWrsnAEiakp1zDRibjPPkh9J2ibYnaoOBgw/640?wx_fmt=png "")  
  
连接成功。  
  
上传木马，上线viper生成监听载荷  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaEZAuRHtSEd2TgLrMFibyzFiav3HdNnwCllZ5TM2TX2dUDhuYgyfDAbXA/640?wx_fmt=png "")  
  
生成elf可执行木马，通过蚁剑上传到/tmp目录下，赋予执行权限：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaYAec2eqNfDaTUgvkhjlGlb2ZtXzaLPMiaowOVMRk4gRG9V8qILUKib3w/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaVMrmp6xOzGArHvriaeNfmAbshQLHquzyXlsdibEW9AbQfP7WFqroDglg/640?wx_fmt=png "")  
  
运行，上线viper，成功  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaeleYqovbaBwu4LYAdEjYuMK3Ze6PuOnhvvhFn1NqfVtqwa8CUqqHaQ/640?wx_fmt=png "")  
  
查看ip，发现还有另一个网卡：192.168.155.128  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaUbIJiaMloCuotjWOmRbVzguXZAjMgZOhajbxpFMzhSkDvczl4libAPLA/640?wx_fmt=png "")  
  
添加内网路由  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwas7p9RemH30Ey7Gf5sibRPYDOF6yoKokjRUicaxYFnibkKz8kd1k2ajicjA/640?wx_fmt=png "")  
  
****  
**渗透Ubuntu靶机**  
  
上传fscan到Centos7靶机上进行扫描：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwavDMpFF7EenPURg40nCdwttiaEk5s5icCtNrQE65p5Or9WRuialsla6ALQ/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwafMAj9nZZcdKmgF94o0KlxGJCBYwDxwz2w1icJmSbE6r5o5zelSq8vRQ/640?wx_fmt=png "")  
  
多出来了一个192.168.155.128的ip应该就是unbuntu的ip，用nps设置内网穿透代理：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaEhcewWsicY9DvojdIDOVVSXseQn6dhGVj8EXcicSCskDPlqJ9y7BsHnw/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaFZFB6Sxy8dsV1s6ibicgKicCicZJeWuq8efEfqoXY1CBNoXeYpfyp3JUyQ/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaUlFFOXicGn4tp4Ea525x7wSJbUJN0ILZo8ibmhursfVriaiaFTPRdrGE7w/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaHfeoxdpdET0sHB52qiclj49iaLzaK9VjRzJyRgw4keFyW0dZ8MBSWAMQ/640?wx_fmt=png "")  
  
访问192.168.155.129，是八哥CMS，看看robots.txt有没有提示：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaetbATFb6TIC9TUFtorNggqmeRYdxiciaUQMVFsjvEtgTHEza8g5fUJsA/640?wx_fmt=png "")  
  
给了登录界面回到首页，右键查看源码  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaFWXja4B8cg9wIsEvicIZGer6CcA4vTiaFfGAbuDNdOcsia7zoRUNMSqYw/640?wx_fmt=png "")  
  
发现sql注入提示  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwaff7UZ4le1ffaeibsibwL4icr60nvxic6q8fmTRCdUtMYxl39Enqg6PUmeA/640?wx_fmt=png "")  
  
验证一下，确实存在sql注入  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwaa2QfFfEUtSfMvnP2tbD4ofrSEGaF3xgaiaNze69Z3OQyTEX8Z9F6ujg/640?wx_fmt=png "")  
  
搞到sqlmap跑一下，记得挂上nps代理  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaGpl0QSfgFGNDNdBMuNmDzfyA4YUpsWUsyuicSibKP5sA3nMHtpMNM2jA/640?wx_fmt=png "")  
  
sqlmap -u "http://192.168.155.129/index.php?r=vul&keyword=1" -D bagecms -T bage_admin -C username,password –dump --proxy "http://43.138.159.166:48888"跑出来管理员的账号密码：admin/123qwe  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwacbbFDZodhstwa2vXcvD4DHuDt8gopyeD8MjvgJd8THzURFShyChXzA/640?wx_fmt=png "")  
  
尝试登录  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwa1UFadvqKvS6SU584qA8r474XTLmb7t5qst94EzJnib2iarTPY8AgdOTQ/640?wx_fmt=png "")  
  
登录成功，拿到第二个flag为:flag{eS3sd1IKarw}  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaGlywsln7HO63j3k25M4XPbB090nmOrGjBGFreuKFx4gqzolHgcaxLw/640?wx_fmt=png "")  
  
模板管理处可以编辑php文件，测试是否可以上传木马  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaV80TVSUtMwv9cWLg9nhoT2LiaX35O3t7S8Sn0A8gdBewENVcvzdm2YA/640?wx_fmt=png "")  
  
测试一下，可以解析phpinfo()  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaJxstZnfwaaWjko3wFcanNCrAKITnAcrJ5P4S19EQoXxuvzpyYfM2WA/640?wx_fmt=png "")  
  
可行，写入一句话木马  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwabrDrwrcup1XVemSh2FWe3tbmdKe9ib5f46Jowyf0Bee4mRMY1fj1fHQ/640?wx_fmt=png "")  
  
哥斯拉连接，连接成功  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwakyRFY78TptFuXSQAsaAm6cKaHPyCbK23bNKjI8RYBoD8983Hf9OGdg/640?wx_fmt=png "")  
  
拿到第三个flag为：flag{23ASfqwr4t2e}  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwa2qcZnxvfx2ibdrOhaZ85815mgzefP0aCmDY2qRVdXicY0zeAgWqjolRQ/640?wx_fmt=png "")  
  
上传木马，上线viper上线成功  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaD8pjKHD45tWiabKWEliaHlf4ia7KlKXE2dSekDjjIUbuQgVlLKCnTkeLg/640?wx_fmt=png "")  
  
添加内网路由  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaZ1hOT2Yhiaz4fhuOHICibD06CmhSJGdcdJ6SmkbmCI62ZAGWY6slKicFQ/640?wx_fmt=png "")  
  
****  
**渗透window7靶机**  
  
上传fscanarm64检测ip 检测到window7 ip为192.168.166.130存在445端口  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaibqbicIS6Sl5MsEQO8eB9EAIUXmD0yp0fG6sP6zAiapxHhxulCw3ibN1icQ/640?wx_fmt=png "")  
  
验证一下永恒之蓝漏洞：直接用msf里面的ms17010 exp进行攻击  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaTBb9kcOn6Coe3WtbAzibdKCvd3bFalQN5aRibeZCAFZ6rBQ5PuRWX67w/640?wx_fmt=png "")  
  
上线成功：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwa98o0MianPn1gGzrke4gTrQOyxTpoOIbvKKmQ7OZFcHoR1DG3e8fDs7w/640?wx_fmt=png "")  
  
查看到第四个flag为flag{2wAdK32lsd}  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaPeVDI8Mq9FXlA8JoJzeC43cZzr4e0MAzgQlWibtKJCwPxuTZYM2ZkicA/640?wx_fmt=png "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/ofvnGicEPbfSWbzciazXPZD4HPXQFmKvLVI0CZzFarPUDeqxbQuCaIw70NQnBSibk30sfZOgTCEzrMNes9b3pet1U89fdYr2aVh/640?wx_fmt=svg "")  
  
  
  
  
靶场总结  
  
1.通过对thinkphp5 RCE漏洞利用拿下第一台靶机；  
  
2.通过sql注入漏洞拿下第二台靶机web界面管理员权限，再通过后台文件上传漏洞拿下第二台靶机；  
  
3.通过永恒之蓝ms17-010漏洞利用拿下第三台靶机；至此，CFS三层靶机已经成功拿下。  
  
  
**安恒信息**  
  
  
✦  
  
杭州亚运会网络安全服务官方合作伙伴  
  
成都大运会网络信息安全类官方赞助商  
  
武汉军运会、北京一带一路峰会  
  
青岛上合峰会、上海进博会  
  
厦门金砖峰会、G20杭州峰会  
  
支撑单位北京奥运会等近百场国家级  
  
重大活动网络安保支撑单位  
  
![](https://mmbiz.qpic.cn/mmbiz_png/HxO8NorP4JXcD09rvv95QjT6Huq4J3ibXvTMgHj1ZVFjcU6KrdiaH8tE833PYqzXWwU7LJPxLOH1GXaT7tSriaw8Q/640?wx_fmt=png "")  
  
  
  
END  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/HxO8NorP4JVpCN0I7y7uVVGpwg1T9LwaqGo46bw6b0SttTKKpB8NvGic8jXsPougAtbiaogsCzJqhKejTloVjibTg/640?wx_fmt=gif "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwa8QGUTE3bRibDDocicX1oHNI4tHNJHXjqwfbQ8CO0jS9geYFiagEZG8tEQ/640?wx_fmt=jpeg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/HxO8NorP4JVpCN0I7y7uVVGpwg1T9Lwam82NKVLIZYoGgoelwbSicma7aeQSYAkjoR3cTEVb2xp0npS0ZXNAxGw/640?wx_fmt=gif "")  
  
**长按识别二维码关注我们**  
  
  
  
