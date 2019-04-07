# 课程内容
1.	面向接口（抽象）编程的概念与好处
2.	IOC/DI的概念与好处
a)	inversion of control
b)	dependency injection
3.	AOP的概念与好处
4.	Spring简介
5.	Spring应用IOC/DI（重要）
a)	xml
b)	annotation
6.	Spring应用AOP（重要）
a)	xml
b)	annotation
7.	Struts2.1.6 + Spring2.5.6 + Hibernate3.3.2整合（重要）
a)	opensessionInviewfilter（记住，解决什么问题，怎么解决）
8.	Spring JDBC
# 面向接口编程（面向抽象编程）
1.	场景：用户添加
2.	Spring_0100_AbstractOrientedProgramming
a)	不是AOP:Aspect Oriented Programming
3.	好处：灵活
# 什么是IOC（DI），有什么好处
1.	把自己new的东西改为由容器提供
a)	初始化具体值
b)	装配
2.	好处：灵活装配
# Spring简介
1.	项目名称：Spring_0200_IOC_Introduction
2.	环境搭建
a)	只用IOC
i.	spring.jar , jarkata-commons/commons-loggin.jar
3.	IOC容器
a)	实例化具体bean
b)	动态装配
4.	AOP支持
a)	安全检查
b)	管理transaction
# Spring IOC配置与应用
## 1.	FAQ:不给提示：
a)	window – preferences – myeclipse – xml – xml catalog
b)	User Specified Entries – add
i.	Location:	D:\share\0900_Spring\soft\spring-framework-2.5.6\dist\resources\spring-beans-2.5.xsd
ii.	URI:   		file:///D:/share/0900_Spring/soft/spring-framework-2.5.6/dist/resources/spring-beans-2.5.xsd
iii.	Key Type:	Schema Location
iv.	Key:		http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
## 2.	注入类型
a)	Spring_0300_IOC_Injection_Type
b)	setter（重要）
c)	构造方法（可以忘记）
d)	接口注入（可以忘记）
## 3.	id vs. name
a)	Spring_0400_IOC_Id_Name
b)	name可以用特殊字符
## 4.	简单属性的注入
a)	Spring_0500_IOC_SimpleProperty
b)	<property name=… value=….>
## 5.	<bean 中的scope属性
a)	Spring_0600_IOC_Bean_Scope
b)	singleton 单例
c)	proptotype 每次创建新的对象
## 6.	集合注入
a)	Spring_0700_IOC_Collections
b)	很少用，不重要！参考程序
## 7.	自动装配
a)	Spring_0800_IOC_AutoWire
b)	byName
c)	byType
d)	如果所有的bean都用同一种，可以使用beans的属性：default-autowire
## 8.	生命周期
a)	Spring_0900_IOC_Life_Cycle
b)	lazy-init (不重要)
c)	init-method destroy-methd 不要和prototype一起用（了解）
## 9.	Annotation第一步：
a)	修改xml文件，参考文档<context:annotation-config />
## 10.	@Autowired
a)	默认按类型by type
b)	如果想用byName，使用@Qulifier
c)	写在private field（第三种注入形式）（不建议，破坏封装）
d)	如果写在set上，@qualifier需要写在参数上
## 11.	@Resource（重要）
a)	加入：j2ee/common-annotations.jar
b)	默认按名称，名称找不到，按类型
c)	可以指定特定名称
d)	推荐使用
e)	不足：如果没有源码，就无法运用annotation，只能使用xml
## 12.	@Component @Service @Controller @Repository
a)	初始化的名字默认为类名首字母小写
b)	可以指定初始化bean的名字
## 13.	@Scope
## 14.	@PostConstruct = init-method; @PreDestroy = destroy-method;

# 什么是AOP
## 1.	面向切面编程Aspect-Oriented-Programming
a)	是对面向对象的思维方式的有力补充
## 2.	Spring_1400_AOP_Introduction
## 3.	好处：可以动态的添加和删除在切面上的逻辑而不影响原来的执行代码
a)	Filter
b)	Struts2的interceptor
## 4.	概念：
a)	JoinPoint  释意:切面与原方法交接点 即 切入点
b)	PointCut  释意:切入点集合
c)	Aspect（切面）释意:可理解为代理类前说明
d)	Advice 释意:可理解为代理方法前说明 例如@Before
e)	Target  释意:被代理对象 被织入对象
f)	Weave  释意:织入
# Spring AOP配置与应用
## 1.	两种方式：
a)	使用Annotation
b)	使用xml
## 2.	Annotation
a)	加上对应的xsd文件spring-aop.xsd
b)	beans.xml <aop:aspectj-autoproxy />
c)	此时就可以解析对应的Annotation了
d)	建立我们的拦截类
e)	用@Aspect注解这个类
f)	建立处理方法
g)	用@Before来注解方法
h)	写明白切入点（execution …….）
i)	让spring对我们的拦截器类进行管理@Component
## 3.	常见的Annotation:
a)	@Pointcut  切入点声明 以供其他方法使用 , 例子如下:

```
@Aspect
@Component
public class LogInterceptor {
	
	@Pointcut("execution(public * com.bjsxt.dao..*.*(..))")
	public void myMethod(){}

	@Around("myMethod()")
	public void before(ProceedingJoinPoint pjp) throws Throwable{
		System.out.println("method before");
		pjp.proceed();
	}
	@AfterReturning("myMethod()")
	public void afterReturning() throws Throwable{
		System.out.println("method afterReturning");
	}
	@After("myMethod()")
	public void afterFinily() throws Throwable{
		System.out.println("method end");
	}
		}
```java
b)	@Before 发放执行之前织入
c)	@AfterReturning 方法正常执行完返回之后织入(无异常)
d)	@AfterThrowing 方法抛出异常后织入
e)	@After 类似异常的finally 
f)	@Around 环绕 类似filter , 如需继续往下执行则需要像filter中执行FilterChain.doFilter(..)对象一样 执行 ProceedingJoinPoint.proceed()方可,例子如下:
@Around("execution(* com.bjsxt.dao..*.*(..))")
		public void before(ProceedingJoinPoint pjp) throws Throwable{
				System.out.println("method start");
				pjp.proceed();//类似FilterChain.doFilter(..)告诉jvm继续向下执行
}
## 4.	织入点语法
a)	void !void
b)	参考文档（* ..）
如果 execution(* com.bjsxt.dao..*.*(..))中声明的方法不是接口实现 则无法使用AOP实现动态代理,此时可引入包” cglib-nodep-2.1_3.jar” 后有spring自动将普通类在jvm中编译为接口实现类,从而打到可正常使用AOP的目的.
## 5.	xml配置AOP
a)	把interceptor对象初始化
b)	<aop:config
i.	<aop:aspect …..
1.	<aop:pointcut
2.	<aop:before
例子：
<bean id="logInterceptor" class="com.bjsxt.aop.LogInterceptor"></bean>
	<aop:config>
		<!-- 配置一个切面 -->
		<aop:aspect id="point" ref="logInterceptor">
			<!-- 配置切入点，指定切入点表达式 -->
			<!-- 此句也可放到 aop:aspect标签外 依然有效-->
			<aop:pointcut
				expression=
				"execution(public * com.bjsxt.service..*.*(..))"
				id="myMethod" />
			<!-- 应用前置通知 -->
			<aop:before method="before" pointcut-ref="myMethod" />
			<!-- 应用环绕通知 需指定向下进行 -->
			<aop:around method="around" pointcut-ref="myMethod" />
			<!-- 应用后通知 -->
			<aop:after-returning method="afterReturning"
				pointcut-ref="myMethod" />
			<!-- 应用抛出异常后通知 -->
			<aop:after-throwing method="afterThrowing"
				pointcut-ref="myMethod" />
			<!-- 应用最终通知  -->
			<aop:after method="afterFinily"
				pointcut="execution(public * om.bjsxt.service..*.*(..))" />
		</aop:aspect>
</aop:config>
Spring整合Hibernate
1.	Spring 指定datasource
a)	参考文档，找dbcp.BasicDataSource
i.	c3p0
ii.	dbcp
iii.	proxool
b)	在DAO或者Service中注入dataSource
c)	在Spring中可以使用PropertyPlaceHolderConfigure来读取Properties文件的内容
2.	Spring整合Hibernate
a)	<bean .. AnnotationSessionFactoryBean>
i.	<property dataSource
ii.	<annotatedClasses
b)	引入hibernate 系列jar包
c)	User上加Annotation
d)	UserDAO或者UserServie 注入SessionFactory
e)	jar包问题一个一个解决
3.	声明式的事务管理
a)	事务加在DAO层还是Service层？
b)	annotation
i.	加入annotation.xsd
ii.	加入txManager bean
iii.	<tx:annotation-driven
例如: <bean id="transactionManager"    			 class="org.springframework.orm.hibernate3.HibernateTransactionManager">
          <property name="sessionFactory">
            <ref bean="sessionFactory" />
          </property>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>
iv.	在需要事务的方法上加：@Transactional
v.	需要注意，Hibernate获得session时要使用SessionFactory.getCurrentSession 不能使用OpenSession
c)	@Transactional详解
i.	什么时候rollback 
1.	运行期异常，非运行期异常不会触发rollback
2.	必须uncheck (没有catch)
3.	不管什么异常，只要你catch了，spring就会放弃管理
4.	事务传播特性：propagation_required
例如: @Transactional(propagation=Propagation.REQUIRED)等同于(@Transactional)
作用,一个方法声明了@Transactional事务后,其内再调用的方法不需要再声明@Transactional.
5.	read_only
例如: @Transactional(propagation=Propagation.REQUIRED,readOnly=true)
当方法声明readOnly=true时,该方法及其调用的方法内都不执行insert update等
d)	xml（推荐，可以同时配置好多方法）
i.	<bean txmanager
ii.	<aop:config 
1.	<aop:pointcut
2.	<aop:advisor pointcut-ref advice-ref
iii.	<tx:advice: id transaction-manager = 
iv.	<property name="packagesToScan">  可定义扫描目标包下所有实体类
例如: <bean id="sessionFactory"
    				 class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
    	<property name="dataSource" ref="dataSource" />
    	<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.OracleDialect</prop>
				<prop key="hibernate.show_sql">true</prop>   
			</props>
		</property>
		<!-- 
    	<property name="annotatedClasses">
    		<list>
    			<value>com.bjsxt.model.TestUser</value>
    			<value>com.bjsxt.model.Log</value>
    		</list>
    	</property>
    	 -->
    	 <!-- 将参数名称设为packagesToScan 可定义扫描目标包下所有实体类 -->
    	 <property name="packagesToScan">
    	 	<list>
    	 		<value>com.bjsxt.model</value>
    	 	</list>
    	 </property>
    </bean>
<bean id="transactionManager"
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory">
            <ref bean="sessionFactory" />
        </property>
    </bean>
    
    <aop:config>
        <aop:pointcut
			expression="execution(public * com.bjsxt.service..*.*(..))"
			id="myServiceMethod" />
        <aop:advisor pointcut-ref="myServiceMethod" advice-ref="txAdvice"/>
    </aop:config>
    
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="save*" propagation="REQUIRED" />
            <tx:method name="add*" propagation="REQUIRED" />
            <tx:method name="update*" propagation="REQUIRED" />
            <tx:method name="del*" propagation="REQUIRED" />
            <tx:method name="cancel*" propagation="REQUIRED" />
            <tx:method name="*" read-only="true" /> 
        </tx:attributes>
    </tx:advice>
e)	HibernateTemplate、HibernateCallback、HibernateDaoSupport（不重要）介绍
i.	设计模式：Template Method(模板方法)
ii.	Callback：回调/钩子函数
iii.	第一种：（建议）
1.	在spring中初始化HibernateTemplate，注入sessionFactory
<bean id="hibernateTemplate" 
		class="org.springframework.orm.hibernate3.HibernateTemplate">
<property name="sessionFactory" ref="sessionFactory" />
</bean>
2.	DAO里注入HibernateTemplate
private HibernateTemplate hibernateTemplate;

@Resource
public void setHibernateTemplate(HibernateTemplate hibernateTemplate) {
	this.hibernateTemplate = hibernateTemplate;
}
3.	save写getHibernateTemplate.save();
public void save(TestUser testUser) {
	hibernateTemplate.save(testUser);
}
iv.	第二种：
1.	从HibernateDaoSupport继承(此方法不好用 可忽略)
2.	必须写在xml文件中，无法使用Annotation，因为set方法在父类中，而且是final的
例如:
首先,新建SuperDAOImpl类(使用Annotation注入--@Component):
@Component
public class SuperDAOImpl {
	private HibernateTemplate hibernateTemplate; //此处定义由spring注入管理
	public HibernateTemplate getHibernateTemplate() {
		return hibernateTemplate;
	}
	@Resource
	public void setHibernateTemplate(HibernateTemplate hibernateTemplate) {
		this.hibernateTemplate = hibernateTemplate;
	}
}
此时,xml中必须要有:
<bean id="hibernateTemplate" 
class="org.springframework.orm.hibernate3.HibernateTemplate">
	<property name="sessionFactory" ref="sessionFactory" />
</bean>
或者,SuperDAOImpl类写成下面代码:
@Component
public class SuperDAOImpl extends HibernateDaoSupport {
	@Resource(name="sessionFactory")
	public void setSuperHibernateTemplate(SessionFactory sessionFactory) {
		super.setSessionFactory(sessionFactory);
	}
}
对应的xml中则可省略
<bean id="hibernateTemplate"………部分
只要包含
<bean id="sessionFactory"……..部分即可

最后,其他类继承SuperDaoImpl类后便可直接使用HibernateTemplate
@Component("u")
public class UserDAOImpl extends SuperDAOImpl implements UserDAO {
	public void save(TestUser testUser) {
		this.getHibernateTemplate().save(testUser);
	}
}
f)	spring整合hibernate的时候使用packagesToScan属性，可以让spring自动扫描对应包下面的实体类
Struts2.1.6 + Spring2.5.6 + Hibernate3.3.2
1.	需要的jar包列表
jar包名称	所在位置	说明
antlr-2.7.6.jar	hibernate/lib/required	解析HQL
aspectjrt	spring/lib/aspectj	AOP
aspectjweaver	..	AOP
cglib-nodep-2.1_3.jar	spring/lib/cglib	代理，二进制增强
common-annotations.jar	spring/lib/j2ee	@Resource
commons-collections-3.1.jar	hibernate/lib/required	集合框架
commons-fileupload-1.2.1.jar	struts/lib	struts
commons-io-1.3.2	struts/lib	struts
commons-logging-1.1.1	单独下载，删除1.0.4(struts/lib)	struts
spring
dom4j-1.6.1.jar	hibernate/required	解析xml
ejb3-persistence	hibernate-annotation/lib	@Entity
freemarker-2.3.13	struts/lib	struts
hibernate3.jar	hibernate	
hibernate-annotations	hibernate-annotation/	
hibernate-common-annotations	hibernate-annotation/lib	
javassist-3.9.0.GA.jar	hiberante/lib/required	hibernate
jta-1.1.jar	..	hibernate transaction
junit4.5		
mysql-		
ognl-2.6.11.jar	struts/lib	
slf4j-api-1.5.8.jar	hibernate/lib/required	hibernate-log
slf4j-nop-1.5.8.jar	hibernate/lib/required	
spring.jar	spring/dist	
struts2-core-2.1.6.jar	struts/lib	
xwork-2.1.2.jar	struts/lib	struts2
commons-dbcp	spring/lib/jarkata-commons	
commons-pool.jar	..	
struts2-spring-plugin-2.1.6.jar	struts/lib	
2.	BestPractice：
a)	将这些所有的jar包保存到一个位置，使用的时候直接copy
3.	步骤
a)	加入jar包
b)	首先整合Spring + Hibernate
i.	建立对应的package
1.	dao / dao.impl / model / service / service.impl/ test
ii.	建立对应的接口与类框架
1.	S2SH_01
iii.	建立spring的配置文件（建议自己保留一份经常使用的配置文件，以后用到的时候直接copy改）
iv.	建立数据库
v.	加入Hibernate注解
1.	在实体类上加相应注解@Entity @Id等
在字段属性的get方法上加--@Column(name = "表字段名")
2.	在beans配置文件配置对应的实体类，使之受管
vi.	写dao service的实现
vii.	加入Spring注解
1.	在对应Service及DAO实现中加入@Component，让spring对其初始化
2.	在Service上加入@Transactional或者使用xml方式（此处建议后者，因为更简单）
3.	在DAO中注入sessionFactory
4.	在Service中注入DAO
5.	写DAO与Service的实现
viii.	写测试
c)	整合Struts2
i.	结合点：Struts2的Action由Spring产生
ii.	步骤：
1.	修改web.xml加入 struts的filter
如下:
<filter>
	<filter-name>struts2</filter-name>
	<filter-class>
		org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
	</filter-class>
</filter>
<filter-mapping>
	<filter-name>struts2</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
2.	再加入spring的listener，这样的话，webapp一旦启动，spring容器就初始化了
如下:
<!-- 指定由spring初始化加载xml配置文件 spring与struts结合必备 -->
<listener>
	<listener-class>
		org.springframework.web.context.ContextLoaderListener
		<!-- 默认寻找xml路径:WEB-INF/applicationContext.xml -->
	</listener-class>
</listener>

<!--整个应用的参数 服务启动时读取. 
	可指定spring初始化文件路径位置 -->
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>
		classpath*:spring/*applicationContext.xml
	</param-value>
</context-param>
3.	规划struts的action和jsp展现
4.	加入struts.xml
a)	修改配置，由spring替代struts产生Action对象
5.	修改action配置
a)	把类名改为bean对象的名称，这个时候就可以使用首字母小写了
b)	@Scope(“prototype”)不要忘记
iii.	struts的读常量：
1.	struts-default.xml 
2.	struts-plugin.xml
3.	struts.xml
4.	struts.properties
5.	web.xml
iv.	中文问题：
1.	Struts2.1.8已经修正，只需要改i18n.encoding = gbk
2.	使用spring的characterencoding
例:
<!-- 过滤器相关配置 ======== 字符编码过滤======== -->
<filter>
	<filter-name>CharacterEncodingFilter</filter-name>
	<filter-class>
		org.springframework.web.filter.CharacterEncodingFilter
	</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>UTF-8</param-value>
	</init-param>
	<init-param>
		<param-name>forceEncoding</param-name>
		<param-value>true</param-value>
	</init-param>
</filter>
3.	需要严格注意filter的顺序
4.	需要加到Struts2的filter前面
v.	LazyInitializationException
1.	OpenSessionInViewFilter
2.	需要严格顺序问题
3.	需要加到struts2的filter前面

附: 
1．
@Autowired 与@Resource 都可以用来装配bean.  都可以写在属性定义上,或写在set方法上
@Autowired (srping提供的) 默认按类型装配 
@Resource ( j2ee提供的 ) 默认按名称装配,当找不到(不写name属性)名称匹配的bean再按类型装配. 
可以通过@Resource(name="beanName") 指定被注入的bean的名称, 要是指定了name属性, 就用 字段名 去做name属性值,一般不用写name属性. 
@Resource(name="beanName")指定了name属性,按名称注入但没找到bean, 就不会再按类型装配了.
@Autowired 与@Resource可作用在属性定义上,  就不用写set方法了(此方法不提倡);


2．
	a.
Action类前加@Component,则Action可由spring来管理,例子如下:
Action中写:
@Component("u") //spring管理注解
@Scope("prototype") //多态
public class UserAction extends ActionSupport implements ModelDriven{
	//内部属性需要有get/set方法 且需要set方法前加@Resource或@Autowired
}

Struts2配置文件中写
<action name="u" class="u"> 

Jsp中
<form method="post" action="u.do" >

b.
Action中也可不加@Component，Action由struts2-spring-plugin管理。此时，如果Action中定义的属性有set方法 则@Autowired 与@Resource也可不写，但是如果没有set方法，则需要在属性前加上@Autowired 或@Resource才能生效。


	3．
Hibernate如果使用load来查询数据，例如：
Service中：
public User loadById(int id) {
	return this.userDao.loadById(id);
}
DAO中：
public User loadById(int id) {
	return (User)this.hibernateTemplate.load(User.class, id);
}
此时，session（应该说的是Hibernate的session）在事物结束（通常是service调用完）后自动关闭。由于使用的是load获取数据，在jsp页面申请取得数据时才真正的执行sql，而此时session已经关闭，故报错。
Session关闭解决方法：
在web.xml中增加filter—openSessionInView，用于延长session在jsp调用完后再关闭
如下所示：
注意：filter –openSessionInView 一定要在 filter—struts2之前调用
	  Filter顺序—先进后出！
<filter>
	<filter-name>OpenSessionInViewFilter</filter-name>
	<filter-class>
org.springframework.orm.hibernate3.support.OpenSessionInViewFilter
</filter-class>
	<init-param>
		<param-name>sessionFactoryBeanName</param-name>
		<param-value>sf</param-value>(此处默认指定的sessionFactory应为” sessionFactory” 默认可省略此行 如果shring配置文件中配置的sessionFactory为”sf” 则此处需要写sf  一般用不到)
	</init-param>
</filter>
<filter-mapping>
	<filter-name> OpenSessionInViewFilter </filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
<filter>
	<filter-name>struts2</filter-name>
	<filter-class>
org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
</filter-class>
</filter>
<filter-mapping>
	<filter-name>struts2</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>














struts2.2.3.1+spring3.1.1+hibernate3.6.10的框架整合基本包，其中根据项目里用的不同， 可以额外+包：
hibernate-distribution-3.6.10.Final
hibernate3.jar                     Hibernate核心库
antlr-2.7.6.jar                      语言转换工,Hibernate利用它实现 HQL 到 SQL
dom4j-1.6.1.jar                    Java的XML API.Hibernate用它来读写配置文件.
jta-1.1.jar                             JTA规范,标准的 JAVA 事务处理接口,当Hibernate使用JTA的时候需要
javassist-3.12.0.GA.jar        代码生成工具, Hibernate用它在运行时扩展
slf4j-api-1.6.1.jar                 hibernate使用的一个日志系统
cglib-2.2.jar                          CGLIB 字节码解释器,如果使用“cglib”则必要
asm-3.3.jar                            ASM字节码库 如果使用“cglib”则必要
hibernate-jpa-2.0-api-1.0.1.Final.jar               hibernate一种低耦合规范
spring-framework-3.1.1.RELEASE
org.springframework.aop-3.1.1.RELEASE.jar    　　　　　　   Spring面向切面编程,提供AOP实现
org.springframework.asm-3.1.1.RELEASE.jar　　　　　　　　 Spring独立的asm程序
org.springframework.aspects-3.1.1.RELEASE.jar　　　　　　  Spring提供对Aspectj框架的整合
org.springframework.beans-3.1.1.RELEASE.jar　　　　　　   Spring IOC的基础实现
org.springframework.context-3.1.1.RELEASE.jar  　　　　　　 Spring提供在基础IOC功能上的扩展服务
org.springframework.context.support-3.1.1.RELEASE.jar 　　  Spring-context的扩展支持.
org.springframework.core-3.1.1.RELEASE.jar   　　　　　　　　  Spring核心工具包
org.springframework.expression-3.1.1.RELEASE.jar　　　　　　Spring表达式语言
org.springframework.instrument-3.1.1.RELEASE.jar 　　　　　　Spring 对服务器的代理接口
org.springframework.instrument.tomcat-3.1.1.RELEASE.jar　　 Spring对Tomcat的连接池集成
org.springframework.jdbc-3.1.1.RELEASE.jar 　　　　　　　　   Spring对JDBC的简单封装
org.springframework.jms-3.1.1.RELEASE.jar   　　　　　　　　    Spring为简化JMS API的使用而作的简单封装
org.springframework.orm-3.1.1.RELEASE.jar  　　　　　    Spring整合第三方ORM框架,以及Spring的JPA实现
org.springframework.oxm-3.1.1.RELEASE.jar  　　　　　　　　   Spring对Object/XMI的映射支持
org.springframework.test-3.1.1.RELEASE.jar 　　　　　　　　    Spring对Junit等测试框架的简单封装
org.springframework.transaction-3.1.1.RELEASE.jar   　  Spring为JDBC、Hibernate、JDO、JPA等提供的一致声明式和编程事务管理
org.springframework.web-3.1.1.RELEASE.jar    　　　　　　　　  SpringWeb下的工具包
org.springframework.web.portlet-3.1.1.RELEASE.jar 　　　　　　SpringMVC的增强 (可以不使用)
org.springframework.web.servlet-3.1.1.RELEASE.jar  　　　　　  Spring对JEE6.0 Servlet3.0的支持
org.springframework.web.struts-3.1.1.RELEASE.jar   　　　　　　Spring整合Struts的时候的支持
aopalliance-1.0.jar                                                                        SpringAOP需要的支持包
pool连接池和驱动 
c3p0-0.9.1.jar       　　　　　　　　　　　　　　　　　　　　C3P0数据库连接池
mysql-connector-java-5.1.13-bin.jar   驱动(自选)
commons
commons-beanutils-1.7.0.jar          提供对Java反射和自省API的封装,包含一些Bean工具类.必须Jar包
commons-collections-3.1.jar           包含一些Apache开发集合类.   必需Jar包
commons-digester-2.0.jar      　　   基于规则的XML文档解析,主要用于XML到Java对象的映射
commons-fileupload-1.2.2.jar       　web文件上传
commons-io-2.0.1.jar        　　　　　Struts2信息传输
commons-lang-2.5.jar      　　　　　包含一些数据类型工具类,是java.lang.*扩展 必需Jar包
commons-logging-1.1.1.jar     　　　包含日志功能 必须Jar包
struts2.2.3.1
ognl-3.0.1.jar      　　　　　　　　　功能强大的表达式语言EL.实现字段类型转化等功能
freemarker-2.3.16.jar       　　　　　strut2用，一种模版技术    
struts2-core-2.2.3.1.jar     　　　　　struts2核心 
struts2-spring-plugin-2.2.3.1.jar      struts2支持spring       
xwork-core-2.2.3.1.jar     　　　　　xwork支持 这个包的版本必需在2.1.6或以上，如果是2.1.4会报错（少一些类）

