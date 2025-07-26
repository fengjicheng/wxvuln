#  弗吉尼亚理工大学 | 利用 ChatGPT 生成漏洞利用代码   
原创 童话  安全学术圈   2023-10-08 11:14  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmHiaQdannS0XZIDKI3wE2f2pqoic3IkiaxAqmk8AdYmrYTRoQ3n1YD1y0w/640?wx_fmt=png "")  
> 原文标题：How well does LLM generate security tests?原文作者：YING ZHANG, WENJIA SONG, ZHENGJIE JI, DANFENG (DAPHNE) YAO, NA MENG原文链接：https://browse.arxiv.org/pdf/2310.00710.pdf主题类型：漏洞分析笔记作者：童话 - https://www.tonghuaroot.com/about主编：黄诚@安全学术圈  
  
# 概述  
  
研发人员在开发软件过程中会大量引入第三方组件提高生产力和软件质量。这些第三方组件存在潜在的安全漏洞，进而被攻击者利用，此类攻击被称之为软件供应链攻击，2022 年记录数量增长了 742%。  
  
业界普遍的做法为通过 SCA 识别程序引入的第三方组件版本及存在的安全漏洞，并指出安全版本以供研发人员修复。  
  
大量的误报导致研发人员确认对此类漏洞报告缺乏信心（甲方痛点），并要求提供漏洞利用代码和 PoC，以证明漏洞的严重程度和修复的必要性。  
  
手动创建漏洞利用代码和 PoC 有挑战性且耗时（甲方安全团队通常缺乏足够的资源做这件事），目前业界无相关工具自动化该过程。  
  
该 paper 通过 ChatGPT-4.0 生成测试用用例（PoC），演示如何利用程序中引入的低版本第三方组件中存在的安全漏洞。  
  
通过利用多种 prompt styles/templates （提示词风格和模版），使用 ChatGPT 生成 55 个测试用例，并演示 24 次攻击。  
  
相比业界知名安全测试用例生成器 TRANSFER 和 SIEGE，ChatGPT 可以生成更多的测试用例并实现了更多的攻击。通过合理优化 ChatGPT 提示词，如漏洞描述、潜在攻击、以及代码环境时，ChatGPT 的效果更好，这将帮助研发人员设计和创建默认安全的应用。  
# 研究问题  
  
2021 年，软件供应链攻击增长 650%，导致 12000 起安全事件。  
  
2022 年，软件供应链攻击以 742% 的年增长率增加，主要利用 JavaScript、Java、.NET 和 Python 的第三方组件漏洞。OWASP 将“易受攻击和过时的第三方组件”列为 2022 年第 6 大安全漏洞。  
  
研发人员创建了如 snyk-test 和 npmaudit 等扫描工具试图解决此类问题，传统的解决方案是分析依赖关系及相关版本的安全漏洞（匹配开源漏洞库，如 CVE）。  
  
接近 99% 的误报率使研发人员对此类工具缺乏信息，并提出如下需求：  
  
在相关漏洞报告中指出具体存在漏洞的函数（相关 API），并提供 PoC，证明该漏洞确实影响相关应用。  
  
基于上述痛点和需求，该 paper 尝试研究使用 ChatGPT 生成相关安全测试用例，触发调用应用引入的相关第三方组件 API，观察程序执行异常行为，如抛出异常或应用无响应。研发人员可以使用生成的测试用例观察漏洞传播路径，评估漏洞的潜在影响，为漏洞是否修复提供决策指标。  
  
其目的为，帮助研发人员减轻软件供应链威胁，促进相关第三方组件漏洞的修复。  
  
相关研究问题如下：  
## RQ1：ChatGPT 生成的安全测试用例效果如何？  
  
创建一个 26 个库和 55 个应用程序的数据集，每个应用引入一个存在漏洞的第三方组件。  
  
对每个应用，向 ChatGPT 提供一个提示词，描述漏洞、应用程序上下文、以及对应的第三方组件信息及生成安全测试用例的需求，并另其提供漏洞利用代码。  
  
ChatGPT 为 55 个应用程序生成相关 PoC 和漏洞利用代码，其中 24 个是有效的。  
## RQ2：在不同类型的提示词下，ChatGPT 的安全测试用例生成性能有何差异？  
  
实验结果表明，合理设计 ChatGPT 提示词对生成漏洞利用代码具有重要意义。  
  
提供第三方组件的安全测试用例，相比其他提示信息更重要，未提供相关 PoC，ChatGPT 则无法生成有效测试用例。  
## RQ3：ChatGPT 与现有的安全测试用例生成工具相比如何？  
  
ChatGPT 的性能超过了业界知名的两款工具 SIEGE 和 TRANSFER。在上述 55 个应用中，ChatGPT 生成了 24 个有效测试用例，TRANSFER 生成了 4 个，SIEGE 则为 0。  
# 举个例子  
  
为了便于讨论，提供一个具体的例子，演示利用第三方组件引入的安全漏洞对应用程序造成的影响。  
  
Bouncy Castle（BC）是一个用于加密的 Java API 集合库，CVE-2020-28052 披露了其在 1.65 和1.66 版本中存在的安全漏洞，OpenBSDBCrypt.checkPassword(...) 在检查密码时比较了错误的数据，导致错误密码被接受为有效密码。  
  
如下为对应的 PoC：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmg91iaBuVkm6JouM6iaibfic7KGHK3Vz7KS4h4wdQ5F6iaicBficZ4Z6ya9ricg/640?wx_fmt=png "")  
  
由于该 PoC 仅针对第三方组件本身，并未展示引入该组件的应用程序是否存在该漏洞。  
  
也就是说，如果应用程序引入了存在漏洞的第三方组件，但是并未调用具体的漏洞函数，则该漏洞并不影响该应用，或者即使调用了了漏洞函数，但是未提供有效证明该应用受到对应漏洞的影响。如果 OWASP Dependency-Check 等工具多次报告此类漏洞，则会让研发人员对其失去信心。  
  
因此，该 paper 主要尝试为应用程序生成对应的安全测试用例，即展示第三方组件的漏洞如何引入进应用程序中，以及攻击者如何利用该漏洞。  
# 方法  
  
针对 ChatGPT 生成安全测试用例的能力进行研究，研究过程分为 3 个阶段，数据集构建、提示词设计、结果验证。  
  
在数据集构建阶段，收集了第三方组件的已知漏洞、漏洞 PoC 以及可能收到该漏洞影响的应用程序。  
  
在提示词构建阶段，结合前期收集的数据集，为每个 ⟨𝐿𝑖𝑏, 𝐴𝑝𝑝⟩ 对指定各种提示词，要求 ChatGPT 利用提供的相关信息生成针对应用程序的漏洞利用 PoC。  
  
在结果验证阶段，收集 ChatGPT 输出，评估测试用例质量。  
## 数据集构建  
  
下表为数据集中包含的第三方组件漏洞和应用信息。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqm8mgzLh4PQ0aRjcYjEZhUZibd2W5tpBFsLWOMceWWxfVaHwG0MlKpYdw/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmQkK5uV7iaatFEBGuNFkmDlPv4vX3njiaIZKMLg3daNsdY3AwnJMu03eA/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmIrt2FDib9mFObhib4lPNOiamFNzPGQ2xpJvN3MrfibOo4ibZTdZlSBQI6Dg/640?wx_fmt=png "")  
  
需要注意的是，我们并不期望 ChatGPT 可以自动化检测出存在漏洞的第三方组件或者应用程序调用了存在漏洞的 API，这些信息和数据由领域专家（安全工程师）提供，ChatGPT 针对这些信息生成针对应用程序本身的安全测试用例（PoC）。  
## 提示词设计  
  
相关 prompt 和场景如下：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqm0QAianYGMSHHTEMaS5HDzM720BKgQVlOLMnVTpcN6icQns69SKbaBluA/640?wx_fmt=png "")  
  
prompt 设计宗旨：检查 ChatGPT 是否能够模仿示例测试用例 𝑇，并且为 𝑀 创建恶意输入，以便 𝑀 也使用恶意输入调用库 API（参考上图）。  
  
同时还提供了替代 prompt 模版的设计，通过去除其中一个变量，生成 5 个变体模版。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmLfeibJmIobrFgzaBia8UPnQNfOc9fF2tZyYLezRAZHd27xUh3WialjibKA/640?wx_fmt=png "")  
## 结果验证  
  
将 ChatGPT 的生成结果引入到应用程序中，手动 debug 解决部分潜在的编译错误，运行对应测试用例并观察实验结果。  
# 实验结果  
  
实验结果如下：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmCH181jyllkALfHNyiasAFYkIsa4ocia01gp1qibSm2ycoHiaZ7AGrJ90Sw/640?wx_fmt=png "")  
  
上表相关字段：  
  
A：Tool Applicability  
  
C：Test Compilability  
  
V：Vulnerability Exploitation  
  
Finding 1: ChatGPT 在生成已知第三方组件漏洞的安全测试用例方面有潜力。在给定 55 个测试生成任务的情况下，其生成了 55 个测试用例，其中有 40 个易于编译，24 个测试成功地利用了漏洞。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmQkcsShKeJtM6gU9Rn06jRibCX4fSKvTqsgsoxrC998zheTRG0wBib75g/640?wx_fmt=png "")  
  
Finding 2：在五个模板变体中，𝑃3 和 𝑃4 导致 ChatGPT 在生成可编译的测试用例时表现较差。ChatGPT 在生成测试用例时，倾向于使用更多的上下文代码和较少与测试生成任务相关的约束条件，从而产生更多可编译的测试。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmJHFywtXOzc1au4HibF6QdPWqRTxS9PGuvxde6EhNxaVnXRWMm67Hlaw/640?wx_fmt=png "")  
  
Finding 3：根据默认提示模板 𝑃 所涵盖的五个信息元素，所有元素都在帮助 ChatGPT 有效生成漏洞利用方面发挥了重要作用。特别是(v)和(vi)，比(iii)，(iv)和(vii)更为重要。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/6Dibw6L070WFBrthQI7ZMfGt5cciavWfqmK57hcrHJUl2SwIhnRI3AlMh6n4KicdZNCAfFeqCIsic8wAqOAm6c5l5Q/640?wx_fmt=png "")  
  
Finding 4：在当前评价标准下，ChatGPT 在工具适用性、测试可编译性和漏洞利用方面表现优于TRANSFER 和 SIEGE。这一结果表明了 ChatGPT 在安全测试生成方面的巨大潜力。  
# 局限性  
  
上述实验结果仅限于数据集中包含的漏洞、第三方组件和应用。未将 ChatGPT 应用于其他编程语言，仅测试了利用 ChatGPT 生成 JUnit 测试用例。同时该数据集未覆盖到更复杂漏洞利用场景，如需要提供网络配置或者 CS 架构的应用。为了将进一步扩充数据集，以使其更具有普适性。  
  
另外，虽然 ChatGPT 的生成结果充满内部随机性，但是根据实验发现，此类问题在 ChatGPT 多次尝试使用相同 prompt 时，产生的结果是相似的。作者团队认为 ChatGPT 的内部随机性不会对实验结果产生显著影响。  
  
由于 ChatGPT 使用 2021 年 9 月之前的数据训练，在我们的实验数据集中对于 2021年9月之前的数据可能会出现过拟合的情况，评估结果可能会高于 ChatGPT 的实际能力。但是，考虑到 ChatGPT 对于截止日期之前已知漏洞的较低成功率（即41%对67%）以及ChatGPT使用的大量训练数据（即570 GB），过拟合问题似乎并不重要。  
# 结论  
  
该 paper 探索了 ChatGPT 在生成安全测试用例领域的应用，展示了安全漏洞如何从第三方组件引入到应用程序中。  
  
ChatGPT 在生成安全测试用例领域效果显著，其能力超过利用复杂程序分析和遗传编程生成多样化测试用例的最先进工具。  
  
ChatGPT 对提示词的质量非常敏感，为了充分发挥 ChatGPT 的优势，我们需要精心设计提示词模板，引入更多的领域知识对生成效果是有帮助的。  
  
ChatGPT 生成的部分测试用例虽然在已知漏洞上未奏效，但是通过生成的测试用例发现了非预期的安全漏洞，作者团队提交了 4 个 CVE，其中 1 个已经被相关开发人员确认，这表明其在漏洞发现上存在潜力，通过和现有的漏洞发现工具集成，可以帮助研发人员高效检测和修复安全漏洞。  
# 思考  
  
对科研的一些思考：对于一些常识性问题，如果我们可以提供数据驱动的证明，那也是有价值的，同时也有可能会有一些意想不到的发现。  
  
从甲方安全视角看该 paper 的价值：为 SCA 漏洞报告补充更多指标（应用级别的漏洞验证代码 PoC），为 push 研发人员升级第三方组件解决相关安全漏洞提高有效证明。  
# 作者信息  
- Ying Zhang 弗吉尼亚理工大学计算机科学系博士生，研究重点是通过静态程序分析进行漏洞检测，旨在帮助开发人员消除安全 API 的误用。https://neuzhangy.github.io/  
  
- 孟娜，自2021年起担任弗吉尼亚理工大学计算机科学系副教授。她的研究兴趣包括软件工程、编程语言、软件安全和人工智能。她的研究小组 NiSE（软件工程创新）进行了各种实证研究，并提出了新颖的自动化方法。https://people.cs.vt.edu/nm8247/  
  
  
  
> [安全学术圈招募队友-ing](http://mp.weixin.qq.com/s?__biz=MzU5MTM5MTQ2MA==&mid=2247484475&idx=1&sn=2c91c6a161d1c5bc3b424de3bccaaee0&chksm=fe2efbb0c95972a67513c3340c98e20c752ca06d8575838c1af65fc2d6ddebd7f486aa75f6c3&scene=21#wechat_redirect)  
 有兴趣加入学术圈的请联系   
**secdr#qq.com**  
  
  
  
