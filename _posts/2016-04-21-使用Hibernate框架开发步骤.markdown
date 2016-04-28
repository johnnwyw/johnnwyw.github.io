---
layout:     post
title:      "使用Hibernate框架开发步骤 "
subtitle:   " "
date:       2016-04-21 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kuaidi.jpg"
catalog:    true
tags:
    - Hibernate
    - Java
    - 后台开发框架
    
---

### 使用Hibernate框架开发步骤

##### 1. 创建Hibernate配置文件

##### 2. 创建持久化类

##### 3. 创建对象－关系映射文件

##### 4. 通过HibernateAPI编写访问数据库的代码

![hibernate](/img/hibernate.jpg)

### 具体实现代码

##### 1. 创建Hibernate配置文件

###### Hibernate配置文件的两个配置项

* hbm2ddl.auto: 该属性可以帮助程序猿实现正向工程，即有java代码生成数据库脚本，进而生成具体的表结构。取值有create|update|create-drop|validate

	1. create:会根据.hbm.xml文件来生成数据表，但是每次运行都会删除上一次的表，重新生成表（即使没有变化）。
	
	2. create-drop:会根据.hbm.xml文件来生成数据表，但SessionFactory关闭时，表就会自动删除。
	
	3. update: **最常用的属性值** ，也会根据.hbm.xml文件来生成数据表，但若.hbm.xml文件和数据库中的表结构不同，Hibernate将会更新数据表结构，但不会删除已有的行和列。
	
	4. validate: 会和数据库中的表进行比较，如果.hbm.xml文件的列在数据表中不存在，则抛出异常。
	
* format_sql: 是否将SQL转化为格式良好的SQL，取值true｜false


```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
		"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
		"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
    <!-- 配置连接数据库的基本信息 -->
    <property name="connection.username">root</property>
    <property name="connection.password">root</property>
    <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
    <property name="connection.url">jdbc:mysql:///hibernate</property>

	<!-- 配置 hibernate 的基本信息 -->
	<!-- hibernate所使用的数据库方言-->
	
	<property name="dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>
	
	<!-- 执行操作时是否在控制台打印SQL -->
	<property name="show_sql">true</property>
	
	<!-- 是否对SQL进行格式化 -->
	<property name="format_sql">true</property>
	
	<!-- 指定自动生成数据表的策略 -->
	<property name="hbm2ddl.auto">update</property>
	
	<!-- 指定关联的.hbm.xml文件 -->
	<mapping resource="com/wyw/hibernate/helloworld/News.hbm.xml"/>
    
    </session-factory>
</hibernate-configuration>
```

##### 2. 创建持久化类

Hibernate不要求持久化类继承任何父类或实现接口，这可以保证代码不被污染，这就是Hibernateb被称为低侵入式设计的原因

***注意事项***

1. 提供一个无参构造器

	使得hibernate可以使用Constructor.newinstance()实例化一个持久化类

2. 提供一个标志属性（identifier property）

	通常映射为数据库的主键字段，如果没有该属性，一些功能将不起作用，如：Session.SaveOrupdaate()
	
3.  为类的持久化类字段声明访问方法（get/set）

	Hibernate对JavaBean风格的属性实行持久化
	
4. 使用非final类

	在运行时生成的代理是Hibernate的一个重要功能，如果持久化类没有实现类和接口，Hibernate使用	CGLIB生成代理，如果使用的是final类，则无法生成CGLIB代理
	
5. 重写equals方法和hashcode方法

	如果需要把持久化类的实例放到Set中（当需要关联映射时），则重写者两个方法





```
public class News {
	private Integer id;
	private String title;
	private String author;
	private Date date;

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getAuthor() {
		return author;
	}

	public void setAuthor(String author) {
		this.author = author;
	}

	public Date getDate() {
		return date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

	public News(String title, String author, Date date) {
		super();
		this.title = title;
		this.author = author;
		this.date = date;
	}

	public News() {
		// TODO Auto-generated constructor stub
	}

	@Override
	public String toString() {
		return "News [id=" + id + ", title=" + title + ", author=" + author + ", date=" + date + "]";
	}

}
```

##### 3. 创建对象－关系映射文件

Hibernate采用XML格式的文件指定对象和关系数据之间的映射，在运行时Hibernate将根据这个映射文件生成各种SQL语句

***映射文件的扩展名为.hbm.xml***

**指定主键的生成方式：native，使用数据库的底层生成方式**


```
<hibernate-mapping>
    <class name="com.wyw.hibernate.helloworld.News" table="NEWS">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="native" />
        </id>
        <property name="title" type="java.lang.String">
            <column name="TITLE" />
        </property>
        <property name="author" type="java.lang.String">
            <column name="AUTHOR" />
        </property>
        <property name="date" type="java.sql.Date">
            <column name="DATE" />
        </property>
    </class>
</hibernate-mapping>

```

##### 4. 通过HibernateAPI编写访问数据库的代码

* Configuration类负责管理Hibernate的配置信息。包括如下内容：

	1. Hibernate运行的底层信息：数据库的URL、用户名、密码、JDBC驱动类，数据库Dialect，数据库连接池等（对应hibernate.cfg.xml文件）。
	2. 持久化类与数据表的映射关系（*.hbm.xml文件）
	
* 创建Configuration的两种方式

	1. 属性文件（hibernate.properties）:Configuration cfg = new Configuration()
	
	2. Xml文件(hibernate.cfg.xml):Configuration cfg = new Configuration().configure()
	
	3. Configuration 的configure方法还支持带参数的访问：
	
		* File file = new File("*.xml")
		* Configuration cfg = new Configuration().configure(file);
	
*  SessionFactory接口针对单个数据库映射关系经过编译后的内存镜像，是线性安全的

	1. SessionFactory对象一旦构造完毕，即被赋予特定的信息
	
	2. SessionFactory是生成Session的工厂
	
	3. 构造SessionFactory很耗资源，一般情况下一个应运中只初始化一个SessionFactory对象
	
	4. Hibernate4新增了一个ServiceRegistry接口，任何基于Hibernate的配置和服务都需要在该对象中注册后才能有效
	
* Session接口

	Session接口是应用程序和数据库之间交互操作的一个**单线程操作**，是Hibernate的运作中心，所有持久化操作必须在session的管理下才可以进行持久化操作，此对象的生命周期很短，Session对象有一个一级缓存，显式执行flush之前，所有的持久化层操作的数据都缓存在session对象处。**相当于JDBC的Connection**
	
![session](/img/session.jpg)
	
	1. 持久化类与Session关联起来后就具有了持久化的能力
	
	2. Session类的方法
	
		*  取得持久化对象的方法 get(),load()
		
		* 持久化对对象的保存、更新和删除：save(),uodate(),saveOrUpdate(),delete()
		
		* 开启事务：beginTransaction()
		
		* 管理Session的方法：isOpen(),flush(),clear(),evict(),close()等
		
* Transaction（事务）


	1.  代表一次原子操作，它具有数据库事务的概念。所有的持久层斗应该在该事务管理下进行，即使是只读操作。
	
		* Tranxaction tx = session.beginTransaction()
		
	2. 常用方法
	
		* commit():提交相关联的session实例
		
		* rollback():撤销事务操作
		
		* wasCommitted():检查事务是否被提交


```
public class HibernateTest {

	@Test
	public void test() {

		// 1.创建一个SessionFactory对象

		SessionFactory sessionFactory = null;

		// 1).创建Configuration对象：对应hibernate的基本配置信息和关系映射信息
		Configuration configuration = new Configuration().configure();

		// 4.0之前创建
		// sessionFactory = configuration.buildSessionFactory();

		// 2)创建一个ServiceRegistry对象：hibernate4.x新添加的对象
		// hinbernate的任何配置和服务都需要在该对象中注册后才能有效

		ServiceRegistry serviceregistry = 
				new ServiceRegistryBuilder().applySettings(configuration.getProperties())
				.buildServiceRegistry();

		sessionFactory = configuration.buildSessionFactory(serviceregistry);

		// 2.创建一个Session对象
		Session session = sessionFactory.openSession();
		
		// 3.开启事务
		Transaction transaction = session.beginTransaction();

		// 4.执行保存操作
		News news = new News("Java","wyw",new Date(new java.util.Date().getTime()));
		session.save(news);

		// 5.提交事务
		transaction.commit();

		// 6.关闭Session
		session.close();

		// 7.关闭SessionFactory对象
		sessionFactory.close();

	}

}

```
