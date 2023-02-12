# 一、项目结构

## 1.1 代码层结构

- 根目录：src/main/java

> 入口启动类及程序的开发目录。主要目录：业务开发、实体层、控制层、数据连接层等

- 数据库实体层：pojo

> 一般数据库一张表对应一个实体类，类属性同表对象一一对应

- 数据持久层：dao

> dao层的作用是访问数据库，向数据库发送sql语句，完成数据的增删改查

- 数据服务接口层：service

> 主要负责业务逻辑应用设计
>
> 1. 设计接口
> 2. 设计实现该接口的实现类serviceImpl
> 3. 调用service接口进行业务处理
> 4. service层调用dao层接口，接受dao层返回数据
> 5. 完成项目的基本设计

- 控制器层：controller

> 主要负责具体业务模块流程的控制
>
> - 功能是请求和响应控制
> - 负责前后端交互，接受前端请求，调用service层，接口service层返回的数据，返回具体的也页面和数据给客户端

- 工具类库：utils
- 配置类：config
- 数据传输对象：dto

## 1.2、资源目录结构

- 资源文件根目录：src/main/resources

> 主要用来存放静态文件和配置文件

- 项目配置文件：resources/application.properties

> 配置项目运行所需的配置数据

- 静态资源目录：resources/static/

> 用于存放静态资源

- mybatis映射文件：resources/mappers/

- 。。。

# 二、整合mybatis

**1、在pom.xml文件中导入坐标**

```
// spring的jar包
<dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-context</artifactId>
     <version>5.2.10.RELEASE</version>
</dependency>

// mysql对应的jar包
<dependency>
     <groupId>mysql</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>5.1.46</version>
</dependency>

// druid的jar包：数据源
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>

// mybatis的jar包
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
</dependency>
 
// spirng操作数据库的jar包
<dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-jdbc</artifactId>
     <version>5.2.10.RELEASE</version>
</dependency>

// spring整合mybatis的jar包
<dependency>
     <groupId>org.mybatis</groupId>
     <artifactId>mybatis-spring</artifactId>
     <version>1.3.0</version>
</dependency>
```

**2、resources包下，创建application.properties文件**

```
jdbcDriver=com.mysql.jdbc.Driver
jdbcUrl=jdbc:mysql://localhost:3306/db1
user=root
password=123456
```

**3、在config包下，创建spring配置文件SpirngConfig.java**

```
// 表示这类是spring的配置类
@Configuration
// 根据结构，扫描创建bean
@ComponentScan("com.itheima")
// 引入外部properties文件
@PropertySource("classpath:jdbc.properties")
// 导入配置对象
@Import({JdbcConfig.class,MybatisConfig.class})
```

**4、在config报下，创建JdbcConfig.java包，创建Druid数据源对象**

```
@Value("${jdbcDriver}")
private String driver;
@Value("${jdbcUrl}")
private String url;
@Value("${user}")
private String user;
@Value("${password}")
private String password;

@Bean
public DataSource dataSource(){
    DruidDataSource ds = new DruidDataSource();
    ds.setDriverClassName(driver);
    ds.setUrl(url);
    ds.setUsername(user);
    ds.setPassword(password);
    return ds;
}
```

**5、在config包下，创建MybatisConfig对象**

```
@Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
        ssfb.setTypeAliasesPackage("com.itheima.domain");
        ssfb.setDataSource(dataSource);
        return ssfb;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }
```

# 三、整合MVC

**1、在pom.xml文件中导入坐标**

```

```

**3、在config包下，创建spring配置文件ServletConfig.java**

- 继承AbstractAnnotationConfigDispatcherServletInitializer

```

```



# 四、注释解释

## 1.1、config包

```
@Configuration：设置当前类是配置类
@ComponentScan("路径")：设置扫描那些包
@PropertySource("name.properties")：引入外部配置数据文件
@Import({Jdbc.class,MyBatisConfig.class})：引入jdbc数据源、mybatis配置文件

@Value("${name}")：注入普通属性
@Bean：设置为bean能都被spring加载为bean，受spring管理

@EnableWebMvc：springMVC的包，功能很多

@EnableTransactionMangment：spirng事务注释
```

​	
