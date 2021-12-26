# 1.Spring

## 1.Spring简介

```
* 是分层的JAVA SE/EE应用全栈轻量级框架
* IOC: 控制反转
* AOP: 面向切面编程
* 提供了展现层Spring MVC 和 持久层Spring JDBCTemplate 以及 业务层事务管理
* 整合开源师姐总舵注明的第三方框架和类库，逐渐成为使用最多的Java EE 企业应用开源框架
```

## 2.Spring快速入门

![1640314531(1)](./1640314531(1).jpg)

```
1. 导入Spring开发的基本包坐标
* pom.xml中配置依赖坐标
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.13</version>
    </dependency>
    
2. 编写Dao接口和实现类
    public interface UserDao {
        public void say();
    }

3. 创建Spring核心配置文件
resources下创建Spring config文件， 命名随意，但是一般叫applicationContext.xml

4. 在Spring配置文件中配置UserDaoImpl
	// Dao接口的实现类UserDaoImpl，配置成bean
	<bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl"></bean>
5. 使用Spring的API获取Bean实例
	// 测试类中使用bean
    public class UserDaoDemo {
        public static void main(String[] args) {
            ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
            UserDao user = (UserDao) app.getBean("userDao");
            user.say();
        }
    }
```

## 3.Spring配置文件

### 3.1Bean配置

```
* 用于配置对象交由Spring来创建
* 默认情况下它调用的是类中的无参构造方法

基本属性：
	* id： Bean实例在Spring容器中的唯一标识
	* class： Bean的全限定名称
```

### 3.2Bean标签范围配置

```
* scope: 指对象的作用范围，取值如下
1. singleton: 默认值，单例的
Bean实例化时机： 当Spring核心配置文件被加载时
对象销毁时机： 当应用卸载、销毁容器是，对象被销毁

2. prototype: 多例的
Bean实例化时机：当调用getBean()方法时实例化Bean
对象销毁时机：当对象长期不使用，被java的垃圾回收器回收

3. request: web项目中，Spring创建一个Bean对象，将对象存入到request域中
4. session: web项目中，Spring创建一个Bean对象，将对象存入到session域中
5. global session: web项目中，应用Portlet环境，如果没有portlet环境那么globalSession相当于session

```

### 3.3Bean声明周期配置

```
* init-method: 指定类中的初始化方法名称

* destory-method: 指定类中销毁的方法名称
eg. 
// 在实现类中定义初始化、销毁方法实现。在.xml文件的bean中配置对应初始化和销毁名称的方法名
<bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl" scope="singleton" init-method="init" destroy-method="destory"></bean>

```

### 3.4Bean实例三种方式

```
* 无参构造方法实例化  最重要
	<bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl"></bean>

* 工厂静态方法实例化  了解
    public class StaticFactory {
        public static UserDaoImpl getUserDao(){
            return new UserDaoImpl();
        }
    }
    <bean id="userDao" class="cn.itcast.dao.factory.StaticFactory" factory-method="getUserDao"></bean>

* 工厂实例方法实例化  了解
    public class Factory {
        public UserDaoImpl getUserDao(){
            return new UserDaoImpl();
        }
    }
    <bean id="factory" class="cn.itcast.dao.factory.Factory"></bean>
    <bean id="userDao" factory-bean="factory" factory-method="getUserDao"></bean>   
```

## 4.Spring依赖注入 IOC

```
* 场景：
	web层   			调用         service层
	service层		调用			dao层
这样的调用方式： web层 =》 service层  =》 dao层，其实我们只关心web层调用service层，所以service层调用dao层的交由Spring管理，我们可以使用 IOC 依赖注入实现
* 依赖注入两种实现方式： 
	1. set方法
	2. 带参构造函数
```

### 4.1 set依赖注入

```
1）UserService类中的set方法
    private UserDaoImpl userDaoImpl;

    public void setUserDaoImpl(cn.itcast.dao.impl.UserDaoImpl userDaoImpl) {
        this.userDaoImpl = userDaoImpl;
    }
2）配置文件 
	* 不使用 p命名空间， 推荐使用这种方式
    <bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl"></bean>
    <bean id="userService" class="cn.itcast.service.impl.UseService">
        <property name="userDaoImpl" ref="userDao"></property>
    </bean>
    name=userService： 指UserService类中 setUserService
    ref： 指用那个Bean注入
    注入普通属性： 不用ref，用value
   * 使用 p 命名空间
   xmlns:p="http://www.springframework.org/schema/p"
   <bean id="userService" class="cn.itcast.service.impl.UseService" p:userDaoImpl-ref="userDao"></bean>
   要添加命名空间
   p:userDaoImpl-ref="对象"， p:userDaoImpl="非引用类型"
    
```

### 4.2 带参构造方法注入

```
1）UserService类中的 带参构造方法
    private UserDaoImpl userDaoImpl;

    public UseService(UserDaoImpl userDaoImpl) {
        this.userDaoImpl = userDaoImpl;
    }

2） xml配置文件
	<bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl"></bean>
    <bean id="userService" class="cn.itcast.service.impl.UseService">
        <constructor-arg name="userDaoImpl" ref="userDao"></constructor-arg>
    </bean>
    name="userDaoImpl"： 构造方法，形参名
    ref="userDao"： Bean的id
```

### 4.3 import划分模块

```
eg 
	<import resource="xxx.xml">
```

## 5.注解开发

```
	Spring是一个 重配置 轻代码的框架， 通过注解减小xml配置文件体积， 简化和提升开发速度。
	
* 步骤：
	1. 使用@Component("BeanId") @Controller、 @Service、 @Repository等代替 xml中的bean配置
	2. 使用@Autowired配合Qualifier("BeanId") 实现依赖注入
		而且使用这种方式注入的话，类中对应的set方法可以不写
	  * 如果知识按照类型注入，可以直接写 @Autowried
	  * 如果按照BeanId注入， 需要@Autowried 和 Qualifier("BeanId")同时写
	  * @Resource: 相当于 @Autowried 和 Qualifier("BeanId")同时写
		
	3. 在xml文件中配置扫描包路径， 告知Spring需要去扫描哪些包的注解，自动创建Bean并注入Spring容器中
	 eg.  <context:component-scan base-package="包名"></context:component-scan>
	
```

### 5.1原始注解

```
* Spring原始注解主要是替代<Bean>的配置
```

![1640513215](D:\java\java-web\ssm\Spring\1640513215.jpg)

### 5.2 新注解

![1640516198(1)](D:\java\java-web\ssm\Spring\1640516198(1).jpg)

```

```

