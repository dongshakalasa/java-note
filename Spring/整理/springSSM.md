# 1、SSM整合

## 1.1、流程分析

(1) 、创建工程

- 创建一个Maven的web工程
- pom.xml添加SSM需要的依赖jar包

(2)、SSM整合

- SpringConfig
  - @Configuration：表示该类是配置类【spirng的配置类】
  - @ComponentScan：扫描Service所在的包【业务层 dao层】
  - @PropertySource：读取外部的properties配置文件
  - @Import：引入整合Mybatis所需的相关配置
    - JdbcConfig.java：第三方数据源配置类
      - 构建DataSource数据源，DruidDataSouroce,需要注入数据库连接四要素， @Bean @Value
      - 构建平台事务管理器，DataSourceTransactionManager,@Bean
    - MybatisConfig.java：Mybatis配置类
      - 构建SqlSessionFactoryBean并设置别名扫描与数据源，@Bean
      - 构建MapperScannerConfigurer并设置DAO层的包扫描
- SpringMvcConfig
  - @Configuration：表示该类是配置类【spirngMVC的配置类】
  -  @ComponentScan：扫描Controller所在的包 
  -  @EnableWebMvc：开启SpringMVC注解支持

## 1.2、配置步骤

### 步骤1：创建Maven的web项目

可以使用Maven的骨架创建

![1630561266760](图\springSSM\1630561266760.png)

### 步骤2:添加依赖

pom.xml添加SSM所需要的依赖jar包

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.itheima</groupId>
  <artifactId>springmvc_08_ssm</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

    // servlet的jar包
  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>

      // jdbc的jar包
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.6</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.16</version>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
          <port>80</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>


```

### 步骤3:创建项目包结构



* config目录存放的是相关的配置类
* controller编写的是Controller类
* dao存放的是Dao接口，因为使用的是Mapper接口代理方式，所以没有实现类包
* service存的是Service接口，impl存放的是Service实现类
* resources:存入的是配置文件，如Jdbc.properties
* webapp:目录可以存放静态资源
* test/java:存放的是测试类

### 步骤4:创建SpringConfig配置类

```java
@Configuration
@ComponentScan({"com.itheima.service"})
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MyBatisConfig.class})
@EnableTransactionManagement
public class SpringConfig {
}
```

### 步骤5:创建JdbcConfig配置类

```java
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }

    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager ds = new DataSourceTransactionManager();
        ds.setDataSource(dataSource);
        return ds;
    }
}
```

### 步骤6:创建MybatisConfig配置类

```
public class MyBatisConfig {
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);
        factoryBean.setTypeAliasesPackage("com.itheima.domain");
        return factoryBean;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }

}
```

### 步骤7:创建jdbc.properties

在resources下提供jdbc.properties,设置数据库连接四要素

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm_db
jdbc.username=root
jdbc.password=root
```

### 步骤8:创建SpringMVC配置类

```java
@Configuration
@ComponentScan("com.itheima.controller")
@EnableWebMvc
public class SpringMvcConfig {
}
```

### 步骤9:创建Web项目入口配置类

```
public class ServletConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    //加载Spring配置类
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
    //加载SpringMVC配置类
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }
    //设置SpringMVC请求地址拦截规则
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
    //设置post请求中文乱码过滤器
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("utf-8");
        return new Filter[]{filter};
    }
}
```

## 1.3、功能模块开发

### 步骤1、创建数据库及表

### 步骤2、编写模型类

### 步骤3、编写dao层

### 步骤4、编写Service接口和实现类

### 步骤5、编写Contorller类

## 1.4、juint测试

### 步骤1、新建测试类

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class BookServiceTest {

}
```

### 步骤2、注入Service类

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class BookServiceTest {

    @Autowired
    private BookService bookService;


}
```

### 步骤3、编写测试方法

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class BookServiceTest {

    @Autowired
    private BookService bookService;

	@Test
	public void testGetAll(){
		List<Book> books = bookService.getAll();
		System.out.println(books);
	}

}
```

## 1.5、统一接口封装

### 步骤1、创建统一结果返回类

```
public class Result{
	private Object data;
	private Integer code;
	private String msg;
}

构造方法

	public Result(){
    }

    public Result(Integer code,Object data) {
        this.code = code;
        this.data = data;
    }

    public Result( Integer code,Object data, String message) {
        this.code = code;
        this.data = data;
        this.message = message;
    }
    
省略：getter and setter方法
```

### 步骤2、封装Controller类的返回值

```
@PostMapping
    public Result save(@RequestBody Book book) {
        boolean flag = bookService.save(book);
        return new Result();
    }
```

## 1.6、统一异常处理

### 1.6.1、原因

- 在我们程序运行中会出现各种各样的一样
  - 框架内部抛出的异常：因使用不合规导致
  - 数据层抛出的异常：因外部服务器故障导致（例如：服务器访问超时）
  - 业务层抛出的异常：因业务逻辑书写错误导致（例如：遍历业务书写操作，导致索引异常等）
  - 表现层抛出的异常：因数据收集、校验等规则导致（例如：不匹配的数据类型间导致异常）
  - 工具类抛出的异常：因工具类书写不严谨不够健壮导致（例如：必要释放的连接长期未释放等）

- 出现异常之后，返回前台的数据，就出现前台没有数据，无法正常显示的情况

![1630656982337](图\springSSM\1630656982337.png)

### 1.6.2、解决思路

- **各个层级均会抛出异样，所以我们把异常抛到表现层进行处理**
- **因为异常的分类很多，所以我们需要对异常进行分类处理**
- **使用AOP技术解决，表现层处理一样代码量大的问题**

### 1.6.3、解决方法

- 异常处理器

### 1.6.4、异常处理器

#### 步骤1、创建异常处理器类

```
//@RestControllerAdvice用于标识当前类为REST风格对应的异常处理器
@RestControllerAdvice
public class ProjectExceptionAdvice {
    //除了自定义的异常处理器，保留对Exception类型的异常处理，用于处理非预期的异常
    @ExceptionHandler(Exception.class)
    public void doException(Exception ex){
      	System.out.println("嘿嘿,异常你哪里跑！")
    }
}

注意：@RestControllerAdvice 自带 @ResponseBody and @Component
```

#### 步骤2、让springMvcConfig配置类扫到改类

### 1.6.5、项目异常处理方案

#### **项目异常分类**

- 业务异常（BusinessException）
  - 不规范的用户行为产生的异常
  - 不规范的用户行为操作产生的异常
- 系统异常（SystemException）
  - 项目运行过程中可预计且无法避免的异常
- 其他异常（Exception）
  - 编程人员未预料到的异常

#### **异常处理方案**

<img src="图\springSSM\image-20221005170433058.png" alt="image-20221005170433058" style="zoom:80%;" />

#### 异常解决方案的具体实现

##### 步骤1:自定义异常类

```
//自定义异常处理器，用于封装异常信息，对异常进行分类
public class SystemException extends RuntimeException{
    private Integer code;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public SystemException(Integer code, String message) {
        super(message);
        this.code = code;
    }

    public SystemException(Integer code, String message, Throwable cause) {
        super(message, cause);
        this.code = code;
    }

}

//自定义异常处理器，用于封装异常信息，对异常进行分类
public class BusinessException extends RuntimeException{
    private Integer code;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public BusinessException(Integer code, String message) {
        super(message);
        this.code = code;
    }

    public BusinessException(Integer code, String message, Throwable cause) {
        super(message, cause);
        this.code = code;
    }

}

```

**说明:**

* 让自定义异常类继承`RuntimeException`的好处是，后期在抛出这两个异常的时候，就不用在try...catch...或throws了
* 自定义异常类中添加`code`属性的原因是为了更好的区分异常是来自哪个业务的

##### 步骤2:处理器类中处理自定义异常

```
//@RestControllerAdvice用于标识当前类为REST风格对应的异常处理器
@RestControllerAdvice
public class ProjectExceptionAdvice {
    //@ExceptionHandler用于设置当前处理器类对应的异常类型
    @ExceptionHandler(SystemException.class)
    public Result doSystemException(SystemException ex){
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员,ex对象发送给开发人员
        return new Result(ex.getCode(),null,ex.getMessage());
    }

    @ExceptionHandler(BusinessException.class)
    public Result doBusinessException(BusinessException ex){
        return new Result(ex.getCode(),null,ex.getMessage());
    }

    //除了自定义的异常处理器，保留对Exception类型的异常处理，用于处理非预期的异常
    @ExceptionHandler(Exception.class)
    public Result doOtherException(Exception ex){
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员,ex对象发送给开发人员
        return new Result(Code.SYSTEM_UNKNOW_ERR,null,"系统繁忙，请稍后再试！");
    }
}
```

## 1.7、拦截器

### 1.7.1、配置拦截器

#### 步骤1、创建拦截器类

让类实现HandlerInterceptor接口，重写接口中的三个方法。

**注意:拦截器类要被SpringMVC容器扫描到。**

```
@Component
//定义拦截器类，实现HandlerInterceptor接口
//注意当前类必须受Spring容器控制
public class ProjectInterceptor implements HandlerInterceptor {
    @Override
    //原始方法调用前执行的内容
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...");
        return true;
    }

    @Override
    //原始方法调用后执行的内容
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...");
    }

    @Override
    //原始方法调用完成后执行的内容
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}
```

#### 步骤2、配置拦截器类

```
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
    }

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        //配置拦截器
        //registry.addInterceptor(projectInterceptor).addPathPatterns("/books" );
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*"
    }
}
```

#### 步骤3、SpringMVC添加SpringMvcSupport包扫描

```
@Configuration
@ComponentScan({"com.itheima.controller","com.itheima.config"})
@EnableWebMvc
public class SpringMvcConfig{
   
}
```

### 1.7.2、配置多个拦截器

#### 步骤1、创建拦截器类

实现接口，并重写接口中的方法

```
@Component
public class ProjectInterceptor2 implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...222");
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...222");
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...222");
    }

}
```

#### 步骤2、配置拦截器类

```
@Configuration
@ComponentScan({"com.itheima.controller"})
@EnableWebMvc
//实现WebMvcConfigurer接口可以简化开发，但具有一定的侵入性
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterceptor projectInterceptor;
    @Autowired
    private ProjectInterceptor2 projectInterceptor2;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //配置多拦截器
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*");
        registry.addInterceptor(projectInterceptor2).addPathPatterns("/books","/books/*");
    }
}
```

### 1.7.3、拦截器要点

#### 拦截器

- preHandle：原始方法之前运行

  - 参数
    - request:请求对象
    - response:响应对象
    - handler:被调用的处理器对象，本质上是一个方法对象，对反射中的Method对象进行了再包装

- postHandle：原始方法运行后运行

  - 参数

    - 前三个参数和上面的是一致的。

    - modelAndView:如果处理器执行完成具有返回结果，可以读取到对应数据与页面信息，并进行调整

      因为咱们现在都是返回json数据，所以该参数的使用率不高。

- afterCompletion：拦截器最后执行的方法

  - 参数
    - 前三个参数与上面的是一致的。
    - ex:如果处理器执行过程中出现异常对象，可以针对异常情况进行单独处理  

- 这三个方法中，最常用的是==preHandle==,在这个方法中可以通过返回值来决定是否要进行放行，我们可以把业务逻辑放在该方法中，如果满足业务则返回true放行，不满足则返回false拦截。

# 2、工具

## 2.1、七牛云

### 步骤1、添加依赖

```
	//文件上传
	<dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.4</version>
    </dependency>
	
	<dependency>
      <groupId>com.qiniu</groupId>
      <artifactId>qiniu-java-sdk</artifactId>
      <version>7.2.0</version>
    </dependency>
```

### 步骤2、封装QiniuUtils类

```
public  static String accessKey = "bSSuERF6U4jDcuJamR70ThcfpyvZchpFn0fAHuh0";
    public  static String secretKey = "SS2n38jQ0ellzqxbTCRcO6R3rjHIkBfR8AQg_vB1";
    public  static String bucket = "zhangves";

    //上传文件
    public static void upload2Qiniu(byte[] bytes, String fileName){
        //构造一个带指定Zone对象的配置类
        Configuration cfg = new Configuration(Zone.zone2());
        //...其他参数参考类注释
        UploadManager uploadManager = new UploadManager(cfg);

        //默认不指定key的情况下，以文件内容的hash值作为文件名
        String key = fileName;
        Auth auth = Auth.create(accessKey, secretKey);
        String upToken = auth.uploadToken(bucket);
        try {
            Response response = uploadManager.put(bytes, key, upToken);
            //解析上传成功的结果
            DefaultPutRet putRet = new Gson().fromJson(response.bodyString(), DefaultPutRet.class);
            System.out.println(putRet.key);
            System.out.println(putRet.hash);
        } catch (QiniuException ex) {
            Response r = ex.response;
            System.err.println(r.toString());
            try {
                System.err.println(r.bodyString());
            } catch (QiniuException ex2) {
                //ignore
            }
        }
    }
```

### 步骤3、Controller层调用
