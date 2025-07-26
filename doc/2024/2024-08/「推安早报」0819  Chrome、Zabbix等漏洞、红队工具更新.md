#  「推安早报」0819 | Chrome、Zabbix等漏洞、红队工具更新   
bggsec  甲方安全建设   2024-08-19 17:32  
  

	#  2024-08-19 「红蓝热点」每天快人一步  

	>   
>   
> 
			1. 推送「新、热、赞」，帮部分人阅读提效
			2. 学有精读浅读深读，艺有会熟精绝化，觉知此事重躬行。推送只在浅读预览
			3. 机读为主，人工辅助，每日数万网站，10w推特速读
			4. 推送可能大众或小众，不代表本人偏好或认可
			5. 因渲染和外链原因，公众号甲方安全建设发送日报或日期,如20240819获取图文评论版pdf
		  
>   
  
### 目录  
>   
> 0x01 【2024-0813】Zabbix监控解决方案发现关键RCE漏洞0x02 【2024-0813】Google Chrome 远程代码执行漏洞0x03 【2024-0814】Chrome渲染器中的对象转换至远程代码执行漏洞分析0x04 【2024-0814】远程禁用Windows事件日志的0day漏洞及免费微补丁0x05 【2024-0814】揭秘#GrimResource：滥用MSC文件格式0x06 【2024-0814】NTLM Relay攻击：网络入侵的终极手段0x07 【2024-0815】Android Jetpack导航库深层次安全漏洞0x08 【2024-0815】Lil Pwny 3.2.0更新：优化Active Directory密码审计0x09 【2024-0816】利用UDL文件进行网络钓鱼攻击的新技术0x0a 【2024-0816】Copy2Pwn漏洞绕过Windows网络保护0x0b 【2024-0816】结合历史泄露与CSS的水坑攻击0x0c 【2024-0816】GitHub项目ggerganov/llama.cpp中的rpc_server::get_tensor功能存在任意地址读取漏洞0x0d 【2024-0818】恶意签名注入：利用Evil Signatures远程删除数据库和邮箱0x0e 【2024-0818】Python脚本模拟IPv6数据包处理漏洞0x0f 【2024-0819】BounceBack：红队操作安全的隐蔽重定向工具0x10 【2024-0819】SpoofDPI：一款快速绕过深度包检测的软件0x11 【2024-0819】CVE-2024-7646：Ingress-NGINX注解验证绕过漏洞深度解析0x12 【2024-0819】Linux内核修复Landlock安全漏洞0x13 【2024-0819】Windows Secure Channel RCE漏洞CVE-2024-38148详解  
  

	###  0x01 Zabbix监控解决方案发现关键RCE漏洞  

	>   
> Zabbix监控解决方案发现了一个关键的远程代码执行（RCE）漏洞（CVE-2024-22116），该漏洞评分为CVSS 9.9，可能导致系统完全妥协。
		  
>   
  



	###  热评   
- 「编者注」:后台漏洞，低权限用户提权场景可用. 高权限用户可以直接写脚本执行命令的.  
  
- Zabbix 监控系统发现严重 RCE 漏洞 CVE-2024-22116  
  

	  

		
	  

	###  关键信息点   
- Zabbix的广泛采用和灵活性增加了安全风险：作为一个流行的监控工具，Zabbix的广泛应用和能够监控各种IT资源的能力，虽然提供了高度的灵活性，但同时也增加了安全风险。  
  
- CVE-2024-22116漏洞的严重性：该漏洞被评为CVSS 9.9分，表明其对组织的潜在影响极大。如果不加修补，可能导致系统完全妥协。  
  
- 受影响的Zabbix版本和修复版本：受影响的版本包括6.4.0至6.4.15和7.0.0alpha1至7.0.0rc2，已在6.4.16rc1和7.0.0rc3版本中修复。  
  
- 升级至最新版本的紧急性：管理员应立即升级到修复版本以防止漏洞被利用。  
  

	
	  
🏷️: 漏洞, Zabbix, RCE, 网络安全  
###  0x02 Google Chrome 远程代码执行漏洞  

	>   
> Google Chrome 存在一个远程代码执行（RCE）漏洞，由于 WASM 类型不一致和 JS-to-WASM 转换过程中的类型混淆，可能导致任意 WASM 类型之间的类型混淆。
		  
>   
  



	###  热评   
- 「编者注」:chrome 123版本前的rce, 带poc  
  
- Chrome 浏览器出现 RCE 漏洞，可导致任意代码执行  
  

	  

		
	  

	###  关键信息点   
- WASM 类型规范化机制存在设计缺陷，导致在不同模块间的类型比较可能出现类型混淆。  
  
- JS-to-WASM 转换过程中的类型检查机制不够严格，容易被攻击者利用来绕过类型检查。  
  
- PartitionAlloc 被认为是 v8 堆沙箱逃逸的一个未被充分关注的攻击向量，尽管它不在 v8 指针压缩笼子的 4GB 范围内，但仍然容易被攻击者利用。  
  
- 攻击者可以通过修改 PartitionAlloc 元数据来实现地址泄露，并通过这个漏洞实现任意地址写入，进而完全控制受害者的系统。  
  

	
	  
🏷️: 漏洞, Google Chrome, 远程代码执行, WASM, 类型混淆  
###  0x03 Chrome渲染器中的对象转换至远程代码执行漏洞分析  

	>   
> 本文详细分析了 Chrome 中的一个类型混淆漏洞 CVE-2024-5830，该漏洞允许远程代码执行（RCE），并通过对 v8 引擎中对象映射和转换机制的深入探讨，展示了如何利用这一漏洞在 Chrome 渲染器沙箱中实现代码执行。
		  
>   
  



	###  热评   
- Chrome 渲染器沙箱漏洞 CVE-2024-5830 导致远程代码执行  
  
- 浏览器漏洞利用：利用JavaScript触发内存管理错误实现代码执行  
  

	  

		
	  

	###  关键信息点   
- 类型混淆漏洞 CVE-2024-5830：该漏洞源于 v8 引擎中对象 map 的更新过程中出现的类型混淆，允许攻击者在 Chrome 渲染器沙箱中执行任意代码。  
  
- 对象映射和转换机制的复杂性：v8 引擎中的对象映射和转换是优化属性访问的关键，但同时也是引入安全漏洞的潜在来源。  
  
- 字典 map 的意外创建：当对象的属性类型发生变化，且原有 map 无法容纳新的属性转换时，会创建一个字典 map，这在某些情况下可能导致类型混淆。  
  
- v8 堆内存的任意读写：通过控制对象的 map 更新过程，可以实现 v8 堆内存的任意读写，这是利用漏洞实现代码执行的关键步骤。  
  

	
	  
🏷️: Chrome, RCE, 类型混淆, v8引擎, 漏洞分析  
###  0x04 远程禁用Windows事件日志的0day漏洞及免费微补丁  

	>   
> 网页主要介绍了一个名为 \"EventLogCrasher\" 的 Windows 事件日志服务远程攻击手段，以及提供了一个免费的微补丁来修复这一 0day 漏洞。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgQI4pmk1kzzYfHgXmLuycg5wHZibxY64gKjhQENPGsaMQzn55xu8kcdw/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgNt2evWtQI7pBibGWx7eMth6ib0UFEW1how0fT6REjQswZGYujn0rsuqQ/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgqTun71um2Ca3ys4EaJ3J8yL49PFHJ5jncWad1tIHmeQcgPYQpyia2wA/640?from=appmsg "")  

	  
  
<<<左右滑动见更多 >>>  

	###  热评   
- EventLogCrasher 0day 漏洞依然有效，可停止所有域电脑的 Windows 事件日志  
  
- 0patch：修复旧漏洞，顺便还能防0day漏洞  
  

	  

		
	  

	###  关键信息点   
- 任何能够进行身份验证的用户都可以利用 \"EventLogCrasher\" 漏洞远程崩溃 Windows 事件日志服务，这对于系统的安全性和可靠性构成了严重威胁。  
  
- Windows 事件日志服务的崩溃会影响到事件的记录和转发，尤其是在第三次崩溃后服务不再自动重启的情况下。  
  
- 即使在事件日志服务停止的情况下，安全和系统事件仍然会被暂时存储，直到服务恢复，这可能会影响攻击者的行动不被记录。  
  
- 微补丁提供了一种快速、无需重启计算机的解决方案，可以防止漏洞的利用，保护系统免受攻击。  
  

	
	  
🏷️: Windows, 漏洞, 补丁, 事件日志, 远程攻击  
###  0x05 揭秘#GrimResource：滥用MSC文件格式  

	>   
> 网页主要介绍了如何利用MSC文件格式通过微软管理控制台（MMC）实现代码执行，以及Outflank Security Tooling（OST）团队在这一领域的研究和开发。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgsxYFnOPwSWF28TmxoeXjPJNa210E8oDP7f5GbGbA9PMfOKNvObejog/640?from=appmsg "")  

	  


	###  热评   
- 揭秘 GrimResource：Elastic 安全团队与红队的攻防博弈  
  
- GrimResource：Elastic揭示利用MSC文件进行初始访问的技术  
  

	  

		
	  

	###  关键信息点   
- MSC文件格式可以被利用来通过MMC执行任意代码，这是一种对抗受限环境中的安全措施（如AppLocker、PowerShell的受限语言模式、WDEG限制和ASR规则）的有效方法。  
  
- Outflank团队在MSC文件格式的研究上取得了进展，发现了一种新的技术，可以在不启动子进程的情况下在MMC中执行代码。  
  
- 尽管MSC文件格式的某些技术被认为太危险，不适合公开披露，但Outflank团队认为分享研究成果对于提高红队能力和组织的安全防御能力至关重要。  
  
- 网页强调了红队模拟和安全检测规则的重要性，并提供了对MSC文件格式利用方法的技术细节。  
  

	
	  
🏷️: MSC文件格式, 代码执行, 微软管理控制台, Outflank Security Tooling, 安全研究  
###  0x06 NTLM Relay攻击：网络入侵的终极手段  

	>   
> 网页主要介绍了NTLM Relay攻击在Active Directory环境中的危害，特别是通过LDAP进行的攻击方式，以及如何通过WebClient服务来诱导目标进行HTTP认证，从而实现对LDAP的NTLM Relay攻击，以及攻击后的后期利用技术。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgAJTym0ENbxiaW5pstTScsJFQmZR5aSzaNmUKBXSEcFwjIBibUQerYiauw/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgvzkYjq3aCWan90vTBWabiaVzXUiclw9gKnCibjTlHfU2nrDGdIXQ0WWqw/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgTWbhAJLUzXHAciakvNRb6Hia3vh8K3PEnXaxClXyThFNIC2icyR5s6n9A/640?from=appmsg "")  

	  
  
<<<左右滑动见更多 >>>  

	###  热评   
- NTLM 中继攻击到 LDAP(S) 的详细分析  
  
- NTLM 继电攻击：从 WebClient 欺骗到设备接管  
  

	  

		
	  

	###  关键信息点   
- NTLM Relay攻击对Active Directory环境构成严重威胁，可能导致任意设备的权限提升，甚至整个域的妥协。  
  
- LDAP是Active Directory的核心组件，通过对LDAP的NTLM Relay攻击，攻击者可以实现对域的控制。  
  
- 攻击链路分为诱导、传输和后期利用，每个阶段都需要特定的条件和技术。  
  
- WebClient服务是诱导HTTP认证的关键，它可以绕过SMB和LDAP之间的协议不兼容问题。  
  

	
	  
🏷️: NTLM, LDAP, Active Directory, 网络攻击, 认证安全  
###  0x07 Android Jetpack导航库深层次安全漏洞  

	>   
> Android Jetpack Navigation库存在深层次的安全漏洞，可能允许攻击者绕过正常的屏幕流程直接访问应用内的任意页面。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgjksZokQNkicOl1uTNSAlCM0PuoHvpMaQCSBjNF78auQd70uPf4hJ2FA/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgY9ucv2XA2pYRBedrc6gQnBWqT7wQjUWajicmzxIibEEuvP6rSPSZJzVw/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgATesiaiaoK5k8ZhruzGhmwdzSqN7icle9PoQEJtY67h47mgcibzWn2SUaQ/640?from=appmsg "")  

	  
  
<<<左右滑动见更多 >>>  

	###  热评   
- 利用隐式深层链接劫持用户会话：Android Jetpack Navigation 安全指南  
  
- 深入探索 Android Jetpack Navigation  
  

	  

		
	  

	###  关键信息点   
- Jetpack Navigation库中的深度链接机制可能会被恶意利用，导致应用的安全性受损。  
  
- 即使开发者没有在应用中声明深度链接，Navigation库自动生成的内部深度链接也可能被攻击者利用。  
  
- Google对于这一问题的文档警告不够充分，未能全面反映潜在的安全风险。  
  
- 开发者应该谨慎使用Jetpack Navigation库，并考虑自己实现导航逻辑以确保应用的安全性。  
  

	
	  
🏷️: Android, Jetpack, 导航库, 安全漏洞, 隐式深层链接  
###  0x08 Lil Pwny 3.2.0更新：优化Active Directory密码审计  

	>   
> Lil Pwny 3.2.0 发布，为主动目录密码审计工具带来了显著的增强，包括对有 I been pwned 密码数据库的本地审计、标准输出的美化和功能增强、自定义密码列表的扩展生成方法、识别用户名与密码相同的账户以及过滤 Active Directory 输出的功能。
		  
>   
  



	###  热评   
- Lil Pwny 3.2.0 更新：简化 Active Directory 密码审计  
  

	  

		
	  

	###  关键信息点   
- 本地审计：Lil Pwny 3.2.0 支持本地审计，提高了安全性，避免了敏感数据的泄露风险。  
  
- 用户体验改进：通过对标准输出的美化和功能增强，提升了用户的使用体验。  
  
- 自定义密码列表增强：通过生成常见变体，提高了对自定义密码列表的审计效率和准确性。  
  
- 账户安全风险识别：新增的功能能够识别出用户名与密码相同的账户，帮助管理员识别高风险账户。  
  

	
	  
🏷️: 密码审计, Active Directory, 更新, 网络安全  
###  0x09 利用UDL文件进行网络钓鱼攻击的新技术  

	>   
> 这篇文章介绍了一种利用UDL（Universal Data Link）文件进行网络钓鱼攻击的新技术。
		  
>   
  



	###  热评   
- 发现钓鱼攻击有效载荷：来自传统知识的收获  
  
- 新型钓鱼攻击利用UDL文件  
  

	  

		
	  

	###  关键信息点   
- UDL文件可以作为一种新的网络钓鱼手段：通过发送UDL文件附件，攻击者可以诱使用户泄露凭证或哈希值。  
  
- UDL文件的工作原理：UDL文件是一个文本文件，可以配置数据库连接信息，包括服务器名称、认证方式和数据库选择等。  
  
- 利用UDL文件捕获NetNTLMv2哈希值：攻击者可以通过诱使用户尝试连接到一个恶意的SQL服务器来捕获哈希值。  
  
- 调整UDL文件的端口以绕过防火墙规则：通过将UDL文件中的端口从1433改为更常见的端口（如80），可能会增加绕过防火墙的成功率。  
  

	
	  
🏷️: 网络钓鱼, UDL文件, 攻击技术  
###  0x0a Copy2Pwn漏洞绕过Windows网络保护  

	>   
> Zero Day Initiative 的威胁研究员发现了 CVE-2024-38213，一种简单有效的方法，可以绕过 Windows 的网络标记保护（Mark-of-the-Web, MotW），导致远程代码执行。这种新型攻击手段被称为 copy2pwn，它利用了 WebDAV 分享文件在复制到本地时没有应用 MotW 的漏洞。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgibEIInicTvwJ8Dgv3z2urcQ0yky8ayYuNn9AtRDKVeENpia2CT8sutzNw/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgcr65Gp9ic7iaOcALBAXJibWf9j3gccaJaXMrPLCricmcInYbx0zR0rr5vQ/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgBSQ084GsJTtEBsaT2DHbAiawmMJsVY1E392wzr2OydQBicYX7fJwW28A/640?from=appmsg "")  

	  
  
<<<左右滑动见更多 >>>  

	###  热评   
- CVE-2024-38213 漏洞利用绕过 Windows 网络保护  
  
- 微软修复高危漏洞CVE-2024-38213，已被攻击者利用  
  

	  

		
	  

	###  关键信息点   
- WebDAV 分享文件的复制粘贴操作可能会绕过网络标记保护，导致远程代码执行的风险。  
  
- 网络标记保护对于防止未知来源的文件执行至关重要，它是 Windows Defender SmartScreen 和 Microsoft Office 受保护视图等安全功能的基础。  
  
- 威胁行为者利用 WebDAV 分享和精心设计的搜索查询来诱导用户执行恶意代码。  
  
- Windows 操作系统在处理来自 WebDAV 分享的文件时存在多个绕过网络标记保护的漏洞，这些漏洞已被微软修复。  
  

	
	  
🏷️: CVE-2024-38213, Copy2Pwn, Windows, WebDAV, 远程代码执行  
###  0x0b 结合历史泄露与CSS的水坑攻击  

	>   
> 本文探讨了结合历史泄露和CSS的水坑攻击方法，即使这些技术已经很老，但仍然有效。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgvQnxW7IIq6NNy845FoNoTZQhuiaUryuUYhEZ1rsoU4s7AcIRqnerYiaA/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTg1ZzcrB4MxiaW6LlibuDzfV4dic1vwm0ia3z90FYyKdAfQE15peF5oricY0Q/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgewr7y83ttKSwDkNFIw7qfWqbkHdbWJnybvpsgFibCBiatA51Usib6NxHQ/640?from=appmsg "")  

	  
  
<<<左右滑动见更多 >>>  

	###  热评   
- 利用CSS历史记录泄露结合水坑攻击  
  
- 利用CSS结合水坑攻击，窃取历史数据  
  

	  

		
	  

	###  关键信息点   
- 客户端攻击技术虽然老旧，但仍然有效：文章指出，尽管所讲述的技术已经存在多年，但它们在现代安全环境中仍然具有攻击价值。  
  
- 字符集对XSS攻击的影响：通过讨论编码差异，文章强调了缺失字符集属性如何导致XSS漏洞，并提到了历史上的一些相关案例。  
  
- 历史泄露作为精准攻击的手段：通过检测用户访问过的网站，攻击者可以更精确地识别和攻击高价值目标。  
  
- CSS样式的运用：文章展示了如何利用CSS样式来实现对用户历史记录的检测，并通过实例代码演示了这一过程。  
  

	
	  
🏷️: 水坑攻击, 历史泄露, CSS, 网络安全  
###  0x0c GitHub项目ggerganov/llama.cpp中的rpc_server::get_tensor功能存在任意地址读取漏洞  

	>   
> GitHub 上 ggerganov/llama.cpp 项目的 rpc_server::get_tensor 功能存在任意地址读取漏洞，可能导致信息泄露，并且已被证实可以与另一个任意地址写入漏洞结合实现远程代码执行（RCE）。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgxBcE9abqhlxfURSxG2rveZrZiaBVlsaQ3OOPbkmF9VqGwEF07xC58Bw/640?from=appmsg "")  

	  


	###  热评   
- 「编者注」:360 老哥挖的 llama_cpp_python rpc 的 rce.  
  

	  

		
	  

	###  关键信息点   
- 该漏洞源于 g 指针的不安全处理，允许用户控制，进而导致任意地址读取。  
  
- 漏洞的危险性在于它不仅可以被用于信息泄露，还可以与其他漏洞相结合，实现更严重的攻击，如远程代码执行（RCE）。  
  
- 漏洞发现者已经提供了一个详细的漏洞利用示例，包括如何构建、复现以及实际的攻击效果。  
  
- 该漏洞的存在强调了在软件开发中进行安全审查和测试的重要性，特别是在处理用户输入和内存操作时。  
  

	
	  
🏷️: 漏洞, 信息泄露, 远程代码执行, GitHub  
###  0x0d 恶意签名注入：利用Evil Signatures远程删除数据库和邮箱  

	>   
> 网页主要讨论了如何利用恶意签名（Evil Signature）远程删除数据库、邮箱和日志文件，通过将特定的恶意签名注入到系统中，诱使端点检测和响应（EDR）系统误以为这些文件是高危性病毒，从而导致EDR系统自动删除这些文件。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTguroGtyjduJ9IGyg8qGJFU5zicgRy79l8iaT72EL4EtUDGO74As7qWRzw/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgRm5oXucTuCqHcYgF8iaicmBtwbvfy4946yPfS0pzjvUBxvOteNiarOmMg/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgibSgk1AN1MvIUEPDM67QRxIbQviaIRia3zmlp8S9B75GJZhSMQxCNrqdQ/640?from=appmsg "")  

	  
  
<<<左右滑动见更多 >>>  

	###  热评   
- 恶意签名注入：远程删除数据库、邮箱和日志文件  
  
- 恶意签名注入：利用恶意签名远程删除数据库、邮箱和日志文件  
  

	  

		
	  

	###  关键信息点   
- 攻击者可以利用EDR系统的检测机制来删除特定文件：通过将恶意签名注入到系统中，可以诱使EDR系统将包含这些签名的文件误认为是高危性病毒，从而导致自动删除这些文件。  
  
- 恶意签名注入技术适用于多种场景：文章提供了多个实际案例，展示了这种技术如何应用于删除Web服务器日志、FTP用户名、邮箱、系统日志、EDR系统日志、Splunk日志以及数据库文件。  
  
- 客户端也可能受到这种攻击：攻击者可以通过客户端攻击向量，如CSRF、XSS、SSRF等，将恶意签名注入到客户端系统中，导致客户端的文件被错误地删除。  
  
- 对于这种攻击的认识和应对：尽管这种攻击技术已经被报告并得到了修复，但需要持续关注，因为任何特权软件都可能受到恶意签名注入的攻击，这可能导致严重的安全问题。  
  

	
	  
🏷️: 恶意软件, EDR系统, 数据安全  
###  0x0e Python脚本模拟IPv6数据包处理漏洞  

	>   
> 网页内容主要展示了一个Python脚本，该脚本模拟了IPv6数据包处理过程中的一个整数下溢漏洞（CVE-2024-38063）。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgP1LcicahZmR3ZxVBgRljrX5SJewzgsHMSHhlyjQS9KJ8ngE8Yx8SK6A/640?from=appmsg "")  

	  


	###  热评   
- CVE-2024-38063 漏洞利用分析  
  
- CVE-2024-38063 漏洞利用分析：无需蓝屏攻击  
  

	  

		
	  

	###  关键信息点   
- CVE-2024-38063.py脚本旨在展示IPv6数据包处理中的整数下溢漏洞。  
  
- 漏洞存在于process_packet方法中，该方法没有检查整数下溢，并且使用了未检查的total_length来写入缓冲区，可能导致缓冲区溢出。  
  
- 通过创建一个具有极大扩展头部长度的恶意数据包，可以触发整数下溢，从而可能导致程序崩溃。  
  
- 脚本中的注释清晰地指出了漏洞的位置和可能的危险操作，如未检查的缓冲区写入。  
  

	
	  
🏷️: Python, IPv6, 漏洞, GitHub  
###  0x0f BounceBack：红队操作安全的隐蔽重定向工具  

	>   
> BounceBack 是一个用于红队运营安全的隐蔽重定向工具，它是一个高度可配置和定制化的反向代理，具有 Web 应用程序防火墙（WAF）功能，能够通过实时流量分析和多种过滤器组合隐藏 C2/钓鱼等基础设施，防止蓝队、沙箱、扫描器等非法访问。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgSFibiadibr5I954YAutrqcDTZlgDkxvVwA5FVJjDpUxN6icJZEBaDT6RMQ/640?from=appmsg "")  

	  


	###  热评   
- BounceBack: 红队行动的隐形重定向工具  
  
- BounceBack：红队操作的安全隐蔽重定向工具  
  

	  

		
	  

	###  关键信息点   
- 隐藏基础设施：BounceBack 的主要目的是隐藏红队的攻击基础设施，防止被蓝队等安全机构检测。  
  
- 高度可配置：工具提供了丰富的配置选项，包括过滤器管道、规则组合、IP 地理位置检查等，以满足不同的操作需求。  
  
- 易于扩展：BounceBack 的项目结构允许用户轻松添加自定义规则和协议，以适应特定的 C2 或操作场景。  
  
- 流量分析：通过实时流量分析和多层次的过滤器，BounceBack 能够有效地识别和拒绝非法流量。  
  

	
	  
🏷️: 红队, 隐蔽重定向, 反向代理, WAF, 流量分析  
###  0x10 SpoofDPI：一款快速绕过深度包检测的软件  

	>   
> SpoofDPI 是一个简单快速的软件，旨在绕过深度数据包检测（DPI）。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgDNMQnooqwNZLR6RnY5b6DYMkaibzeMSxpUOxCReTgic2XEqavkEN3ia8w/640?from=appmsg "")  

	  


	###  热评   
- Go语言编写的简单快速反审查工具  
  
- SpoofDPI: 反审查利器，像鲨鱼一样在网络中穿梭  
  

	  

		
	  

	###  关键信息点   
- SpoofDPI 的设计目的是绕过 DPI，特别是针对 TLS 握手过程中的客户端请求。  
  
- 软件支持多种操作系统和架构，提供了灵活的安装和使用方式。  
  
- SpoofDPI 通过将客户端请求的第一个字节单独发送的方法，来避免 DPI 的检测。  
  
- 尽管 HTTPS 通信大多数情况下能够隐藏请求细节，但客户端请求中的域名信息仍然以明文形式出现，可能会被 DPI 检测到。  
  

	
	  
🏷️: DPI绕过, 网络安全, 软件工具, GitHub项目  
###  0x11 CVE-2024-7646：Ingress-NGINX注解验证绕过漏洞深度解析  

	>   
> CVE-2024-7646 是一种影响流行的 Kubernetes 组件 ingress-nginx 的安全漏洞，允许攻击者绕过注解验证，可能导致对敏感集群资源的未授权访问，该漏洞的 CVSS v3.1 基础分数为 8.8（高）。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgu3TxCICXnnyrl2weANNrekozFXZ5gwgTE6M6oviaa2LmYxfXlbhhSow/640?from=appmsg "")  

	  


	###  热评   
- Ingress-NGINX 出现漏洞 CVE-2024-7646: 注解验证绕过  
  
- Ingress-NGINX 漏洞CVE-2024-7646：注解验证绕过  
  

	  

		
	  

	###  关键信息点   
- Ingress-nginx 是一个流行的 Kubernetes ingress 控制器，用于管理集群内服务的外部访问，充当反向代理和负载均衡器。  
  
- 该漏洞的严重性体现在，它可能导致对集群的完全破坏，包括对机密性、完整性和可用性的影响。  
  
- 尤其对于多租户环境和缺乏适当访问控制的集群来说，这个漏洞危险性极高。  
  
- 攻击者可以通过创建包含特殊注解的恶意 Ingress 对象来利用这一漏洞，从而绕过验证并执行任意命令。  
  

	
	  
🏷️: Kubernetes, ingress-nginx, 漏洞, 安全  
###  0x12 Linux内核修复Landlock安全漏洞  

	>   
> Mickaël Salaün 报告了 Landlock 的安全漏洞（CVE-2024-42318），该漏洞允许进程逃逸沙箱并绕过限制，已在 Linux 6.11-rc1 中修复，并已回退到多个版本的 Linux 内核。
		  
>   
  



	###  热评   
- Landlock 漏洞 CVE-2024-42318 允许进程逃逸沙箱  
  

	  

		
	  

	###  关键信息点   
- Landlock 漏洞（CVE-2024-42318）: 该漏洞允许进程逃逸沙箱，已得到修复和测试，确保不会再次发生。  
  
- 内核更新的重要性: 修复漏洞只需更新内核，而不需要更新沙箱程序。  
  
- Landlock 的作用: Landlock 是一种深度防御机制，增强了系统的安全性，但不应作为唯一的安全层。  
  
- 安全机制的叠加: 通过 seccomp 过滤器等技术可以进一步减少任意代码执行和系统调用的风险。  
  

	
	  
🏷️: Linux, 安全漏洞, Landlock, 内核修复  
###  0x13 Windows Secure Channel RCE漏洞CVE-2024-38148详解  

	>   
> 网页主要介绍了 Windows Secure Channel 中的一个Use-After-Free（UAF）漏洞（CVE-2024-38148），该漏洞可能被利用实现远程代码执行。
		  
>   
  
  

	  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgibU3Q3FcLBSsdZarw6UrVFAibuXlX5esxRyWPMibgthaFnJhUSsNDGmoA/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgmFZ5XMZpmlib7IcjR7yfkUgibsficjUuuS7LYicgG8ibzc7TrXYSRaZ3xfw/640?from=appmsg "")  

	
	![](https://mmbiz.qpic.cn/sz_mmbiz_png/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTgiaX4Jw6GGMawTJx1tM1eNPMN3sOibal88cwTXPARcaibauh63b2XZxkKA/640?from=appmsg "")  

	  
  
<<<左右滑动见更多 >>>  

	###  热评   
- Windows 安全通道 RCE 分析：MSRC 认定为拒绝服务  
  
- Windows Secure Channel 远程代码执行漏洞 CVE-2024-38148 简介  
  

	  

		
	  

	###  关键信息点   
- 微软官方对 CVE-2024-38148 的定义为 DoS 问题可能不完全准确，实际上是一个 UAF 漏洞。  
  
- 通过补丁对比，可以发现补丁的作用是屏蔽了某个字段的赋值操作，这一点是防止漏洞利用的关键。  
  
- UAF 问题的产生是因为在 CSsl3TlsContext::CSsl3TlsContext 函数中未能更新 M1 的第一个字段，导致该字段仍然指向已释放的 a2 结构体。  
  
- 实际测试确认了 CTls13ServerContext::CleanupConnectedState 函数中存在 UAF 问题，这可能导致远程代码执行。  
  

	
	  
🏷️: Windows, 漏洞, RCE, Secure Channel, UAF  
  
  

	  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icqm3vRUymZnpzXhHJwRsVFic7Sia3MqiaTg86C3EVW7ZBOD533reH1QnsMrQpNICvlegQ9GQz0uVvc9WnJvFe5mZg/640?wx_fmt=jpeg "")  

	  

		快来和老司机们一起学习吧  
  
