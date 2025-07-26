#  “AI+Security”系列第4期（一）之“洞” 见未来：AI 驱动的漏洞挖掘新范式   
原创 知识分享者  安全极客   2024-12-24 10:35  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8QmTLhv0jB8GS6Wtic69pG44V8Gib7ccD3FZolnOVkdOPafA3YULibw9S5AEkdO8sstRLGNFVDj7SgRg/640?wx_fmt=jpeg&from=appmsg "")  
  
在数字化浪潮下，安全漏洞问题日益严峻，成为各行业发展的重大挑战。近日，“AI+Security” 系列第 4 期线下活动于北京成功举办，聚焦 “洞” 见未来：AI 驱动的漏洞挖掘新范式，汇聚了安全领域的众多专家。  
  
本次活动由  
**安全极客、DataCon 社区、InForSec 网络安全研究国际学术论坛**以及  
**清华校友总会 AI 大数据专委会**联合主办，众多行业精英参与，包括**奇安信安全研究员尹斌、华清未央 CEO 朱文宇、水木羽林技术专家张强、云起无垠引擎负责人李唯、长亭科技联合创始人龚杰、云起无垠 CEO 沈凯文**以及  
**前华为高级安全专家、简世咨询高级顾问孙志敏**，共同探讨 AI 技术在漏洞挖掘领域的前沿应用、面临挑战及未来发展方向，为筑牢国家网络安全屏障提供思路与智慧。  
  
**分享一：LLM辅助的模糊测试增强技术**  
  
  
在当今数字化浪潮之下，模糊测试作为一种自动化软件测试手段，于漏洞挖掘及评估范畴的重要性愈发凸显，其测试范畴涉猎广泛，从操作系统内核、数据库，再到各类协议等均有涉及。  
**水木羽林技术专家张强**针对大语言模型（LLM）在嵌入式操作系统模糊测试以及数据库模糊测试领域的探索与实践，展开了深入剖析。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4zTXRTXLpYkPiaYgz9ic9SzXvL1hOLjiceXtHZEF65nTXV04TZXzjA9qOA/640?wx_fmt=png&from=appmsg "")  
  
张强博士指出，嵌入式操作系统如今广泛扎根于工业物联网、航天航空等关键领域，然而，随着其应用场景的拓展，安全漏洞隐患也呈现出与日俱增之势，诸如令人警醒的 BadAlloc 系列漏洞以及 URGENT/11 系列漏洞等。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4icxRdTibQrA9cWBpgSNEJtuG37LBnbGhxtmJRUruIib23cyibgB0vqZHOQ/640?wx_fmt=png&from=appmsg "")  
  
聚焦于 LLM 加持下的嵌入式操作系统内核模糊测试，嵌入式操作系统既有着广泛的应用面，又关乎关键领域的安全命脉。内核模糊测试的对象囊括了传统操作系统内核、面向特定领域的操作系统内核以及广义内核，并且不同类型的内核均匹配有专门的测试工具与注入途径，输入生成策略也花样繁多。  
  
过往研究表明，尽管已有关于 LLM 的相关探索，但嵌入式内核因其架构多元等复杂特性，仍存在诸多难题。覆盖引导测试依循特定流程运作，面对现存挑战，借助大语言模型生成能够触发内核模块的用户态程序、精准提取深层状态路径等手段，可有效破局。其框架设计分两个关键阶段推进，实验评测结果显示，所采用的 ECG 方法在漏洞挖掘以及代码覆盖率上成效明显，且消耗的 token 数量较少。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4T9zlnpn7micPbicabccYdsgdDcJelSGnrhvvSeqDZI7xITyjHhy92vWA/640?wx_fmt=png&from=appmsg "")  
  
对于数据库模糊测试，张强博士提及，大语言模型虽已在此崭露头角，却也饱受诸如产生幻觉、长文本理解瓶颈等问题的困扰，在驱动合成、输入生成以及漏洞探测等核心环节，均面临不小挑战。对此，张强给出了极具针对性的建议：  
**首先**，摒弃一次性完成驱动程序合成的做法，转而依据已识别出的错误，反复向 LLM 查询并持续修复错误；  
**其次**，广泛收集函数原型、测试用例，或是梳理函数间的连接规则，为提示工程提供有力支撑；  
**最后**，针对复杂系统而言，采用传统的程序分析方法往往比单纯倚重大语言模型更为稳妥、切实可行。  
  
**分享二：基于大模型的漏洞挖掘技术与实践**  
  
  
在当今的网络安全领域，漏洞挖掘无疑是重中之重，其方法也是多种多样，涵盖了静态代码分析、模糊测试，以及大模型与之相结合的创新手段。来自  
**奇安信**的  
**尹斌**先生针对这些不同的漏洞挖掘路径，分别进行了深入且细致的分享，为行业带来诸多启示。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm40J1tZNnqW05Y7SyzgKF7xeZQ8EZftpia4UbwV47qGPntWva9s4Omwjw/640?wx_fmt=png&from=appmsg "")  
  
首先聚焦于  
**大模型融合静态代码分析**挖掘漏洞这一前沿领域。静态代码分析作为一种强大的漏洞检测技术，具有无需运行程序即可查找潜在漏洞的显著优势，其所运用的常见技术手段包含符号执行等，在工具层面，像 CodeQL 这类经典之作更是广为人知。以 DiverseVul 项目为例，它创新性地通过爬取海量数据，并对大模型进行针对性微调等一系列操作，惊喜地发现大模型在漏洞检测性能方面得到了飞跃式提升，这其中特定的预训练任务更是发挥了关键支撑作用。再看 GPTScan，它将目光聚焦于智能合约逻辑漏洞检测这一细分赛道，巧妙地把大模型与静态分析有机融合，通过严谨的步骤，先是精准过滤函数，接着科学判断可达性，随后提取关键的变量语句并辅以静态确认等，成功挖掘出诸多之前未曾发现的新漏洞。  
  
其次，在  
**大模型融合模糊测试**挖掘漏洞板块，模糊测试以其独特的通过输入异常数据来探寻漏洞的方式，在安全领域被广泛应用，诸如天象模糊测试平台，就以支持多种语言和架构的优势备受瞩目。在经典的 Fuzz 驱动生成方法中，研究人员巧妙设计提示策略，充分利用各类不同信息来生成并修复驱动程序，其中 ALL 策略脱颖而出，展现出更为卓越的效果。后续的增量研究更是乘胜追击，通过持续优化提示词以及精细调整调用结构，进一步推动了驱动生成效果迈向新高度。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4r640gLcMbOCOdHWx1ZPibmv5HZnvRGP2mpiciao4qWTJ2dPCJQIW6SINQ/640?wx_fmt=png&from=appmsg "")  
  
再者在  
**大模型融合 Agent**挖掘漏洞方面，基于大语言模型（LLM）构建的 Agent 展现出非凡实力，它被赋予了推理、规划、执行以及存储等一系列关键能力。以 Naptime 项目为例，它开创性地给予大模型更为充裕的推理时间，巧妙搭建交互式环境，高度模拟人类编程测试流程，在此过程中灵活调用各类工具并严谨验证漏洞。而 Big Sleep 项目作为 Naptime 的进阶版本，更是取得了亮眼成果，成功揪出 SQLite 中的 stack overflow 漏洞，其操作流程环环相扣，从深入理解代码起始，历经细致分析、精巧构造输入、反复调试等多个关键步骤，最终精准总结出漏洞成因。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4icWq3KXkaY5Kzab7XtZUmBWVBZnk0I0Ezzg7XcDkVMbTZTiaMbSoox4w/640?wx_fmt=png&from=appmsg "")  
  
最后来探讨大模型挖掘漏洞实践展现出的多元能力。从全流程能力来看，它以对程序的理解为起点，借助大模型的代码解析能力，分析程序架构、逻辑，找出潜在风险点；接着利用大模型融合静态代码分析、模糊测试等技术优势，挖掘程序深处的漏洞；最后完成漏洞修复报告的撰写，在报告中清晰呈现漏洞详情、修复建议以及风险评估等内容，形成从发现问题到解决问题的完整流程，为漏洞挖掘工作提供实用且有效的指引，助力从业者高效开展工作。  
  
**分享三：机器语言大模型 Machine Language Model**  
  
  
在网络空间的运行体系里，机器语言处于核心地位。然而，现阶段像 ChatGPT 这类广为人知的大模型，却在理解机器语言方面存在短板。与此同时，闭源软件分析更是面临重重困境：一方面，深度检测手段匮乏，面对无源码软件时检测工作举步维艰；另一方面，软件生态迁移效率低下，一旦涉及跨平台等迁移需求，往往不得不重新开发。再者，逆向工程难度颇高，不仅过度依赖国外工具，而且人工分析时准确率与效率双双受限，严重阻碍了软件国产化的推进步伐，那些由国外公司主导的行业软件，国产化替代更是难上加难，就连基于规则的恶意代码及漏洞检测，效果和效率也不尽人意。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4G11tibZ3EPnyNGNzKtb0lRL8z2B14H7cZcstsuoIP8Cgt1d9UichtWRQ/640?wx_fmt=png&from=appmsg "")  
  
面对如此现状，  
**华清未央 CEO 朱文宇博士**分享了机器语言大模型（MLM）这一前沿成果。他通过与自然语言大模型（GPT - 4o）以及代码大模型（DeepSeek Coder V2）细致对比后发现，MLM 在反编译代码领域展现出独特优势，具备多项突出能力：  
  
**其一，卓越的代码分类能力。**它能够迅速判别代码类型，精准判断代码敏感性，从而精准锁定目标代码，为逆向工程、恶意代码分析、漏洞挖掘以及性能瓶颈定位等工作提供有力辅助。例如，面对一段汇编代码，它可以给出关于其可能涉及功能的概率预估，像精准判断某函数有 75.98% 的概率与屏幕截图功能紧密相关。  
  
**其二，精准的代码相似性检测能力。**这一能力广泛应用于供应链成分分析、代码克隆检测、恶意代码家族甄别、相似漏洞挖掘、补丁比对分析以及逆向工程辅助等诸多关键领域。它的工作原理是通过严谨计算函数间的语义相似性分数，进而明确哪些函数与目标函数相似，哪些与之截然不同。  
  
**其三，具备语义摘要能力。**MLM能够对给定的二进制代码生成自然语言解释，用一个单词或者简短的几句话概括代码的功能语义，辅助分析人员快速理解代码语义，并为模块，程序，文件的整体摘要提供基础。  
  
**其四，出色的语义恢复能力。**该能力在逆向工程、恶意代码分析、漏洞挖掘等工作中发挥关键作用，比如针对给定的汇编代码，它能够进行反编译操作，将其转换为类 C 语言代码，尽可能还原代码背后的逻辑，当然，由于汇编代码本身的复杂性以及上下文较强的依赖性，还原结果或许无法做到百分之百精准。  
  
**分享四：模糊测试技术与AI的前沿探索**  
  
  
在不同的应用场景下，Fuzzing 技术始终面临着使用门槛过高的难题。这要求测试人员必须对被测项目有着极为深入的了解，过程中涉及大量的人工操作，并且对测试人员自身素质提出了严苛要求。幸运的是，大语言模型（LLM）的诞生为突破这一困境带来了曙光，有望降低 Fuzzing 的使用门槛。  
**云起无垠引擎负责人李唯**聚焦于 AI 在源码和固件这两个关键领域的探索实践，致力于全方位提升 Fuzzing 的效率与效果，力求挖掘出更多隐藏的安全漏洞。  
  
源码模糊测试方面，传统的模糊测试驱动生成存在诸多难点。  
**一方面**，人工编写成本居高不下，测试人员不仅需要人工识别库的攻击面，还要编写符合接口调用逻辑的复杂测试驱动，为了触发更多被测试代码，往往还得设法增加驱动的复杂度，同时更要精准引入合适的头文件和库，并确保整个编译链接过程顺利通过。**另一方面**，静态分析的准确率偏低且误报率颇高，在识别某些接口参数间的语义联系时力不从心，想要依据程序结构信息构造合理的上下文更是困难重重。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4B43yJNlYwLrMUlNUAOb1AaoaCTJES3G08O1wEynLLGibPGicSAs7D85g/640?wx_fmt=png&from=appmsg "")  
  
鉴于此，李唯表示，结合静态分析技术与自动编译链接框架，我们精心设计了基于 LLM 生成 Fuzz 驱动的创新框架，这一框架让测试更加高效、精准。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm42chLYY2IBIEiabhoA24alLwZjQiakdic4Gfk4OEpHDHNTdM0hJlB600Og/640?wx_fmt=png&from=appmsg "")  
  
在固件模糊测试与 AI 的结合探索，李唯表示，我们选择了种子生成优化这一方向。具体实施策略如下：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4JF2zrm7CWMZnIHdmibQT4KZvt6QFS0iaSfQOjaOXoib2AJ6HNYBmXHq4A/640?wx_fmt=png&from=appmsg "")  
  
首先，充分发挥大模型强大的理解能力，对 BusyBox 中集成的各类工具展开深入剖析，力求精准把握每一种工具的使用方式。  
  
其次，依据不同工具各自的特点，利用大模型针对性地生成与之适配的测试数据。  
  
随后，将这些初始数据进行专业的种子清洗。通过严谨筛选，去除那些无效或干扰性强的数据，只保留真正合适、最具潜力的数据作为最终用于测试的种子。  
  
最后，调用AFL，凭借其专业性能，利用筛选出的优质种子开启全面测试流程。如此一来，便能为固件的安全漏洞检测注入强劲动力，保障固件在复杂多变的运行环境中维持稳定可靠的状态。  
  
**圆桌讨论**  
  
  
在圆桌环节，奇安信安全研究员尹斌、华清未央 CEO 朱文宇、长亭科技联合创始人龚杰、云起无垠 CEO 沈凯文以及前华为高级安全专家、简世咨询高级顾问孙志敏围绕“大模型赋能漏洞”进行了重点探讨。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8Sfro61jFwDky6GtXJbXUm4EBpd9wy9xrMoVEJVycT1iaYoz1mhJeNkHrtOP8oZmQeOziaMwJuwRzibA/640?wx_fmt=png&from=appmsg "")  
  
1. 目前AI编程越来越火，比如像cursor、codeium之类的AI编辑器出现以后，写出来的代码是不是就天然和安全了？是不是漏洞挖掘就没那么重要了？  
  
2. 现在大模型的能力越来越强了。在科研界和工业界，有没有可能在未来通过大模型的预测和分析能力，就能把漏洞挖掘问题彻底解决掉？是哪些因素影响了直接使用大模型进行漏洞挖掘的效果？  
  
3. 除了漏洞挖掘，漏洞的可利用性验证、漏洞的修复也是从业者们非常关注的点。各位专家认为现在的漏洞自动化验证和修复存在的最大瓶颈是什么？大模型的出现给漏洞验证和漏洞修复带来了怎样的能力提升？  
  
**写在最后**  
  
  
此次活动旨在通过行业专家的经验分享和思维碰撞，促进 AI 驱动的漏洞挖掘技术不断进步，为筑牢国家网络安全屏障贡献智慧和力量。后续安全极客也将针对本期活动四位嘉宾的分享与圆桌讨论带来更详细的内容整理，欢迎大家的持续关注。  
  
未来，安全极客社区也将围绕“AI+Security” 开展更多相关主题活动，汇聚更多行业专家、分享更多技术干货，共同促进行业发展。  
  
[](https://mp.weixin.qq.com/s?__biz=MzkzNDUxOTk2Mw==&mid=2247495405&idx=1&sn=67249648d5c312b5c178b23b077d28f3&scene=21#wechat_redirect)  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vWuBpewLia8SbEa20DfsSwTdtZvGHcMdnaeeCU3zmv6KREjeTkJ8NPf8CUpib4ejMVtx8KlQvDPiav7IxVTl6Qe4w/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
[](http://mp.weixin.qq.com/s?__biz=MzkzNDUxOTk2Mw==&mid=2247493750&idx=1&sn=27bd578179e5abbdc8907b669519bb8f&chksm=c2b95d82f5ced4945cf8844013563398cb3a885ea96a2ee2b60bfcc26d77ebffe78a35285646&scene=21#wechat_redirect)  
  
[](http://mp.weixin.qq.com/s?__biz=MzkzNDUxOTk2Mw==&mid=2247493759&idx=1&sn=0aed37ae210bde25a6b16a745301b71d&chksm=c2b95d8bf5ced49d12eb8cc6192c4e091bf11b6ffe99d4025467ea98b9d04cad89ba0ea91710&scene=21#wechat_redirect)  
  
[](http://mp.weixin.qq.com/s?__biz=MzkzNDUxOTk2Mw==&mid=2247493770&idx=1&sn=2c6d24403cda8f0ef45cadb10e1bfebd&chksm=c2b95d7ef5ced4686e39951e21153c81f0a1e57cabf0937e0d996e6621385745d3ee30d98c11&scene=21#wechat_redirect)  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vWuBpewLia8Q8ZzB8H1iavVTGLzQKrmiaV9ZINGu1cbRLSnUrgib5SPL2ibfOu7IicnWewfFoticsJsNECqJXia5mV8tWw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
  
