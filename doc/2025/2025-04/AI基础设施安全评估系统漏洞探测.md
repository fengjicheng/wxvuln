#  AI基础设施安全评估系统|漏洞探测   
Tencent  渗透安全HackTwo   2025-04-24 16:00  
  
0x01 工具介绍 AI Infra Guard(AI Infrastructure Guard) 是一个高效、轻量、易用的AI基础设施安全评估工具，专为发现和检测AI系统潜在安全风险而设计。注意：现在只对常读和星标的公众号才展示大图推送，建议大家把渗透安全HackTwo"设为星标⭐️"否则可能就看不到了啦！  
**下载地址在末尾**  
  
0x02 功能简介  
## 🚀 快速预览  
## WEBUI命令行  
##   
## 🚀 项目亮点  
- **高效扫描**  
  
- 支持 28 种 AI 框架指纹识别  
  
- 涵盖 200+ 安全漏洞数据库  
  
- **易于使用**  
  
- 开箱即用，无复杂配置  
  
- 指纹、漏洞YAML规则定义  
  
- 灵活的匹配语法  
  
- **轻量级**  
  
- 核心组件简洁高效  
  
- 二进制体积小，资源占用低  
  
- 跨平台支持  
  
## 📊 AI组件覆盖情况  
组件名称漏洞数量anythingllm8langchain33Chuanhugpt0clickhouse22comfy_mtb1ComfyUI-Prompt-Preview1ComfyUI-Custom-Scripts1comfyui1dify11fastchat-webui0fastchat1feast0gradio42jupyterlab6jupyter-notebook1jupyter-server13kubeflow4kubepi5llamafactory1llmstudio0ollama7open-webui8pyload-ng18qanything2ragflow2ray4tensorboard0vllm4xinference0triton-inference-server7##   
## 0x03更新说明5672e24 update version to 0.1devcaa95c8 更新对version latest的处理e4b2a8e auto-update from HunYuan:2025-03-07 10:56:21 | Updated 2 vulnerability fingerprints29dcf15 update serverc5dd937 auto-update from HunYuan:2025-03-05 19:11:31 | Updated 2 vulnerability fingerprintsfe46d98 新增xinference指纹3a8b2e9 update readme_cnacd9d17 update readme0x04 使用介绍  
### 使用  
### WEBUI 可视化操作  
```
./ai-infra-guard -ws
```  
### 本地一键检测  
```
./ai-infra-guard -localscan
```  
## 单个目标  
```
./ai-infra-guard -target [IP/域名] 
```  
## 多个目标  
```
./ai-infra-guard -target [IP/域名] -target [IP/域名]
```  
## 从文件读取  
```
./ai-infra-guard -file target.txt
```  
## AI分析  
```
# hunyuan token
./ai-infra-guard -target [IP/Domain] -ai -hunyuan-token [Hunyuan token]
# deepseek token
./ai-infra-guard -target [IP/Domain] -ai -deepseek-token [Deepseek token]
```  
## 🔍 指纹匹配规则  
  
AI Infra Guard 基于WEB指纹识别组件，指纹规则在data/fingerprints目录中，漏洞匹配规则在data/vuln目录中。  
### 示例：Gradio 指纹规则  
```
info:
  name: gradio
  author: Security Team
  severity: info
  metadata:
    product: gradio
    vendor: gradio
http:
  - method: GET
    path: '/'
    matchers:
      - body="<script>window.gradio_config = {" || body="document.getElementsByTagName(\"gradio-app\");"
```  
  
🛠️ 指纹匹配语法  
```
匹配位置
标题（title）
正文（body）
请求头（header）
图标哈希（icon）

```  
  
逻辑运算符  
```
= 模糊匹配
== 全等
!= 不等
~= 正则匹配
&& 与
|| 或
() 括号分组
```  
##   
0x05 内部VIP星球介绍-V1.4（福利）        如果你想学习更多渗透测试技术/应急溯源/免杀/挖洞赚取漏洞赏金/红队打点欢迎加入我们内部星球可获得内部工具字典和享受内部资源和内部交流群，每1-2天更新1day/0day漏洞刷分上分(2025POC更新至3700+)，包含网上一些付费工具及BurpSuite自动化漏洞探测插件，AI代审工具等等。shadon\Quake\Fofa高级会员，CTFshow等各种账号会员共享。详情直接点击下方链接进入了解，觉得价格高的师傅可后台回复" 星球 "有优惠券名额有限先到先得！全网资源最新最丰富！（🤙截止目前已有1700多位师傅选择加入❗️早加入早享受）  
**👉****点击了解加入-->>内部VIP知识星球福利介绍V1.4版本-1day/0day漏洞库及内部资源更新**  
  
  
结尾  
  
# 免责声明  
  
  
# 获取方法  
  
  
**公众号回复20250425获取下载**  
  
# 最后必看  
  
  
      
文章中的案例或工具仅面向合法授权的企业安全建设行为，如您需要测试内容的可用性，请自行搭建靶机环境，勿用于非法行为。如  
用于其他用途，由使用者承担全部法律及连带责任，与作者和本公众号无关。  
本项目所有收录的poc均为漏洞的理论判断，不存在漏洞利用过程，不会对目标发起真实攻击和漏洞利用。文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用。  
如您在使用本工具或阅读文章的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。本工具或文章或来源于网络，若有侵权请联系作者删除，请在24小时内删除，请勿用于商业行为，自行查验是否具有后门，切勿相信软件内的广告！  
  
  
  
# 往期推荐  
  
  
**1. 内部VIP星球福利介绍V1.4星球介绍(0day推送)**  
  
**2. 最新BurpSuite2025.2.1专业版（新增AI模块）**  
  
**3. 最新Nessus2025.02.10版下载**  
  
**4. 最新xray1.9.11高级版下载Windows/Linux**  
  
**5. 最新HCL AppScan Standard 10.2.128273破解版下载**  
  
  
渗透安全HackTwo  
  
微信号：关注公众号获取  
  
后台回复星球加入：  
知识星球  
  
扫码关注 了解更多  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6qFFAxdkV2tgPPqL76yNTw38UJ9vr5QJQE48ff1I4Gichw7adAcHQx8ePBPmwvouAhs4ArJFVdKkw/640?wx_fmt=png "二维码")  
  
  
  
喜欢的朋友可以点赞转发支持一下  
  
