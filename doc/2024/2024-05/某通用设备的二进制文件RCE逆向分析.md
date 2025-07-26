#  某通用设备的二进制文件RCE逆向分析   
原创 轩公子 & 华雄  轩公子谈技术   2024-05-11 15:56  
  
**点击上方蓝字关注我们**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8HT5F1sS4z6uEpdo8sb8vlVDU98ibVfdg9TxyytZ1MvteIwOpY8VjEhA/640?wx_fmt=png&from=appmsg "")  
  
**事情的起因源于这篇文章**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8mdCfz5hHibga5iblKSPD7SFCLRF1STJKbXdcCyQ5MhBmExSYNFK8ahRg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpzuCdmZhf0U3Q79qrezBDngWjExlSVJlEAYwqY0fv1ibZvCCCfAUibcJw/640?wx_fmt=png&from=appmsg "")  
  
点进去发现，是团队成员写的一篇文章，在逆向分析中，缺少点东西，于是搞一下 尝试分析一下。逆向咱不会，但咱有人啊 @Vlan911  
  
先check一下，得知该文件为arm架构 32位  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpghksPczOPaGYZynp1w2khTKiaibRp47XKfrZUDibfqiaaKmd8dGyWFGXvA/640?wx_fmt=png&from=appmsg "")  
  
打开虚拟机，找到32位的IDA，打开，不然位数不一样，不能查看伪代码。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpmF9C63qqbHyZxIyNO1MVZ87ibl2ibwJWIkXIYzSrxt57lkWJTkiavhwxA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8HT5F1sS4z6uEpdo8sb8vlVDU98ibVfdg9TxyytZ1MvteIwOpY8VjEhA/640?wx_fmt=png&from=appmsg "")  
  
**使用alt + t 全局搜索system**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8mdCfz5hHibga5iblKSPD7SFCLRF1STJKbXdcCyQ5MhBmExSYNFK8ahRg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpSW0Uq9PQqmbSo4CSNyw6wM19gvuurpqytM1HP789ibGiciaCudfqTNIibQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lp2f57ia8ibYsrPoUme4WacibFDBkMXZAMek0uQ0P3WMBhrxCOxPl4qLDZA/640?wx_fmt=png&from=appmsg "")  
  
经过排查，找到了触发函数位置 sub_1DBD2  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpTtF1sbHKRxYqtxFCPsJbIFXLooAecKDKPZDofwMuBic3QxWGshrvdLg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpSiaF0LPqM4C0QVE6ibXVibFtpPdknJvsgv9JlwnRn6MxEDkb4QXqXQrwA/640?wx_fmt=png&from=appmsg "")  
  
如果对一个二进制文件进行分析，挖掘其漏洞可能需要以下步骤：  
  
文件信息收集：了解二进制文件的基本信息，如文件类型、架构等。  
  
反汇编与代码分析：仔细分析反汇编后的代码，查找可能的命令注入点、不安全的函数调用等。  
  
字符串分析：查找可疑的字符串，如命令执行相关的指令或参数。  
  
函数调用跟踪：跟踪关键函数的调用，看它们是否与潜在的漏洞相关。  
  
环境变量检查：检查程序是否对环境变量进行了不当处理，可能导致命令执行漏洞。  
  
输入输出分析：深入研究程序的输入和输出处理，看是否存在漏洞可被利用。  
  
权限检查：确认程序在执行过程中是否存在权限提升的可能。  
  
漏洞利用尝试：根据分析结果，尝试构造特定的输入来触发可能的漏洞。  
  
静态分析：使用反汇编工具对二进制文件进行反汇编，查看代码逻辑，检查是否存在可疑的系统调用或命令执行相关的代码片段。  
  
动态分析：通过调试工具运行二进制文件，观察其在执行过程中的行为，特别是在输入特定数据时的反应。  
  
模糊测试：使用模糊测试工具向二进制文件输入各种异常或畸形的数据，观察是否引发异常或出现可疑的行为。  
  
检查权限和输入验证：查看程序是否对输入进行了充分的验证和权限控制，是否存在可被利用的漏洞。  
  
参考漏洞数据库：查阅相关的漏洞数据库，了解类似二进制文件中可能存在的已知漏洞。  
  
  
这种其实是在进行代码审计，结合java审计思路，就要去寻找sub_1DBD2 这个函数被谁调用了  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8HT5F1sS4z6uEpdo8sb8vlVDU98ibVfdg9TxyytZ1MvteIwOpY8VjEhA/640?wx_fmt=png&from=appmsg "")  
  
**全局搜索**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8mdCfz5hHibga5iblKSPD7SFCLRF1STJKbXdcCyQ5MhBmExSYNFK8ahRg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpo1VZLBXTzWZU7on30IzNfFLoibScToWuPXnrbq5YNXOmJcMlu4rpQJw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8HT5F1sS4z6uEpdo8sb8vlVDU98ibVfdg9TxyytZ1MvteIwOpY8VjEhA/640?wx_fmt=png&from=appmsg "")  
  
**在第一条被调用**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8mdCfz5hHibga5iblKSPD7SFCLRF1STJKbXdcCyQ5MhBmExSYNFK8ahRg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpyvOKcX1ITg3YWHibvv5icibV7O4YwhMhd0F8vrxZw1ThfrU2P8IErOaew/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lp6oLk56t1NAtFhekmgYOt5Be208FNpWibicfqxCew07sABxDfYbTwIEnQ/640?wx_fmt=png&from=appmsg "")  
  
第13行被调用了，且传入了v3 v2 v1三个参数，v3又在第10行，通过pingname进行赋值  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8HT5F1sS4z6uEpdo8sb8vlVDU98ibVfdg9TxyytZ1MvteIwOpY8VjEhA/640?wx_fmt=png&from=appmsg "")  
  
**现在需要知道，谁调用了sub_171CE**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8mdCfz5hHibga5iblKSPD7SFCLRF1STJKbXdcCyQ5MhBmExSYNFK8ahRg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpyibtGicKprYTLFqfcEibI0yYYia2Dp6WK3q0nV4HJhAD9UIgQaAOT21gEw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpAh8ZrT7af05eMlbRqzjsTB4prRy4Qxj1sHrlvW2PYxwuYPHhGbsRug/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8HT5F1sS4z6uEpdo8sb8vlVDU98ibVfdg9TxyytZ1MvteIwOpY8VjEhA/640?wx_fmt=png&from=appmsg "")  
  
**f5查看伪代码**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8mdCfz5hHibga5iblKSPD7SFCLRF1STJKbXdcCyQ5MhBmExSYNFK8ahRg/640?wx_fmt=other&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lplWWMz4m196TRz7krIGHFsLaRXib7IWymG09gn5MArvcOEXVThWhg9ibw/640?wx_fmt=png&from=appmsg "")  
  
这里写了if，如果v5的值为219，则调用sub_171CE。  
  
那么往上翻，看看v5怎么来的，由addr_value进行赋值  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpKqINmSS2NZJ7gmSAaDyu2Xr2Gn1hpO5JHWtKhbyEfeUQlANniaKY14A/640?wx_fmt=png&from=appmsg "")  
  
我们接着找，sub_11B80，发现在sub_23F60函数中的87行存在  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpgC6bnIKgzPsgtcHr6NH9w61zWiaewibXh0kkT1AMeKibe8ibh5UiaXYx6cQ/640?wx_fmt=png&from=appmsg "")  
  
然而sub_23F60，又在文件入口处存在，也就是main函数  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpZxbc6xuNsnoZpZXxLxY8nJ9uoGHz65zxuDYNSx2ZMXyeCMoNfRzpyw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpTUqQJrp5iboAbSUvcfTAYwARXhy8XBBpVs80xQvMydmBw639A6QHznw/640?wx_fmt=png&from=appmsg "")  
  
所以可得，这俩个参数以post方式进行传参  
  
  
```
POST /main/deal


addr_value=219&pingname=|whoami;
```  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8HT5F1sS4z6uEpdo8sb8vlVDU98ibVfdg9TxyytZ1MvteIwOpY8VjEhA/640?wx_fmt=png&from=appmsg "")  
  
**效果图如下**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn40p01o1ARheAXq2yL7nAI8mdCfz5hHibga5iblKSPD7SFCLRF1STJKbXdcCyQ5MhBmExSYNFK8ahRg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BAby4Fk1HQZ2ylLTwHHHYHt8LiajSE7lpBDSTWAkxwIrSnvy7UwmS5I2WiaxndO6CJab1rspvKX2FzDHVZcdRP2A/640?wx_fmt=png&from=appmsg "")  
  
  
  
