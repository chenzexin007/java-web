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

