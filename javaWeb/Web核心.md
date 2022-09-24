# Web核心

## HTTP

HTTP即超文本传输协议，规定了浏览器和服务器之间数据传输的规则。

HTTP协议特点：

1. 基于TCP协议：面向连接，安全
2. 基于请求-响应模型的：一次请求对应一次响应
3. HTTP协议是无状态的协议：对于事务处理没有记忆能力。每次请求-响应都是独立地

- 缺点：多次请求间不能共享数据
- 优点：速度快



HTTP请求数据格式

请求数据分为三部分：

1. 请求行：请求数据的第一行，其中GET表示请求方式，/表示请求资源路径，HTTP/1.1表示协议版本
2. 请求头：第二行开始，格式为key：value形式
3. 请求体：POST请求的最后一部分，存放请求参数



- GET请求和POST请求的区别：

1. GET请求参数在请求行中，没有请求体。POST请求请求参数在请求体中
2. GET请求参数大小有限制，POST没有



HTTP响应数据格式

响应数据分为三部分：

1. 响应行：响应数据的第一行。其中HTTP/1.1表示协议版本，200表示响应状态码，OK表示状态码描述
2. 响应头：第二行开始，格式为key：value形式
3. 响应体：最后一部分。存放响应数据



## 状态码

### 一、状态码大类



| 状态码分类 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| 1xx        | **响应中**——临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它 |
| 2xx        | **成功**——表示请求已经被成功接收，处理已完成                 |
| 3xx        | **重定向**——重定向到其它地方：它让客户端再发起一个请求以完成整个处理。 |
| 4xx        | **客户端错误**——处理发生错误，责任在客户端，如：客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
| 5xx        | **服务器端错误**——处理发生错误，责任在服务端，如：服务端抛出异常，路由出错，HTTP版本不支持等 |

状态码大全：https://cloud.tencent.com/developer/chapter/13553 



### 二、常见的响应状态码

| 状态码 | 英文描述                               | 解释                                                         |
| ------ | -------------------------------------- | ------------------------------------------------------------ |
| 200    | **`OK`**                               | 客户端请求成功，即**处理成功**，这是我们最想看到的状态码     |
| 302    | **`Found`**                            | 指示所请求的资源已移动到由`Location`响应头给定的 URL，浏览器会自动重新访问到这个页面 |
| 304    | **`Not Modified`**                     | 告诉客户端，你请求的资源至上次取得后，服务端并未更改，你直接用你本地缓存吧。隐式重定向 |
| 400    | **`Bad Request`**                      | 客户端请求有**语法错误**，不能被服务器所理解                 |
| 403    | **`Forbidden`**                        | 服务器收到请求，但是**拒绝提供服务**，比如：没有权限访问相关资源 |
| 404    | **`Not Found`**                        | **请求资源不存在**，一般是URL输入有误，或者网站资源被删除了  |
| 428    | **`Precondition Required`**            | **服务器要求有条件的请求**，告诉客户端要想访问该资源，必须携带特定的请求头 |
| 429    | **`Too Many Requests`**                | **太多请求**，可以限制客户端请求某个资源的数量，配合 Retry-After(多长时间后可以请求)响应头一起使用 |
| 431    | **` Request Header Fields Too Large`** | **请求头太大**，服务器不愿意处理请求，因为它的头部字段太大。请求可以在减少请求头域的大小后重新提交。 |
| 405    | **`Method Not Allowed`**               | 请求方式有误，比如应该用GET请求方式的资源，用了POST          |
| 500    | **`Internal Server Error`**            | **服务器发生不可预期的错误**。服务器出异常了，赶紧看日志去吧 |
| 503    | **`Service Unavailable`**              | **服务器尚未准备好处理请求**，服务器刚刚启动，还未初始化好   |
| 511    | **`Network Authentication Required`**  | **客户端需要进行身份验证才能获得网络访问权限**               |





## Tomcat

Tomcat是一个轻量级的Web服务器，支持Servlet/JSP少量JavaEE规范，也成为Web容器，Servle容器

关闭方式：Ctrl+c(正常关闭)



### 插件配置

- Maven Tomcat插件目前只有Tomcat7版本，没有更高的版本可以使用
- 使用Maven Tomcat插件，要想修改Tomcat的端口和访问路径，可以直接修改pom.xml

```xml
<build>
    <plugins>
    	<!--Tomcat插件 -->
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
                <configuration>
                    <port>80</port><!--访问端口号 -->
                    <!--项目访问路径
                        未配置访问路径: http://localhost:80/tomcat-demo2/a.html
                        配置/后访问路径: http://localhost:80/a.html
                        如果配置成 /hello,访问路径会变成什么?
                            答案: http://localhost:80/hello/a.html
						配置访问路径需谨慎
                    -->
<!--                    <path>/</path>-->
                </configuration>

        </plugin>
    </plugins>
</build>
```

****





## Mybatis

Mybatis是一款优秀的持久层框架，用于简化JDBC开发

使用步骤：

一.添加依赖



二.在resources目录下添加mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--起别名-->
    <typeAliases>
        <package name="com.itheima.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///db1?useSSL=false&amp;useServerPrepStmts=true"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--扫描mapper-->
        <package name="com.itheima.mapper"/>
    </mappers>
</configuration>
```

注：

1.类别别名

```xml
    <typeAliases>
        <package name="com.itheima.pojo"/>
    </typeAliases>
```

这样配置后，映射文件中的resultTypey便不需要使用全类名，而是可以直接使用类名来书写，例如com.itheima.User会简化成user(不区分大小写)



2.映射文件

加载映射文件可以采用下列代码

```
<mapper resource="org/mybatis/example/BlogMapper.xml"/>
```

注：如果Mappe接口名称和sql映射文件名称相同，并在同一目录下，则可以使用包扫描方式简化sql映射文件的加载

```xml
<!--扫描mapper-->
<package name="com.itheima.mapper"/>
```



```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

注：

resultType为返回值的类型



三.获取SqlSession

可以采用工具类来简化该步骤

```javascript
public class SqlSessionFactoryUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {
        //静态代码块会随着类的加载而自动执行，且只执行一次

        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }


    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }
}

```



四.执行



五.释放资源

sqlSession.close()；



#### Mapper代理开发

1. 定义域SQL映射文件同名的Mapper接口，并且将Mapper接口和sql映射文件放置在同一目录下(**一种解决方法是在映射文件所处目录下也创建一个和接口相同的文件夹结构，注意创建时使用的是.分隔还是使用/分隔**)
2. 设置sql映射文件的namespace属性为Mapper接口全限定名
3. 在Mapper接口中定义方法，方法名就是sql映射文件中sql语句的id，并保持参数类型和返回值类型一致
4. 编码

- 通过SqlSession的getMapper方法获取Mapper接口的代理对象
- 调用对应方法完成sql的执行





#### 注解开发

对于一些简单的操作可以不用配置映射文件，而是直接在接口的上方使用注解进行开发，这里参数的名称与对象相同即可

```java
@Insert("insert into tb_user values(null,#{username},#{password})")
void add(User user);
```









注意：

java里实体类参数名称和数据库里数据的字段名称不相同时，需要使用resultMap进行一个映射，定义在xml文件下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.BrandMapper">

    <resultMap id="brandResultMap" type="brand">
        <result property="brandName" column="brand_name" />
        <result property="companyName" column="company_name" />
    </resultMap>
</mapper>
```



此时使用注解开发的话还需要加上ResultMap

```

```



## Servlet

Servlet是JavaEE规范之一，其实就是一个接口，将来我们需要定义Servlet类实现Servlet接口，并由web服务器运行Servlet





### 执行流程

Servlet对象由web服务器创建，Servlet方法由web服务器调用



### 生命周期



### 体系结构



### Request&Response

#### Request

基于一种通用的获取参数思想，request对象为我们提供了如下方法:

- 获取所有参数Map集合

```
Map<String,String[]> getParameterMap()
```

- 根据名称获取参数值（数组）

```
String[] getParameterValues(String name)
```

- 根据名称获取参数值(单个值)

```
String getParameter(String name)
```



Request请求参数中文乱码处理

- 请求参数如果存在中文数据，则会乱码

- 解决方案：

  POST：设置输入流的编码

  GET：转换为字节码数组再进行解码(通用解决方法，对POST也有效)

  ```java
  String username = request.getParameter("username");
  username=new String(username.getBytes(StandardCharsets.ISO_8859_1),StandardCharsets.UTF_8);//转换为字节数组后再进行解码
  ```

  



Requeset请求转发

请求转发：一种在服务器内部的资源跳转方式

```java
request.getRequestDispatcher("/req4").forward(request,response); //请求转发
```

- 请求转发资源间共享数据：使用Request对象

- ```java
  request.setAttribute(); //存储数据到request域中
  request.getAttribute();	//根据key，获取值
  request.removeAttribute();	//根据key，删除该键值对
  ```

- 请求转发特点：

1. 浏览器地址栏路径不发生变化
2. 只能转发到当前服务器的内部资源
3. 一次请求，可以在转发的资源间使用request共享数据



#### Resoponse

- 响应数据分为三部分：

1. 响应行

   ```java
   setStatus();	//设置响应状态码
   ```

2. 响应头

   ```java
   setHeader();	//是指响应头键值对
   ```

3. 响应体





##### Response完成重定向

重定向：一种资源跳转方式

```java
response.sendRedirect("resp2");
```

- 重定向特点：

1. 浏览器地址栏路径发生变化
2. 可以重定向到任意位置的资源
3. 两次请求，不能在多个资源使用request共享资源



路径问题：

- 浏览器使用：需要加虚拟目录(项目访问路径)
- 服务端使用：不需要加虚拟目录

动态获取虚拟目录可以使用：

```java
request.getContextPath(); //服务端动态获取虚拟目录
```



##### 响应字符数据

```java
	//获取字符输出流
    PrintWriter writer = response.getWriter();
	 writer.write("aaa");
```
注意：

- 该流不需要关闭，随着响应的结束，response对象销毁，由服务器关闭

- 中文数据乱码：因为通过response获取的字符输出流默认编码：ISO-8859-1，修改为utf-8即可



##### 响应字节数据

```java
FileInputStream fls = new FileInputStream("src/main/resources/img/atri.png");
ServletOutputStream os = response.getOutputStream();

//使用工具类完成流的对拷
IOUtils.copy(fls,os);
fls.close();
```

通过导入IOUtils工具类可以方便的完成流的对拷



SqlSessionFactory工具类抽取

通过工具类可以优化代码，使得Factory只被产生一次，节约了资源消耗，且减少代码重复

```java
public class SqlSessionFactoryUtils {
    private static SqlSessionFactory sqlSessionFactory;

    //静态代码块会随着类的加载而自动执行，且只执行一次
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }

}
```





## JSP

jsp，即java server pages，Java服务端页面，是一种动态的网页技术，其中既可以定义HTML，JS，CSS等静态内容，还可以定义Java代码的动态内容



步骤

1.导入jsp坐标

**注意：scope需要设置为provided**

2.创建jsp文件



3.编写html标签和java代码



### jsp脚本

jsp脚本用于在jsp页面内定义java代码

- jsp脚本分类

1. <%...%>：内容会直接放到_jspService方法之中
2. <%=...%>：内容会放到out.print()中，作为out.print()的参数
3. <%!...%>：内容会放到_jspService()方法之外，被类直接包含

详细内容参见JSP



## Mvc模式和三层架构

MVC是一种分层开发的模式，其中：

业务模型：处理业务

视图：界面展示

控制器：处理请求，调用模型和视图



### 三层架构

即表现层，业务逻辑层，数据访问层(Web,Servcice,Dao)

数据访问层：对数据库的CRUD基本操作

业务逻辑层：对业务逻辑进行封装，组合数据访问层中基本功能，形成复杂的业务逻辑功能

表现层：接受请求，封装数据，调用业务逻辑层，响应数据



Service先写对应接口，再写其具体实现类。Servlet使用接口:实现类的多态形式

这样写的好处是如果Service层里的方法发生了改变，Servlet不需要修改太多代码，只需要切换对应的实现类即可，这样Service层和Web层的耦合性就解除了







## 会话跟踪

会话：用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含多次请求和响应



会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间共享数据



### Cookie

Cookie：客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问



发送Cookie

```java
/*发送Cookie*/
Cookie cookie = new Cookie("usernaem","zhangsan");
response.addCookie(cookie);
```



获取Cookie

```java
Cookie[] cookies = request.getCookies();
        for (Cookie cookie : cookies) {
            String name = cookie.getName();
            if (name.equals("username")){
                String value = cookie.getValue();
                System.out.println("username:"+value);
            }
```



#### 使用细节

存活时间：

默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁

setMaxAge(int seconds)：设置Cookie存储时间

1. 整数：将Cookie写入浏览器所在电脑的硬盘，持久化存储，到时间自动删除
2. 负数：默认值，Cookie在当前浏览器内存中，当浏览器关闭，则Cookie自动销毁
3. 零：删除对应Cookie



Cookie存储中文：

如需要存储，则需要进行转码：URL编码

```java
String value = URLEncoder.encode("张三","UTF-8");
```



### Session

基本使用

```java
HttpSession session = request.getSession(); //获取Session
session.setAttribute("username","zhangsan");	//存储数据到session域中
session.getAttribute("useranme");	//根据key，获取值
session.removeAttribute("username");	//根据key，删除该键值对
```

#### 使用细节

Session钝化，活化：

- 钝化：在服务器正常关闭后，Tomcat会自动将Session数据写入硬盘的文件中
- 活化：再次启动服务器后，从文件中加载数据到Session中



Session销毁：

默认情况下，无操作，30分钟后销毁

手动可以调用Session对象的invalidate()方法



### 小结





## Filter

filter表示过滤器，是JavaWeb三大组件(Servlet,Filter,Listener)之一，过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能，比如权限控制，统一编码处理，敏感字符处理等

使用步骤

1. 定义类，实现Filter接口，并重写其所有方法
2. 配置Filter拦截资源的路径，在类上定义@WebFilter注解
3. 在doFilter中定义操作并放行



注：放行前一般对Request进行处理，放行后一般对Response进行处理



拦截路径配置

- 拦截具体的资源：/index.jsp：只有访问index.jsp才会被拦截
- 目录拦截：/user/*：访问/user下的所有资源，都会被拦截
- 后缀名拦截：*.jsp：访问后缀名为jsp的资源
- 拦截所有：/*：访问所有资源都会被拦截



过滤器链：一个web应用，可以配置多个过滤器



## Listener

Listener表示监听器，是JavaWeb三大组件之一

目前主要使用的为ServletContextListener：用于对Context对象进行监听(创建，销毁)



## AJAX

AJAX：异步的javaScript和XML



### Axios异步框架

get请求

```javascript
axios({
    method:"get",
    url:"http://localhost/project1/axiosServlet?username=zhangsan"
}).then(function (resp){
    alert(resp.data);
})
```



post请求

```javascript
axios({
    method:"post",
    url:"http://localhost/project1/axiosServlet",
    data:"username=zhangsan"
}).then(function (resp){	//then内为回调函数
    alert(resp.data);
```



实例

```

```



#### 请求方式别名

为了方便起见，Axios已经为所有支持的请求方式提供了别名

get请求

```javascript
axios.get("url")
.then(function (resp){
    alert(resp.data);
})
```



post请求

```javascript
axios.post("url","参数")
.then(function (resp){
    alert(resp.data);
})
```



注意

axios里不能直接使用this指定Vue里的数据，需要进行提升

```javascript
mounted(){
    var _this=this;
    axios({
        method:"get",
        url:"http://localhost/brand-case//SearchAllServlet"
    }).then(function (resp){
        _this.tableData=resp.data();
    })
}
```



### JSON

概念：JavaScript Object Notation。JavaScript对象表示法

```javascript
var json={
    "key1":"value1",
    "key2":"value2"
};
```

value的数据类型为：

- 数字(整数或浮点数)
- 字符串(在双引号中)
- 逻辑值(true或false)
- 数组(在方括号中)
- 对象(在花括号中)
- null



获取数据

变量名.key





JSON数据和Java对象转换

使用Fastjson这个api即可实现

```java
String s = JSON.toJSONString(user);
System.out.println(s);

User jsonObject = JSON.parseObject("{\"password\":\"123\",\"useranme\":\"zhangsan\"}",User.class);
System.out.println(jsonObject);
```



## Vue

Vue是一套前端框架，免除原生JavaScript中的DOM操作，简化书写



1.引入Vue.js



2.在JS代码区域，创建Vue核心文件，进行数据绑定

```javascript
<script>
new Vue({
    el: "#app",
    data(){
        return {
            brand:{}
        }
    },
    methods:{
        submitForm(){
            // 发送ajax请求，添加
            var _this = this;
            axios({
                method:"post",
                url:"http://localhost:8080/brand-demo/addServlet",
                data:_this.brand
            }).then(function (resp) {
                // 判断响应数据是否为 success
                if(resp.data == "success"){
                    location.href = "http://localhost:8080/brand-demo/brand.html";
                }
            })
        }
    }
})
</script>
```

3.编写视图

```html
<div id="app">
  <input v-model="username">
  <!--插值表达式-->
  {{username}}
</div>
```



### Vue常用指令

v-bind：为Html标签绑定属性值，如设置href，css样式等

v-model：在表单元素上创建双向数据绑定

v-on：为Html标签绑定事件

v-if	v-else	v-else-if：条件性的渲染某元素，判定为true是渲染，否则不渲染

v-show：与根据条件展示某元素区别在于切换的是display属性的值

v-for：列表渲染，遍历容器的元素或者对象的属性

```html
<div id="app">
    <div v-for="(addr,i) in addrs">
        {{i}}--{{addr}}
        <br>
    </div>
</div>
```



```
<input type="button" value="click" v-on:click="show()">
```



### Vue生命周期

生命周期有八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法(钩子)

mounted：挂载完成，Vue初始化成功，HTML页面渲染成功

- 发送异步请求，加载数据

```javascript
new Vue({
    el:"#app",
    mounted(){
        alert("vue挂在完毕，发送异步请求")
    }
})
```



## Element

一套基于Vue的网站组件库，用于快速构建网页

1.引入Element的css，js文件和Vue.js

```javascript
<script src="js/vue.js"></script>
<script src="element-ui/lib/index.js"></script>
<link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
```



2.创建Vue核心对象

```javascript
  new Vue({
    el:"#app",
  })
```

3.官网复制Element组件代码



Element布局

Layout布局：通过基础的24分栏，迅速简便地创建布局



Container布局容器：用于布局的容器组件，方便快速搭建页面的基本结构