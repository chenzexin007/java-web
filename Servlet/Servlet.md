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

