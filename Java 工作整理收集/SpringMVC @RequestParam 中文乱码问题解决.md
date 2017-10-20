SpringMVC @RequestParam 中文乱码问题解决
========================================

>客户端用GET方式请求SpringMVC时，如果用采用如下方式则中文没有出现乱码：
```java
http://127.0.0.1:8080/SpringMVCHibernate4/rest/user/getUserById0/100-张三  
@RequestMapping(value="/getUserById0/{id}-{name}",method=RequestMethod.GET)  
@ResponseBody   
public String getUserById0(@PathVariable Long id, @PathVariable("name")String userName) {  
    User user = new User();  
    user.setUserName(userName);  
    user.setId(id);  
    return GsonUtil.toJson(user);  
}  
```

>但是如果采用如下方式则中文会出现乱码：
```java
http://127.0.0.1:8080/SpringMVCHibernate4/rest/user/getUserById2?id=100&name=张三  
@RequestMapping(value="/getUserById2",method=RequestMethod.GET)  
@ResponseBody  
public  String getUserById2(@RequestParam Long id,   
    RequestParam("name") String userName) {  
    User user = new User();  
    user.setUserName(userName);//乱码  
    user.setId(id);  
    return GsonUtil.toJson(user);  
}  
```

>web.xml配置了字符编码过滤器，但是依然是乱码：
```xml
<filter>  
  <filter-name>encodingFilter</filter-name>  
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
      <init-param>  
          <param-name>encoding</param-name>  
          <param-value>UTF-8</param-value>  
      </init-param>  
  <init-param>  
     <param-name>forceEncoding</param-name>  
     <param-value>true</param-value>  
  </init-param>  
</filter>  
<filter-mapping>  
  <filter-name>encodingFilter</filter-name>  
  <url-pattern>/*</url-pattern>  
</filter-mapping>  
```

>web.xml配置了字符编码过滤器，但是依然是乱码：
```xml
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1"  redirectPort="8443" URIEncoding="UTF-8" />
```