# SpringMVC

## 1.快速入门

![1640590938(1)](./1640590938(1).jpg)

```
开发步骤：
	* 导入SpringMVC相关坐标
		pom.xml中
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.14</version>
        </dependency>	

	* 配置SpringMVC核心控制器DispathcerServlet
	  web.xml中配置
      <servlet>
        <servlet-name>DispathcerServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
      </servlet>
      <servlet-mapping>
        <servlet-name>DispathcerServlet</servlet-name>
        <url-pattern>/</url-pattern>
      </servlet-mapping>		
	
	* 创建Controller类和视图
	* 使用注解配置Controller类中业务方法的映射地址
        @Controller
        public class UseController {
        	// 这个是访问的路径
            @RequestMapping("/quick")
            public String save(){
                System.out.println("Spring runing");
                // return的是jsp文件，表示要展示哪一个
                return "success.jsp";
            }
        }	
	
	* 配置SpringMVC核心文件spring-mvc.xml
		主要是添加context声明空间
		还有扫描包
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:context="http://www.springframework.org/schema/context"
               xsi:schemaLocation="http://www.springframework.org/schema/beans 		http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

            <context:component-scan base-package="cn.itcast.controller"></context:component-scan>

        </beans>
	* 客户端发起请求测试
	直接启动tomcat测试
```

## 2.springMVC的执行流程

![1640596494(1)](./1640596494(1).jpg)

```
* 用户发送请求到前端控制器DispatcherServlet
* DispatcherServlet收到请求调用Handler Mapping处理映射器
* 处理器映射找到巨日的处理器（可以根据xml哦诶之、注解进行查找），生成处理器对象及处理拦截器（如果有则生成）一并返回给DispatcherServlet
* DispatcherServlet调用HandlerAdapter处理器适配器
* HandlerAdapter经过适配调用具体的处理器(Controller, 也叫后端控制器)
* Controller执行完成返回ModelAndView
* HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet
* DispatcherServlet将ModelAndView传给ViewReslover视图解析器
* ViewReslover解析猴返回具体View
* DispatcherServlet根据View进行渲视图（即模型数据填充至视图中）。DispatcherServlet响应用户。
```

