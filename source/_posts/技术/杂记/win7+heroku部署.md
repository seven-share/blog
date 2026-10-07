---
title: win7+heroku部署
date: 2019-06-02 13:00
top: 0
categories: 
  - 技术
  - 杂记
---
### win7+heroku部署
---
#### 程序是根据flask web开发，过程较为精简，总体和书上一样，本文仅是注意点和修改
#### 目标：部署上线
#### 1. 修改manage.py，方便进行一句话部署
    @manager.command
    def deploy():
        # run deployment tasks
        from flask_migrate import upgrade,migrate
        migrate()
        upgrade()
        Role.insert_roles()
        User.add_self_follows()
- 注意需要使用migrate

#### 2. heroku准备
- 注册需要使用gmail
- 登录前，将文件夹托管给本地的git，login之后直接即可push
    - 注意设置.gitignore文件
    <pre>
        __pycache__
        .vscode
        *.sqlite
    </pre>
    - ### 注意migrations文件，version里面清空，但是加上一个.gitkeep文件，这样在上传的时候version才会有，否则git空文件夹无法上传
- 第一次创建的postgres数据库无需将数据库设为主数据库，默认即是
- 创建命令变了
    - heroku addons:add heroku-postgresql:hobby-dev

#### 3. 进一步配置
- 配置heroku服务器所需配置
- 添加选择heroku配置的环境变量
- 配置电子邮件和密码的环境变量
- 查看环境变量
<pre>
heroku config
</pre>
- 向requirements.txt添加部署所需的依赖
- 添加Procfile
    - 注意manage是启动文件的名字，app是flask实例化的名字
<pre>
web：gunicorn manage：app
</pre>
- 启用flask-sslify，注意配置变化为
<pre>
if app.config['SSL_REDIRECT']:
        from flask_sslify import SSLify
        sslify = SSLify(app)
</pre>
- 好像源代码里面反了  
配置文件里开发模式，SSL_DISABLE=False  
heroku部署模式里是,SSL_DISABLE=True

- 代理服务器支持改为ssl，https

#### 4. 上传和配置
- git push heroku master
- 可以git clone下来，查看上传文件
- heroku run python manage.py deploy
- 可以进入shell，伪造一些数据
<pre>
heroku run python manage.py sehll
fake.users()
fake.posts()
注意数据库无法rollback，所以会有报错，忽略就好
</pre>

- 可以使用pgadmin进行连接数据库，查看数据库情况

#### migrat失败
- 在数据库文件里有一个表是记录上传的过程的表，和migration/versions里的文件相对应，所以上传之前把version里的文件全部删除
- 如果heroku数据库已经记录迁移过程，versioins文件又被删除，只能reset数据库，在heroku网页端可以操作



