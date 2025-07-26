#  高危逻辑漏洞-Web3+Web2前端结合的ABI任意调用实现链上交易免gas   
 格格巫和蓝精灵   2025-03-26 22:25  
  
01  
  
—  
  
**前言**  
  
这次的漏洞案例依旧是Defi场景的漏洞，漏洞来源为测试环境的真实业务案例，本漏洞案例仅用于技术学习，禁止用于非法入侵行为，  
于此内容采取的任何行动均由个人全权负责, 使用这些信息应遵守适用的法律、法规和道德标准。本文首发于先知社区  
  
  
02  
  
—  
  
**DAPP和智能合约是如何交互的？**  
  
**1、从智能合约的代码部署到用户使用DAPP进行交互包含以下几个步骤：**  
1. 编写智能合约的代码(一般使用 Solidity)  
  
1. 编译智能合约的代码变成可在 EVM 上执行的 bytecode(binary code)，同时编译后还可以获得智能合约相关ABI(描述合约接口的 JSON 文件)  
  
1. 通过一个交易(transaction)部署  
智能合约，实际上是把 bytecode 存储在链上，并取得一个专属于这个合约的地址  
  
1. 如果程序员要写个APP调用这个智能合约，就需要把信息发送到这个合约的地址(一样的也是通过一个 transaction)。Ethereum 节点会根据输入的信息，选择要执行合约中的哪一个 function 和要输入的参数  
  
1. 要如何知道这这个智能合约提供哪些 function 以及应该要传入什么样的参数呢？这些信息就是记录在智能合约的 ABI里！  
  
  
ABI全称application binary interface  
是一个标准化的接口描述格式，它详细规定了智能合约对外提供的功能（函数）和数据结构（包括状态变量和事件），使得外部世界能够准确地与智能合约进行交互。  
 其结构类似于JSON。  
  
通过ABI，开发者可以编写前端应用或后台服务，来调用智能合约函数、读取状态变量和监听事件，从而实现对智能合约的完整交互。当智能合约被编译后，编译器会自动生成ABI文件，供其他应用程序使用。  
  
我们可以将它简单的理解成为是智能合约版本的WEB API接口。  
  
移动  
端，Web端都可以通过调用ABI来调用智能合约，也就是说ABI某种程度上是打通Web2和Web3的一种调用通道。  
  
      
以移动端的Dapp为例  
，移动端配置ABI文件，用户操作APP后，APP根据用户请求生成  
ABI编码，然后调用后端的服务器接口来对这段ABI编码进行解析，如果解析没有问题，就根据编码内容开始调用链上的方法，这个时候ABI编码在链上调用时被称为  
Calldata,  
。  
  
    一段时间后，交易上链成功，返回展示。  
  
      
在Web端也是一样的  
，只不过生成ABI编码的地方从APK变成了网页前端JS。  
  
  
**2、ABI和ABI编码**  
  
```
pragma solidity ^0.8.0;

contract SimpleContract {
    uint public myNumber;

    function setNumber(uint _number) public {
        myNumber = _number;
    }

    function getNumber() public view returns (uint) {
        return myNumber;
    }
}
```  
  
  
**3、获得ABI JSON**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISlYdXNBcxOUfTtTM2qZicUDrjSNiaCJoLNoBjHtJvyyOAweEnV2B9mHUQ/640?wx_fmt=png&from=appmsg "")  
  
  
这里我还用AI生成了上面代码的ABI，发现不一致，但是没关系我们只需要关注实际的function即可，因为在  
使用 Remix IDE 编译合约时，生成的 ABI 会包含声明的参数自动生成一个 getter 函数。  
  
****  
**4、生成ABI 编码（值等同于calldata）**  
  
https://abi.hashex.org  
  
生成的内容为0x + 函数选择器哈希的前4个字节 + 0填充  +   
参数值  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISwOoQMXbQu1ERDJwuyVFljDGH1NkMBOjGRThoh0pYq4FTxeDwuBfJww/640?wx_fmt=png&from=appmsg "")  
  
  
**5、ABI编码如何转换为可读**  
  
https://abi.yiew.cn  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISibT39yk1oaVPQJD6TnU0BEPs5YG7acRZM36C7zHjStOb2d4O1KrkEmg/640?wx_fmt=png&from=appmsg "")  
  
h  
ttps://calldata.swiss-knife.xyz/decoder  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISVIW5c0OwWwEoZhAnLHTibPHL7k69OtlpPpXXNQvAfUH6D2FoI6iceSRQ/640?wx_fmt=png&from=appmsg "")  
  
**6、快速获取交易的calldata(ABI编码值)**  
  
直接去区块链浏览器找到合约地址，然后再区块链浏览器发送请求，抓包获取如：  
https://etherscan.io/token/0x6982508145454ce325ddbe47a25d4ec3d2311933#readContract（PEPE代币官方地址）  
  
**查看别人的**  
  
****  
随便点一条  
Transaction查看即可  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISKbY94frIFH0w95Y1T6LarKOF38A9Dq6H9KLn0ppQclicEEErSg4QwoA/640?wx_fmt=png&from=appmsg "")  
  
**抓包生成**  
  
  
注意看，这里我们获取了data和to，这两个值非常关键，这是所有DAPP调合约的方式。  
  
data就是我们上面花了很大篇幅说的abi编码（数  
值  
同于calldata），to则为调用的目标合约地址。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib436zicnTxRiaEwdA3SM7QrG1ibx7UlWU6TNzlT17yPfNaC5sjTyNWouRTSA/640?wx_fmt=png&from=appmsg "")  
  
      
  
看到etherscan里的Query按钮点击后查询，应该也可以非常直观的感受到abi编码(calldata)的作用了和它是怎么生成的了。  
  
    花了非常多的篇幅解释完了ABI是什么，接下来进入漏洞环节。  
  
    以下如果出现calldata和ABI编码在我这都代表的是一个意思。  
  
  
  
03  
  
—  
  
**漏洞详情**  
  
网站支持通过申购和赎回来购买Token，这里的Token就像基金份额一样，只不过申购的钱从支付宝上的余额变成了虚拟稳定币USDC，构建一个以真实世界为基准的链上RWA项目。  
  
以下测试环境均为sepolia测试链环境，但不影响漏洞的真实性和具备的危害性。  
  
  
**1、点击申购**  
  
   
   这里使用的是magic Link，magic Link是一款无助记词管理的钱包，用户只需使用邮箱登录后，就可以创造一个web3钱包地址与合约其进行交互，交易过程产生的gas费由项目方承担。项目方引入magic钱包主要是为了降低使用门槛，让更多的用于参与交易和项目玩法。  
  
    
  当然项目也支持用户使用自己的钱包，但是这样交易的gas fee就得自己出啦。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISFWHFw9OSgd6SobzdA89iajBaHVkqB551uOZwedOadSBnBc7SIIXLibZw/640?wx_fmt=png&from=appmsg "")  
  
  
在成功注册Magic钱包后，系统会给我们自动分配一个钱包地址  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPnwEsmFKzW0bJR3dj3JISKrRyYWcHP1hn6yfD6ic1CgH5cMAg7D3NB0Cw/640?wx_fmt=png&from=appmsg "")  
  
  
  
**2、网站打断点**  
  
在点击申购之前，对网站进行断点  
(如何找到这个断点？console控制台打印了一些信息，从控制台进入然后寻找大概的范围，然后最终确定的几个断点，多点击几次申购，和取消申购以确认断点范围）  
  
  
**Step1:**  
  
例如如下控制台出现了  
methods, params，我们就可以通过这个地方去定位JS  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISftMsagiaFBWNUbAPVb4269zNDPVPp6tO2uWBg4iboGLCX9QOibeQFCVRQ/640?wx_fmt=png&from=appmsg "")  
  
最终定位到console.log("[ methods, params ] >", i, u)  
  
多次打断点调试这部分的数据  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43dE9KK0hkZXQ1hqM08MoAP1UXQGG04gGibTjH6cMG5IvySzUAknTdWZw/640?wx_fmt=png&from=appmsg "")  
  
这里能得到i和u这两个值  
  
i=subscribe（申购）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43CiaMRHRRh5fq78pIicd6zicyKpk4cBVF1SPBv3qMIfibRIV1jmfCicfy3aQ/640?wx_fmt=png&from=appmsg "")  
  
u=如下数组（现在我们还不知道u是什么意思）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43cRdSwSCG5VTLlMyy9YbWXld7ALibMniboMZT3PQNNorXiaszrLeDI14yA/640?wx_fmt=png&from=appmsg "")  
  
  
于是我们看看前面的JS代码参数有没有提供什么信息，发现function e(t)提供了说明，调用的是   
subscribe(address,address,uint256,uint256)  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43Zx8beiaeqkZXbFcE8ibL4aXibiaPic7vib2PzK6S6qfZGZZ9dyArpMcpI9xQ/640?wx_fmt=png&from=appmsg "")  
  
  
**Step2:**  
  
让我们按F8接着往下看   
E = O.encodeFunctionData(i, u),  
这里太长我就不截全了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43dtibb1RG1eQ3Gd4V5FJQpI4GdC9ib55oD2YHAPLiaVKIC9pOkftBnCERA/640?wx_fmt=png&from=appmsg "")  
  
这就是  
subscribe(address,address,uint256,uint256)的ABI编码和传参  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43URkLWuPIeQVLnOygBy2s1SbHN19AV0YURy0Uojvdia5IYjicWoBeq2pg/640?wx_fmt=png&from=appmsg "")  
  
四个参数的意思分别为  
- 申购币种地址（项目方发布的，类似于你要选哪个基金）  
  
- 交易用户使用的稳定币token合约地址（USDC）  
  
- 转账金额（申购金额）  
  
- 时间戳  
  
手动生成编码，对比一致。  
  
但是具体这个申购合约里具体执行了什么代码，这点我们是不得而知的。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPn9HW5ajYoeJ4iat5YONyqaiavK9BJrx3HSRricTz8RVllLBZOmqfmDstNw/640?wx_fmt=png&from=appmsg "")  
  
DAPP调用合约的关键data字段如何获取现在我们大概清楚了。  
  
理论上只要替换该data，我们就能调用该合约的任意方法  
，但是具体能否调用成功还是得看这个方法是怎么写的。  
  
  
**Step3:**  
  
继续观察上面的JS，发现to字段也给了我们，  
前往查看这个地址，然后查看这个地址有多少USDC（测试币）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib430mwPM6ydMubV7lmmLugk48tabPnHIGNjSmvHp1ld4urZbH3eSXIlmQ/640?wx_fmt=png&from=appmsg "")  
  
  
可以看到USDC（测试）有非常多，因此大概可以推测该地址应该为项目方的钱包。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43dPqjqib2weaBucByIfSvDffwMxs40C9bd01H060uRNxEbq7YVKQ82VA/640?wx_fmt=png&from=appmsg "")  
  
  
上面的这个0x9744c开头的地址我们称为  
交易合约地址  
  
这个交易合约地址上也部署着subscribe、redeem、issuerReunFund等方法  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPnwq7oxFZq5rmfIwdldjnibpxJnMfnLXutic1bx7FLAsZUtzwm55vZuf6Q/640?wx_fmt=png&from=appmsg "")  
  
  
**Step4:**  
  
继续往下走，多断几个点  
  
发现存在requestSafeSignTx方法，会将chainID,to,data,nonce进行打包，进行签名返回  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPniaRBTuE6AwiaVgf1F35bpYZMj6LjBvCV04Q3FJkhicicJnzQt0swj9RP2A/640?wx_fmt=png&from=appmsg "")  
  
  
这里的to和data就是上面step3和step2所得出来的值  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPnKD5y6ZUdyD6LPWDz4T84Deq9zXHmO4N6lAgD98Sr8nlxsvvhAAakgQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPnn4icsymXYkS78V7AQQahWGJQCpRFWTuWzWQjHaq6uicg5FcRY4liasEHw/640?wx_fmt=png&from=appmsg "")  
  
  
所以在经过上面生成了第一步的abi的to和data后，网站会通过  
requestSafeSignTx方法进行签名，然后调用send方法  
  
此时会通过api/xxx/xxx/transaction/send接口将其发送到后端处理，然后再进行上链，可往下看。  
  
  
  
**Step5:**  
  
上面的过程都是前端JS生成的，生成数据后请求API发送到后端进行解析，猜测项目方会先进行检测是否符合格式，数据符合格式，则发起链上交易，调用合约代码，然后返回是否上链成功  
  
接口请求如下：  
```
POST /api/xxx/xxx/transaction/send HTTP/2
Host: User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36
Content-Type: application/json;charset=UTF-8

{"chainId":"11155111","data":"0x6a7612020000000000000000000000009744c11004a16cfee68a45a69dde7913e098f4f500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000847fb801be0000000000000000000000007586dd23244d51ffe7d30b6cb839bb1718fb5ced000000000000000000000000c40fa5d8cf408baa63019137033d2698377fb2430000000000000000000000000000000000000000000000000000000000d59f800000000000000000000000000000000000000000000000000000000066f61690000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000041fbe678c6f71241843079f6f32ec6fb2d9036852ee826405b859fd0b90255c08b56e5e06cabad9a075de14a3a1e2973ebb1fe737690fe15cc1f3fbbad9e5618e51c00000000000000000000000000000000000000000000000000000000000000","nonce":32}
```  
  
经过abi解析后内容  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPn6O5hT1cesRCBII7PY7S9icHf7yslaXfRDWuFvyo7OTksACRSLlRic95Q/640?wx_fmt=png&from=appmsg "")  
  
我们重点关注4个字段，  
可以看到实际上传输的内容和我们想象的有一些出入。  
- **‍data**  
  
- **signatures**  
  
- **to**  
  
- **method**  
  
execTransaction为调用的方法，从名字可以得知字面意思为"执行转账"  
  
to 这里是项目方的钱包（接收USDC）  
  
data就是我们上面step2的  
subscribe的abi编码和传参（支付USDC）  
  
signatures为  
execTransaction需要的函数，猜测是  
requestSafeSignTxsheng所生成  
  
  
**step6:**  
  
实际上后台解析后调用的是一个safe多签钱包的中转合约，通过在交易的外部添加  
execTransaction来传递实际的交易  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPncsXYb3qXQCdrTNT6nZ15qlcHkbxxjOn3nnIbLVribeKxbqsWG3iaCxFQ/640?wx_fmt=png&from=appmsg "")  
  
理论上我们可以通过  
burp拦截该接口进行修改值data值，但是这里存在sginarture，也即签名，理论上我们只要破解了这个signature  
也能达到实现任意合约调用的作用，但是这里通过JS调用则更为简单。  
  
**一般的调用:**  
  
{  
  
  "to": "0x9744c11004a16............",    //合约地址  
  
  "data": "0x12345678..."    // 这是调用某个方法的ABI编码数据  
  
}  
  
  
**现在的调用:**  
  
{  
  
  "to": "项目方后台定义的地址，我们暂时不知",  
  
  "data": {  
  
    "exectrasation": {  
  
      "data": "0x12345678...",  // 这是调用subscribe方法的ABI编码数据  
  
                                { address1,  
address2,  
uint256,uint256 }  
  
      "to": "0x9744c11004a16cfee68a45a69dde7913e098f4f5"  
  
    }  
  
  }  
  
}  
  
  
**流程总结：**  
- 钱包A（用户）调用项目方后台定义的合约地址B的  
exectrasation方法，来进行转账，  
  
- exectransation会调用  
的交易合约地址上的  
subscribe方法，  
之后扣除钱包A的钱  
  
- 然后通过exectransation将钱转入  
0x9744c  
交易  
合约地址  
  
**step7:**  
  
上面申购操作完成后，系统会返回给我们链上的hash，查看链上交易以验证我们的猜想。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPnWDva0j9MbPTHzu8GeK4ribWEz2uFTccfC6nBDiaN2N7Bh9SBoAeJ5UiaA/640?wx_fmt=jpeg "")  
  
  
1、0xeeB7B5b7...  调用了    
0xb891626e...    
的 Exec Transaction 方法  
  
2、0xb91627e... 向 0x9744C110.. 转账14个USDC  
  
  
现在我们终于知道了上面说的这个to地址原来是我们自己的钱包，Magic钱包分配给我们的钱包是一个safe多签钱包。  
  
整理完了整个交易的流程后，我们便可开始漏洞利用了  
  
  
  
04  
  
—  
  
**漏洞利用**  
  
  
**1、篡改ABI调用任意合约的mint方法**  
  
**第一步：将data(E)值修改为如下**  
  
0x40c10f19000000000000000000000000176791d147bef3f62dadde535604f339a1758e4400000000000000000000000000000000000000000000000000000000000f4240  
  
该abi编码解析值如下，意思是调用xxx地址上的mint合约方法，来mint一个币。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib434tQrmo1lNKicOibl6b5MwXFBuEKUYMrADnQqDqtRxPYMLgLo6XXXamMA/640?wx_fmt=jpeg&from=appmsg "")  
  
如下所示，进行替换calldata值  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43H84iayibz0f0R5JcIVFicRYgic1WM3ABmKuUzfez8ONOh4qdUjhKcGXI8Q/640?wx_fmt=png&from=appmsg "")  
  
**第二步：将to修改为其他地址**  
  
——0x9744c11004a16cfee68a45a69dde7913e098f4f5修改为0x8e913be54763a751942a1af9d2f3d04a2d8078e3（某个ERC20token的合约地址）  
  
然后一直按F8发送交易，可以看到我们成功调用了其他合约的mint方法，成功的给某个地址生成了1个币  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPnxUibMTSFZbSEV4xRzVdW5Wvd2tgRYedKTV0opzQVwCM5Fdcs8t1h9bA/640?wx_fmt=png&from=appmsg "")  
  
  
可以查看下图的解析  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVQEN15xpC4wWCOicLRqWstPnXOosXmYcC7ZgHtRWgduhX5k7tN0iaaWAdyUyJEicq848sKm2IGyiazCPg/640?wx_fmt=png&from=appmsg "")  
  
  
**2、漏洞利用：篡改ABI调用合约的burn方法**  
  
该合约是ERC20的token合约地址(测试ERC TOKEN)，也是上面项目申购过程中所使用的代币，直接调用它的burn方法(可以让所有人的钱都直接消失)  
  
测试：让自己的钱burn掉，将上面第一步的data(E)值修改为如下  
  
0x9dc29fac000000000000000000000000b891626e1b89c572d2a9d3324473d0b280f45cad0000000000000000000000000000000000000000000000000000000000989680  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVStFJC6ZDEK2MMiaiaiaenpmISrOicE9v4Qtia3v8CcA4qwHCXTTM3t8icxqicOFlunaUaQQVcUVv5u8ZkMQ/640?wx_fmt=png&from=appmsg "")  
  
此时调用了合约里的burn方法，然后燃烧掉了地址为  
  
0xb891626e1b89c572d2a9d3324473d0b280f45cad  
  
  
10个USDC代币  
  
可以看到向address(0)地址转账了10个代币  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZmiaibBst1BVTKv6kePLQ23omMt97o3ib43m7SHAzVBqdELwUrqib0tibuibAEcXNRGsU1wsqL9BuSBUo1sYUZ9bF1icQ/640?wx_fmt=png&from=appmsg "")  
  
  
注意：上面的Burn和mint都基于ERC20的标准，ERC标准里有burn和mint方法，且基本方法的参数都是一致的  
  
  
  
**3、这里为什么可以调用burn和mint方法？**  
  
原因是这里我使用的是测试链上的环境，测试链上的burn及mint都没有做太多限制，但是如果在是真实链上的利用的话是调用一些公共方法，例如tranform（交易）等任意的Public方法。  
  
  
  
**4、最后我们来总结一下流程：**  
  
****  
1、用户点击申购。****  
  
    2、项目方的EOA钱包会发起调用Magic给用户分配的safe钱包的transation方法。  
  
    3、safe钱包调用exectransation进行中转，加签安全调用申购方法，拿到申购的钱然后转给项目方交易合约。  
  
    4、之后用户钱包扣款，项目方收款，项目方之后再进行线下申购（类似基金），一笔正常的交易完成。  
  
  
  
05  
  
—  
  
**如何修复该问题**  
  
  
      
梳理完逻辑会发现这个漏洞是一个表面逻辑非常简单的漏洞，主要的原因就在于后端没有校验调用的合约是否符合规则。  
  
  
    上面说项目方使用Magic钱包来方便用户提供交易，交易所产生的gasfee就由项目方出了，攻击者可以修改to和date来选择任何合约和方法，利用这个漏洞来帮助任意人来发起任意交易（除去一些有特殊限制的方法  
），众所周知，链上交易的gas费还是比较高的。  
  
  
    开发只需要设置一个白名单，存放合约地址。后端解析处理禁止调用非白名单地址内合约地址，这样就能避免攻击者利用该漏洞来调用任意的合约方法进行发起任意交易。  
  
