---
title: git配置
date: 2019-06-02 14:00
top: 0
categories: 
  - 技术
  - 杂记
---
### git配置
---
1. 
<pre>
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
</pre>
2. 配置ssh key  
ssh-keygen -t rsa -C "your_email@example.com"

3. 复制.ssh/id_rsa.pub的内容到github
4. ssh -T git@github.com查看是否配置成功
