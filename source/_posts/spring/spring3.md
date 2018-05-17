---
title: spring总结（三）———— AOP
date: 2017-09-17 19:10:45
tags: JavaEE框架
categories: spring
---

# AOP（面向切面编程）
引用知乎上的一段话来解释AOP：
<!--more-->
> 这种在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。

> AOP 实现的关键就在于 AOP 框架自动创建的 AOP 代理，AOP代理则可分为静态代理和动态代理两大类，其中静态代理是指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强；而动态代理则在运行时借助于 JDK 动态代理、CGLIB 等在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强。
Spring AOP是运行时增强，而AspectJ是编译时增强。
转载链接：http://www.jianshu.com/p/fe8d1e8bd63e

## AOP相关术语解释
+ JoinPoint(连接点)：被拦截的所有方法
+ PointCut(切入点)：指我们对哪些JoinPoint拦截的定义
+ Advice(通知)：指拦截到JoinPoint后所要做的事
+ Target(目标对象)：代理的目标对象
+ Proxy(代理对象)：一个类被AOP增强后，就产生一个结果代理类
+ Aspect(切面)：是切点和通知的结合


## 静态代理
需要增强的类（目标对象），和增强了目标对象的类（代理对象）必须继承同一个接口。
　　目标对象------实现上面的接口的一个实现类
　　代理对象------实现上面的接口的一个实现类,持有目标对象的引用,在调用目标对象方法前后加入代码。
但这样的弊端就是，每增强一个方法，都需要创建一个代理类，代码任务繁重。
所以引入了自动代理，jvm自动帮我们生成代理类。

## 动态代理的两种方式
### JDK代理
使用jdk的动态代理技术可以为指定的目标对象创建一个代理对象，**要求目标对象必须至少实现一个接口**
```
 　　　  // 创建一个目标对象
		final IUserDao target = new UserDaoImpl();
		// 为target创建一个代理对象
		IUserDao proxy = (IUserDao) Proxy.newProxyInstance(target.getClass()
				.getClassLoader(), target.getClass().getInterfaces(),
				new InvocationHandler() {
					// 使用此方法在代理对象中拦截目标对象的方法
					public Object invoke(Object proxy, Method method,
							Object[] args) throws Throwable {
						Object object = method.invoke(target, args);// 调用目标对象的方法
						System.out.println("在代理对象中拦截到了一个目标对象的方法："
								+ method.getName());// 通知
						return object;
					}
				});
		/*
		 * proxy为代理类，执行下列三个方法，都会执行invoke方法，
		 * 在该invoke方法中，先调用原先的方法，再打印一行字，增强了原对象方法
		 */
		proxy.save("小明");
		proxy.delete();
		proxy.update();
```

### cglib代理
使用cglib创建代理对象，不要求目标对象必须实现接口，要求目标对象不能是final的类,**使用cglib创建的代理对象是目标对象类的一个子类对象**
```
 //创建一个目标对象
	final UserDao target = new UserDao();
	//创建一个用于产生代理对象的实例
	Enhancer en = new Enhancer();
    //指定为谁创建代理对象
	en.setSuperclass(target.getClass());
	en.setCallback(new MethodInterceptor() {
    //拦截的方法
    public Object intercept(Object obj, Method method, Object[] args,
		MethodProxy proxy) throws Throwable {
		System.out.println("在代理对象中拦截到了一个目标对象的方法：" + method.getName());
		return method.invoke(target, args);
		}
		});
		//创建代理对象
		UserDao proxy = (UserDao) en.create();
		proxy.save("xiaobai");
		proxy.delete();
		proxy.update();
```

因Spring AOP使用效果不佳，不在此介绍，我们学习更优秀的**aspectJ框架**实现AOP

# aspectJ(比Spring AOP更优秀的AOP实现框架)

## 使用XML配置aspectJ
1.在spring配置文件中引入aop的约束头信息
```
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	   xmlns:context="http://www.springframework.org/schema/context"
	   xmlns:aop="http://www.springframework.org/schema/aop"
	   xsi:schemaLocation=" http://www.springframework.org/schema/beans 
       				http://www.springframework.org/schema/beans/spring-beans.xsd
				http://www.springframework.org/schema/context 
				http://www.springframework.org/schema/context/spring-context.xsd
				http://www.springframework.org/schema/aop 
				http://www.springframework.org/schema/aop/spring-aop.xsd
```
2.在spring配置文件中开启aspectJ框架的自动代理功能
```
<!-- 开启自动代理功能 -->
	<aop:aspectj-autoproxy/>
```
3.编写一个目标类
4.编写一个切面类
5.在beans.xml中注册目标对象和切面类
```
	<!-- 注册目标对象 -->
	<bean id="userDao" class="cn.itcast.aop.aspectJ.xml.UserDao"/>
	<!-- 注册切面类 -->
	<bean id="myAdvisor" class="cn.itcast.aop.aspectJ.xml.MyAdvisor"/>
```
6.使用框架提供的方式配置AOP切面
```
<!-- 使用xml方式配置aop切面 -->
	<aop:config>
		<!-- 切面 = 通知 +切点 -->
		<aop:aspect id="myAspect" ref="myAdvisor">
			<!-- 定义切点表达式 -->
			<aop:pointcut expression="execution(* cn.itcast.aop.aspectJ.xml.*Dao.*(..))"
				 id="myPointCut"/>
			<!-- 前置通知 -->
			<aop:before method="before1" pointcut-ref="myPointCut"/>
			<!-- 后置通知 -->
			<aop:after-returning method="afterReturing1" returning="ret" pointcut-ref="myPointCut"/>
			<!-- 环绕通知 -->
			<aop:around method="around1" pointcut-ref="myPointCut"/>
			<!-- 异常通知 -->
			<aop:after-throwing method="afterThrowing1" throwing="ex" pointcut-ref="myPointCut"/>
			<!-- 最终通知 -->
			<aop:after method="after1" pointcut-ref="myPointCut"/>
		</aop:aspect>
	</aop:config>
```

## 注解方式配置aspectJ
1.在spring配置文件中引入aop的约束头信息
```
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	   xmlns:context="http://www.springframework.org/schema/context"
	   xmlns:aop="http://www.springframework.org/schema/aop"
	   xsi:schemaLocation=" http://www.springframework.org/schema/beans 
       				http://www.springframework.org/schema/beans/spring-beans.xsd
				http://www.springframework.org/schema/context 
				http://www.springframework.org/schema/context/spring-context.xsd					
				http://www.springframework.org/schema/aop 						
				http://www.springframework.org/schema/aop/spring-aop.xsd
```
2.在spring配置文件中开启aspectJ框架在自动代理功能
```
<!-- 开启自动代理功能 -->
	<aop:aspectj-autoproxy/>
```
3.编写一个目标类
4.编写一个切面类，在类上使用@Aspect注解，在类的方法上使用@Before注解定义前置通知
```
public class MyAdvisor {	
    @Pointcut(value="execution(* cn.hugang.aop.aspectJ.annotation.*Dao.*(..))")
	public void myPointCut(){};
	
	// 定义一个前置通知方法,通过切点表达式描述要拦截的目标对象的方法
	@Before(value = "MyAdvisor.myPointCut()")//引用上面定义的切点表达式
	public void before1(JoinPoint jp) {            //连接点对象
		//System.out.println("【前置通知】拦截的的方法：" + jp.getSignature().getName());
	}
｝
```
5.在spring配置文件中注册目标对象和切面类
6.读取spring配置文件，从spring工厂中获取一个代理对象
```
    ApplicationContext ctx = new ClassPathXmlApplicationContext("cn/hugang/aop/aspectJ/annotation/beans.xml");
		UserDao userDao = (UserDao) ctx.getBean("userDao");
		userDao.save("");
		userDao.delete();
		userDao.update();
		System.out.println(userDao.getClass().getSimpleName())
```
7.后置，环绕，异常，最终通知
```
	//统一抽取一个切点表达式
	@Pointcut(value="execution(* cn.hugang.aop.aspectJ.annotation.*Dao.*(..))")
	public void myPointCut(){};
	
	//后置通知
//returning="ret"   ret:拦截到的方法执行后的返回值
	@AfterReturning(value="MyAdvisor.myPointCut()",returning="ret")
	public void afterReturing1(JoinPoint jp,Object ret){
		//System.out.println("【后置通知】拦截的的方法：" + jp.getSignature().getName() + ",此方法的执行结果为：" + ret);
	}
	
	//环绕通知
	@Around(value="MyAdvisor.myPointCut()")
	public Object around1(ProceedingJoinPoint pjp) throws Throwable{
		//System.out.println("【环绕通知】前");
		//必须在环绕通知中手动调用目标对象的方法，否则目标对象的方法将被拦截，不会执行
		Object value = pjp.proceed();//调用目标对象的方法
		//System.out.println("【环绕通知】后");
		return value;
	}
	
	//异常通知
	@AfterThrowing(value="MyAdvisor.myPointCut()",throwing="ex")  //ex:异常的信息
	public void afterThrowing1(JoinPoint jp,Throwable ex){
		System.out.println("【异常通知】执行了，拦截到的目标方法为：" + jp.getSignature().getName() + ",异常信息为：" + ex);
	}

	//最终通知
	@After(value="MyAdvisor.myPointCut()")
	public void after1(JoinPoint jp){
		System.out.println("【最终通知】拦截到目标方法：" + jp.getSignature().getName());
	}
```