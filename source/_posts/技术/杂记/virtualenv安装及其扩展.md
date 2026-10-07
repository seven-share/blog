---
title: virtualenv安装及其扩展
date: 2019-06-02 12:00
top: 0
categories: 
  - 技术
  - 杂记
---
### virtual安装及其扩展
---
#### 因为ubantu系统自带python2和python3，所以安装过程需要注意
- 安装pip，注意是安装python3-pip
- sudo pip3 install virtualenv
- sudo pip3 install virtualenvwrapper
    - 注意是使用sudo进行的pip安装
- virtualenvwrapper.sh这个文件位置，众说纷纭，我的是在/usr/local/bin（无法修改）
- ctrl+h可以将隐藏文件显示
- 创建保存虚拟环境的位置
    - mkdir $HOME/.virtualenvs
- 找到~/.bashrc并打开,插入如下代码块
    ```
        export WORKON_HOME=$HOME/.virtualenvs
        export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
        source /usr/local/bin/virtualenvwrapper.sh
    ``` 
    - 第一句是指定虚拟环境保存目录
    - 指定虚拟环境，默认使用python3
    - 指定运行文件
- 参考网页
    - [virtualenvwrapper错误解决](https://www.jianshu.com/p/842eced0df69)
    - [错误原因解释](http://blog.csdn.net/mbl114/article/details/78089741)

#### ubantu连接github
- [参考网址](https://www.jianshu.com/p/6c61b13e8bdb)