---
title: vscode调试flask
date: 2019-06-02 18:00
top: 0
categories: 
  - 技术
  - 杂记
---
### vscode 调试flask
---
#### 首先选择python的运行的虚拟环境
- ctrl+shift+p，找到python：选择解释器，选择自己所建的虚拟环境即可
#### 之后进入左侧调试选项，点击上面的设置按钮，自动生成launch.json，再选择flask的调试模式，点击运行即可
***
#### 问题，无法进入调试模式，一直处于开发模式
- 问题的解决看似简单，对于我来说，真的过程曲折，里面还真有很多我没接触到的知识
- 调试模式的好处：修改保存后，会自动重新启动服务器；返回的错误信息比较详细等
- 首先vscode启动flask的方式和我平常用的最多的app.run()是不一样的，它使用的是命令行启动的方式，所以app.run（debug=True）或者其他设置debug=True的方式启动debug是无效的

#### 解决 
- 先看一遍vscode启动flask的文档[vscode官方文档](https://code.visualstudio.com/docs/python/tutorial-flask#_create-and-run-a-minimal-flask-app)
- 再扫一遍flask文档就能看个大概，[flask官网的Quickstart](http://flask.pocoo.org/docs/1.0/quickstart/)，发现vscode的启动模式和flask文档里的命令行启动方式一样
- 所以进入调试模式的方法在flask的文档里已经给了，想进入调试模式首先需要export FLASK_ENV=development
- 但是点击启动debug按钮就直接运行了，没有给我设置变量的机会，笨一点的就是停止运行，设置变量，再运行debug，另外export或windows里的se环境变量t之后只会在这个cmd内有效，关闭或切换原本设置的都会变为无效
- 仔细观察debug点击后的按钮，观察其自动生成的执行代码，发现其会自动设置很多东西，例如python的运行的虚拟环境，设置FLASK_APP等
<pre>
set "FLASK_APP=app.py" && set "FLASK_ENV=development" &&
set "PYTHONIOENCODING=UTF-8" && set "PYTHONUNBUFFERED=1" && E:\pyhton\virtualenv\daily\Scripts\python.exe c:\Users\Administrator\.vscode\extensions\ms-python.python-2018.6.0\pythonFiles\PythonTools\visualstudio_py_launcher.py c:\Users\Administrator\Desktop\vsTry 4288 34806ad9-833a-4524-8cd6-18ca4aa74f14 RedirectOutput,RedirectOutput -m flask run "
</pre>
- 所以最好的方法就是将FLASK_ENV也让vscode启动时帮我们设置好，观察FLASK_APP的设置方式，同理即可推出,注意下面env里的设置
<pre>
{
    "name": "Python: Flask (0.11.x or later)",
    "type": "python",
    "request": "launch",
    "module": "flask",
    "env": {
        "FLASK_APP": "app.py",
        "FLASK_ENV":"development"
    },
    "args": [
        "run"
    ]
}
</pre>

#### 备注
- FLASK_ENV:development是flask1.0以上版本的设置方式,之前版本是FLASK_DEBUG=1
- 我开始使用了flask-script，原本应该python app.py runserver启动，vscode一直使用run启动，让我不理解，其实这是两种启动方式，一种是代码启动，一种是命令行启动
- 使用命令行启动的时候，也不会执行app.py里if __name__ == '__main__'后的代码，同时代码启动设置的debug=True在命令行启动时也无效
- 网上搜索的vscode的flask配置有很多项，应该是之前的，因为之前选择了python的虚拟环境，所以之后flask的配置少了很多项
- 命令行启动的时候，默认FLASK_APP是wsig.py或app.py，默认会寻找文件里面名字是app或application的应用接口，如果应用接口没找到，会再自动寻找工厂函数的名字create_app或make_app
    - 如果使用别的名字例如manage.py作为启动文件，配置里修改成"FLASK_APP": "manage.py"即可
    - 自定义应用接口名字方法：FLASK_APP=hello:app2


***
------------------------分割线----------------------
***
#### 说来惭愧，在自己胡乱点击过程中，发现了一个调试很好理解的调试flask的方法
#### 直接调试python文件
- 和命令行启动一样的方法，vscode可以调试python文件，但还差后面的runserver参数
- 所以我们加上参数就解决了(注意启动调试的时候，一定要选中flask的启动文件)
<pre>
{
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "args": [
                "runserver"
            ],
}
</pre>
- 这个方法真的很好理解，和我之前命令行启动flask一样
- 我发现和之前启动有个小小的不同，启动后
    - 之前启动显示Environment: developement，
    - 而直接用python启动文件，显示Environment: production
    - 下面的debug模式都是开启的，所以，目前我没发现什么不同

***
------------------------分割线----------------------
***
#### 实践的出真知
- 开启flask调试模式后，保存文件可以直接自己重新启动flask
- 后来想断点调试的时候，发现，如果开启flask调试模式，vscode就无法进入断点，很悲伤
- 不开启flask调试模式，vscode断点调试，命令行终端同样会提示错误信息
- 所以，绕了一圈，还是我目前还是不开启调试模式，用断点来调试
- 不知道有没有人尝试成功flask开启调试模式，同时进行断点调试，如果可以，求告知