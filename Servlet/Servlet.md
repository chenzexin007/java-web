# Servlet

## 概念

```
* Servlet就是一个接口， 定义了Java类被浏览器访问到(tomcat识别)的规则。
* 将来我们定义一个类，实现Servlet接口，复写方法。
```

## 快速入门

```
1.创建javaEE项目
* 或者配置tomcat
* Servlet是在javaEE中的，需要导入

2.定义一个Servlet接口的实现类
public class ServletDemo1 implements Servlet{}

3.实现接口中的方法

4.配置Servlet
* 在web.xml中配置
    <servlet>
        <servlet-name>bieming</servlet-name>
        <servlet-class>cn.itcast.web.servlet.ServletDemo1</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>bieming</servlet-name>
        <url-pattern>/testServlet</url-pattern>
    </servlet-mapping>
```

## 执行原理

![image-20211217005332281](./image-20211217005332281.png)

```
1.当服务器接收到客户端浏览器的请求后，解析url，回去访问的Servlet资源路径
2.查找web.xml文件，是否有对应的<url-pattern>
3.如果有，则找到对应的<servlet-class>全类名
4.tomcat会将字节码文件加载进内存，并创建其对象
5.对象调用方法
```

## 生命周期

### init

```
被创建时：执行init，只执行一次；
* 什么时候创建
 * 默认第一次访问时，Servlet被创建
 * 可以配置Servlet的创建时机
 	1）第一次访问时，创建： 
 	<load-on-startup>为负数
 	2）在服务启动时，创建
 	<load-on-startup>为非负整数
 
* Servlet的init方法只执行一次，说明Servlet是单例对象
	* 多个用户同事访问时，可能存在线程安全问题；
	* 解决：尽量不要在Servlet中定义成员变量，即使定义了也不要修改它的值；
```



### service

```
提供服务：
每次访问Servlet时(调用接口时)执行，可执行多次；
```



### destory

```
被销毁时也就是服务正常关闭时执行，只执行一次
只有服务器正常关闭时会执行
destory是Servlet被销毁前执行，一般用于释放资源
```



### getServletInfo

```
获取Servlet的一些信息： 版本、作者等；
```



### getServletConfig

```
获取ServletConfig对象
ServletConfig: Servlet的配置对象
```



## Servlet3.0

### 好处

```
* 支持注解配置， 可以不需要web.xml
```

### 步骤

```
* 创建javaEE项目，选择servlet3.0+，可以不创建web.xml
* 定义一个类，实现Servl接口
* 复写方法
* 在类上使用WebServlet注解进行配置
	* 包名是 import javax.servlet.annotation.WebServlet;
	* @WebServlet("资源路径")
```

## Servlet的体系结构

### GenericServlet

```
* Servlet接口的实现类， 我们继承GenericServlet就可以写一个Servlet类了
* 定义： 
	* 将Servlet接口中其他方法做了默认空实现，只将service方法作空实现
	* 将来定义Servlet类时，可以寄继承GenericServlet，实现service方法即可
	
```



### HttpServlet

```
定义：对http协议的一种封装，简化操作
步骤：
	* 定义类继承HttpServlet
	* 复写doGet/doPost方法
```



