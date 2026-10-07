---
title: 部署flask-gunicorn-nginx-supervisor
date: 2019-06-02 16:00
top: 0
categories: 
  - 技术
  - 杂记
---
#### flask-gunicorn-ngnix-supervisor部署总结
---
#### 杂序
- 折腾了好多天，真的是有bug，查资料，问人，再查资料，再尝试，不断地尝试，逐步理解每一步，很感谢帮助过我的人，我把我的部署做个总结分享给大家希望可以作为其他的参考，刚从小白过来，可能有错的，希望指出

#### 环境和工具
- Ubuntu16.04，本地机部署(完全可以参考，同样可以部署到vps)  
- 使用flask-gunicorn-ngnix-supervisor  
- 数据库使用mysql
- flask源程序是使用的flask web开发那本书的程序

#### 主要参考
- [腾讯云部署Python3网站程序](https://zhuanlan.zhihu.com/p/28718966)
- [阿里云Python+Flask环境搭建](https://zhuanlan.zhihu.com/p/22126999)
- [Python日记——nginx+Gunicorn部署你的Flask项目](http://blog.csdn.net/qq_32198277/article/details/52432890)
- [ supervisor ERROR (spawn error)](http://blog.csdn.net/sinat_21302587/article/details/76836283)
- [使用 supervisor 管理进程(需要翻墙)](http://liyangliang.me/posts/2015/06/using-supervisor/)
- [Flask Gunicorn Supervisor Nginx 项目部署小总结(需要翻墙)----真的是很好的教程，还有下面的参考都可以看看](https://gist.github.com/binderclip/f6b6f5ed4d71fa64c7c5)
- [连接mysql参考](http://blog.csdn.net/werewolf_st/article/details/45933949)
- [Ubuntu16.04安装MySQL数据库和和可视化工具MySQL Workbench](http://blog.csdn.net/DreamHome_S/article/details/78137151)
- [ ubuntu安装mysql可视化工具MySQL-workbench及简单操作](http://blog.csdn.net/jgirl_333/article/details/48575281)
- [ubuntu下mysql可视化工具mysql-workbench简单操作](http://blog.csdn.net/YiLiang_/article/details/68923870)
- [Docker 添加用户到docker组](https://www.cnblogs.com/timelyxyz/p/6859822.html)
- [用docker部署flask+gunicorn+nginx](http://www.cnblogs.com/xuanmanstein/p/7692256.html)

####  源码部署
- 将源码放到指定的位置的方法多种多样，可以使用git，scp等
- 如果想部署到云端，使用ssh远程登录即可
- 数据库，我之前一直不明白数据库怎么办，可以在本地或虚拟主机直接安装mysql，启动，程序连接即可
- 开启mysql数据库，创建数据库，把数据库名字记下来
- 新建虚拟环境，可以使用pyenv，virtualenv等
- 进入虚拟环境后，pip install -r requirement.txt
- 修改数据库的地址，python manage.py deploy，进行数据库的迁移
- 然后直接python manage.py runserver确认程序运行无误

#### gunicorn，nginx，supervisor的关系
- 首先gunicorn自己就可以作为flask的服务器，因为对静态文件支持不好，所以使用nginx进行转发
- supervisor是一个进程管理工具，如果有程序停止，可以自动的重启，方便维护
- 请求从前端来了，首先是通过nginx，进行判断，如果访问的是gunicorn监听的端口，就会进行转发
- nginx可以进行分发，只监听80端口，然后再分给其他的进程监听的端口，同时可以直接访问静态文件

#### gunicorn部署
- 首先是安装gunicorn，我是通过pip安装到了虚拟环境
- 直接使用gunicorn命令行把程序运行起来
- 例子：gunicorn manage:app -b localhost:8000 -w 3
- 注意是在虚拟环境下运行，同时在自己的项目目录里，这样才能找到manage
- 我这里是使用的flask-scrip插件进行启动的，是manager.run()
- manage:app，manage是文件名字，app是create_app()对应的变量名字
- 如果没使用flask-scrip插件，直接是app.run()的话，则是文件名:app
- 首先确认这里是可以部署成功的，可以访问localhost：8000

#### nginx
- ngnix部署不难，安装，配置即可
- nginx -t，命令可以检查配置是否正确
- 如果配置成功，则直接访问localhost即可
- 注意上面是需要8000端口，这里已经不需要了
- 原理是http默认访问80端口，所以实际访问的是localhost：80
- nginx监听的是80端口，然后转发到了8000端口

#### supervisor
- 最后一步就是使用supervisor管理进程，管理gunicorn运行flask
- supervisord是server端，supervisorctl是client端
- supervisor网上说只能切换到python2.7环境去安装，不支持pyhton3
- 我直接使用的apt install supervisor安装的，感觉挺顺
- 然后就是进行配置，重新读取配置，重启
- 关键点在于command，注意gunicorn命令的启动路径，直接写成绝对路径
- 例子：/home/seven/Desktop/niceblog/niceblogEnv/bin/gunicorn manage:app -b localhost:8000 -w 3
- 启动后，大功告成

#### **完结**

##### __踩坑__
- sudo netstat -ntpl这个命令很好用，gunicorn，nginx或supervisor部署完后，可以查看是否启动监听了指定端口
- supervisorctl status查询状态
- supervisorctl tail programname stdout查询日志，这里的信息比较全面，error spwn，Exited too quickly这些都不明确，查看日志分析问题
- 如果是找不到module或application，supervisor的配置文件可能是directory 指定项目目录不对

##### __附__
##### nginx结构解读
- 首先看/etc/nginx里的nginx.conf，最后
    ```
    include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
- 所以我觉得把配置文件放入这两个文件夹都可以
- 同时原本的defualt文件需要备份，个人觉得直接剪切到cong.d即可，因为不会被引用
- defualt监听的是80端口，静态资源在root /var/www/html;所以一开始会是nginx默认网页

##### supervisor和gunicorn
- 原本gunicorn是可以有配置文件的，我看配置文件配置比较简单，就直接在supervisor的command里执行了
- 可以配置[inet_http_server]，这样可以直接在网页端查看错误信息
- [include]files = etc/supervisor/conf.d/*.conf，所以是引用的conf.d里的文件
- 在conf.d新建配置文件即可