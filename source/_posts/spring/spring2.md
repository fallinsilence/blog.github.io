---
title: spring小结（二）————依赖注入
date: 2017-09-17 19:06:21
categories: spring
tags: javaEE框架
---
## DI（依赖注入）

　　之前我们写一个web项目时，在Servlet类中创建Service对象，调用service中的方法处理业务逻辑，在Service类中，又创建Dao对象，调用dao中的方法处理访问数据库的逻辑，这样一层一层的向下调用。这样做的结果是，类与类之间的耦合度太高，DI的目的就是降低耦合度。
<!--more-->
    
　　之前我们需要一个对象时，就得自己new一个新对象；现在我们把需要的类通过配置文件或注解方式放到spring工厂，它负责帮我们创建对象；我们在Servlet类中需要创建Service对象，可以说Servlet类依赖Service对象，现在有了Spring工厂，不需要我们创建Service对象，我们只需从Spring工厂拿Service对象即可，Spring工厂把Servlet类所依赖的Service对象给注入进来了，这就是依赖注入。
　　
依赖注入依然是两种方式：xml方式和注解方式

#### 注解方式

- setter方法注入---为属性提供相应的setter方法，通过调用此方法注入属性值

```
    <bean id="userDao2" class="cn.itcast.spring.di.UserDao">
        <property name="name" value="二麻子" />
        <property name="age" value="88" />
    </bean>
    
    private List<String> list;
    private Set<String> set;
    private String[] strs;
    private Map<String, String> map;
    private Properties pros;

<!-- 使用setter方法注入集合类型的属性 -->
    <bean id="userDao4" class="cn.itcast.spring.di.UserDao">
        //List
        <property name="list">
            <list>
                <value>value1</value>
                <value>value2</value>
                <value>value3</value>
            </list>
        </property>
        
        //Set
        <property name="set">
            <set>
                <value>v1</value>
                <value>v2</value>
                <value>v3</value>
            </set>
        </property>

        //数组
        <property name="strs">
            <list>
                <value>str1</value>
                <value>str2</value>
                <value>str3</value>
            </list>
        </property>

        //Map
        <property name="map">
            <entry key="k1" value="v1"></entry>
            <entry key="k2" value="v2"></entry>
            <entry key="k3" value="v3"></entry>
        </property>

        //properpies文件
        <property name="pros">
            <props>
                <prop key="key1">value1</prop>
                <prop key="key2">value2</prop>
            </props>
        </property>
    </bean>
```
- 构造方法注入----通过类的构造方法注入属性值
```
    <bean id="userDao1" class="cn.itcast.spring.di.UserDao">
        <constructor-arg index="0" value="小白" />
        <constructor-arg index="1" value="20" />
    </bean>
```

#### 注解方式
对于Bean属性的依赖注入分为两类，一类是对于属性是String类型或者基本数据类型Spring容器提供了@Value这个注解，另一类是对于属性是对象的提供了@Autowired和@Resource这两个注解。

1．Resource注解:可以按照对象的类型注入，也可以按照对象的唯一标识注入
  @Resource(name="userDao")  //从工厂拿出userDao对象
  
2．AutoWired注解：只能按照对象的类型注入，如果需要按照对象的唯一标识注入，需要使用另外一个注解@Qualifier(value="userDao")使用
注意：Resource和AutoWired使用在属性上