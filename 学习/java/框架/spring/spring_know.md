[TOC]
##使用注解
```
<!-- 使用 annotation --> 
 <context:annotation-config />
 
 <!-- 使用 annotation 自动注册bean,并检查@Controller, @Service, @Repository注解已被注入 -->
 <context:component-scan base-package="*" />

``` 
##常见注解所对应的jar包
```
@Autowired
import org.springframework.beans.factory.annotation.Autowired;
spring-beans.jar


@Controller
import org.springframework.stereotype.Controller;
spring.jar或spring-context.jar
```
##常见注解
```
在持久层、业务层和控制层分别采用 @Repository、@Service 和 @Controller 对分层中的类进行注释，
而用 @Component 对那些比较中立的类进行注释. 这里就是说把这个类交给Spring管理，重新起个名字叫userManager，由于不好说这个类属于哪个层面，就用@Component
```