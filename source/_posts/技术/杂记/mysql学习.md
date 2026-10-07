---
title: mysql学习
date: 2020-05-14
top: 0
categories: 
  - 技术
  - 杂记
---
### sql笔记
---
#### 感受
- 感觉学完不用，只是有了一个大概的轮廓和印象
- 很多再用都需要查，有待使用熟练吧

#### 安装
- 这次我使用的时zip安装，费了一些周折
- [参考教程](https://zhuanlan.zhihu.com/p/112765207)
- [mysql必知必会官网](https://forta.com/books/0672327120/)

#### 启动和关闭mysql服务器
- net start mysql
- net stop mysql

#### 连接
- mysql - u 用户名 -p
- system cls 清屏命令

#### show
- show databases，查看数据库
- use 数据库，使用某数据库
- show tables，查看数据表
- show columns from，查看表头

#### select
- select 列名 from 表
- select 列名，列名，列名 from 表
- select * from 表
- select distinct 列名，列名 from 表，只返回数据不同的行，distinct会对后面所有列生效
- select 列名 from 表 limite 5
- select 列名 from 表 limite 5，4，限制为5行，跳过前四行
- select 表名.列名 from 数据库.表名

#### 排序
- select 列名，列名，列名 from 表 order by 列名 desc/asc，默认升序asc，desc降序
- select 列名，列名，列名 from 表 order by 列名 limit 4，注意limit要在order by 之后

#### where
- select 列名 from 表 where 条件 order by 列名，注意order by需要在where之后
- <>不等于，between为在两者之间，包括头尾，between 5 and 10
- where 列名 is null 或者使用is not null
- and,or,not，in，优先计算and，强制某顺序可加括号
- 列名 in (值1，值2，值3)

#### 通配符
- select 列名，列名，列名 from 表 like 'xxx'
- % 任意数量的字符，_仅适配一个字符

#### 正则匹配
- select 列名，列名，列名 from 表 REGEXP '正则表达式'
- .xxx .匹配任意一个字符
- 1000|2000，或
- [123] ton,匹配1或2或3，与1|2|3 ton不一样，最后匹配的是或3 ton，可以考虑加括号
- [1-9],[a-z],匹配这之间的内容
- 转义匹配，\\.表示. ,\\为转义，具体详查
- 匹配字符类[:alnum:],任意字母和数字，[:alpha:]，任意字符
- 匹配多个实例
    - *，0个或多个
    - +，1个或多个
    - ?，0个或一个
    - {n}，匹配指定数目
    - {n,}，不少于指定数目的匹配
    - {n,m}，匹配数目范围n到m
    - 例如tasks+，匹配taskss，tasksss等
- ^，文本开头，$文本结尾，[[:<:]]词语的开头，[[:>:]]词语的结尾

#### 计算字段
- select 列名*列名 as xxx，可使用加减乘除
- concat()，字符串连接，trim去除空格，ltrim去除左空格，rtrim去除右空格

#### 数据处理的函数
- 文本处理，left(),length(),locate(),lower(),upper(),substring()等，具体详查
- 日期处理，adddate(),addtime(),date(),day(),month()等，具体详查

#### 汇总数据
- avg()，count()，max()，min()，sum()
- select avg(xxx) as xxx from 表

#### 汇总
- select xx from xx group by 表 having 条件 order by xx
- 注意顺序select from where group by having order by limit

#### 子查询
- 将两个sql语句进行嵌套
- select xxx，(sql语句，注意列名前加上表名，例子：orders.id,customers.id) from xx

#### 连结表
- select xxx,xxx,xxx from 表1，表2 where 表1.xx=表2.xx
- 如果不给where限制条件，表1将和表2的每一行进行联结
- 上面的操作为等值联结，又称内部联结
- 也可以，select xxx，xxx，xxx from 表1 inner join 表2 on 表1.xx=表2.xx
- 也可以多表连结

#### 高级联结
- 可以使用as给表命名别名
- 在同一个表内进行联结操作，使用别名比较方便明了
- 表.*，表示表中所有列
- 外部连结
    - select xxx，xxx，xxx from 表1 left/right join 表2 on 表1.xx=表2.xx
    - left join表示左边的不动，选择所有行，右边表进行条件匹配添加
    - right join表示右边不动，选择所有行，左边表进行条件匹配添加

#### 组合查询
- union 将多个select查询结果组合在一起，注意查询到列名需要一致
- 默认union会去重复，如果想全部保留，使用union all
- 可以在所有的组合查询最后使用order by 对上面所有的结果汇总生效

#### 全文本搜索
- 在创建表的时候就对其进行定义fulltext
- 使用方法：select xx from xx where match(列名) aganist('文本xxx')
- select xx from xx match(列名) aganist('文本xxx')，如果不加where则返回所有行里该文本出现的次数
- 查询拓展：select xx from xx match(列名) aganist('文本xxx' with query expantion)，将返回一些相关的行
- 布尔查询：select xx from xx match(列名) aganist('文本xxx -xxx' in boolean mode)
    - +表示必须存在，-表示不出现，<降低等级值，>增加等级值，()组成子表达式
    - ~取消一个词的排序值，*词尾通配符，""定义一个短语

#### 插入数据
- insert into xxx表 values(xxx,xxx,xxx)，和表中列的顺序一致，空的需要用null
- insert into xxx表(xxx,xxx,xxx) values(xxx,xxx,xxx)，前后相对应。为空的，前面不写即为默认值
- 插入多行
  - 
  <pre>
  insert into xxx表 values
                    (xxx,xxx,xxx),
                    (xxx,xxx,xxx);
  </pre>
  
- 插入检索数据
  - insert into xxx表(xxx,xxx,xxx) select xxx，xxx，xxx from xxx表，前后所需填写的值相对应即可，名字不一定相同

#### 更新和删除
- update xxx表 set xx=xxx，xx=xxx where xx=xx
- delete from xx表 where xx=xx
- 为了严谨，可以使用select提前进行查看，之后再更新或删除
- 如果没有where会更新所有行，慎重

#### 创建和操纵表
- <pre>
  create table 表名
    (
      列名 int not null auto_increment,
      列名 char(10) null default 1,
      primary key(列名，列名)
    )engine=innodb
  </pre>

- primary key的值一定唯一，如果主键是多列，则他们的组合值唯一
- 引擎innodb可靠的事务处理引擎，无法全文搜索，memory适合临时表，myisam，可全文搜索，不支持事务处理
- 更新表 alter table 表名 add 列名 char(10) null default 1,同上创建表格时列的定义方法
- alter table 表名 drop 列名，删除列
- 指定外键：alter table 表名 add foreign key (xx) reference 表名(列名)
- alter table 表名 add constraint 外键名字 foreign key (xx) reference 表名(列名)
- alter table 表名 drop foreign key 外键名，撤销外键
- 删除表：drop table 表名
- 重命名表：rename table 表名 to 表名

#### 视图
- 视图可以理解为一个虚拟的表
- 视图名字唯一，如果视图检索select中有order by则视图中的order by会被覆盖
- create view 视图名 as 查询语句，创建视图
- show create view viewname 查看创建视图
- drop view viewname 删除视图
- 跟新视图一般是先删除，后创建，或者使用create or replace view as 查询语句
- 视图一般用于检索后的展示，一般不会将视图再进行其他操作，例如分组，联结，子查询等

#### 使用储存过程
- 感觉就是函数
- 创建储存过程
    - create procedure 函数名()
    begin
    xxxxxx,执行过程
    end
- 删除储存过程，drop procedure 函数名
- 传递参数
    - create procedure 函数名(
        out 变量名1 decimal(8,2),
        in 变量名2 int
    )
    begin
        select xxx
        into 变量名1
        from 表名
    end
- decimal(8，2)表示一共8位，小数2位
- 变量需要使用@xxx进行标识
- 调用call 函数名(xxx,@xxx)
- in直接传递所需值就可以，out需要用@xx进行标识，方便后面使用
- select @xxx，@xxx  会将变量展示出来
- 前面-- 表示注释
- 展示储存过程 show create procedure 函数名
- 展示函数创建人、创建时间等 show procedure status like '函数名'

#### 游标
- 使用declare 游标名 cursor for 查询语句
- open 游标名，fetch 游标名 操作 例如into xxx
- close 游标名
- fetch 每次会取出一行

#### 触发器
- create trigger 触发器名 after insert on 表名
    for each row select new.xxx
- new 表示新生成的行，after insert中可以使用，即插入后，同时可以使用new进行数据更新
- before insert，插入前
- before delete，可以使用old访问未删除的行，alter delete
- before update，after update，old只读，new可用于数据更新

#### 事务处理
-  start transacition
- rollback，回退
- commit，提交
- savepoint 节点名 ，保留节点
- rollback to 节点名
- set autocommit=0取消自动提交

#### 管理用户
- use mysql
- select user from user，展示数据库的用户
- create user 用户名 identified by '密码'
- rename user 原用户名 to 新用户名
- drop user 用户名，删除用户
- show grants for 用户名，展示用户权限
- 赋予权限 grants xxx操作 on 数据库.* to 用户名，该数据的所有表的搜索权限
- revoke  xxx操作 on 数据库.* from 用户名，撤销权限
- set password for 用户名=password('新密码')