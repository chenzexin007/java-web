# Mybatis

## 1.入门

### 1.1概念

```
操作数据库的，对jdbc的封装。
使用xml配置：
	sqlMapConfig.xml: 配置 环境（全局配置入口，包括事务控制、数据源、映射sql的mapper）
	xxxMapper.xml: 配置对应模块的sql语句
自动封装数据到对象，不再需要在dao层手动处理数据
```



### 1.2例子

<img src="./1644992938(1).jpg" alt="1644992938(1)"  />

```
1. pom.xml中添加坐标
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.26</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.1</version>
    </dependency>
2. 创建account表
3. 创建account表对应的实体类
4. 创建mybatis的全局环境配置文件 sqlMapConfig.xml文件
5. 创建对应模块的xxxMapper.xml文件，录入sql语句，并指定resultType
6. 编写测试类
	* 获取核心配置文件
	* 创建工厂对象
	* 创建session会话对象
	* 通过session会话对象调用指定mapper的sql语句
	* 关闭session会话，释放资源
```

## 2.增删改

```
* 当改变表数据时，例如增删改，这个时候需要提交事务
* jdbc会自动提交事务
* mybatis不会自动提交事务，所以需要我们操作完后，手动提交事务
```

### 2.1增

```
1. xxxMapper.xml中添加 插入sql
	<!--    添加用户-->
	parameterType： 参数类型
	resultType： 结果类型
    <insert id="insterAccount" parameterType="cn.itcast.domain.Account">
        insert into account values(#{id}, #{name}, #{wallet})
    </insert>
  
 2. 测试类，记得提交事务
    @Test
    public void test2() throws IOException {
        Account a = new Account();
        // 这里我们的id是自增的，所以可以不用传
        a.setName("mybatis02");
        a.setWallet(200);

        // 获取核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
		// 创建工厂对象
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
		// 获取session绘画对象
        SqlSession sqlSession = build.openSession();

        // 具体操作（执行哪个mapper的sql）
        sqlSession.update("accountTable.insterAccount", a);

        // 提交事务
        sqlSession.commit();

        // 释放资源
        sqlSession.close();
    } 
```

### 2.2改、删

```
* 增删改的代码基本一直，所以这里只列出核心代码

1. 改
	<!--    修改用户-->
    <update id="updateAccount" parameterType="cn.itcast.domain.Account">
        update account set name=#{name}, wallet=#{wallet} where id=#{id}
    </update>
    
    Account a = new Account(10, "mybatis02", 2000);
    sqlSession.update("accountTable.updateAccount", a);
    
 2. 删
 	<!--    删除用户-->
    <delete id="deleteAccount" parameterType="java.lang.Integer">
        delete from account where id=#{id}
    </delete>
    
    sqlSession.delete("accountTable.deleteAccount", 10);
```

## 3. Mybatis核心配置文件描述

```
* 主要配置的层级关系
```



![1645001493(1)](./1645001493(1).jpg)

### 3.1environments

```
* 数据库的环境配置， 支持多环境配置
```



![1645001587(1)](./1645001587(1).jpg)

```
* 其中，事务管理器(transactionManger)类型有两种, 一般都是使用JDBC

* 其中，数据源(dataSource)类型有三种， 一般使用POOLED
```

![1645001702(1)](./1645001702(1).jpg)

### 3.2mapper标签

```
* 通过路径，加载映射文件，有多种加载路径方式
* 一般使用第一种或者最后一种
```

![1645002188(1)](./1645002188(1).jpg)

### 3.3properties

```
* 引入外部properties，解耦
```

![1645002756(1)](./1645002756(1).jpg)

### 3.4typeAliases

```
* 设置别名
```

![1645003394(1)](./1645003394(1).jpg)

### 3.5 typeHandlers类处理器

#### 3.5.1概念

```
* mybatis会使用内置的typeHandler处理我们 java->数据库、数据库->java 的数据类型转换
* 但内置的typeHandlers并不能满足我们的需求、场景，所以需要自定义typeHandlers
```

#### 3.5.2自定义typeHandler

```
步骤：
	第一步： 实现org.apache.ibatis.type.TypeHandler接口 或 继承org.apache.ibatis.type.BaseTypeHandler
	第二步： 实现 或 重写 里面的四个方法， 一个是 java->数据库  三个是 数据库->java
	第三步： 在mybatis的核心配置文件sqlMapConfig.xml中配置typeHandlers
	第四步： 写测试类测试
```

![1645086874(1)](./1645086874(1).jpg)

## 4.代理对象实现dao

### 4.1与传统对比

`代理对象`与`传统实现`的区别在于： 不再需要给dao的接口写**实现类**，mybatis会自动根据mapper命名空间把**mapper的sql**和**接口的方法**做映射。

### 4.2 代理实现的注意点

```
1. Mapper.xml文件中的namespace与mapper接口的全限命名相同
2. Mapper接口方法名和Mapper。xml中定义的每一个statement的id相同
3. Mapper接口方法的输入参数类型和Mapper。xml中定义的每个sql的parameterType的类型相同
4. Mapper接口方法的输出类型和Mapper。xml中定义的每个sql的resultType的类型相同
5. 写测试类
	与之前不用的是： 拿到sqlSession后，我们不再使用命名空间的方式调用sql，而是获取对应的mapper，然后再调用sql
	AccountInter mapper = sqlSession.getMapper(AccountInter.class);
	Account account = mapper.findById(2);
```

![d0389ba76eef7004181f6717128c4e1](./d0389ba76eef7004181f6717128c4e1.png)

### 4.3例子

![1645068055(1)](./1645068055(1).jpg)

## 5.动态sql

### 5.1场景

```
* 有些sql语句是不固定的，会根据搜索条件而改变。
* 例如条件搜索，有可能根据name或者name、age、id一起关联查找等
```

### 5.2例子

#### 5.2.1if控制

```
* 核心代码
    <select id="findByInfo" resultType="cn.itcast.domain.Account" parameterType="cn.itcast.domain.Account">
        select * from account
        <where>
            <if test="id!=0">
                and id=#{id}
            </if>
            <if test="name!=null">
                and name=#{name}
            </if>
            <if test="wallet">
                and wallet=#{wallet}
            </if>
        </where>
    </select>
* 根据if条件动态生成 sql语句
```



![1645078355(1)](./1645078355(1).jpg)

#### 5.2.2foreach遍历

```
* 核心代码
    <select id="findByIds" parameterType="list" resultType="cn.itcast.domain.Account">
        select * from account 
        <where>
            <foreach collection="list" open="id in (" close=")" separator="," item="id">
                #{id}
            </foreach>
        </where>
    </select>
* 根据foreach条件动态拼接生成 sql语句
```



![1645079687(1)](./1645079687(1).jpg)

#### 5.2.3sql片段抽取

```
* 将重复的sql语句抽出，作为公共代码引入，解耦
* 核心代码
	* 抽离
    <sql id="selectAccount">
        select  * from account
    </sql>	
    * 引入
    <select id="findById" parameterType="int" resultType="cn.itcast.domain.Account">
        <include refid="selectAccount"></include>
        where id=#{id}
    </select>
```

![1645080403(1)](./1645080403(1).jpg)




