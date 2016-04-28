---
layout:     post
title:      "Spring AOP "
subtitle:   " "
date:       2016-04-06 13:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb3.jpg"
catalog:    true
tags:
    - Spring
    - Java
    - AOP
    - 后台开发框架
    
---

### Spring AOP(前置通知&后置通知)

#### 前置通知



#### 配置

- 加入jar包

	aopalliance-1.0.0.jar
	
	aspectj.weaver-1.8.0.jar
	
	spring-aop-4.1.5.RELEASE.jar
	
	spring-aspects-4.1.5.RELEASE.jar
	
    commons.logging-1.1.1.jar
	
    spring-beans-4.1.5.RELEASE.jar
	
    spring-context-4.1.5.RELEASE.jar
	
    spring-core-4.1.5.RELEASE.jar
	
    spring-expression-4.1.5.RELEASE.jar




- 在配置文件中加入aop的命名空间

	xmlns:aop="http://www.springframework.org/schema/aop"
	
- 基于注解的方式

	1. 在配置文件中加入如下配置：使AspjectJ 注解起作用：自动为匹配的类生成代理对象
	
		<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
		
	2. 把横切关注点的代码抽象到切面的类中
	
		
		a. 切面首先是一个IOC容器中的bean，即加入@Component注解
		
		b. 切面海需要加入@Aspect注解
		
	3. 在类中声明各种通知
	
		a. 生命一个方法
		b. 在方法前加入@Before注解
		
	4. 可以在通知方法中声明一个类型为JoinPoint的参数，然后就能访问链接的细节，如方法名称和参数值
	
	
##### 代码如下

```
//把这个类声明为一个切面：需要把该类放入到IOC容器中、再声明为一个切面
@Aspect
@Component
public class LoggingAspect {
	/**
	 * 	在com.wyw.spring.ioc.impl.ArithmeticCalculator接口的每一个实现类的每一个方法开始之前执行的一段代码
	 */
	//声明该方法是一个前置通知：在目标方法开始之前执行
	@Before("execution(public int com.wyw.spring.ioc.impl.ArithmeticCalculator.*(..))")
	public void beforeMethod(JoinPoint joinpoint){
		String methodName = joinpoint.getSignature().getName();
		List<Object> args = Arrays.asList(joinpoint.getArgs());
		System.out.println("the method " + methodName + " begins with "+args);
	}
	//在方法正常结束后执行的代码
	//返回通知是可以访问到方法的返回值的
	@AfterReturning(value = "execution(public int com.wyw.spring.ioc.impl.*.*(..))",
			returning = "result")
	public void afterReturning(JoinPoint joinPoint,Object result){
		String methodName = joinPoint.getSignature().getName();
		System.out.println("the method " + methodName + " ends with " + result);
		
	}
	//在后置通知中不能访问目标方法执行的结果
	//声明该方法是一个后置通知：在目标方法开始之后（无论是否发生异常）执行
	@After("execution(public int com.wyw.spring.ioc.impl.*.*(..))")

	public void afterMethod(JoinPoint joinPoint){
		String methodName = joinPoint.getSignature().getName();
		System.out.println("the method " + methodName + " ends ");

	}
	//在方法异常时会执行的代码
	//可以访问到方法出现异常对象；且可以指定出现特定异常时再执行代码
	@AfterThrowing(value = "execution(public int com.wyw.spring.ioc.impl.*.*(..))",
			throwing="ex")
	public void afterThrowing(JoinPoint joinPoint,Exception ex){		
		String methodName = joinPoint.getSignature().getName();
		System.out.println("the method " + methodName + " occurs exception with "+ ex);
	}
	//环绕通知需要携带ProceedingJoinPoint类型的参数
	//环绕通知类似动态代理的全过程：roceedingJoinPoint类型的参数可以决定是否执行目标方法
	//且环绕通知必须有返回值，返回值即为目标方法的返回值
	/*@Around("execution(public int com.wyw.spring.ioc.impl.*.*(..))")
	public Object aroundMethod(ProceedingJoinPoint pjp){
		Object result = null;
		String methodName = pjp.getSignature().getName();
		List<Object> args = Arrays.asList(pjp.getArgs());
		try {
			//前置通知
			System.out.println("the method " + methodName + " begins with "+ args);
			result = pjp.proceed();
			//返回通知
			System.out.println("the method " + methodName + " ends with " + result);

		} catch (Throwable e) {
			//异常通知
			System.out.println("the method " + methodName + " occurs exception with "+ e);
			throw new RuntimeException(e);
		}
		//后置通知
		System.out.println("the method " + methodName + " ends ");

		return result;
		
	}*/
```

##### ArithmeticCalculator接口

```
public interface ArithmeticCalculator {
	public int add(int i,int j);
	public int sub(int i,int j);
	public int mul(int i,int j);
	public int div(int i,int j);

}
```

##### ArithmeticCalculator实现类


```
@Component
public class ArithmeticCalculatorImp implements ArithmeticCalculator {

	@Override
	public int add(int i, int j) {
		int result = i + j;
		return result;
	}

	@Override
	public int sub(int i, int j) {
		int result = i + j;
		return result;
	}

	@Override
	public int mul(int i, int j) {
		int result = i + j;
		return result;
	}

	@Override
	public int div(int i, int j) {
		int result = i + j;
		return result;
	}

}
```

##### spring配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.1.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd">

	<!-- 配置自动扫描的包 -->
	<context:component-scan base-package="com.wyw.spring.ioc.impl"></context:component-scan>

	<!-- 使AspjectJ 注解起作用：自动为匹配的类生成代理对象 -->
	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>


```

##### 测试类


```

public class Main {

	public static void main(String[] args) {
		
		//1. 创建Spring的IOC容器
		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
		//2.从IOC容器中获取bean实例
		ArithmeticCalculator arithmeticCalculator = ctx.getBean(ArithmeticCalculator.class);
		//3.使用bean
		int result = arithmeticCalculator.add(3, 6);
		System.out.println("result: "+result);
		
		result = arithmeticCalculator.div(18, 6);
		System.out.println("result:" + result);
		
	}
}
```

##### 运行结果

```
the method add begins with [3, 6]
the method add ends 
the method add ends with 9
result: 9
the method div begins with [18, 3]
the method div ends 
the method div ends with 6
result:6
```
	
	
	



