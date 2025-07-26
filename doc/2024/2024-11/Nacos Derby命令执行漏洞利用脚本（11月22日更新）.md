#  Nacos Derby命令执行漏洞利用脚本（11月22日更新）   
XiaomingX  网络安全者   2024-11-22 16:01  
  
===================================  
免责声明请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任。工具来自网络，安全性自测，如有侵权请联系删除。  
0x01 工具介绍  
Nacos Derby命令执行漏洞利用脚本，默认使用User-Agent绕过漏洞进行利用。  
Nacos 是一款用于服务注册、发现和配置管理的开源平台。在其早期版本（≤2.4.0-BETA）中，Nacos 默认集成了 Apache Derby 作为嵌入式数据库。由于 Derby 数据库默认未设置访问控制，攻击者可以未经授权地访问特定接口，执行任意 SQL 语句，最终导致远程命令执行漏洞。  
0x02 安装与使用  
  
漏洞原理：  
1. 未授权访问： 在默认配置下，Nacos 的 Derby 数据库接口未设置访问控制，允许任何人访问。  
  
1. SQL 注入： 攻击者通过特定的 HTTP 请求，向 Nacos 的 Derby 接口发送恶意 SQL 查询。例如，访问以下 URL：  
```
http://<Nacos服务器地址>:8848/nacos/v1/cs/ops/derby?sql=select * from users

```  
  
  
这将返回用户表中的所有数据。  
  
1. 远程命令执行： 更为严重的是，攻击者可以利用 Derby 的特定功能，通过 SQL 语句执行任意代码。例如，使用 CALL 语句加载远程的恶意 JAR 包，并执行其中的代码，从而在受害者服务器上执行任意命令。  
  
修复措施：  
- 升级 Nacos： 官方已在最新版本中修复了此漏洞，建议将 Nacos 升级至最新版本。  
  
- 禁用 Derby 接口： 如果不使用嵌入式数据库，建议禁用 Derby 接口，防止未经授权的访问。  
  
- 启用身份验证： 配置 Nacos 的身份验证机制，确保只有授权用户才能访问相关接口。  
  
此漏洞的利用可能导致数据泄露、服务中断，甚至服务器被完全控制，危害极大。因此，强烈建议受影响的用户尽快采取上述措施进行防护。  
## POC/EXP - 使用方法  
```
nacos_derby_rce.py [-h] -u URL [-a TOKEN]
```  
  
****  
**0x03 下载链接**  
  
1、通过阅读原文，到项目地址下载  
  
2、  
网盘下载  
链接:  
https://pan.quark.cn/s/277a5414fe16  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0JJXjA8siccy4GnbA3TEdicu93dQibFIT2oAl5QXvLicnqzM8SusKyhmOFe3CQKapyl4u4eNknxibv3dES3icqoLdjKw/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
  
**·****今 日 推 荐**  
**·**  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0JJXjA8siccy4GnbA3TEdicu93dQibFIT2oobgOibTw1uhLCpBqFwCKZlsfGogKXxelGIgvwicOgdxz0KDoFf4RDM8Q/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
  
