#  SSP-高效 Spring 框架漏洞扫描与利用工具   
原创 sspsec  SSP安全研究   2024-12-02 07:35  
  
# SSP - Spring框架漏洞扫描工具  
## 🚀 简介  
  
在日常渗透工作中，遇到很多Spring框架搭建的服务，很多都是Whitelabel Error Page页面，对于Spring的扫描工具github中也有很多项目 python的 java的，但使用go的就很少。因为自身电脑的缘故 使用go编译的工具 加入环境变量中 能让我很方便的使用命令行来使用这些命令行工具，做到打开命令行就能进行 "随手一测"。而像python，java写的类似图形化的工具，我需要进入到相关目录中运行或者双击才能进行使用，对于我这种非常不喜欢找文件的人来说，使用go编译的可执行文件加入环境变量中使用，对我来说是极大的方便。  
  
因此，我决定开发SSP工具，用Go语言实现一个轻量、高效的Spring漏洞扫描和利用工具，同时也练习自己的Go编程能力。  
  
SSP工具支持探测和利用多个Spring框架版本的漏洞，帮助用户快速发现Spring服务中的安全隐患。  
## 🛠️ 功能特性  
- **单URL和批量URL扫描**  
：支持对单个URL及批量URL进行信息泄露和漏洞探测，方便快速发现潜在安全问题。  
- **多漏洞支持**  
：涵盖多个Spring框架漏洞，包括RCE、信息泄露、文件上传、反序列化漏洞等。  
- **代理支持**  
：支持通过HTTP代理进行扫描，满足受限网络环境的需求，支持带有用户名和密码的代理格式。  
- **请求延迟功能**  
：用户可以设置请求之间的延迟时间（单位：秒），避免对目标系统造成过大压力。  
- **彩色输出**  
：在终端中显示彩色文本，使扫描过程更加直观，增强可读性。  
- **动态横幅**  
：启动时显示炫酷的彩色横幅，包含工具名称、作者信息及GitHub链接，提升用户体验。  
## 🔥 支持的漏洞  
  
本工具支持探测和利用以下Spring框架常见漏洞：  
- **2023 JeeSpringCloud 任意文件上传漏洞**  
- **CVE-2022-22947**  
 Spring Cloud Gateway SpEL RCE漏洞  
- **CVE-2022-22963**  
 Spring Cloud Function SpEL RCE漏洞  
- **CVE-2022-22965**  
 Spring Core RCE漏洞  
- **CVE-2021-21234**  
 任意文件读取漏洞  
- **SnakeYAML RCE漏洞**  
- **Eureka XStream反序列化漏洞**  
- **Jolokia配置不当导致RCE漏洞**  
- **CVE-2018-1273**  
 Spring Data Commons RCE漏洞  
## 📜 使用方法  
### 🔍 执行单个URL信息泄露扫描  
```
ssp -u http://example.com
```  
  
通过此命令，您可以快速扫描单个URL，获取Spring框架信息泄露端点。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Oum0kexPoVr0uXZl0icNHHTqs3YzbSI78dRCyX7oXyLFLDBAZRPH1UOGTqq3Uz1BOXRf5cRocvSVUOqJeMhhF5g/640?wx_fmt=png&from=appmsg "")  
### 📄 批量URL信息泄露扫描  
```
ssp -uf urls.txt
```  
  
通过此命令，您可以扫描文件中包含的多个URL，方便批量处理。  
### 🧪 单个目标漏洞扫描  
```
ssp -v http://example.com
```  
  
工具将自动探测该URL是否存在漏洞，并进入漏洞利用模块（如果发现漏洞）。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Oum0kexPoVr0uXZl0icNHHTqs3YzbSI78oIlu8rNVqiaCicIASAR57CRKia1YMcBQBjEM6ax19Zj1XdODrY1hrnW6A/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Oum0kexPoVr0uXZl0icNHHTqs3YzbSI780pwmkloc51Ia0jDpcmVTR0jiaTkyMCZwPTIqOc6WdJfXJTR9cjxgsJQ/640?wx_fmt=png&from=appmsg "")  
### 🧰 批量目标漏洞扫描  
```
ssp -vf urls.txt
```  
  
通过此命令，可以批量扫描多个目标URL，进行漏洞探测和利用。  
### 🕹️ 使用代理进行扫描  
```
ssp -u http://example.com -p http://127.0.0.1:8080
```  
  
在受限的网络环境中，您可以通过HTTP代理来执行扫描。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Oum0kexPoVr0uXZl0icNHHTqs3YzbSI78Trq0qzCrmzicmt7BmSLFFr8UgicEJ1mmYkia5K7ZlN8y0icpEicYLZiaILnQ/640?wx_fmt=png&from=appmsg "")  
### ⏱️ 设置请求延迟  
```
ssp -u http://example.com -delay 1
```  
  
为了防止对目标服务器造成压力，您可以设置请求之间的延迟时间（单位：秒）。  
## 🧑‍💻 命令行参数  
<table><thead><tr style="box-sizing: border-box;margin-top: 0px;"><th valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;font-style: normal;font-weight: bold;text-align: left;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">参数</span></section></th><th valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;font-style: normal;font-weight: bold;text-align: left;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">说明</span></section></th></tr></thead><tbody><tr style="box-sizing: border-box;margin-top: 0px;"><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><code style="box-sizing: border-box;margin: 0px;padding: 0px;font-style: normal;font-weight: 400;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;background: rgb(243, 241, 241);border: 0px !important;color: rgb(88, 88, 88);font-size: 16px;line-height: 18px;"><span style="box-sizing: border-box;color: rgb(88, 88, 88);margin-top: 0px;background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">-</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">u</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">&lt;url&gt;</span></span></code></td><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">对单个URL进行信息泄露扫描</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(248, 248, 248);"><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><code style="box-sizing: border-box;margin: 0px;padding: 0px;font-style: normal;font-weight: 400;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;background: rgb(243, 241, 241);border: 0px !important;color: rgb(88, 88, 88);font-size: 16px;line-height: 18px;"><span style="box-sizing: border-box;color: rgb(88, 88, 88);margin-top: 0px;background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">-</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">uf</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">&lt;file&gt;</span></span></code></td><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">批量扫描文件中的多个URL进行信息泄露检测</span></section></td></tr><tr style="box-sizing: border-box;"><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><code style="box-sizing: border-box;margin: 0px;padding: 0px;font-style: normal;font-weight: 400;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;background: rgb(243, 241, 241);border: 0px !important;color: rgb(88, 88, 88);font-size: 16px;line-height: 18px;"><span style="box-sizing: border-box;color: rgb(88, 88, 88);margin-top: 0px;background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">-</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">v</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">&lt;url&gt;</span></span></code></td><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">对单个URL进行漏洞扫描</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(248, 248, 248);"><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><code style="box-sizing: border-box;margin: 0px;padding: 0px;font-style: normal;font-weight: 400;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;background: rgb(243, 241, 241);border: 0px !important;color: rgb(88, 88, 88);font-size: 16px;line-height: 18px;"><span style="box-sizing: border-box;color: rgb(88, 88, 88);margin-top: 0px;background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">-</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">vf</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">&lt;file&gt;</span></span></code></td><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">批量扫描文件中的多个URL进行漏洞扫描和利用</span></section></td></tr><tr style="box-sizing: border-box;"><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><code style="box-sizing: border-box;margin: 0px;padding: 0px;font-style: normal;font-weight: 400;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;background: rgb(243, 241, 241);border: 0px !important;color: rgb(88, 88, 88);font-size: 16px;line-height: 18px;"><span style="box-sizing: border-box;color: rgb(88, 88, 88);margin-top: 0px;background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">-</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">p</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">&lt;proxy&gt;</span></span></code></td><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">使用HTTP代理，格式为 </span><code style="box-sizing: border-box;margin: 0px;padding: 0px;font-style: normal;font-weight: 400;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;background: rgb(243, 241, 241);border: 0px !important;color: rgb(88, 88, 88);font-size: 16px;line-height: 18px;"><span style="box-sizing: border-box;color: rgb(88, 88, 88);margin-top: 0px;background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">http</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">:</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">//username:password@host:port</span></span></code></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(248, 248, 248);"><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><code style="box-sizing: border-box;margin: 0px;padding: 0px;font-style: normal;font-weight: 400;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;background: rgb(243, 241, 241);border: 0px !important;color: rgb(88, 88, 88);font-size: 16px;line-height: 18px;"><span style="box-sizing: border-box;color: rgb(88, 88, 88);margin-top: 0px;background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">-</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">delay</span></span><span style="box-sizing: border-box;color: rgb(88, 88, 88);background: rgb(243, 241, 241);display: inline-block;padding: 0px 2px;font-size: 14px;font-family: consolas, menlo, courier, &#34;monospace&#34;, &#34;Microsoft Yahei&#34; !important;"><span leaf="">&lt;s&gt;</span></span></code></td><td valign="top" style="box-sizing: border-box;margin: 0px;padding: 0.5rem 1rem;border: 1px solid rgb(233, 235, 236);"><section><span leaf="">设置请求之间的延迟时间，单位为秒</span></section></td></tr></tbody></table>## ⚠️ 注意事项  
- 使用本工具进行漏洞扫描时，请务必遵守法律法规，仅在授权的范围内进行操作。  
- 任何未经授权的系统扫描和利用行为可能触犯法律，用户应自行承担责任。  
- 扫描过程中，请保持网络环境安全，避免对生产环境造成影响。  
## 📢 免责声明  
  
本工具仅供技术研究和教育使用，请勿用于非法活动。若使用者因此产生任何法律责任，作者概不负责。  
## ⭐ 最后  
  
如果您觉得本工具有用，请为作者主页点个⭐，并关注公众号：**SSP安全研究**  
。  
  
如果有任何问题或者希望添加新功能，请提交工单给我！我们会尽力改进工具，提供更多功能和支持！  
  
