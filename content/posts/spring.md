---
title: Spring
date: 2020-02-21T19:20:57+08:00
lastmod: 2020-02-21T19:20:57+08:00
author: spc
cover: /img/landscape16.jpg
categories: ["java"]
tags: ["spring"]
# showcase: true
draft: false
---

spring? , ?要用spring , Spring怎么用? , 怎么实现？


<!--more-->

# spring 

* Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架。
  * Spring MVC 基于Spring和Servlet的Web应用框架
    * Spring Boot 集成度和自动化程度更高

servlet server  ~let 是小的意思  
servlet 小服务器  代表例子Tomcat Jetty  
servlet把http请求 封装成对象 给webapp  

## 没spring前
* main
  * 方便但多了代码是灾难
* model(模块化)
  * 方便维护，但依赖关系复杂


## spring容器


* bean是豆意思，在spring是一个容器中最小工作单元，其实是一个java对象，
* BeanFactory/ApplicationContext
   * 容器本身对应的java对象
* 依赖注入（DI） Dependency Injection
  * （spring帮你实例化对象中加些属性，那些属性有你xml，properties等等写上的相关依赖）
  * 都是单例的？？
  * @Autowired @Resource @Inject 
  * 用Autowired就要在xml 加<context:autotation-config />
* 控制反转（IoC）Inversion of Control
  * 自己只需要声明什么依赖什么，有东西会帮你自动组装好



使用步骤
bean id= class=…   > 在xml里面 
依赖要而外配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" 
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd 
       http://www.springframework.org/schema/context 
       http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 识别@Autowried -->
    <context:autotation-config />  
    <!-- 配置bean -->
    <bean id="id名字" class="全限定类名"/>
    <bean id="id名字" class="全限定类名"/>
    <bean id="id名字" class="全限定类名"/>
   
</beans>
```



不会自己手写ioc容器，它的步骤是什么？
1. 定义Bean  (properties/xml/...)
2. 加载bean定义  
3. 实例化bean   (用map存)
4. 查找依赖，实现自动注入(动态字节码加属性)

```java
//projectName: simple-ioc0-container
public class MyIoCContainer {
    // 实现一个简单的IoC容器，使得：
    // 1. 从beans.properties里加载bean定义
    // 2. 自动扫描bean中的@Autowired注解并完成依赖注入
    public static void main(String[] args) {
        MyIoCContainer container = new MyIoCContainer();
        container.start();
        OrderService orderService = (OrderService) container.getBean("orderService");
        orderService.createOrder();
    }

    Map<String, Object> beans = new HashMap<>();

    // 启动该容器
    public void start() {
        Properties properties = new Properties();
        try {
            properties.load(MyIoCContainer.class.getResourceAsStream("/beans.properties"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        //往beans加入实例对象
        //beanName用的是 属性名而不是类型名称，看properties就知道了
        properties.forEach((beanName, beanClass) -> {
            try {
                Class<?> klass = Class.forName((String) beanClass);
                Object beanInstance = klass.getDeclaredConstructor().newInstance();
                beans.put((String) beanName, beanInstance);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        });
        beans.forEach(this::dependencyInject);
    }
    //只用Autowried依赖来举例子（实际上还有很多依赖）
    private void dependencyInject(String beanName, Object beanInstance) {
        List<Field> dependencyInject = Stream.of(beanInstance.getClass().getDeclaredFields())
                .filter(field -> field.getAnnotation(Autowired.class) != null)
                .collect(Collectors.toList());

        dependencyInject.forEach(field -> {
            try {
                String fieldName = field.getName();
                Object dependencyBeanInstance = beans.get(fieldName);
                field.setAccessible(true);
                field.set(beanInstance, dependencyBeanInstance);
            } catch (IllegalAccessException e) {
                throw new RuntimeException(e);
            }
        });
    }

    // 从容器中获取一个bean
    public Object getBean(String beanName) {
        return beans.get(beanName);
    }
}
```

Bean的定义除获取xml还有java支持的properties


### 默认情况下是不给修改private成员，怎么办？
Field.setAccessible(true)

### 在创建实例中的循环依赖是没办法解决的

