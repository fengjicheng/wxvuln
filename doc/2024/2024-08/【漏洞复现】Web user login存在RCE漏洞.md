#  【漏洞复现】Web user login存在RCE漏洞   
原创 pitaya  Sec探索者   2024-08-16 08:03  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCkJrGicxw4mL5UYpL9RmBdKdft5iatHZicb4BrxO3ENyQOEVKKDeSwTG2Jw/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "组 61 拷贝 2.png")  
  
点击蓝字，关注Sec探索者，一起探索网络安全技术  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCkYN4sZibCVo6EFo0N9b7Kib4I4N6j6Y10tynLOdgov9ibUmaNwW5yeoCbQ/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCkhic5lbbPcpxTLtLccZ04WhwDotW7g2b3zBgZeS5uvFH4dxf0tj0Rutw/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCk524CiapZejYicic1Hf8LPt8qR893A3IP38J3NMmskDZjyqNkShewpibEfA/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任。如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
  
**01******  
  
**漏洞描述**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Melo944GVOJECe5vg2C5YWgpyo1D5bCkEPVCSE8TicyQLuettC2pcGgfe3PY8L2lHia8ZWLcNr1Fz7p3pb69Voow/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
Web user login存在RCE漏洞  
  
  
**02******  
  
**漏洞环境**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Melo944GVOJECe5vg2C5YWgpyo1D5bCkEPVCSE8TicyQLuettC2pcGgfe3PY8L2lHia8ZWLcNr1Fz7p3pb69Voow/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
FOFA语法：body="/images/raisecom/back.gif" && title=="Web user login"  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOI0HJaxTm6wJiaRSt5JapOfNx0JWEokBoq8DgHN9ZdXvSLnkJ3k3WYo3uczOG2cjicAmNpicLjWW9F4A/640?wx_fmt=png&from=appmsg "")  
  
  
**03******  
  
**漏洞复现**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Melo944GVOJECe5vg2C5YWgpyo1D5bCkEPVCSE8TicyQLuettC2pcGgfe3PY8L2lHia8ZWLcNr1Fz7p3pb69Voow/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
构造payload发送请求  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOI0HJaxTm6wJiaRSt5JapOfNCgVvBt12oEAibLhicfOeEMjZzIu4AcdZwypa0XzagpV4l8QUeOlmiah5w/640?wx_fmt=png&from=appmsg "")  
  
验证访问  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOI0HJaxTm6wJiaRSt5JapOfNZh2zCW33uAhmXw7sB2oa5S82H9syBia4ic7PprBGEqaiaia52O0WbnMwbw/640?wx_fmt=png&from=appmsg "")  
  
  
**04******  
  
**漏洞修复建议**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Melo944GVOJECe5vg2C5YWgpyo1D5bCkEPVCSE8TicyQLuettC2pcGgfe3PY8L2lHia8ZWLcNr1Fz7p3pb69Voow/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
建议联系厂商进行处理  
  
  
**05******  
  
**nuclei批量检测**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Melo944GVOJECe5vg2C5YWgpyo1D5bCkEPVCSE8TicyQLuettC2pcGgfe3PY8L2lHia8ZWLcNr1Fz7p3pb69Voow/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Melo944GVOI0HJaxTm6wJiaRSt5JapOfNu3xPgjRDYJn3BXmJJ7plt4pQlGuQYKT7jEHaRwicK4ib5qFxhqH0lRIg/640?wx_fmt=other&from=appmsg "")  
  
  
**06******  
  
**圈子服务介绍**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Melo944GVOJECe5vg2C5YWgpyo1D5bCkEPVCSE8TicyQLuettC2pcGgfe3PY8L2lHia8ZWLcNr1Fz7p3pb69Voow/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
无论你是新手还是行业老手，我们都致力于为你提供最新、最优质的安全资源和交流平台。立即加入Sec探索者专属的内部圈子，与我们一起探索安全领域的无尽可能性！以下是我们圈子为内部成员提供的核心服务：  
  
**1、最新漏洞情报：**  
提供最新的漏洞情报，第一时间复现和分析互联网暴露的重点高危漏洞，确保大家能第一时间掌握最新的漏洞动态  
  
**2、内部漏洞知识库：**  
漏洞知识库正在持续建设中，我们将会提供详细的漏洞复现教程和poc，帮助大家深入理解漏洞的工作原理和利用方式。所有漏洞都是我们自己成功复现才会添加到知识库中，减少大家试错时间成本，我们将收集相对完整的漏洞信息、漏洞描述、分析以及针对每个漏洞的利用方法和防护建议，致力于打造一个全面且专业的漏洞知识库。   
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOIhIqialXOQXWAkxoVr7t6q9eibfquDx4FZlibMakPt41tX7VsRibv1u4qDjTh4HrK1uYB8CrWlibAslgQ/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
**3、漏洞综合利用工具：**  
提供多种漏洞利用工具，这些工具经过内部严格测试和大家的反馈，确保其有效性和安全性。无论大家是在进行渗透测试、漏洞验证，还是其他安全研究，这些工具都能为大家提供强有力的支持。  
  
工具持续开发：https://www.yuque.com/charonlight/sec_explorer_tools  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Melo944GVOLytxy6Wrib0vcHkJC0yAnFtQkVhEUKibibbNFVZSVpcuTuxtic8TkoR5SU4Dd6GFkiaGPL15gMmE4ySPA/640?wx_fmt=jpeg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Melo944GVOLytxy6Wrib0vcHkJC0yAnFt25e1oxdKficQxJlMZPJV72ScrFBJTt8aSLsZYXlzIDjvBGfgRwzVCsA/640?wx_fmt=jpeg&from=appmsg "")  
  
**4****、更多资源分享：**  
我们会分享最新的安全资源，包括报告、工具、代码样本等，让大家始终能够获得前沿的信息和资源。随着成员人数的增加，我们提供的资源也会越来越多。我们将不断丰富和更新我们的服务内容，以满足大家不断增长的需求。   
  
加入内部圈子（好的资源永远都在小圈子里面），我们期待与大家一起共同成长，探索网络安全的无限可能！   
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Melo944GVOL3R3zZeiabfhvDjSZbDZPLic02GRl5xzsPotye6mcUcYjF8k4Yunc4RoUXiaxGZQya1iaTrYc19r1PTg/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
也欢迎各位师傅们进我们公开的交流群，有任何技术问题都可以在群内进行交流讨论，进群方式如下  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOIOyOhEZkrWlcianYlTNGEkfJEF11Yn0qaX7ZiaJb41nIVoBIRxkRbRZZKnibkOCtU3tz7rzKzhicWpibA/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Melo944GVOIoFwyAA0ACwDMtpMoricB1Kzzs8yibZoLEuzHJicbULwe1bsha86W6mr3D6XNcqzzkhMktNfY7CZNhg/640?wx_fmt=jpeg&from=appmsg "")  
  
**pitaya**  
  
  
微信号：  
Ff39959  
  
需要进群请加运营备注   
加群  
 哦  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOIOyOhEZkrWlcianYlTNGEkfPibJvibwNCZnfziaYeyoC2cwp0lAyGv1WEYKHgNhp0JdCj6motcL74vtA/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Melo944GVOIOyOhEZkrWlcianYlTNGEkfxOuWBhteCiaRdaHtePHhJMovro0Xia8kibfibrTD6TZPkMibu0pzvicIzHLg/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
  
