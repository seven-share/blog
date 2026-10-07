---
title: wamp环境下配置vscode调试php
date: 2019-06-02 9:00
top: 0
categories: 
  - 技术
  - 杂记
---
### wamp环境下配置vscode调试php
---
1. 点击进入localhost网页  
![](https://ae01.alicdn.com/kf/U7e47fbc851fa4d77aa7630980e16bd9fu.png)
2. 左下角，点击phpinfo（），右键选择网页源代码复制
3. [将复制的源代码粘贴到该网址](https://xdebug.org/wizard.php)（注意一定是网页源代码）
4. 他已经把结果都告诉你了  
![](https://ae01.alicdn.com/kf/U38cb2a1336b345f78dbe1304ea85d208w.png)
5. 下载该文件，放置到步骤2所说的指定目录，复制步骤3的配置代码到php.ini
6. php.ini有两种方法可以找到  
a. 直接在wamp图标下找到php->php.ini  
b. 打开wamp文件夹，在类似这样的目录下E:\wamp\bin\apache\apache2.4.17\bin找到php.ini文件，注意是在apache目录下找到
7. [xdebug]  
zend_extension ="E:/wamp/wamp/bin/php/php7.0.0/ext/php_xdebug-2.5.4-7.0-vc14-x86_64.dll"  
xdebug.remote_enable = On  
xdebug.remote_autostart=1  
xdebug.remote_port = 9009  
xdebug.profiler_enable = On  
xdebug.profiler_enable_trigger = off  
xdebug.profiler_output_name = cachegrind.out.%t.%p  
xdebug.profiler_output_dir ="E:/wamp/wamp/tmp"  
xdebug.show_local_vars=0  
**除了复制过来的，注意最终要的就是前面三行，开启xdebug和监听端口号**
8. wamp默认是9000，vscode默认也是9000，但是注意查看端口是否被占用  
[查询端口是否被占用的方法](http://jingyan.baidu.com/article/3c48dd34491d47e10be358b8.html)  
我因为查到9000被占用，所以改为9009
9. 之后打开vscode，可能会提示你无法解析php
![](https://ae01.alicdn.com/kf/U7b20e4a67a2c43ae90bba4dc49bb8f29n.png)  
将wamp中php下的php.exe写入配置  
**注意要将php.exe加入环境变量**
10. 点击调试，选择php语言，进入调试的配置项  
将端口改为和上面xdebug的端口号一致  
打断点，用网页访问该文件，即可看到断点。

