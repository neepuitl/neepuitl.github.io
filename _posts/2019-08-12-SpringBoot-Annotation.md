---
layout:     post
title: SpringBoot注解详解
date: 2019-08-08 17:27:48
categories: Springboot
summary: 对Springboot注解的理解与使用
---
# 注解的含义
>Java 注解用于为 Java 代码提供元数据。作为元数据，注解不直接影响你的代码执行，但也有一些类型的注解实际上可以用于这一目的。Java 注解是从 Java5 开始添加到 Java 的。<br/>

这是比较官方的说法。于我而言，我更倾向于把注解理解成`代码的标签`,其作用就是对对象进行某些角度解释。
# 注解对于Springboot的重要性
注解是springboot的核心内容，在springboot中，通过各种组合注解，极大地简化了spring项目的搭建和开发。因此，我们需要知道主键的基本概念以及使用方法，才能够更加理解Springboot框架。
<hr/>

# 注解分类
## 1.按照声明周期

- 源码注解（仅在.java文件中存在，经过编译就不存在了）
- 编译时注解（在编译时起作用，即在.class文件中也仍然存在）
- 运行时注解（在运行阶段起作用，有时会影响程序的运行逻辑）

## 2.按照来源

- JDK自带的注解
- 第三方框架注解
- 自定义注解

## 3.元注解
 - 对注解进行注解的注解
 <hr/>

 # 注解列表 

- @Autowired
自动导入依赖的bean。

- @Bean：产生一个bean,并交给spring管理。相当于XML中配置的bean。

- @Component
泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。可配合CommandLineRunner使用，在程序启动后执行一些基础任务。

- @ComponentScan
组件扫描，可自动发现和装配一些Bean。

- @Configuration
等同于spring的XML配置文件；使用Java代码可以检查类型安全。

- @Controller：用于定义控制器类，在spring项目中由控制器负责将用户发来的URL请求转发到对应的服务接口（service层），一般这个注解在类中，通常方法需要配合注解@RequestMapping。


- @EnableAutoConfiguration
自动配置。你可以将@EnableAutoConfiguration或者@SpringBootApplication注解添加到一个@Configuration类上来选择自动配置。如果发现应用了你不想要的特定自动配置类，你可以使用@EnableAutoConfiguration注解的排除属性来禁用它们。

- @Import：用来导入其他配置类。

- @ImportResource：用来加载xml配置文件。

- @Inject：等价于默认的@Autowired，只是没有required属性；

- @JsonBackReference
解决嵌套外链问题。

- @JsonProperty 
此注解用于属性上，作用是把该属性的名称序列化为另外一个名称，如把trueName属性序列化为name：
``` java
@JsonProperty("name")
private String trueName; 
```
- @PathVariable
获取参数。

- @Qualifier：当有多个同一类型的Bean时，可以用@Qualifier(“name”)来指定。与@Autowired配合使用。@Qualifier限定描述符除了能根据名字进行注入，但能进行更细粒度的控制如何选择候选者，

- @Repository：使用@Repository注解可以确保DAO或者repositories提供异常转译，这个注解修饰的DAO或者repositories类会被ComponetScan发现并配置，同时也不需要为它们提供XML配置项。

- @RepositoryRestResourcepublic
配合spring-boot-starter-data-rest使用。

- @RequestMapping：提供路由信息，负责URL到Controller中的具体函数的映射。

- @ResponseBody：表示该方法的返回结果直接写入HTTP response body中，一般在异步获取数据时使用，用于构建RESTful的api。在使用@RequestMapping后，返回值通常解析为跳转路径，加上@esponsebody后返回结果不会被解析为跳转路径，而是直接写入HTTP response body中。比如异步获取json数据，加上@Responsebody后，会直接返回json数据。

- @Resource(name=”name”,type=”type”)：没有括号内内容的话，默认byName。与@Autowired干类似的事。

- @RestController
是@Controller和@ResponseBody的合集,表示这是个控制器bean,并且是将函数的返回值直 接填入HTTP响应体中,是REST风格的控制器。

- @Service：一般用于修饰service层的组件

- @SpringBootApplication
包含了@ComponentScan、@Configuration和@EnableAutoConfiguration注解。其中@ComponentScan让spring Boot扫描到Configuration类并把它加入到程序上下文。

- @Value：注入Spring boot application.properties配置的属性的值。
<hr/>

# 核心注解详解：
## @SpringBootApplication
申明让spring boot自动给程序进行必要的配置，这个配置等同于：@Configuration ，@EnableAutoConfiguration 和 @ComponentScan 三个配置。
示例代码：
```java
package com.example.myproject; 
import org.springframework.boot.SpringApplication; 
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication // same as @Configuration @EnableAutoConfiguration @ComponentScan 
public class Application { 
    public static void main(String[] args) { 
    SpringApplication.run(Application.class, args); 
    } 
}
```
## @ResponseBody：
表示该方法的返回结果直接写入HTTP response body中，一般在异步获取数据时使用，用于构建RESTful的api。在使用@RequestMapping后，返回值通常解析为跳转路径，加上@responsebody后返回结果不会被解析为跳转路径，而是直接写入HTTP response body中。比如异步获取json数据，加上@responsebody后，会直接返回json数据。该注解一般会配合@RequestMapping一起使用。示例代码：
``` java
@RequestMapping(“/test”) 
@ResponseBody 
    public String test(){ 
    return "index"; 
}
```

## @Controller：
用于定义控制器类，在spring 项目中由控制器负责将用户发来的URL请求转发到对应的服务接口（service层），一般这个注解在类中，通常方法需要配合注解@RequestMapping。示例代码：
``` java
@Controller
@RequestMapping("/hello")
public class HelloController {

    @GetMapping("/say/{id}")
    public String say(){
        //返回index.html
        return "index";
    }
}
```

## @RestController：
用于标注控制层组件(如struts中的action)，@ResponseBody和@Controller的合集。示例代码：
```java
@RestController
@RequestMapping("/hello")
public class HelloController {

    @GetMapping("/say/{id}")
    public String say(@RequestParam(value = "id",required = false,defaultValue = "0") Integer id){

        return "id:"+id;
    }

}
```
<hr/>

# JPA注解
需先在Maven中引入依赖
```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

- @Column：如果字段名与列名相同，则可以省略。

- @Entity：@Table(name=”“)：表明这是一个实体类。一般用于jpa这两个注解一般一块使用，但是如果表名和实体类名相同的话，@Table可以省略

- @GeneratedValue(strategy = GenerationType.SEQUENCE,generator = “repair_seq”)：表示主键生成策略是sequence（可以为Auto、IDENTITY、native等，Auto表示可在多个数据库间切换），指定sequence的名字是repair_seq。

- @Id：表示该属性为主键。

- @JoinColumn（name=”loginId”）:一对一：本表中指向另一个表的外键。一对多：另一个表指向本表的外键。

- @JsonIgnore：作用是json序列化时将Java bean中的一些属性忽略掉,序列化和反序列化都受影响。

- @MappedSuperClass:用在确定是父类的entity上。父类的属性子类可以继承。

- @NoRepositoryBean:一般用作父类的repository，有这个注解，spring不会去实例化该repository。

- @OneToOne、@OneToMany、@ManyToOne：对应hibernate配置文件中的一对一，一对多，多对一。

- @SequenceGeneretor(name = “repair_seq”, sequenceName = “seq_repair”, allocationSize = 1)：name为sequence的名称，以便使用，sequenceName为数据库的sequence名称，两个名称可以一致。

- @Transient：表示该属性并非一个到数据库表的字段的映射,ORM框架将忽略该属性。如果一个属性并非数据库表的字段映射,就务必将其标示为@Transient,否则,ORM框架默认其注解为@Basic。@Basic(fetch=FetchType.LAZY)：标记可以指定实体属性的加载方式
<hr/>

# springMVC相关注解
- @PathVariable:路径变量，示例代码
```java
RequestMapping(“user/get/mac/{macAddress}”) 
public String getByMacAddress(@PathVariable String macAddress){ 
//do something; 
} 
```
参数与大括号里的名字一样要相同。

- @RequestMapping：@RequestMapping(“/path”)表示该控制器处理所有“/path”的UR L请求。RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。 
用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。该注解有六个属性： 
params:指定request中必须包含某些参数值是，才让该方法处理。 
headers:指定request中必须包含某些指定的header值，才能让该方法处理请求。 
value:指定请求的实际地址，指定的地址可以是URI Template 模式 
method:指定请求的method类型， GET、POST、PUT、DELETE等 
consumes:指定处理请求的提交内容类型（Content-Type），如application/json,text/html; 
produces:指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回

- @RequestParam：用在方法的参数前面。示例代码：
```java
@RequestParam 
String a =request.getParameter(“a”)。
```
<hr/>

# 全局异常处理:

- @ControllerAdvice：包含@Component。可以被扫描到。统一处理异常。

- @ExceptionHandler（Exception.class）：用在方法上面表示遇到这个异常就执行以下方法。
<hr/>

# 参考文章：
[Spring Boot 注解—基本知识](https://www.jianshu.com/p/d74ed7374841)<br/>
[SpringBoot注解最全详解(整合超详细版本)](https://blog.csdn.net/weixin_40753536/article/details/81285046)<br/>
[[springBoot系列]--springBoot注解大全](https://www.cnblogs.com/tanwei81/p/6814022.html)


















