---
title: 使用IntelliJ IDEA整合maven+Spring+SpringMVC+Mybatis框架详细步骤然后整合Shiro配置
---
1. 新建Maven项目： 
点击File -> New -> Project -> Maven -> 勾选 Create from archetype -> 选择 maven-archetype-webapp (注意：此处不要错选成上面的cocoom-22-archetype-webapp) 
![1](/images/integrationProject/1.png)
![2](/images/integrationProject/2.png)

在弹出的new project 选项卡中填写GroupId和Artifactid，其中GroupID是项目组织唯一的标识符，实际对应JAVA的包的结构，是main目录里java的目录结构，ArtifactID是项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称。对于入门练习，这两项可以随意填写。
![3](/images/integrationProject/3.png)

之后点击next 选择Maven版本（其中IDEA 专业版自带Maven，也可以选择自己下载的maven）。之后填写项目名称和项目地址，完成后点击Finish，完成项目骨架的创建。 
![4](/images/integrationProject/4.png)

2. 在新建的项目中添加所需要的文件/文件夹

创建之后的项目如图所示，我们需要在这之上新建一些目录。 
![5](/images/integrationProject/5.png)

在项目的根目录下新建target文件夹，系统自动将其设置为“Excluded” 
![6](/images/integrationProject/6.png)
 
在src/main目录下新建Directory：“java”，并将其设置为“Source Root”（即：此项目默认的代码文件源目录） 
![7](/images/integrationProject/7.png)
![8](/images/integrationProject/8.png)

在刚才新建的java文件下新建“com”包，再在com包下新建四个包，分别命名为：pojo,service,dao,controller。（如果出现下图中所示的包名重叠的情况，可以点击图中所示的图标，将“Hide empty middle package取消掉”） 
![9](/images/integrationProject/9.png)
上面新建的四个包：pojo,service,dao,controller，其所存放的分别是：

pojo: 存放自定义的java类。如：paper类，user类，book类等，每个类的属性设为private，并提供public属性的getter/setter方法让外界访问
service：定义接口，包含系统所提供的功能。（之后还会在service包下再新建impl包）。
dao：定义接口，包含与数据库进行交互的功能。
controller：控制器，负责接收页面请求，转发和处理。

1、创建maven工程，在pom.xml文件中导入需要的jar包依赖：

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.test</groupId>
  <artifactId>ssm</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  
	<!-- 集中定义版本号 -->
	<properties>
		<junit.version>4.12</junit.version>
		<spring.version>4.1.3.RELEASE</spring.version>
		<mybatis.version>3.2.8</mybatis.version>
		<mybatis.spring.version>1.2.2</mybatis.spring.version>
		<mybatis.paginator.version>1.2.15</mybatis.paginator.version>
		<mysql.version>5.1.32</mysql.version>
		<slf4j.version>1.6.4</slf4j.version>
		<jackson.version>2.4.2</jackson.version>
		<druid.version>1.0.9</druid.version>
		<httpclient.version>4.3.5</httpclient.version>
		<jstl.version>1.2</jstl.version>
		<servlet-api.version>2.5</servlet-api.version>
		<jsp-api.version>2.0</jsp-api.version>
		<joda-time.version>2.5</joda-time.version>
		<commons-lang3.version>3.3.2</commons-lang3.version>
		<commons-io.version>1.3.2</commons-io.version>
		<commons-net.version>3.3</commons-net.version>
		<!-- 因为官方的不支持mybatis逆向工程，在源码上有修改，需要自己手动编译 -->
		<pagehelper.version>3.4.2-fix</pagehelper.version>
		<commons-fileupload.version>1.3.1</commons-fileupload.version>
		<jedis.version>2.7.2</jedis.version>
		<solrj.version>4.10.3</solrj.version>
		<swagger.version>2.4.0</swagger.version>
	</properties>
 
 
	<dependencies>
		<!-- 时间操作组件 -->
		<dependency>
			<groupId>joda-time</groupId>
			<artifactId>joda-time</artifactId>
			<version>${joda-time.version}</version>
		</dependency>
		<!-- Apache工具组件 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>${commons-lang3.version}</version>
		</dependency>
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-io</artifactId>
			<version>${commons-io.version}</version>
		</dependency>
		<dependency><!-- 通信使用的，如http，ftp -->
			<groupId>commons-net</groupId>
			<artifactId>commons-net</artifactId>
			<version>${commons-net.version}</version>
		</dependency>		
		<!--Jackson Json处理工具包 -->
			<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>${jackson.version}</version>
		</dependency>
		<!-- httpclient -->
		<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>httpclient</artifactId>
			<version>${httpclient.version}</version>
		</dependency>
		<!-- 单元测试 -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>${junit.version}</version>
			<scope>test</scope>
		</dependency>
		<!-- 日志处理 -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<!-- Mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>${mybatis.version}</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>${mybatis.spring.version}</version>
		</dependency>
		<dependency>
			<groupId>com.github.miemiedev</groupId>
			<artifactId>mybatis-paginator</artifactId>
			<version>${mybatis.paginator.version}</version>
		</dependency>
		<!-- 分页插件 -->
		<dependency>
			<groupId>com.github.pagehelper</groupId>
			<artifactId>pagehelper</artifactId>
			<version>${pagehelper.version}</version>
		</dependency>
		<!-- MySql -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${mysql.version}</version>
		</dependency>
		<!-- 连接池 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>${druid.version}</version>
		</dependency>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aspects</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<!-- JSP相关 -->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>${servlet-api.version}</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jsp-api</artifactId>
			<version>${jsp-api.version}</version>
			<scope>provided</scope>
		</dependency>
		<!-- 文件上传组件 -->
		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>${commons-fileupload.version}</version>
		</dependency>
		<!-- Redis客户端 -->
		<dependency>
			<groupId>redis.clients</groupId>
			<artifactId>jedis</artifactId>
			<version>${jedis.version}</version>
		</dependency>
		<!-- solr客户端 -->
		<dependency>
			<groupId>org.apache.solr</groupId>
			<artifactId>solr-solrj</artifactId>
			<version>${solrj.version}</version>
		</dependency>
		<!-- springfox-swagger2 -->
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger2</artifactId>
			<version>${swagger.version}</version>
		</dependency>
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger-ui</artifactId>
			<version>${swagger.version}</version>
		</dependency>
	</dependencies>
    
        <build>
		<plugins>
			<!-- 配置Tomcat插件 -->
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.2</version>
			</plugin>
			<!-- java编译插件 -->
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.2</version>
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
					<encoding>UTF-8</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
 

2、整合SpringMVC：

（1）在web.xml文件中配置前端控制器：

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="serviceMarket" version="2.5">
	
	<display-name>ssm-test</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
 
	<!-- 加载spring相关配置文件 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:spring/applicationContext*.xml</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener> 
	
	<!-- 解决post乱码 -->
	<filter>
		<filter-name>CharacterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>utf-8</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>CharacterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
	
	<!-- springmvc的前端控制器 -->
  	<servlet>
		<servlet-name>ssm-test</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 加载springmvc的配置文件(处理器映射器、处理器适配器等等) -->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring/springmvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>ssm-test</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
 
</web-app>
（2）编写springmvc.xml文件：

在文件中配置与springmvc框架相关的配置，比如：springmvc.xml约束、处理器映射器、处理器适配器、视图解析器、Handler等配置。

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
 
	<!-- 使用注解驱动：自动配置处理器映射器与处理器适配器 -->
	<mvc:annotation-driven />
 
	<!-- 注解方式：配置Handler，自动扫描该包，使SpringMVC认为包下用了@controller注解的类是控制器 -->
        <!-- 批量加载：组件扫描方式： -->
	<context:component-scan base-package="com.zwp.controller" />
 
 	<!-- 视图解析器：解析jsp页面，默认使用jstl标签，classpath下得有jstl的包 -->    
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
             <!-- 配置前后缀 -->
             <property name="prefix" value="/WEB-INF/jsp/"></property>
             <property name="suffix" value=".jsp"></property>
        </bean>
	
	<!-- 静态资源处理，不拦截静态资源，如css、js、imgs -->
	<mvc:resources location="/static/css/" mapping="/static/css/**"/>
	<mvc:resources location="/static/js/" mapping="/static/js/**"/>
	<mvc:resources location="/static/img/" mapping="/static/img/**"/>
 
	<!-- 文件上传相关配置 -->
	<bean id="multipartResolver"
	    class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
	    <property name="defaultEncoding" value="UTF-8"></property>
	    <property name="maxUploadSize" value="10240000"></property>
	</bean>
</beans>
（3）编写Handler：

//User类的Handler
//需要在类上面加@Controller注解，这样才能被自动扫描到
@Controller
@RequestMapping("/user")
public class UserController {
 
	private static Logger log=LoggerFactory.getLogger(UserController.class);
      
    @RequestMapping(value="/login",method=RequestMethod.GET)  
    public String login(HttpServletRequest request,Model model){  
        String username = request.getParameter("username");  
        String password = request.getParameter("password");
        System.out.println("username:"+username+";password:"+password);
        User user=null;
        if (username.equals("zwp") && password.equals("123456")) {
             user = new User();  
             user.setAge(11);
             user.setId(1);
             user.setPassword("123456");
             user.setUsername("zwp");
             
             log.debug(user.toString());
        }
       
        model.addAttribute("user", user);  
        return "index";  
    }  
}

创建db.properties文件
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.jdbcUrl=jdbc:mysql://localhost:3306/system?characterEncoding=utf8
jdbc.user=root
jdbc.password=root
注意在其他配置的参数可能要改为对应以上路径

整合MyBatis-Generator自动生成代码
（2）利用MyBatis-Generator自动创建实体类、Mybatis映射文件以及Dao接口。
使用这个插件可以快速生成一些代码,包含 实体类/Mapper接口/*Mapper.xml文件
在pom.xml中添加代码

<plugins>
      <plugin>
        <!--Mybatis-generator插件,用于自动生成Mapper和POJO-->
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>1.3.2</version>
        <configuration>
          <!--配置文件的位置-->
          <configurationFile>yourLocation/mybatis-generator-config.xml</configurationFile>
          <verbose>true</verbose>
          <overwrite>true</overwrite>
        </configuration>
        <executions>
          <execution>
            <id>Generate MyBatis Artifacts</id>
            <goals>
              <goal>generate</goal>
            </goals>
          </execution>
        </executions>
        <dependencies>
          <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.2</version>
          </dependency>
        </dependencies>
      </plugin>
    </plugins>
注意,plugins标签是build标签的子标签

添加好之后,我们就需要配置mybatis-generator-config.xml配置文件中的代码
旧的
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC
        "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>

    <!-- 本地数据库驱动程序jar包的全路径 -->
    <classPathEntry location=""/>

    <context id="context" targetRuntime="MyBatis3">
        <commentGenerator>
            <property name="suppressAllComments" value="false"/>
            <property name="suppressDate" value="true"/>
        </commentGenerator>

        <!-- 数据库的相关配置 -->
        <jdbcConnection driverClass="" connectionURL="" userId="" password=""/>

        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- 实体类生成的位置 -->
        <javaModelGenerator targetPackage="目标包" targetProject="目标项目classpath">
            <property name="enableSubPackages" value="false"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- *Mapper.xml 文件的位置 -->
        <sqlMapGenerator targetPackage="目标包" targetProject="目标项目classpath">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- Mapper 接口文件的位置 -->
        <javaClientGenerator targetPackage="目标包" targetProject="目标项目classpath" type="XMLMAPPER">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!-- 相关表的配置 -->
        <table tableName="表名" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"
               enableUpdateByExample="false"/>
    </context>
</generatorConfiguration>

新的
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC
        "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>

    <!-- 本地数据库驱动程序jar包的全路径 -->
    <classPathEntry location="H:\Company\repository\repository\repository\mysql\mysql-connector-java\5.1.32\mysql-connector-java-5.1.32.jar"/>

    <context id="context" targetRuntime="MyBatis3">
        <commentGenerator>
            <property name="suppressAllComments" value="false"/>
            <property name="suppressDate" value="true"/>
        </commentGenerator>

        <!-- 数据库的相关配置 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost:3306/system" userId="root" password="root"/>

        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- 实体类生成的位置 -->
        <javaModelGenerator targetPackage="com.mainPage.home.user.pojo" targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- *Mapper.xml 文件的位置 -->
        <sqlMapGenerator targetPackage="resources/mapper" targetProject="src/main">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- Mapper 接口文件的位置 -->
        <javaClientGenerator targetPackage="com.mainPage.home.user.mapper" targetProject="src/main/java" type="XMLMAPPER">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!-- 相关表的配置 -->
        <table tableName="sys_user" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"
               enableUpdateByExample="false"/>
    </context>
</generatorConfiguration>
随后,在idea的右侧栏点击Maven,选中添加的Mybatis-generator插件并运行,就可以得到相应的代码啦~

在Intellij IDEA添加一个“Run运行”选项，使用maven运行mybatis-generator-maven-plugin插件
点击 菜单run中Edit Configurations,会出现
![10](/images/integrationProject/10.png)
点击+号，选择maven，会出现
![11](/images/integrationProject/11.png)
 
在name和Commond line分别填上mybatis-generator:generate -e如上图所示，apply和ok
最后点击generator，生成model，mapper，dao
![12](/images/integrationProject/12.png)

//User实体类
public class User {
 
	private Integer id;
	private String username;
	private String password;
	private Integer age;
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	
	@Override
	public String toString() {
		return "User [id=" + id + ", username=" + username + ", password=" + password + ", age=" + age + "]";
	}
}
（4）配置日志文件：

在项目根目录下面创建log4j.properties日志文件，下面是日志文件的基本配置，可以满足大多数项目的需求。

log4j.rootLogger=INFO,Console,File  
#定义日志输出目的地为控制台  
log4j.appender.Console=org.apache.log4j.ConsoleAppender  
log4j.appender.Console.Target=System.out  
#可以灵活地指定日志输出格式，下面一行是指定具体的格式  
log4j.appender.Console.layout = org.apache.log4j.PatternLayout  
log4j.appender.Console.layout.ConversionPattern=[%c] - %m%n  
  
#文件大小到达指定尺寸的时候产生一个新的文件  
log4j.appender.File = org.apache.log4j.RollingFileAppender  
#指定输出目录  
log4j.appender.File.File = logs/ssm.log  
#定义文件最大大小  
log4j.appender.File.MaxFileSize = 10MB  
# 输出所以日志，如果换成DEBUG表示输出DEBUG以上级别日志  
log4j.appender.File.Threshold = ALL  
log4j.appender.File.layout = org.apache.log4j.PatternLayout  
log4j.appender.File.layout.ConversionPattern =[%p] [%d{yyyy-MM-dd HH\:mm\:ss}][%c]%m%n
（5）启动项目，进行测试：
IDEA配置tomcat： 
点击上方的 Run 选项，选择Edit Configuration
![13](/images/integrationProject/13.png)

选择default -> tomcat -> local 选择下载安装到本地的tomcat服务器的地址
![14](/images/integrationProject/14.png)

切换到Deployment选项页 点击 + 号选择 Arctifact ，
![15](/images/integrationProject/15.png)

添加 项目名:war exploded 打包 
![16](/images/integrationProject/16.png)
最后在Application context 中选择 空白 那一项，点击 Apply 应用。 
![17](/images/integrationProject/17.png)
访问地址：http://localhost:8080/user/login?username=zwp&password=123456，如果出现下面页面则代表映射成功。
![18](/images/integrationProject/18.png)
 
至此，SpringMVC的配置就已经成功。
3、Spring和Mybatis框架的整合：

整合思路：由Spring通过单例方式管理SqlSessionFactory；spring和mybatis整合生成代理对象，使用SqlSessionFactory创建SqlSession，持久层的mapper都需要有spring进行管理。

（1）配置dao层，即配置sqlSessionFactory：（我的配置在applicationContext-dao.xml文件中）

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">
 
	<!-- 1.加载配置文件 -->
	<!-- 第一种加载多个.properties文件的方法 -->
	<context:property-placeholder location="classpath:resource/*.properties"/>
	
	<!-- 2.配置数据库连接池 -->
	<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
      destroy-method="close">
    <property name="url" value="jdbc:mysql://localhost:3306/system" />
    <property name="username" value="root" />
    <property name="password" value="root" />
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    <property name="maxActive" value="10" />
    <property name="minIdle" value="5" />
</bean>
	
	<!-- 3.让spring创建并管理sqlsessionfactory，使用mybatis和spring整合包中的 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 配置数据源，数据库连接池 -->
		<property name="dataSource" ref="dataSource" />
		<!-- 加载mybatis的全局配置文件 -->
		<property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml" />
	</bean>
	
	<!-- 4.mapper批量扫描：从mapper包中扫描出mapper,自动创建代理对象并且在spring容器中注册
	 	需要遵循规范：mapper接口类名和mapper.xml映射文件名称一致，且在同一目录下，
	 	自动扫描出来的mapper的bean的id为类名(首字母小写)-->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.zwp.mapper" />
	</bean>
</beans>
创建并编写SqlMapConfig.xml文件：

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
		PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
		"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<plugins>
		<!-- com.github.pagehelper为PageHelper类所在包名 -->
		<plugin interceptor="com.github.pagehelper.PageHelper">
			<!-- 设置数据库类型 Oracle,Mysql,MariaDB,SQLite,Hsqldb,PostgreSQL六种数据库 -->
			<property name="dialect" value="mysql" />
		</plugin>
	</plugins>
</configuration>
（2）配置service层：（我的配置在applicationContext-service.xml文件中）

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">
 
	<!-- 自动扫描service层的接口与实现类 -->
	<context:component-scan base-package="com.zwp.service" />
</beans>
 
（3）配置事务：（我的配置在applicationContext-trans.xml文件中）

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">
 
	<!-- 事务管理器 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<!-- 数据源 -->
		<property name="dataSource" ref="dataSource" />
	</bean>
	
	<!-- 通知 -->
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<!-- 传播行为 -->
			<tx:method name="add*" propagation="REQUIRED" />
			<tx:method name="create*" propagation="REQUIRED" />
			<tx:method name="insert*" propagation="REQUIRED" />
			<tx:method name="save*" propagation="REQUIRED" />
			<tx:method name="update*" propagation="REQUIRED" />
			<tx:method name="delete*" propagation="REQUIRED" />
			<tx:method name="find*" propagation="SUPPORTS" read-only="true" />
			<tx:method name="select*" propagation="SUPPORTS" read-only="true" />
			<tx:method name="get*" propagation="SUPPORTS" read-only="true" />
		</tx:attributes>
	</tx:advice>
	<!-- 切面 -->
	<aop:config>
		<aop:advisor advice-ref="txAdvice"
			pointcut="execution(* com.zwp.service.*.*(..))" />
	</aop:config>
</beans>
至此，Spring和Mybatis框架的整合就完成了。

 

4、测试：

（1）创建测试用表：
![19](/images/integrationProject/19.png)

（3）建立Service和实现类：

//UserService接口类：
@Service
public interface UserService {
	
	public User getUserById(int userId);
	
}
//UserService的实现类
@Service
public class UserServiceImpl implements UserService {
	
	//注入UserMapper接口
	@Autowired
	private UserMapper userMapper;
 
	@Override
	public User getUserById(int userId) {
		User user = userMapper.selectByPrimaryKey(userId);
		return user;
	}
}
（3）建立UserController类：

//User类的Handler
//需要在类上面加@Controller注解，这样才能被自动扫描到
@Controller
@RequestMapping("/user")
public class UserController {
	
	@Autowired
	private UserService userService;
 
	private static Logger log=LoggerFactory.getLogger(UserController.class);
      
	@RequestMapping(value="/getUserById/{userId}",method=RequestMethod.GET)
	public @ResponseBody User getUserById(@PathVariable Integer userId){
		
		return userService.getUserById(userId);
	}
	
    @RequestMapping(value="/login",method=RequestMethod.GET)  
    public String login(HttpServletRequest request,Model model){  
        String username = request.getParameter("username");  
        String password = request.getParameter("password");
        System.out.println("username:"+username+";password:"+password);
        User user=null;
        if (username.equals("zwp") && password.equals("123456")) {
             user = new User();  
             user.setAge(11);
             user.setId(1);
             user.setPassword("123456");
             user.setUsername("zwp");
             
             log.debug(user.toString());
        }
       
        model.addAttribute("user", user);  
        return "index";  
    }  
}
（4）部署项目，输入地址：http://localhost:8080/user/getUserById/2，出现下面响应结果，则表示整合成功了。
![20](/images/integrationProject/20.png)
至此，Maven+Spring+SpringMVC+Mybatis的整合就完成了。
最后
在pom.xml还要修改为
<pagehelper.version>4.1.6</pagehelper.version>
否则会找不到PageHelper
在applicationContext-dao.xml的sqlSessionFactory里面加上
<!--扫描mapper.xml文件-->
<property name="mapperLocations" value="classpath:mapper/*.xml"/>

整合shiro
1.创建数据库
数据库准备三张表：t_user用户表，t_role角色表，t_permission权限表



CREATE TABLE `t_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(255) DEFAULT NULL,
  `password` varchar(255) DEFAULT NULL,
  `role_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;
CREATE TABLE `t_role` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `rolename` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
CREATE TABLE `t_permission` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `permission_name` varchar(255) DEFAULT NULL,
  `role_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
//分别向三张表中插入数据
INSERT INTO `t_permission` VALUES ('1', 'user:create', '1');
INSERT INTO `t_permission` VALUES ('2', 'student:*', '2');
INSERT INTO `t_permission` VALUES ('3', 'student:*', '1');
 
 
INSERT INTO `t_role` VALUES ('1', 'admin');
INSERT INTO `t_role` VALUES ('2', 'teacher');
 
 
INSERT INTO `t_user` VALUES ('1', 'yangqing', '123', '1');
INSERT INTO `t_user` VALUES ('2', 'jack', '1', '2');
INSERT INTO `t_user` VALUES ('3', 'marry', '234', null);
INSERT INTO `t_user` VALUES ('4', 'json', '1234', null);
配置文件
向pom.xml添加依赖
<!-- 添加Servlet支持 -->
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.1.0</version>
</dependency>

<dependency>
  <groupId>javax.servlet.jsp</groupId>
  <artifactId>javax.servlet.jsp-api</artifactId>
  <version>2.3.1</version>
</dependency>

<!-- 添加Spring支持 -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>4.1.7.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-tx</artifactId>
  <version>4.1.7.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context-support</artifactId>
  <version>4.1.7.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-web</artifactId>
  <version>4.1.7.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aop</artifactId>
  <version>4.1.7.RELEASE</version>
</dependency>

<!-- 添加日志支持 -->
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>

<dependency>
  <groupId>c3p0</groupId>
  <artifactId>c3p0</artifactId>
  <version>0.9.1</version>
</dependency>

<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-core</artifactId>
  <version>1.2.4</version>
</dependency>

<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-web</artifactId>
  <version>1.2.4</version>
</dependency>

<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-spring</artifactId>
  <version>1.2.4</version>
</dependency>

并把<pagehelper.version></pagehelper.version>改为4.1.6

向web.xml文件添加
<!-- shiro过滤器定义 -->
<filter>
  <filter-name>shiroFilter</filter-name>
  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
  <init-param>
    <!-- 该值缺省为false,表示生命周期由SpringApplicationContext管理,设置为true则表示由ServletContainer管理 -->
    <param-name>targetFilterLifecycle</param-name>
    <param-value>true</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>shiroFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

<!-- 编码过滤器 -->
<filter>
  <filter-name>encodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <async-supported>true</async-supported>
  <init-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>encodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

并在load-on-startup的下一行添加（可加可不加）
<async-supported>true</async-supported>

在applicationContext-dao.xml添加
<!-- 自定义Realm -->
<bean id="myRealm" class="com.sys.util.MyRealm"/>

<!-- 安全管理器 -->
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
    <property name="realm" ref="myRealm"/>
</bean>

<!-- Shiro过滤器 -->
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
    <!-- Shiro的核心安全接口,这个属性是必须的 -->
    <property name="securityManager" ref="securityManager"/>
    <!-- 身份认证失败，则跳转到登录页面的配置 -->
    <property name="loginUrl" value="/login.jsp"/>
    <!-- 权限认证失败，则跳转到指定页面 -->
    <property name="unauthorizedUrl" value="/tUser/unauthorized"/>
    <!-- Shiro连接约束配置,即过滤链的定义 -->
    <property name="filterChainDefinitions">
        <value>
            /tUser/login=anon
            /jsp/home/login/login.jsp=anon
            /logout=logout
            /admin=authc
            /student=roles[teacher]
            /tUser/teacher=perms["user:create"]
            /**=authc
        </value>
    </property>
</bean>

<!-- 保证实现了Shiro内部lifecycle函数的bean执行 -->
<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

<!-- 开启Shiro注解 -->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor"/>
<bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
    <property name="securityManager" ref="securityManager"/>
</bean>

<!-- 配置事务通知属性 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!-- 定义事务传播属性 -->
    <tx:attributes>
        <tx:method name="insert*" propagation="REQUIRED" />
        <tx:method name="update*" propagation="REQUIRED" />
        <tx:method name="edit*" propagation="REQUIRED" />
        <tx:method name="save*" propagation="REQUIRED" />
        <tx:method name="add*" propagation="REQUIRED" />
        <tx:method name="new*" propagation="REQUIRED" />
        <tx:method name="set*" propagation="REQUIRED" />
        <tx:method name="remove*" propagation="REQUIRED" />
        <tx:method name="delete*" propagation="REQUIRED" />
        <tx:method name="change*" propagation="REQUIRED" />
        <tx:method name="check*" propagation="REQUIRED" />
        <tx:method name="get*" propagation="REQUIRED" read-only="true" />
        <tx:method name="find*" propagation="REQUIRED" read-only="true" />
        <tx:method name="load*" propagation="REQUIRED" read-only="true" />
        <tx:method name="*" propagation="REQUIRED" read-only="true" />
    </tx:attributes>
</tx:advice>

<!-- 配置事务切面 -->
<aop:config>
    <aop:pointcut id="txPoint"
                  expression="execution(* com.sys.service.*.*(..))" />
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint" />
</aop:config>

SqlMapConfig.xml
在configuration里面添加
<settings>
		<!-- 开启驼峰命名法 -->
		<setting name="mapUnderscoreToCamelCase" value="true"/>
	</settings>
	
	<!-- 别名 -->
	<typeAliases>
		<package name="com.yang.entity"/>

spring-mvc.xml
<!-- 视图解析器 -->
<bean id="viewResolver"
      class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/"></property>
    <property name="suffix" value=".jsp"></property>
</bean>

dao接口

UserDao.java

package com.yang.dao;
 
import java.util.Set;
 
import com.yang.entity.User;
/**
 * created by yangqing on 2018年2月19日 下午9:32:29
 */
public interface UserDao {
 
	/**
	 *  通过用户名查找用户
	 *  @param username
	 *  @return User
	 */
	public User getByUserName(String username);
	
	/**
	 *  通过用户名查找该用户所有的角色并保存在Set集合中
	 *  @param username
	 *  @return Set<String>
	 */	
	public Set<String> getRoles(String username);
	
	/**
	 *  通过用户名查找该用户所有的权限并保存在Set集合中
	 *  @param username
	 *  @return Set<String>
	 */	 
	public Set<String> getPermissions(String username);
}
//UserDao接口的实现

UserMapper.xml   （该文件在src/main/resources/mapper文件夹下）

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yang.dao.UserDao">
		
	<select id="getByUserName" parameterType="String" resultType="User">
		select * from t_user where username=#{username}
	</select>
	
	<select id="getRoles" parameterType="String" resultType="String">
		select r.rolename from t_user u,t_role r where u.role_id=r.id and u.username=#{username}
	</select>
	
	<select id="getPermissions" parameterType="String" resultType="String">
		select p.permission_name from t_user u,t_role r,t_permission p where u.role_id=r.id and p.role_id=r.id and u.username=#{username}
	</select>
 
 
</mapper> 
服务层

UserService.java

package com.yang.service;
 
import java.util.Set;
import com.yang.entity.User;
 
/**
 * 
 * created by yangqing on 2018年2月19日 下午9:44:23
 */
public interface UserService {
	/**
	 *  通过用户名查找用户
	 *  @param username
	 *  @return User
	 */
	public User getByUserName(String username);
	
	/**
	 *  通过用户名查找该用户所有的角色并保存在Set集合中
	 *  @param username
	 *  @return Set<String>
	 */
	public Set<String> getRoles(String username);
	
	/**
	 *  通过用户名查找该用户所有的权限并保存在Set集合中
	 *  @param username
	 *  @return Set<String>
	 */	
	public Set<String> getPermissions(String username);
}

UserServiceImpl.java

package com.yang.service.impl;
 
import java.util.Set;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
 
import com.yang.dao.UserDao;
import com.yang.entity.User;
import com.yang.service.UserService;
/**
 * created by yangqing on 2018年2月19日 下午9:45:51
 */
@Service
public class UserServiceImpl implements UserService{
 
	@Autowired
	private UserDao userDao;
	
	public User getByUserName(String username) {
		return userDao.getByUserName(username);
	}
 
	public Set<String> getRoles(String username) {
		return userDao.getRoles(username);
	}
 
	public Set<String> getPermissions(String username) {
		return userDao.getPermissions(username);
	}
	
}
Controller层

UserController.java

package com.yang.controller;
 
 
import javax.servlet.http.HttpServletRequest;
 
 
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
 
 
import com.yang.entity.User;
 
 
/**
 * 
 * created by yangqing on 2018年2月19日 下午9:48:55
 */
@Controller
public class UserController {
		
	@RequestMapping("/login")
	public String login(User user,HttpServletRequest request){
		
		//获取当前用户
		Subject subject=SecurityUtils.getSubject();
		UsernamePasswordToken token=new UsernamePasswordToken(user.getUsername(), user.getPassword());
		try{
			//为当前用户进行认证，授权
			subject.login(token);
			request.setAttribute("user", user);
			return "success";
			
		}catch(Exception e){
			e.printStackTrace();
			request.setAttribute("user", user);
			request.setAttribute("errorMsg", "用户名或密码错误！");
			return "login";
		}
	}
	
	@RequestMapping("/teacher")
	public String index() {
		return "index";
	}
}
自定义realm

MyRealm.java

package com.yang.realm;
 
import java.util.Set;
 
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.springframework.beans.factory.annotation.Autowired;
 
import com.yang.entity.User;
import com.yang.service.UserService;
 
/**
 * created by yangqing on 2018年2月19日 下午9:57:09
 */
public class MyRealm extends AuthorizingRealm {
 
	@Autowired
	private UserService userService;
 
	/**
	 * 授权方法
	 */
	@Override
	protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
		
		/**
		 * 注意principals.getPrimaryPrincipal()对应
		 * new SimpleAuthenticationInfo(user.getUserName(), user.getPassword(), getName())的第一个参数
		 */
		//获取当前身份
		String userName = (String) principals.getPrimaryPrincipal();
		SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
		
		//从数据库中查找该用户有何角色和权限
		Set<String> roles = userService.getRoles(userName);
		Set<String> permissions = userService.getPermissions(userName);
		
		//为当前用户赋予对应角色和权限
		info.setRoles(roles);
		info.setStringPermissions(permissions);
		
		return info;
	}
 
	/**
	 * 认证方法
	 */
	@Override
	protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
		//获取用户名
		String username = (String) token.getPrincipal();
		
		//从数据库中查找用户信息
		User user = userService.getByUserName(username);
		if (user == null) {
			return null;			
		}
		AuthenticationInfo info = new SimpleAuthenticationInfo(user.getUsername(), user.getPassword(), getName());
		return info;
	}
 
}
4.前端页面展示
login.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>登录页面</title>
</head>
<body>
<form action="${pageContext.request.contextPath }/login" method="post">
	用户名:<input type="text" name="username" value="${user.username }"/><br/>
	密码:<input type="password" name="password" value="${user.password }"><br/>
	<input type="submit" value="login"/><br>
	<font color="red">${errorMsg }</font>
</form>
</body>
</html>
success.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>登录成功</title>
</head>
<body>
 
欢迎你,<shiro:principal/>!<br>
 
<shiro:hasRole name="admin">
	具备admin角色才能看到这句话<br>
</shiro:hasRole>
 
<shiro:hasRole name="teacher">
	具备teacher角色才能看到这句话<br>
</shiro:hasRole>

<shiro:hasPermission name="user:create">
	具备user:create权限才能看到这句话<br>
</shiro:hasPermission>
 
<shiro:hasPermission name="student:update">
	具备student:update权限才能看到这句话<br>
</shiro:hasPermission>
<br>
 
<shiro:hasPermission name="{student:update,user:*}">
	具备student:update,user:*权限才能看到这句话<br>
</shiro:hasPermission>
 
<a href="teacher">需要user:create权限才能访问</a><br>
<a href="logout">安全退出</a>
</body>
</html>
5.测试结果
分别使用用户名为yangqing和jack的账号登录，测试如下：

tip：如果未登录（未通过认证）在地址栏输入http://localhost:8080/shiroDemo/success.jsp默认跳回login.jsp页面（被shiro拦截了，具体请参考上述配置文件 /**=authc部分）

错误用户试验
![21](/images/integrationProject/21.png)
使用yangqing登录成功之后的页面
![22](/images/integrationProject/22.png)

点击  需要user:create权限才能访问  链接之后的页面
![23](/images/integrationProject/23.png)
使用jack登录成功之后的页面
![24](/images/integrationProject/24.png)

jack用户 点击  需要user:create权限才能访问  链接之后的页面
![25](/images/integrationProject/25.png)

spring集成Mybatis通用Mapper
一、添加通用mapper相关依赖
<dependency>
	<groupId>tk.mybatis</groupId>
	<artifactId>mapper</artifactId>
	<version>3.3.7</version>
</dependency>
<dependency>
	<groupId>javax.persistence</groupId>
	<artifactId>persistence-api</artifactId>
	<version>1.0</version>
</dependency>


二、配置spring整合
applicationContext-dao.xml
<!-- 配置扫描包，加载mapper代理对象 -->
<bean class="tk.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="basePackage" value="Angel.mapper" />
</bean>

注意：这里和spring配置扫描mapper文件是一样的，不一样的是，将org.mybatis.......换成了tk.mybatis........

三、具体应用
3.1，TbUserMapper接口
package Angel.mapper;

import tk.mybatis.mapper.common.Mapper;
import Angel.pojo.TbUser;


public interface TbUserMapper extends Mapper<TbUser>{

}

3.2，TbUserMapper.xml文件
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="Angel.mapper.TbUserMapper" >  
  <resultMap id="BaseResultMap" type="Angel.pojo.TbUser" >  
    <id column="id" property="id" jdbcType="BIGINT" />  
    <result column="username" property="username" jdbcType="VARCHAR" />  
    <result column="password" property="password" jdbcType="VARCHAR" />  
    <result column="phone" property="phone" jdbcType="VARCHAR" />  
    <result column="email" property="email" jdbcType="VARCHAR" />  
    <result column="created" property="created" jdbcType="TIMESTAMP" />  
    <result column="updated" property="updated" jdbcType="TIMESTAMP" />  
  </resultMap>  
  
</mapper>

在这个里面，没有写任何的方法实现，仅有的代码，是为了避免实体属性名和字段名 不统一而写的。

3.3，userServiceImpl里面的实现（省略接口）
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import Angel.mapper.TbUserMapper;
import Angel.pojo.TbUser;
import Angel.service.UserService;

@Service(value="userService")  
public class UserServiceImpl implements UserService {  
  
    @Autowired  
    private TbUserMapper userMapper;  
      
    @Override
	public List<TbUser> selectAll() {
		
		return userMapper.selectAll();
	} 
    
} 

附：通用接口所提供 的公共方法
![26](/images/integrationProject/26.png)


从上图可以看出，引入公共mapper 后，已经具有其基础的数据库操作方法。
3.4，UserController文件
@Autowired
	private UserService userService;

	@RequestMapping("/user/select")
	@ResponseBody
	public List<TbUser> selectUser() {

		List<TbUser> list = userService.selectAll();

		return list;
	}

结果：
![27](/images/integrationProject/27.png)

jsp页面传值到后台出现乱码就在apache-tomcat-7.0.68\conf\server.xml里面找到以下内容URIEncoding="UTF-8" useBodyEncodingForURI="true"添加到一下文本中
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8" useBodyEncodingForURI="true"/>
