#  工具集：TomcatScanPro【tomcat自动化漏洞扫描利用工具】   
wolven Chan  风铃Sec   2024-12-19 04:05  
  
### 简介  
  
本项目是一个针对  
**Tomcat**  
服务的弱口令检测。除了支持  
**CVE-2017-12615**  
漏洞的多种利用方式外，还集成了  
**CNVD-2020-10487**  
漏洞（Tomcat AJP 协议本地文件包含漏洞）的利用功能，帮助用户高效检测和利用漏洞获取服务器敏感信息。工具同时支持对多个 URL 进行并发检测，并通过动态线程池机制，优化资源利用率，提升检测效率。  
### 功能  
#### 1.CVE-2017-12615 漏洞检测  
  
工具支持三种利用方式：  
```
```  
- 成功上传后，工具会尝试访问并执行上传的 JSP 文件，判断是否能远程执行代码。  
  
- 对每种利用方式的结果分别记录成功或失败状态。  
  
#### 2. CNVD-2020-10487 (AJP 协议本地文件包含漏洞)  
- 工具利用 AJP 协议进行本地文件包含（LFI）攻击，默认读取 WEB-INF/web.xml 文件，但文件路径和判断条件可以通过配置文件灵活调整。  
  
- 支持对目标文件中的关键字（例如 "Welcome"）进行自定义判断，确定文件读取成功与否。  
  
- 检测到文件包含成功后，详细记录成功的 URL 和读取到的敏感文件路径。  
  
#### 3.弱口令检测  
- 支持通过用户名与密码组合进行弱口令暴力破解。  
  
- 若登录成功，工具会自动尝试上传 WebShell 文件，提供远程管理和代码执行能力。  
  
- 登录成功以及 WebShell 上传的结果都会详细记录在日志文件中。  
  
#### 4.后台部署 WAR 包  getshell  
- 在弱口令破解成功后，工具会尝试通过 Tomcat 管理后台上传  
WAR  
包，以获取远程代码执行权限。  
  
- 部署的  
WAR  
包会自动在服务器上解压并生成 JSP Shell 文件，访问该文件后便可以获取 Shell 权限。  
  
- 支持通过配置文件自定义  
Shell  
文件的内容。  
  
## 使用方法  
  
通过以下命令安装所需模块：  
```
```  
1. 准备包含URL、用户名和密码的文本文件（data文件夹中），分别命名为  
urls.txt  
、  
user.txt  
和  
passwd.txt  
。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qGTEdaLg0Hniaias23icx8XFlkQqIlTic9ibuaibEurbVvP7VGQDpILrd2uBia55aBYicWED2FFThgTIUfXcBhYzvEWNsQ/640?wx_fmt=png&from=appmsg "")  
  
  
1. urls.txt  
保存格式：  
https://127.0.0.1/  
或者  
https://127.0.0.1/manager/html  
脚本会自行判断检测  
  
1. 在  
config.yaml  
中配置文件路径和其他设置。  
  
1. 运行脚本，将会在  
success.txt  
文件中记录成功利用漏洞信息。  
  
```
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qGTEdaLg0Hniaias23icx8XFlkQqIlTic9ibuM905wfSibzhHnQOfcXeDdWRNMVfTQRnK00NfiatVem96mFvicuia3sFrMw/640?wx_fmt=png&from=appmsg "")  
## 注意事项  
- 使用此脚本时请遵守相关法律法规，不要进行未授权的操作。  
  
- 本脚本仅供教育和测试用途，不承担任何由不当使用造成的后果。  
  
项目地址：  
  
https://github.com/lizhianyuguangming/TomcatScanPro  
  
  
