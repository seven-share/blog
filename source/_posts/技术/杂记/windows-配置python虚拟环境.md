---
title: windows-配置python虚拟环境
date: 2019-06-02 11:00
top: 0
categories: 
  - 技术
  - 杂记
---
### 在windows下配置python的虚拟运行环境
---
#### 因为版本问题，所以需要配置不同的虚拟环境，有很多需要关注的点，在这里做一个记录
- [主要参考](https://github.com/davidmarble/virtualenvwrapper-win)
- [安装参考](http://www.cnblogs.com/suke99/p/5355894.html)
- 注意点
    - WORK_HOME需要添加到系统变量中，这样每次添加的心得虚拟环境会放到该位置
        - 注意虚拟环境文件位置和自己创建的项目位置可以不同
    - 将python目录下的scripts文件夹加入PTAH环境变量中
        - 这样可以在任何地方使用virtualenvwrapper的命令行
        - [过程参考](https://jingyan.baidu.com/article/db55b6099d1e0d4ba30a2fc0.html)