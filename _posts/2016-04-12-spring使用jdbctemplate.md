---
layout:     post
title:      "Spring JdbcTemplate "
subtitle:   " "
date:       2016-04-12 08:00:00
author:     "Johnnwen"
header-img: "img/post-bg-kb4.jpg"
catalog:    true
tags:
    - Spring
    - Java
    - JdbcTemplate
    - 后台开发框架
    
---

### Spring使用JdbcTemplate

##### 配置数据库资源文件db.properties

```
user1=root
password=root
driverclass=com.mysql.jdbc.Driver
jdbcurl=jdbc:mysql:///product
initPoolSize=5
maxPoolSize=10
```

#####  Spring配置文件


```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd">

	<context:component-scan base-package="com.wyw.spring.jdbc"></context:component-scan>
	<!-- 导入资源文件 -->
	<context:property-placeholder location="classpath:db.properties"/>
	<!-- 配置c3p0数据源 -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="user" value="${user1}"></property>
		<property name="password" value="${password}"></property>
		<property name="driverClass" value="${driverclass}"></property>
		<property name="jdbcUrl" value="${jdbcurl}"></property>
		
		<property name="initialPoolSize" value="${initPoolSize}"></property>
		<property name="maxPoolSize" value="${maxPoolSize}"></property>
	</bean>
	
	<!-- 配置Spring的JdbcTemplate -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
</beans>
```

##### UserInfo类

```

public class UserInfo {
	private String name;
	private String password;
	private String realname;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getRealname() {
		return realname;
	}
	public void setRealname(String realname) {
		this.realname = realname;
	}
	@Override
	public String toString() {
		return "UserInfo [name=" + name + ", password=" + password
				+ ", realname=" + realname + "]";
	}
}

```

##### UserInfoDao类

```
@Repository
public class UserInfoDao {
	
	
	/**
	 * JdbcTemplate类被设计成为线程安全的，所以可以在IOC容器中声明它的单个实例，并将这个实例注入到所有的DAO实例中
	 * 不推荐使用JdbcDaoSupport,而推荐JdbcTempate作为Dao类的成员变量
	 */
	@Autowired
	private JdbcTemplate jdbcTemplate;
	
	public UserInfo get(String name){
		String sql = "select username as \"name\",password,realname from userinfo where username = ?";
		//UserInfo userInfo = jdbcTemplate.queryForObject(sql, UserInfo.class,"admin");
		RowMapper<UserInfo> rowMapper = new BeanPropertyRowMapper<UserInfo>(UserInfo.class);
		UserInfo userInfo = jdbcTemplate.queryForObject(sql, rowMapper,name);
		return userInfo;
	}
}
```
##### JDBCTest测试类

```

public class JDBCTest {
	private ApplicationContext ctx = null;
	private JdbcTemplate jdbcTemplate = null;
	private UserInfoDao userInfoDao = null;
	
	{
		ctx = new ClassPathXmlApplicationContext("beans-properties.xml");
		jdbcTemplate = (JdbcTemplate)ctx.getBean("jdbcTemplate");
		userInfoDao = ctx.getBean(UserInfoDao.class);
		
	}
	@Test
	public void testUserInfoDao(){
		System.out.println(userInfoDao.get("admin"));
	}
	/**
	 * 获取单个列的值，或做统计查询
	 * 使用queryForObject(String sql, Class<Long> requiredType) 方法
	 */
	@Test
	public void testQueryForObject2(){
		String sql = "select count(username) from userinfo";
		long count = jdbcTemplate.queryForObject(sql, Long.class);
		System.out.println(count);
		
	}
	
	/**
	 * 查到实体类的集合
	 * 注意：调用的不是queryForList方法
	 */
	@Test
	public void testQueryForList(){
		String sql = "select username as \"name\",password,realname from userinfo";
		RowMapper<UserInfo> rowMapper = new BeanPropertyRowMapper<UserInfo>(UserInfo.class);
		List<UserInfo> userInfos = jdbcTemplate.query(sql, rowMapper);
		System.out.println(userInfos);

	}
	/**
	 * 从数据库中获取一条记录，实际获取对应的一个对象
	 * 注意：不是调用的JdbcTemplate.queryForObject(String sql, Class<UserInfo> requiredType, Object... args)方法
	 * 而需要调用JdbcTemplate.queryForObject(String sql, RowMapper<UserInfo> rowMapper, Object... args)方法
	 * 其中的RowMapper指定如何去映射结果集的行，常用的实现类为BeanPropertyRowMapper
	 * 使用SQL中列的别名完成列名和类的属性名的映射，例如username name
	 * 不支持级联属性，JdbcTemplate是一个JDBC小工具，而不是ORM框架
	 */
	@Test
	public void testQueryForObject(){
		String sql = "select username as \"name\",password,realname from userinfo where username = ?";
		//UserInfo userInfo = jdbcTemplate.queryForObject(sql, UserInfo.class,"admin");
		RowMapper<UserInfo> rowMapper = new BeanPropertyRowMapper<UserInfo>(UserInfo.class);
		UserInfo userInfo = jdbcTemplate.queryForObject(sql, rowMapper,"admin");
		
		System.out.println(userInfo);
		
	}
	/**
	 * 执行批量更新：批量的INSERT UPDATE DELETE
	 * 最后一个参数是Object[]的List类型：因为修改一条记录需要一个Object数组,多条就需要多个Object数组
	 */
	@Test
	public void testBatchUpdate(){
		String sql = "insert into userinfo(username,password,realname) values(?,?,?)";
		
		List<Object[]> batchArgs = new ArrayList<Object[]>();
		batchArgs.add(new Object[]{"AA","123","aa"});
		batchArgs.add(new Object[]{"BB","123","bb"});
		batchArgs.add(new Object[]{"CC","123","cc"});
		batchArgs.add(new Object[]{"DD","123","dd"});
		batchArgs.add(new Object[]{"EE","123","ee"});
		batchArgs.add(new Object[]{"FF","123","ff"});

		jdbcTemplate.batchUpdate(sql,batchArgs);
		
	}
	/**
	 * 执行INSERT UPDATE DELETE
	 */
	@Test
	public void testUpdate(){
		String sql = "UPDATE userinfo set realname = ? WHERE username = ?";
		jdbcTemplate.update(sql,"wyw","admin");
	}

	@Test
	public void test() throws SQLException {
		DataSource dataSource = (DataSource)ctx.getBean("dataSource");
		System.out.println(dataSource.getConnection());
		
	}

}
```

#####  测试方法的输出

* testUserInfoDao()


> UserInfo [name=admin, password=admin, realname=wyw]


* testQueryForObject2()


> 7


* testQueryForList()


>
 [UserInfo [name=AA, password=123, realname=aa], UserInfo [name=admin, password=admin, realname=wyw], UserInfo [name=BB, password=123, realname=bb], UserInfo [name=CC, password=123, realname=cc], UserInfo [name=DD, password=123, realname=dd], UserInfo [name=EE, password=123, realname=ee], UserInfo [name=FF, password=123, realname=ff]]








