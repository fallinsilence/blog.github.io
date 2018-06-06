---
title: spring小结（一）———把类加入Spring工厂
date: 2017-09-17 18:49:24
categories: spring
tags: JavaEE框架
---
　　之前我们需要一个对象时，就得自己new一个新对象；现在我们把需要的类通过配置文件或注解方式放到spring工厂，它负责帮我们创建对象，我们需要哪一个类的对象时，就从工厂取出对象，我们无需再手动创建对象。
<!--more-->
　　那么如何把需要的类加入Spring工厂呢？我们有两种方式：配置文件方式和注解方式。

#### 配置文件方式（用于不是自己编写的类，无法在类上加注解)
beans.xml(spring配置文件)
```
    <!-- 加入约束  -->
    xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
                        http://www.springframework.org/schema/beans/spring-beans.xsd"
                        
    <!-- 通过bean标签可以注册一个类，由spring框架负责创建此类的一个对象 -->
    <!-- id:此对象的一个唯一标识，可以通过此id从spring工厂中获取一个对象 
    class:类的全路径 scope: prototype(原型) Singleton(单例，默认)-->
    <bean scope="prototype" id="userService" class="cn.itcast.spring.UserService" >
```
Singleton：单例-----同一个类，在spring工厂中只有唯一的一个实例(同一个对象)
Prototype：原型-----同一个类，在spring工厂中有多个实例
    

>  Action:一般使用原型
        Service:一般使用单例
        Dao:一般使用单例

##### 三种实例化bean的方式
1.调用无参构造方法创建对象
```
    //通过构造方法创建UserService对象
    <bean id="userService" class="cn.itcast.spring.UserService" />
```

2.使用静态工厂方法创建对象
```
    <bean id="userService" class="cn.itcast.spring.StaticUserServiceFactory" factory-method="createUserService" />

    //在StaticUserServiceFactory类创建UserService对象
    public class StaticUserServiceFactory {
        public static UserService createUserService(){
            return new UserService();
    }
}
```

3.使用实例工厂方法创建对象
```
    <bean id="factory" class="cn.itcast.spring.InstaceUserServiceFactory"></bean>
    <bean id="userService" factory-bean="factory" factory-method="createUserService"></bean>

    public class InstaceUserServiceFactory {
        public UserService createUserService(){
            return new UserService();
    }
}
```

  
#### 注解方式（用于自己编写的类）

1.在beans.xml中加入约束 

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation=" http://www.springframework.org/schema/beans 
                            http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context 
                            http://www.springframework.org/schema/context/spring-context.xsd">
```
2.在beans.xml中配置组件扫描和支持注解：
```
    <!-- 配置组件扫描:由spring框架负责扫描指定的包下的类,为该包下的添加了注解的类创建对象  -->
    <context:component-scan base-package="cn.itcast.spring.annotation"/>
    <!-- 加入spring的注解支持：引入spring的注解解析器 -->
    <context:annotation-config/>
```
3.编写一个类，在类上加入注解： 
```
//等价于在beans.xml中配置<bean id="" class="">

@Repository------Dao层
@Controller-----Action层
@Service-------Service层
@Component("test")//通用的，test唯一标识
四个注解作用一样，起区分作用

@Scope("prototype")//指定bean的作用域为原型方式
```

在工厂放入需要的类后，就可以在需要时取出类的对象了
```
    <!-- 读取beans文件，创建spring工厂 -->
    ApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
    <!-- 通过id从spring工厂获取一个对象 -->
    UserDao userDao = (UserDao) ctx.getBean("userDao");  //（userDao）类名，首字母小写
```

#### 使用多个XML文件
![](https://raw.githubusercontent.com/fallinsilence/usedPictures/master/moreXML.jpg)



