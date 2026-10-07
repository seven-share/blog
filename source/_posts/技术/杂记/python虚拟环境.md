---
title: python虚拟环境搭建
date: 2019-06-03
top: 0
categories: 
  - 技术
  - 杂记
---
### python和vscode的环境搭建
---
#### 安装python
- 这次轻车熟路，真的顺很多
- 安装的时候注意选择将python添加至PATH

#### win10 虚拟环境的搭建
- pip install virtualenv
- pip install virtualenvwrapper-win
- 设置环境变量 WORKON_HOME 指定虚拟环境的保存路径
- 将python目录下的scripts文件夹加入PTAH环境变量中
  - 这样可以在任何地方使用virtualenvwrapper的命令行

#### [参考网址](https://github.com/seven-share/dailyCode/blob/master/win-%E9%85%8D%E7%BD%AEpython%E8%99%9A%E6%8B%9F%E7%8E%AF%E5%A2%83.md)

#### vscode的命令行修改
- 尴尬，我也不知道为什么win10的命令行cmd在vscode里开始会出现空行
- powershell，用起来不顺
- 于是想把cmder嵌入，方法如下
---
- "terminal.integrated.shell.windows": "cmd.exe",
- "terminal.integrated.env.windows": {"CMDER_ROOT": "[cmder_root]"},
- "terminal.integrated.shellArgs.windows": ["/k", "[cmder_root]\\vendor\\init.bat"],
- [cmder_root]换成cmder的安装路径
- [参考网址](https://blog.csdn.net/leonhe27/article/details/81210000)

#### 虚拟环境管理，为什么没选pipenv
- 咱这行，用熟不用生，哈哈，开个玩笑
- 网上对于pipenv好评如潮，肯定是好的
- 有一个缺点，虚拟环境对应者项目，而我不想每次做一个小demo都新创建一个虚拟环境
- virtualenvwrapper可以多个项目使用同一个环境