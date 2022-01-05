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
* 处理器映射找到具体的处理器（可以根据xml哦诶之、注解进行查找），生成处理器对象及处理拦截器（如果有则生成）一并返回给DispatcherServlet
* DispatcherServlet调用HandlerAdapter处理器适配器
* HandlerAdapter经过适配调用具体的处理器(Controller, 也叫后端控制器)
* Controller执行完成返回ModelAndView
* HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet
* DispatcherServlet将ModelAndView传给ViewReslover视图解析器
* ViewReslover解析猴返回具体View
* DispatcherServlet根据View进行渲视图（即模型数据填充至视图中）。DispatcherServlet响应用户。
```

## 3.SpringMVC组件解析

### 3.1SpringMVC注解解析

```
* @RequestMapping
作用： 用于建立请求URL和处理请求方法之间的对应关系
位置：
	* 类上： 请求URL的第一级访问目录，此处不写相当于 /
	* 方法上： 请求URL的第二级访问目录
属性：
	* value：用于指定请求的URL
    * method： 用于指定请求的方式
    * params： 用于指定限制请求参数的条件
    	eg： params={"accountName"}, 请求必须携带参数accountName
```

![1640657234(1)](./1640657234(1).jpg)

### 3.2 SpringMVC组件扫描

```
* 注意： SpringMVC只扫描Controller包下的Bean， 一般在spring-mvc.xml下配置扫描
		Spring扫描Spring要的Bean的包，例如dao包、service包等， 一般在applicationContext.xml下配置扫描
		各扫各的，不要冲突，不要重复，多扫
		
* spring-mvc.xml配置扫描两个步骤
	1.引入context命名空间
		xmlns:context="http://www.springframework.org/schema/context"
		xsi:schemaLocation="http://www.springframework.org/schema/beans 		http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"
                
     2. 配置要扫描的包
     <context:component-scan base-package="cn.itcast.controller">
     	// <context:include-filter type="" expression=""/>  配置那些要扫描
     	// <context:exclude-filter type="" expression=""/>  配置那些不扫描
     </context:component-scan>
```

## 4.SpringMVC的数据响应

### 4.1页面跳转

- 直接放回字符串形式

  ![1640659632(1)](./1640659632(1).jpg)

- 返回ModelAndView

![1640660356(1)](./1640660356(1).jpg)

- 其他两种方式

  ```
  * SpringMVC内部处理ModelAndView
      @RequestMapping("/quick3")
      public ModelAndView save03(ModelAndView modelAndView){
          // 添加数据
          modelAndView.addObject("age", "18");
  
          // 设置视图
          modelAndView.setViewName("/success.jsp");
  
          return modelAndView;
      }
      
  * SpringMVC内部处理Model
      @RequestMapping("/quick4")
      public String save04(Model model){
          model.addAttribute("age", "18");
  
          return "/success.jsp";
      }
      
  * SpringMVC内部处理HttpServletRequest 
      @RequestMapping("/quick5")
      public String save05(HttpServletRequest request){
          request.setAttribute("age", "18");
  
          return "/success.jsp";
      }
  ```



### 4.2 回写数据

```
    // 回写数据  -- 字符串
    @RequestMapping("/quick6")
    public void save06(HttpServletResponse response) throws IOException {
        response.getWriter().print("Hello SpringMVC");
    }
    
    @RequestMapping("/quick7")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public String save07() throws IOException {
        return "Hello SpringMVC";
    } 
    
    // 回写数据  -- 字符串对象
    @RequestMapping("/quick8")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public String save08() throws IOException {
        User user = new User();
        user.setAge(18);
        user.setName("张三");
        // 这个对象转json的依赖要安装三个： jackson-core、jackson-annotations、jackson-databind版本都选用2.9.0
        ObjectMapper objectMapper = new ObjectMapper();
        String s = objectMapper.writeValueAsString(user);
        return s;
    }
    
* 在spring-mvc.xml中
	* 加入 mvc命名空间
	* 使用<mvc:annotation-driven></mvc:annotation-driven>   ！！！这个在spring-mvc.xml文件中一般都是必须配置的
	* SpringMVC三大组件： 处理器映射器、处理器适配器、视图解析器
	* 使用<mvc:annotation-driven>就会自动加载RequestMappingHandlerMapping（处理映射器）和RequestMappingHandlerAdapter（处理适配器）、
	默认底层就会继承jackson进行对象集合的json格式字符串的转换
	
```

## 5.SpringMVC请求参数

### 5.1获取基本数据类型

```
* Controller中的业务方法的参数名称要与请求的name一致，参数会自动映射匹配
	// 请求地址 http://localhost:8089/user/quick10?name=%22lilei%22&age=18
    @RequestMapping("/quick10")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save10(String name, int age) throws IOException {
        System.out.println(name + "---" + age);
    }
```

### 5.2获取POJO参数类型

```
* Controller中的业务方法的POJO参数的属性名与请求参数的name一致，参数会自动映射匹配
	// 请求地址 http://localhost:8089/user/quick11?name=%22lilei%22&age=18
    @RequestMapping("/quick11")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save11(User user) throws IOException {
        System.out.println(user);
    }
```

### 5.3获取数组类型参数

```
* Controller中的业务方法数组名称与请求参数的name一致，参数值会自动映射匹配
	// 请求地址 http://localhost:8089/user/quick12?str=111&str=222&str=333
    @RequestMapping("/quick12")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save12(String[] str) throws IOException {
        System.out.println(Arrays.asList(str));
    }
```

### 5.4 获取集合类型参数

```
* 第一种： 利用获取POJO类型自动匹配的特性， 我们将封装一个集合类VO，通过VO做形参，这样SpringMVC会帮我们自动匹配
    @RequestMapping("/quick13")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save13(VO vo) throws IOException {
        System.out.println(vo);
    }
```

![1640673942(1)](./1640673942(1).jpg)

```
* 第二种： 当使用ajax提交时，指定contentType为json形式，那么在方法参数位置使用@RequestBody可以直接接收集合数据而无需使用POJO进行包装。
	* 配置ajax
	$.ajax({
		type: "POST",
		url: "http://localhost:8089/user/quick14",
		data: xxx,
		contentType: "application/json;charset=utf-8"
	})
    @RequestMapping("/quick14")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save14(@RequestBody List<User> userList) throws IOException {
        System.out.println(userList);
    }
```

### 5.6 开放静态资源访问

```
* 因为web.xml中，我们配置了 / 下的匹配都将使用DispathcerServlet处理，我们访问的所有路径都会被拦截，然后到扫描包的Controller中匹配路径。
如果找不到就会报错。像图片，jsp文件等静态资源都是不用DispathcerServlet处理的，所以需要配置静态资源访问权限。

第一种：
	<mvc:resources mapping="/js/**" location="/js/"></mvc:resources>
	mapping表示访问地址， location表示资源实际地址，
	表示： 通过/js/** 路径访问的，将会去/js/下查找资源，而不是交由DispathcerServlet处理

第二种： 
	<mvc:default-servlet-handler/>
	DispathcerServlet匹配不到后，交由tomcat处理匹配资源
```

![1640675660(1)](./1640675660(1).jpg)

### 5.7 配置全局Filter解决参数乱码

```
* 在web.xml文件中配置全局Filter
  <!--  配置过滤器解决乱码问题-->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

### 5.8参数绑定注解 @RequestParam

```
* vlaue: 请求参数名称
* required： 是否必须携带该参数，默认true
* defaultValue： 当没有指定请求参数时，则使用指定的默认值赋值
    @RequestMapping("/quick15")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save15(@RequestParam(value="name", required = false, defaultValue = "默认值")  String userName) throws IOException {
        System.out.println(userName);
    } 
```

### 5.9自定义转换器

```
这个一般SpringMVC提供的就够用了，了解即可
参考地址：
https://www.cnblogs.com/kitor/p/11093030.html
```

### 5.10获取Servlet相关API

```
* SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入，常用的对象如下
- HttpServletRequest
- HttpServletResponse
- HttpSession

    @RequestMapping("/quick16")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save16(HttpServletRequest request, HttpServletResponse response, HttpSession session) throws IOException {
        System.out.println(request);
        System.out.println(response);
        System.out.println(session);
    }

```

### 5.11 获取请求头信息

```
* 通过@RequestHeader获取指定请求头信息
    @RequestMapping("/quick17")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save17(@RequestHeader(value = "User-Agent", required = false) String userAgent) throws IOException {
        System.out.println(userAgent);
    }

 * 通过@CookieValue获取cookie
    @RequestMapping("/quick18")
    @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
    public void save18(@CookieValue(value = "_ga") Cookie cookie) throws IOException {
        System.out.println(cookie);
    }
```

### 5.12文件上传

```
* 第一步： 导入坐标
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.3</version>
        </dependency>
        
* 第二步： 配置文件上传解析器   srping-mvc.xml文件中
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver" >
        <property name="maxUploadSize" value="50000"></property>
        <property name="maxUploadSizePerFile" value="50000"></property>
        <property name="defaultEncoding" value="UTF-8"></property>
    </bean>

```

- 单文件上传

  ```
      
  * 第三步： form表单提交
  	method： post
  	enctype="multipart/form-data"
  	
      <form action="${pageContext.request.contextPath}/user/quick19" method="post" enctype="multipart/form-data">
          名字:<input type="text" name="name">
          上传文件：<input type="file" name="uploadFile">
          <input type="submit" value="提交">
      </form>	
      
  * 第四步： 配置接口，获取并存储文件
      @RequestMapping(value = "/quick19", method = RequestMethod.POST)
      @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
      public void save19(String name, MultipartFile uploadFile) throws IOException {
          String originalFilename = uploadFile.getOriginalFilename();
          System.out.println(originalFilename);
          uploadFile.transferTo(new File("D:\\upload\\"+originalFilename));
      }
  ```

- 多文件上传

  ```
  * 第三步： form表单提交
  	method： post
  	enctype="multipart/form-data"
  	
      <form action="${pageContext.request.contextPath}/user/quick19" method="post" enctype="multipart/form-data">
          名字:<input type="text" name="name">
          上传文件：<input type="file" name="uploadFile1">
          上传文件：<input type="file" name="uploadFile2">
          <input type="submit" value="提交">
      </form>	
      
  * 第四步： 配置接口，获取并存储文件
      @RequestMapping(value = "/quick19", method = RequestMethod.POST)
      @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
      public void save19(String name, MultipartFile uploadFile1, MultipartFile uploadFile2) throws IOException {
          String originalFilename1 = uploadFile1.getOriginalFilename();
          System.out.println(originalFilename1);
          uploadFile1.transferTo(new File("D:\\upload\\"+originalFilename1));
          
          String originalFilename2 = uploadFile2.getOriginalFilename();
          System.out.println(originalFilename2);
          uploadFile2.transferTo(new File("D:\\upload\\"+originalFilename2));
      }
      
  -------------------------------------------  数组方式接收  ------------------------------------------------   
   * 第三步： form表单提交
  	method： post
  	enctype="multipart/form-data"
  	
      <form action="${pageContext.request.contextPath}/user/quick20" method="post" enctype="multipart/form-data">
          名字:<input type="text" name="name">
          上传文件：<input type="file" name="uploadFile">
          上传文件：<input type="file" name="uploadFile">
          <input type="submit" value="提交">
      </form>
      
  * 第四步： 配置接口，获取并存储文件
      @RequestMapping(value = "/quick20", method = RequestMethod.POST)
      @ResponseBody  // 告知SpringMVC这个是回写字符串，不是回显页面
      public void save20(String name, MultipartFile[] uploadFile) throws IOException {
          for (MultipartFile multipartFile : uploadFile) {
              String originalFilename = multipartFile.getOriginalFilename();
              multipartFile.transferTo(new File("D:\\upload\\"+originalFilename));
          }
      }  
   
  ```

  