# MySQL

## 安装

https://blog.csdn.net/bobo553443/article/details/81383194

## 卸载

1. 去mysql安装目录找到my.ini文件， 复制 dataDir = "C:/***"， 数据的存储路径

2. 直接到控制面板卸载mysql

3. 路径输入：C:\ProgramData\MySQL回车，并删除MySQL文件夹

   

## 启动

1. 服务中找到它，启动

2. 命令窗口启动

   ```
   // 启动
   net start mysql
   // 关闭
   net stop mysql
   
   // 登录
   // 登录自己
   mysql -u用户名 -p密码
   mysql -uroot -p123456
   或者后面再秘闻输入密码，防止明文给别人看到
   mysql -uroot -p
   输入密码：***
   
   // 登录别的主机mysql
   mysql -h主机ip -u用户名 -p密码
   
   
   // 退出
   exit 或 quit
   ```

   

# SQL

## 概念

- Structured Query Language: 结构化查询语言
- 定义了所有操作关系型数据库的规则，每一种数据库操作方式有一些不一样的地方，我们称为“方言”

## 通用语法

1. SQL 语句可以单行或多行书写，以 ; 结束。

2. 可以使用空格和缩进来增强代码可读性。

3. SQL语句不区分大小写，但建议关键字用大写。

   ```
   SHOW DATABASES;  // 显示所有数据库
   ```

   

4. 三种注释

   ```
   //单行注释
   -- 注释内容
   # 注释内容
   #注释内容
   注意： -- 一定要有空格， #要不要空格都可以
   
   // 多行注释
   /* 注释内容 */
   ```

   

## SQL分类

1. DDL （数据定义语言）
   - 用来定义数据库对象： 数据库、表、列等；
   - 关键字：create、drop、alter等；

2. DML  （数据操作语言）
   - 用来对数据库中表的数据进行增删改；
   - 关键字：insert、delete、update等；

3. DQL  （数据库查询语言）
   - 用来查询数据库中表的记录；
   - 关键字： select、where等；

4. DCL  （数据库控制语言）

   - 用来定义数据库的访问权限和安全等级，及创建用户；

   - 关键字：GRANT、REVOKE等;

     

### DDL: 操作数据库、表

1. 操作数据库：CRUD

   - C（create）：创建

     ```
     * 创建数据库
     	create database 数据库名;
     * 如果不存在该数据库，再创建
     	create database if not exits 数据库名;
     * 创建数据库，并指定字符集
     	create datatbase 数据库名 character set 字符类型;
     * 练习： 如果不存在 db4 才创建， 并指定字符集是 gbk
     	create database if not exits bd4 character set gbk; 
     ```

   - R（Retrieve）：查询

     ```
     * 查询所有数据库的名称
     	show databases;
     * 查询某个数据库的字符集
     	show create database 数据库名称;
     ```

   - U（Update）： 修改

     ```
     * 修改数据库的字符集
     	alter datatbase 数据库名称 charater set 字符集;	
     ```

   - D（Delete）：删除

     ```
     * 删除数据库
     	drop database 数据库名;
     * 删除数据库，如果存在的话
     	drop database if exists 数据库名;
     ```

   - 使用数据库

     ```
     * 查询当前使用的数据库
     	select database();
     * 切换数据库
     	use 数据库名;
     ```

     

2. 操作表

   - C（Create）：创建

     ```
     * 创建表
     	create table 表名(
     		字段名1 数据类型,
     		字段名2 数据类型,
     		.
     		.
     		字段名n 数据类型
     	)
     * 复制表
     	create table 新表 like 被复制表;
     ```

   - R（Retrieve）：查询

     ```
     * 查询某个数据库中的所有表
     	show tables;
     * 查询某个表里面的结构
     	desc 表名;
     ```

   - U（Update）：修改

     ```
     * 修改表名
     	alter table 表名 rename to 新表名;
     * 修改表的字符集
     	alter table 表名 charater set 字符集;
     * 添加一列
     	alter table 表名 add 列名 数据类型;
     * 修改列名称	类型
     	alter table 表名 change 列名 新列名 新数据类型;
     	alter table 表名 modify 列名 新数据类型;
     ```

   - D（Delete）：删除

     ```
     * 删除表
     	drop table 表名;
     * 删除表，如果存在的话
     	drop table if exists 表名;
     ```

     

### DML：增删改表数据

1. 添加数据

   ```
   * 语法
   	insert into 表名(列1,, 列2, .....) values(值1, 值2, ....);
   * 注意
   1. 列名和值的顺序要求对应；
   2. 如果是每列的数据都添加，可以不写(列名);
   3. 除了数字类型，其他value都建议用""包裹;
   ```

2. 删除数据

   ```
   * 语法
   	delete from 表名 [where 条件];
   	delete from student where id=1231;
   * 注意
   1. 如果不加条件，将删除表的所有记录;
   2. 如果要删除表中的所有记录，应该先删除整个表，再新建新的表
    	TRUNCATE TABLE 表名;
   ```

3. 修改数据

   ```
   * 语法
   	update 表名 set 列名1 = 值1, 列名2 = 值2, .... [where 条件];
   * 注意
   where不加的话会修改所有记录，所以一般都是要加上的;
   ```

   

### DQL： 查询表中的记录

```
select * from 表名；
```

1. 语法

   ```
   select 
   	字段列表
   from 
   	表名列表
   where
   	条件列表
   group by
   	分组字段
   having
   	分组之后的条件
   order by
   	排序
   limit
   	分页限定
   ```

   

2. 基础查询

   ```
   1.多个字段查询
   select 字段1, 字段2, ... from 表名;
   
   2.去重
   distinct
   
   3.计算列
   select 字段1, 字段2, 字段1+字段2 from 表名;  // 一般只对数值类型计算，字符串拼接比较少
   ifnull(字段名, 替换后的值); // 解决null的问题
   
   4.起别名
   as 可以省略
   select 字段1 as 别名 from 表名;
   select 字段1 别名 from 表名;
   
   ```

   

3. 条件查询

   ```
   1.where子句后跟条件
   2.运算符
   >、 <、 <=、 >=、 =、 <>
   select * from 表名 WHERE age <= 20;
   
   between...and
   select * from 表名 WHERE age BETWEEN 20 AND 30;
   
   in(集合)
   select * from 表名 WHERE IN(20,25,30);
   
   like 模糊查询
   	占位符： _: 单个任意字符
   			%: 0-多个任意字符
   eg. 姓马的
   select * from 表名 WHERE name like "马%";
   eg. 第二个字是马的
   select * from 表名 WHERE name like "_马%";
   eg. 名字有马的
   select * from 表名 WHERE name like "%马%";
   
   is null
   select * from 表名 WHERE age IS NULL;
   
   and 或 &&
   select * from 表名 WHERE age < 20 AND age > 18;
   or 或 ||
   
   not 或 ！
   select * from 表名 WHERE age IS NOT NULL;
   
   ```

4. 排序查询

   ```
   * 语法
   	order by
   * 排序方式
   	默认升序： ASC
   	降序： DESC
   eg. 按照数学成绩排序，数学成绩一样的话按英语成绩排序
   select * from 表名 ORDER BY math ASC, english ASC;
   ```

5. 聚合函数（对一列数据的操作）

   ```
   1. count: 计算个数
   	一般选择非空的键： 主键
   	count(*)
   eg. select count(id) from student;
   
   2. max 计算最大值
   eg. select MAX(math) from studeng;
   
   3. min 最小值
   eg. select MIN(math) from studeng;
   
   4. sum 计算和
   eg. select SUM(math) from studeng;
   
   5. avg 平均值
   eg. select AVG(math) from studeng;
   
   
   ```

6. 分组查询

   ```
   1.语法
   	group by
   2.注意
   	* 分组之后，查询的字段只能是 分组字段、聚合函数
   eg. select sex, AVG(math), COUNT(math) from student GROUP BY sex;
   	* where和having的区别
   	1）where在分组前进行限定，如果不满足就不分组；having在分组后限定，如果不满足就不查询；
   	2）where后不跟聚合函数，having后可以跟聚合函数；
   eg. 按照性别分组，分别查询男女同学的数学平均分，分数低于70分不参与计算，分组之后人数要大于2的
   select sex, AVG(math), COUNT(id) from student where match > 70 GROUP BY sex having COUNT(id) > 2;
   ```

7. 分页查询

   ```
   * 语法
    limit 开始索引， 每页查询条数
    公式： 开始索引 = （当前的页数 -1） * 每页条数
   eg.
   select * from student LIMIT 开始索引, 结束索引;
   ```

   

## 约束

```
概念：
	对表中的数据进行限定，保证数据的正确性，有效性和完整性。
```



### 主键约束

```
1. 注意
 * 非空且唯一
 * 一张表只有一个主键
 * 主键就是表中记录的唯一标识
2. 在建表时， 添加主键约束
CREATE TABLE stu{
	id INT PRIMARY KEY,
	name VARCHAR(20)
};
3. 删除主键
ALTER TABLE stu DROP PRIMARY KEY;
4. 创建表后，再添加主键
ALTER TABLE stu MODIFY id INT PRIMARY KEY;
```



### 非空约束

```
* 语法： not null
1. 创建表时添加约束
create table stu{
	id INT,
	name VARCHAR(20) NOT NULL -- name为非空
};

2. 创建表后，再添加约束
ALTER TABLE stu modify name VARCHAR(20) NOT NULL;
```



### 唯一约束

```
* 语法： UNIQUE
1.注意，唯一约束允许有NULL，但是只能有一个NULL
2.创建表时添加唯一约束
CREATE TABLE stu{
	id INT,
	name VARCHAR(20) UNIQUE
}
3. 创建后设置唯一约束
ALTER TABLE stu MODIFY name VACHAR(20) UNIQUE;
```



### 外键约束

