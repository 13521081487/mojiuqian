> ### Spring Security 安全框架的应用说明
* 在SpringBoot或者Maven项目的pom.xml文件中添加以下依赖
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
----------
> #### Spring Security 认证

----------


**启动项目之后**

 1. 在控制台中找到如下日志:
    &emsp;Using generated security password: e75b507c-f279-4acb-955a-3ceef7e5d793
 2. 在浏览器地址栏中输入项目的地址如:http://localhost:8099/加随便一个Controller中的接口如下:
    ```java
    @RestController
    @RequestMapping(value = "/user")
    public class UserController {
    
        //先随便写个方法
        @GetMapping(value="/list")
        public Object dsfsd(){
            return  new Object();
        }
    }
    ```
    * 那么我们就在地址栏中输入http://localhost:8080/user/list,然后回车
 3. 最后你会惊奇的发现,项目中多了一个登陆页面,这个登陆页面是Spring-Security自带的登录页面,它默认给我们一个用户名为user的账户,密码就是控制台输出的 security password:e75b507c-f279-4acb-955a-3ceef7e5d793
```code 
默认的配置主要来自于
org.springframework.boot.autoconfigure.security.SecurityProperties.User
public static class User{
    private String name = "user";
    private String password = UUID.randomUUID().toString();
    private List<String> roles = new ArrayList();
    private boolean passwordGenerated = true;
}

```
 4. 当然我们也可以在yml文件或者properties文件中配置账户和密码,配置如下:
```code
spring.security.user.name=admin
spring.security.user.password=123456
//spring-security的默认登录,被称为httpBasicLogin。显示它不是我们产品上想要的，我们前端一般是通过表单提交的方式进行用户登录验证的，所以我们就需要自定义自己的认证逻辑了。如果不想使用,可以这样配置
security.basic.enabled = false
```

> 自定义用户认证逻辑

* 说明:每个系统肯定是有自己的一套用户体系的，所以我们需要自定义自己的认证逻辑以及登录界面。这里我们需要先对SpringSecurity进行相应的配置
* 新建一个SecurityConfig类,代码如下:
```java
//表单验证
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin() //定义当需要用户登录时候,转到的登录页面.
            .loginPage("/login.html")  //设置登录页面
            .loginProcessinUrl("/user/login") //自定义的登录接口
            .add()
            .authorizeRequests() //定义那些URL需要被保护.哪些不需要被保护
            .antMatchers("/login.html").permitAll() //设置所有人都可以访问的登录页面
            .anyRequest() //任何请求,登录后可以访问
            .authenticated()
            .and()
            .csrf().disable(); //关闭csrf防护 csrf关闭是因为不使用session
    }
}
```
