## IDE：
> `Jetbrains IDEA 2018.3`

## 应用场景
## 组件插件功能说明
* ### `SpringBoot` 
  * Spring Boot是一个简化Spring开发的框架。
  * 在使用Spring Boot时只需要配置相应的Spring Boot就可以用所有的Spring组件，简单的说，spring boot就是整合了很多优秀的框架，不用我们自己手动的去写一堆xml配置然后进行配置。从本质上来说，Spring Boot就是Spring,它做了那些没有它你也会去做的Spring Bean配置。
* ### `logback`
  * Log4j是Apache的一个开放源代码项目，通过使用Log4j，我们可以控制日志信息输送的目的地是控制台、文件、数据库等；我们也可以控制每一条日志的输出格式；通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。 
  * Log4j有7种不同的log级别，按照等级从低到高依次为：TRACE、DEBUG、INFO、WARN、ERROR、FATAL、OFF。如果配置为OFF级别，表示关闭log。 
  * Log4j支持两种格式的配置文件：properties和xml。包含三个主要的组件：Logger、appender、Layout。
  * Spring Boot1.4以及之后的版本已经不支持log4j，log4j2和log4j是一个作者，只不过log4j2是重新架构的一款日志组件，他抛弃了之前log4j的不足，以及吸取了优秀的logback的设计重新推出的一款新组件。
* ### `MyBatis-Plus`
  > 使用Mybatis发现需要在mapper.xml中写很多重复的简单CRUD（增删改查），使用MybatisPlus可以大大简化这部分代码，在` MyBatis` 的基础上只做增强不做改变，为简化开发、提高效率而生。 
  * #### 特性
    * 无侵入
      > 只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
    * 损耗小
      > 启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
    * 强大的 CRUD 操作
      > 内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
    * 支持 Lambda 形式调用
      > 通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错,Java8中让人又爱又恨的Lambda表达式
    * 支持多种数据库
      > 支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer2005、SQLServer 等多种数据库
    * 支持主键自动生成
      > 支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
    * 支持 XML 热加载
      > Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
    * 支持 ActiveRecord 模式
      > 支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
    * 支持自定义全局通用操作
      > 支持全局通用方法注入（ Write once, use anywhere ）
    * 支持关键词自动转义
      > 支持数据库关键词（order、key……）自动转义，还可自定义关键词
    * 内置代码生成器
      > 采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
    * 内置分页插件
      > 基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
    * 内置性能分析插件
      > 可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
    * 内置全局拦截插件
      > 提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作
    * 内置 Sql 注入剥离器
      > 支持 Sql 注入剥离，有效预防 Sql 注入攻击
  * #### 核心功能
    * 代码生成器
      > AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller等各个模块的代码，极大的提升了开发效率。
    * CRUD 接口
      > 即增删改查数据库接口
    * 条件构造器
      > 几乎涵盖所有的数据库查询where条件，并支持原始sql。
    * 分页插件
      > 可通过mybatis-plus 自动分页。
    * Sequence主键
      > 实体主键支持Sequence，自动生成主键。
  * #### 扩展插件
    * 通用枚举
      > 解决了繁琐的配置，让 mybatis 优雅的使用枚举属性！
    * 性能分析插件
      > 性能分析拦截器，用于输出每条 SQL 语句及其执行时间
    * 动态数据源
      > 基于springboot的快速集成多数据源
    * 分布式事务
      > 支持 rabbit 实现可靠消息分布式事务
* ### `Lombok`
  * #### Lombok能以简单的注解形式来简化java代码，提高开发人员的开发效率。例如开发中经常需要写的javabean，都需要花时间去添加相应的getter/setter，也许还要去写构造器、equals等方法，而且需要维护，当属性多时会出现大量的getter/setter方法，这些显得很冗长也没有太多技术含量，一旦修改属性，就容易出现忘记修改对应方法的失误。
  * #### Lombok能通过注解的方式，在编译时自动为属性生成构造器、getter/setter、equals、hashcode、toString方法。出现的神奇就是在源码中没有getter和setter方法，但是在编译生成的字节码文件中有getter和setter方法。这样就省去了手动重建这些代码的麻烦，使代码看起来更简洁些。
* ### `Swagger2`
  * #### 由于Spring Boot能够快速开发、便捷部署等特性，相信有很大一部分Spring Boot的用户会用来构建RESTful API。而我们构建RESTful
  * #### API的目的通常都是由于多终端的原因，这些终端会共用很多底层业务逻辑，因此我们会抽象出这样一层来同时服务于多个移动端或者Web前端。
* ### `JWT`
  * #### JSON Web Token（缩写 JWT）是目前最流行的跨域认证解决方案
  * #### 随着技术的发展，分布式web应用的普及，通过session管理用户登录状态成本越来越高，因此慢慢发展成为token的方式做登录身份校验，然后通过token去取redis中的缓存的用户信息，随着之后jwt的出现，校验方式更加简单便捷化，无需通过redis缓存，而是直接根据token取出保存的用户信息，以及对token可用性校验，单点登录更为简单。
* ### `Shiro`
  * #### Apache Shiro 是 Java 的一个安全（权限）框架。
  * #### Shiro 可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE 环境，也可以用在 JavaEE 环境。
  * #### Shiro 可以完成：认证、授权、加密、会话管理、与Web 集成、缓存等。

## 使用`IDEA`创建`SpringBoot`项目
* ### 创建新项目，在 IntelliJ IDEA 欢迎界面点击 Create New Project
  ![](https://upload-images.jianshu.io/upload_images/5369422-e52b95c8279cc15d.png)
* ### 在 New Project 窗口选中 Spring Initializr，设置 Project SDK，选择默认的 Spring Initializr，点击 Next
  ![](https://upload-images.jianshu.io/upload_images/5369422-dde497e3057fc64f.png)
* 输入 Project Metadata 相关信息，注意 Artifact 中不能输入大写字母，点击 Next
  ![](https://upload-images.jianshu.io/upload_images/5369422-1e646719b4e4bd3d.png)
* 选择需要添加的依赖，此处只选择 Web，点击 Next
  ![](https://upload-images.jianshu.io/upload_images/5369422-9354ab4aa6c941ad.png)
* 输入 Project Name 和 Project Location，暂时不关心 More Settings 中内容，点击 Finish
  ![](https://upload-images.jianshu.io/upload_images/5369422-e98a06afd9de89c1.png)
* IDEA 自动生成的目录结构
  ![](https://upload-images.jianshu.io/upload_images/5369422-b7ba94d9290c558f.png)

## 整合
* ### `MyBatis-Plus`
  * #### 修改pom.xml引入MyBatis-Plus依赖
    ```xml
    <dependencies>
        ...
        <!-- https://mvnrepository.com/artifact/com.baomidou/mybatis-plus-boot-starter -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.1.1</version>
        </dependency>
        ...
    </dependencies>
    ```
  * #### `application.yml`配置文件
    ```yml
    server:
      port: 8080
      servlet:
        context-path: /api　　#项目名称
    
    
    spring:
      profiles:
        active: local #@spring.active@  #默认使用本地配置文件
    
      mvc:
        static-path-pattern: /static/**
        view:
          prefix: /WEB-INF/view
        devtools:
          restart:
            enabled: false
            additional-paths: src/main/java
            exclude: static/**,WEB-INF/view/**
        servlet:
          multipart:
            max-request-size: 100MB
            max-file-size: 100MB
    
      freemarker:
        allow-request-override: false
        cache: false
        check-template-location: true
        charset: utf-8
        content-type: text/html
        expose-request-attributes: false
        expose-session-attributes: false
        expose-spring-macro-helpers: true
        suffix: .ftl
        settings:
          auto_import: /spring.ftl as spring
          template_update_delay: 0
    
    
    mybatis-plus:
      mapper-locations: classpath*:/mapper/**Mapper.xml
      #实体扫描，多个package用逗号或者分号分隔
      typeAliasesPackage: com.gadfly.spring_api.entity
      global-config:
        #主键类型  0:"数据库ID自增", 1:"用户输入ID",2:"全局唯一ID (数字类型唯一ID)", 3:"全局唯一ID UUID";
        id-type: 0
        #字段策略 0:"忽略判断",1:"非 NULL 判断"),2:"非空判断"
        field-strategy: 2
        #驼峰下划线转换
        db-column-underline: true
        #刷新mapper 调试神器
        refresh-mapper: true
        #逻辑删除配置（下面3个配置）
        logic-delete-value: 0
        logic-not-delete-value: 1
      configuration:
        map-underscore-to-camel-case: true
        cache-enabled: false
    
    #logging
    logging:
      path: maven-logs
      level:
        com.gadfly:  debug  #这个改成自己项目路径,不然可能报错
    
    spring:
      profiles: local
      datasource:
        url: jdbc:mysql://127.0.0.1:3306/spring_boot?useUnicode=true&characterEncoding=utf8&useServerPrepStmts=false&serverTimezone=Hongkong
        username: spring_boot
        password: spring_boot
        db-name: spring_boot
        filters: wall,mergeStat

    ```
  * #### 配置创建`MyBatis-Plus`代码生成器
    * ##### 创建`common`包
    * ##### 创建代码生成器`MpGenerator.java`
      ```java
      package com.gadfly.spring_api.config;

      import com.baomidou.mybatisplus.annotation.DbType;
      import com.baomidou.mybatisplus.annotation.FieldFill;
      import com.baomidou.mybatisplus.generator.AutoGenerator;
      import com.baomidou.mybatisplus.generator.InjectionConfig;
      import com.baomidou.mybatisplus.generator.config.*;
      import com.baomidou.mybatisplus.generator.config.converts.MySqlTypeConvert;
      import com.baomidou.mybatisplus.generator.config.po.TableFill;
      import com.baomidou.mybatisplus.generator.config.po.TableInfo;
      import com.baomidou.mybatisplus.generator.config.rules.DbColumnType;
      import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
      
      import java.util.ArrayList;
      import java.util.HashMap;
      import java.util.List;
      import java.util.Map;
      
      public class MpGenerator {
      	/**
      	 * <p>
      	 * MySQL 生成演示
      	 * </p>
      	 */
      	public static void main(String[] args) {
      
      		// 自定义需要填充的字段
      		List<TableFill> tableFillList = new ArrayList<>();
      		tableFillList.add(new TableFill("ASDD_SS", FieldFill.INSERT_UPDATE));
      
      		AutoGenerator mpg = new AutoGenerator();
      		// 选择 freemarker 引擎，默认 Veloctiy
      		// mpg.setTemplateEngine(new FreemarkerTemplateEngine());
      
      		// 全局配置
      		GlobalConfig gc = new GlobalConfig();
      		gc.setOutputDir(System.getProperty("user.dir") + "/src/main/java");
      		gc.setFileOverride(true);
      		gc.setActiveRecord(true);// 不需要ActiveRecord特性的请改为false
      		gc.setEnableCache(false);// XML 二级缓存
      		gc.setBaseResultMap(true);// XML ResultMap
      		gc.setBaseColumnList(true);// XML columList
      		//gc.setKotlin(true);//是否生成 kotlin 代码
      		gc.setAuthor("GadflyBSD");
      
      		// 自定义文件命名，注意 %s 会自动填充表实体属性！
      		gc.setMapperName("%sMapper");
      		gc.setXmlName("%sMapper");
      		gc.setServiceName("%sService");
      		gc.setServiceImplName("%sServiceImpl");
      		gc.setControllerName("%sController");
      		mpg.setGlobalConfig(gc);
      
      		// 数据源配置
      		DataSourceConfig dsc = new DataSourceConfig();
      		dsc.setDbType(DbType.MYSQL);
              /*dsc.setTypeConvert(new MySqlTypeConvert(){
                  // 自定义数据库表字段类型转换【可选】
                  public DbColumnType processTypeConvert(String fieldType) {
                      System.out.println("转换类型：" + fieldType);
                      // 注意！！processTypeConvert 存在默认类型转换，如果不是你要的效果请自定义返回、非如下直接返回。
                      return super.processTypeConvert(fieldType);
                  }
              });*/
      		dsc.setDriverName("com.mysql.cj.jdbc.Driver");
      		dsc.setUsername("spring_boot");
      		dsc.setPassword("spring_boot");
      		dsc.setUrl("jdbc:mysql://127.0.0.1:3306/spring_boot?useUnicode=true&useSSL=false&characterEncoding=utf8");
      		mpg.setDataSource(dsc);
      
      		// 策略配置
      		StrategyConfig strategy = new StrategyConfig();
      		// strategy.setCapitalMode(true);// 全局大写命名 ORACLE 注意
      		//strategy.setTablePrefix(new String[] { "tlog_", "tsys_" });// 此处可以修改为您的表前缀
      		strategy.setNaming(NamingStrategy.underline_to_camel);// 表名生成策略
      		//strategy.setInclude(new String[]{"app_certificate"}); // 需要生成的表,注释掉生成全部表
      		// strategy.setExclude(new String[]{"test"}); // 排除生成的表
      		// 自定义实体父类
      		// strategy.setSuperEntityClass("com.baomidou.demo.TestEntity");
      		// 自定义实体，公共字段
      		// strategy.setSuperEntityColumns(new String[] { "test_id", "age" });
      		// 自定义 mapper 父类
      		// strategy.setSuperMapperClass("com.baomidou.demo.TestMapper");
      		// 自定义 service 父类
      		// strategy.setSuperServiceClass("com.baomidou.demo.TestService");
      		// 自定义 service 实现类父类
      		// strategy.setSuperServiceImplClass("com.baomidou.demo.TestServiceImpl");
      		// 自定义 controller 父类
      		// strategy.setSuperControllerClass("com.baomidou.demo.TestController");
      		// 【实体】是否生成字段常量（默认 false）
      		// public static final String ID = "test_id";
      		//strategy.setEntityColumnConstant(true);
      		// 【实体】是否为构建者模型（默认 false）
      		// public User setName(String name) {this.name = name; return this;}
      		//strategy.setEntityBuilderModel(true);
      		strategy.setTableFillList(tableFillList);
      		mpg.setStrategy(strategy);
      
      		// 包配置
      		PackageConfig pc = new PackageConfig();
      		pc.setParent("com.gadfly.spring_api");
      		pc.setController("controller");
      		pc.setXml("mapper");
      		mpg.setPackageInfo(pc);
      
      		// 注入自定义配置，可以在 VM 中使用 cfg.abc 【可无】  ${cfg.abc}
      		InjectionConfig cfg = new InjectionConfig() {
      			@Override
      			public void initMap() {
      				Map<String, Object> map = new HashMap<String, Object>();
      				map.put("abc", this.getConfig().getGlobalConfig().getAuthor() + "-mp");
      				this.setMap(map);
      			}
      		};
      
      		// 自定义 xxListIndex.html 生成
      		/*List<FileOutConfig> focList = new ArrayList<FileOutConfig>();
      		focList.add(new FileOutConfig("/templates/list.html.vm") {
      			@Override
      			public String outputFile(TableInfo tableInfo) {
      				// 自定义输入文件名称
      				return "F:/idea-maven/maven/src/main/resources/static/" + tableInfo.getEntityName() + "ListIndex.html";
      			}
      		});
      		cfg.setFileOutConfigList(focList);
      		mpg.setCfg(cfg);*/
      
      
      		// 关闭默认 xml 生成，调整生成 至 根目录
              /*TemplateConfig tc = new TemplateConfig();
              tc.setXml(null);
              mpg.setTemplate(tc);*/
      
      		// 自定义模板配置，可以 copy 源码 mybatis-plus/src/main/resources/templates 下面内容修改，
      		// 放置自己项目的 src/main/resources/templates 目录下, 默认名称一下可以不配置，也可以自定义模板名称
      		TemplateConfig tc = new TemplateConfig();
      		tc.setController("/templates/controller.java.vm");
      		tc.setService("/templates/service.java.vm");
      		tc.setServiceImpl("/templates/serviceImpl.java.vm");
      		tc.setEntity("/templates/entity.java.vm");
      		tc.setMapper("/templates/mapper.java.vm");
      		tc.setXml("/templates/mapper.xml.vm");
      		// 如上任何一个模块如果设置 空 OR Null 将不生成该模块。
      		mpg.setTemplate(tc);
      
      		// 执行生成
      		mpg.execute();
      
      		// 打印注入设置【可无】
      		System.err.println(mpg.getCfg().getMap().get("abc"));
      	}
      }
      ```
    * ##### 创建文件`DatatablesJSON.java`
      ```java
      package com.gadfly.spring_api.common;

      import java.io.Serializable;
      import java.util.List;
      
      public class DatatablesJSON<T> implements Serializable {
      	//当前页面
      	private int draw;
      	//总页数
      	private long recordsTotal;
      	private long recordsFiltered;
      
      	// 响应状态 默认false
      	private Boolean success = false;
      	// 响应消息
      	private String error;
      
      	public List<T> getData() {
      		return data;
      	}
      
      	public void setData(List<T> data) {
      		this.data = data;
      	}
      
      	// 存放的数据
      	private List<T> data;
      
      	public int getDraw() {
      		return draw;
      	}
      
      	public void setDraw(int draw) {
      		this.draw = draw;
      	}
      
      	public long getRecordsTotal() {
      		return recordsTotal;
      	}
      
      	public void setRecordsTotal(long recordsTotal) {
      		this.recordsTotal = recordsTotal;
      	}
      
      	public long getRecordsFiltered() {
      		return recordsFiltered;
      	}
      
      	public void setRecordsFiltered(long recordsFiltered) {
      		this.recordsFiltered = recordsFiltered;
      	}
      
      	public Boolean getSuccess() {
      		return success;
      	}
      
      	public void setSuccess(Boolean success) {
      		this.success = success;
      	}
      
      	public String getError() {
      		return error;
      	}
      
      	public void setError(String error) {
      		this.error = error;
      	}
      
      }
      ```
    * ##### 创建文件`JSONResult.java`
      ```java
      package com.gadfly.spring_api.common;
  
      import java.io.Serializable;
      
      /**
       * ajax 传参返回值
       */
      public class JSONResult<T> implements Serializable {
      
      	// 响应状态 默认false
      	private Boolean success = false;
      	// 响应消息
      	private String message;
      	// 存放的数据
      	private T data;
      
      	public JSONResult() {
      		super();
      	}
      
      	public Boolean getSuccess() {
      		return success;
      	}
      
      	public void setSuccess(Boolean success) {
      		this.success = success;
      	}
      
      	public String getMessage() {
      		return message;
      	}
      
      	public void setMessage(String message) {
      		this.message = message;
      	}
      
      	public T getData() {
      		return data;
      	}
      
      	public void setData(T data) {
      		this.data = data;
      	}
      
      }
      ```
  * #### 创建`MyBatis-Plus`代码生成器模板
    > 在`src/main/resources/templates`中创建模板文件

    * 模板文件`controller.java.vm`
      ```java
      package ${package.Controller};

      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RequestParam;
      import org.springframework.web.bind.annotation.RequestMethod;
      	#if(${restControllerStyle})
      import org.springframework.web.bind.annotation.RestController;
      	#else
      import org.springframework.stereotype.Controller;
      	#end
          #if(${superControllerClassPackage})
      import ${superControllerClassPackage};
          #end
      import org.springframework.beans.factory.annotation.Autowired;
      import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
      import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
      import ${package.Service}.${table.serviceName};
      import ${package}.common.DatatablesJSON;
      import ${package}.common.JSONResult;
      import ${package.Entity}.${entity};
      import org.slf4j.Logger;
      import org.slf4j.LoggerFactory;
      
      /**
       *   @description : ${entity} 控制器
       *   @author ${author}
       *   @since ${date}
       */
      #if(${restControllerStyle})
      @RestController
      #else
      @Controller
      #end
      @RequestMapping("#if(${package.ModuleName})/${package.ModuleName}#end/#if(${controllerMappingHyphenStyle})${controllerMappingHyphen}#else${table.entityPath}#end")
      #if(${superControllerClass})
      public class ${table.controllerName} extends ${superControllerClass} {
      #else
      public class ${table.controllerName} {
      #end
      	private final Logger logger=LoggerFactory.getLogger(${table.controllerName}.class);
      
      	@Autowired
      	public ${table.serviceName} ${table.entityPath}Service;
      
      	/**
      	 * @description : 获取分页列表
      	 * ---------------------------------
      	 * @author : ${author}
      	 * @since : Create in ${date}
      	 */
      	@RequestMapping(value = "/get${entity}List", method = RequestMethod.POST)
      	public Object get${entity}List(${entity} param,@RequestParam(value = "draw", defaultValue = "0") Integer draw,
      	@RequestParam(value = "length") Integer length,
      	@RequestParam(value = "start") Integer start){
      		DatatablesJSON<${entity}> resJson=new DatatablesJSON<>();
      		try{
      ##			Integer pageNo=getPageNo(start,length);
      			Integer pageNo=1;
      			Page<${entity}> page=new Page<${entity}>(pageNo,length);
      			${table.entityPath}Service.page(page,new QueryWrapper<${entity}>(param));
      			resJson.setDraw(draw++);
      			resJson.setRecordsTotal(page.getTotal());
      			resJson.setRecordsFiltered(page.getTotal());
      			resJson.setData(page.getRecords());
      			resJson.setSuccess(true);
      		}catch(Exception e){
      			resJson.setSuccess(false);
      			resJson.setError("异常信息:{"+e.getClass().getName()+"}");
      			logger.info("异常信息:{}"+e.getMessage());
      		}
      		return resJson;
      	}
      
      	/**
      	 * @description : 通过id获取${entity}
      	 * ---------------------------------
      	 * @author : ${author}
      	 * @since : Create in ${date}
      	 */
      	@RequestMapping(value = "/get${entity}ById", method = RequestMethod.GET)
      	public Object get${entity}ById(String id){
      		JSONResult<${entity}> resJson=new JSONResult<>();
      		try{
      		${entity} param= ${table.entityPath}Service.getById(id);
      			resJson.setData(param);
      			resJson.setSuccess(true);
      		}catch(Exception e){
      			resJson.setSuccess(false);
      			resJson.setMessage("异常信息:{"+e.getClass().getName()+"}");
      			logger.info("异常信息:{}"+e.getMessage());
      		}
      		return resJson;
          }
      
      	/**
      	 * @description : 通过id删除${entity}
      	 * ---------------------------------
      	 * @author : ${author}
      	 * @since : Create in ${date}
      	 */
      	@RequestMapping(value = "/delete${entity}ById", method = RequestMethod.GET)
      	public Object delete${entity}ById(String id){
      		JSONResult<${entity}> resJson=new JSONResult<>();
      		try{
      			resJson.setSuccess(${table.entityPath}Service.removeById(id));
      		}catch(Exception e){
      			resJson.setSuccess(false);
      			resJson.setMessage("异常信息:{"+e.getClass().getName()+"}");
      			logger.info("异常信息:{}"+e.getMessage());
      		}
      		return resJson;
      	}
      
      	/**
      	 * @description : 通过id更新${entity}
      	 * ---------------------------------
      	 * @author : ${author}
      	 * @since : Create in ${date}
      	 */
      	@RequestMapping(value = "/update${entity}ById", method = RequestMethod.POST)
      	public Object update${entity}ById(${entity} param){
      		JSONResult<${entity}> resJson=new JSONResult<>();
      		try{
      			resJson.setSuccess(${table.entityPath}Service.updateById(param));
      		}catch(Exception e){
      			resJson.setSuccess(false);
      			resJson.setMessage("异常信息:{"+e.getClass().getName()+"}");
      			logger.info("异常信息:{}"+e.getMessage());
      		}
      		return resJson;
      	}
      
      	/**
      	 * @description : 添加${entity}
      	 * ---------------------------------
      	 * @author : ${author}
      	 * @since : Create in ${date}
      	 */
      	@RequestMapping(value = "/add${entity}", method = RequestMethod.POST)
      	public Object add${entity}(${entity} param){
      		JSONResult<${entity}> resJson=new JSONResult<>();
      		try{
      			resJson.setSuccess(${table.entityPath}Service.save(param));
      		}catch(Exception e){
      			resJson.setSuccess(false);
      			resJson.setMessage("异常信息:{"+e.getClass().getName()+"}");
      			logger.info("异常信息:{}"+e.getMessage());
      		}
      		return resJson;
      	}
      }
      ```
    * 模板文件`entity.java.vm`
      ```java
      package ${package.Entity};

      #foreach($pkg in ${table.importPackages})
      import ${pkg};
      #end
      #if(${swagger2})
      import io.swagger.annotations.ApiModel;
      import io.swagger.annotations.ApiModelProperty;
      #end
      #if(${entityLombokModel})
      import lombok.Data;
      import lombok.EqualsAndHashCode;
      import lombok.experimental.Accessors;
      #end
      
      /**
       * <p>
       * $!{table.comment}
       * </p>
       *
       * @author ${author}
       * @since ${date}
       */
      #if(${entityLombokModel})
      @Data
          #if(${superEntityClass})
      	@EqualsAndHashCode(callSuper = true)
          #else
      	@EqualsAndHashCode(callSuper = false)
          #end
      @Accessors(chain = true)
      #end
      #if(${table.convert})
      @TableName("${table.name}")
      #end
      #if(${swagger2})
      @ApiModel(value="${entity}对象", description="$!{table.comment}")
      #end
      #if(${superEntityClass})
      public class ${entity} extends ${superEntityClass}#if(${activeRecord})<${entity}>#end {
      #elseif(${activeRecord})
      public class ${entity} extends Model<${entity}> {
      #else
      public class ${entity} implements Serializable {
      #end
      
      #if(${entitySerialVersionUID})
      	private static final long serialVersionUID=1L;
      #end
      ## ----------  BEGIN 字段循环遍历  ----------
      #foreach($field in ${table.fields})
      
          #if(${field.keyFlag})
              #set($keyPropertyName=${field.propertyName})
          #end
          #if("$!field.comment" != "")
              #if(${swagger2})
      		@ApiModelProperty(value = "${field.comment}")
              #else
      		/**
      		 * ${field.comment}
      		 */
              #end
          #end
          #if(${field.keyFlag})
          ## 主键
              #if(${field.keyIdentityFlag})
      	@TableId(value = "${field.name}", type = IdType.AUTO)
              #elseif(!$null.isNull(${idType}) && "$!idType" != "")
      	@TableId(value = "${field.name}", type = IdType.${idType})
              #elseif(${field.convert})
      	@TableId("${field.name}")
              #end
          ## 普通字段
          #elseif(${field.fill})
          ## -----   存在字段填充设置   -----
              #if(${field.convert})
      	@TableField(value = "${field.name}", fill = FieldFill.${field.fill})
              #else
      	@TableField(fill = FieldFill.${field.fill})
              #end
          #elseif(${field.convert})
      	@TableField("${field.name}")
          #end
      ## 乐观锁注解
          #if(${versionFieldName}==${field.name})
      	@Version
          #end
      ## 逻辑删除注解
          #if(${logicDeleteFieldName}==${field.name})
      	@TableLogic
          #end
      	private ${field.propertyType} ${field.propertyName};
      #end
      ## ----------  END 字段循环遍历  ----------
      
      #if(!${entityLombokModel})
          #foreach($field in ${table.fields})
              #if(${field.propertyType.equals("boolean")})
                  #set($getprefix="is")
              #else
                  #set($getprefix="get")
              #end
      
      	public ${field.propertyType} ${getprefix}${field.capitalName}() {
      			return ${field.propertyName};
      			}
      
              #if(${entityBuilderModel})
      	public ${entity} set${field.capitalName}(${field.propertyType} ${field.propertyName}) {
              #else
      	public void set${field.capitalName}(${field.propertyType} ${field.propertyName}) {
              #end
      		this.${field.propertyName} = ${field.propertyName};
              #if(${entityBuilderModel})
      		return this;
              #end
      	}
          #end
      #end
      
      #if(${entityColumnConstant})
          #foreach($field in ${table.fields})
      	public static final String ${field.name.toUpperCase()} = "${field.name}";
          #end
      #end
      
      #if(${activeRecord})
      	@Override
      	protected Serializable pkVal() {
          #if(${keyPropertyName})
      		return this.${keyPropertyName};
          #else
      		return null;
          #end
      	}
      
      #end
      #if(!${entityLombokModel})
      	@Override
      	public String toString() {
      		return "${entity}{" +
          #foreach($field in ${table.fields})
              #if($!{foreach.index}==0)
      			"${field.propertyName}=" + ${field.propertyName} +
              #else
      			", ${field.propertyName}=" + ${field.propertyName} +
              #end
          #end
      			"}";
      	}
      #end
      }
      ```
    * 模板文件`mapper.java.vm`
      ```java
      package ${package.Mapper};

      import ${package.Entity}.${entity};
      import ${superMapperClassPackage};
      
      /**
       * <p>
       * $!{table.comment} Mapper 接口
       * </p>
       *
       * @author ${author}
       * @since ${date}
       */
      #if(${kotlin})
      interface ${table.mapperName} : ${superMapperClass}<${entity}>
      #else
      public interface ${table.mapperName} extends ${superMapperClass}<${entity}> {
      
      		}
      #end
      ```
    * 模板文件`mapper.xml.vm`
      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="${package.Mapper}.${table.mapperName}">
      
          #if(${enableCache})
              <!-- 开启二级缓存 -->
              <cache type="org.mybatis.caches.ehcache.LoggingEhcache"/>
      
          #end
          #if(${baseResultMap})
              <!-- 通用查询映射结果 -->
              <resultMap id="BaseResultMap" type="${package.Entity}.${entity}">
                  #foreach($field in ${table.fields})
                      #if(${field.keyFlag})##生成主键排在第一位
                          <id column="${field.name}" property="${field.propertyName}" />
                      #end
                  #end
                  #foreach($field in ${table.commonFields})##生成公共字段
                      <result column="${field.name}" property="${field.propertyName}" />
                  #end
                  #foreach($field in ${table.fields})
                      #if(!${field.keyFlag})##生成普通字段
                          <result column="${field.name}" property="${field.propertyName}" />
                      #end
                  #end
              </resultMap>
      
          #end
          #if(${baseColumnList})
              <!-- 通用查询结果列 -->
              <sql id="Base_Column_List">
                      #foreach($field in ${table.commonFields})
                          ${field.name},
                      #end
                      ${table.fieldNames}
              </sql>
      
          #end
      </mapper>
      ```
    * 模板文件`service.java.vm`
      ```java
      package ${package.Service};

      import com.baomidou.mybatisplus.plugins.Page;
      import ${package.Entity}.${entity};
      import ${superServiceClassPackage};
      import com.dcy.model.BootStrapTable;
      
      import java.util.List;
      
      /**
       * <p>
       * $!{table.comment} 服务类
       * </p>
       *
       * @author ${author}
       * @since ${date}
       */
      public interface ${table.serviceName} extends ${superServiceClass}<${entity}> {
      
          /**
           *  分页查询
           * @param bootStrapTable
           * @param ${table.entityPath}
           * @return
           */
          Page<${entity}> selectPage(BootStrapTable<${entity}> bootStrapTable,${entity} ${table.entityPath});
      
          List<AppCertificate> selectList(${entity} ${table.entityPath});
      }
      ```
    * 模板文件`servicelmpl.java.vm`
      ```java
      package ${package.ServiceImpl};

      import com.baomidou.mybatisplus.mapper.EntityWrapper;
      import com.baomidou.mybatisplus.plugins.Page;
      import com.dcy.model.BootStrapTable;
      import ${package.Entity}.${entity};
      import ${package.Mapper}.${table.mapperName};
      import ${package.Service}.${table.serviceName};
      import ${superServiceImplClassPackage};
      import org.springframework.stereotype.Service;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.transaction.annotation.Transactional;
      import com.dcy.utils.lang.StringUtils;
      
      import java.util.List;
      /**
       * <p>
       * $!{table.comment} 服务实现类
       * </p>
       *
       * @author ${author}
       * @since ${date}
       */
      @Service
      @Transactional
      public class ${table.serviceImplName} extends ${superServiceImplClass}<${table.mapperName}, ${entity}> implements ${table.serviceName} {
      
      
      @Autowired
      private ${table.mapperName} ${table.entityPath}Mapper;
      
      @Override
      public Page<${entity}> selectPage(BootStrapTable<${entity}> bootStrapTable, ${entity} ${table.entityPath}) {
      		EntityWrapper<${entity}> entityWrapper = new EntityWrapper<${entity}>();
      		getEntityWrapper(entityWrapper,${table.entityPath});
      		return super.selectPage(bootStrapTable.getPagePlus(),entityWrapper);
      		}
      
      @Override
      public List<${entity}> selectList(${entity} ${table.entityPath}) {
      		EntityWrapper<${entity}> entityWrapper = new EntityWrapper<${entity}>();
      		getEntityWrapper(entityWrapper,${table.entityPath});
      		return super.selectList(entityWrapper);
      		}
      
      /**
       *  公共查询条件
       * @param entityWrapper
       * @return
       */
      public EntityWrapper<${entity}> getEntityWrapper(EntityWrapper<${entity}> entityWrapper,${entity} ${table.entityPath}){
      		//条件拼接
          #foreach($field in ${table.fields})
              #if(!${field.keyFlag})
      				if (StringUtils.isNotBlank(${table.entityPath}.${getprefix}${field.capitalName}())){
      				entityWrapper.like(${entity}.${field.name.toUpperCase()},${table.entityPath}.${getprefix}${field.capitalName}());
      				}
              #end
          #end
      		return entityWrapper;
      		}
      }
      ```
* ### `logback`
  * #### 修改`pom.xml`引入`logback`依赖
    ```xml
    <dependencies>
      <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>commons-dbcp</groupId>
            <artifactId>commons-dbcp</artifactId>
            <version>1.4</version>
        </dependency>
        <dependency>
            <groupId>commons-pool</groupId>
            <artifactId>commons-pool</artifactId>
            <version>1.6</version>
            <!-- dbcp需要用到 -->
        </dependency>  
    </dependencies>
    ```
  * #### 日志级别
    > log4j2也有五种日志隔离级别,分别为:
    * trace:追踪,是最低的日志级别,相当于追踪程序的执行,一般不怎么使用
    * debug:一般在开发中,都将其设置为最低的日志级别.
    * info:输出感兴趣的信息,我在开发中一般都会用info去输出.
    * warn:警告.有些时候,虽然程序不会报错,但是还是需要告诉程序员的.
    * error:错误,这个在开发中也挺常用的,
    * fetal:极其重大的错误,这个一旦发生,程序基本上也要停止了,,目前限于开发经验,我还没有碰到要碰到它的地方.
    > 日志通过Logger对象输出,Logger对象的获取见下文,输出的API基本如下:
      `Logger对象.日志级别("日志内容");`
  * #### `classpath`下建立`logback-spring.xml`
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!-- 日志级别从低到高分为TRACE < DEBUG < INFO < WARN < ERROR < FATAL，如果设置为WARN，则低于WARN的信息都不会输出 -->
    <!-- scan:当此属性设置为true时，配置文档如果发生改变，将会被重新加载，默认值为true -->
    <!-- scanPeriod:设置监测配置文档是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。
                     当scan为true时，此属性生效。默认的时间间隔为1分钟。 -->
    <!-- debug:当此属性设置为true时，将打印出logback内部日志信息，实时查看logback运行状态。默认值为false。 -->
    <configuration scan="true">
    
        <contextName>logback</contextName>
    
        <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径--><!--设置系统日志目录-->
        <property name="LOG_PATH" value="./"/>
        <property name="APPDIR" value="logback_logs"/>
        <!-- 设置一个表示颜色的变量 -->
        <property name="CONSOLE_LOG_PATTERN"
                  value="%date{yyyy-MM-dd HH:mm:ss}    | %highlight(%-5level)  | %boldYellow(%thread)  | %boldGreen(%logger)   | %msg%n"/>
    
    
        <!-- 说明：
              1、日志级别及文件
                  日志记录采用分级记录，级别与日志文件名相对应，不同级别的日志信息记录到不同的日志文件中
                  例如：error级别记录到log_error_xxx.log或log_error.log（该文件为当前记录的日志文件），而log_error_xxx.log为归档日志，
                  日志文件按日期记录，同一天内，若日志文件大小等于或大于2M，则按0、1、2...顺序分别命名
                  例如log-level-2013-12-21.0.log
                  其它级别的日志也是如此。
              2、文件路径
                  若开发、测试用，在Eclipse中运行项目，则到Eclipse的安装路径查找logs文件夹，以相对路径../logs。
                  若部署到Tomcat下，则在Tomcat下的logs文件中
              3、Appender
                  FILEDEBUG对应debug级别，文件名以log-debug-xxx.log形式命名
                  FILEERROR对应error级别，文件名以log-error-xxx.log形式命名
                  FILEWARN对应warn级别，文件名以log-warn-xxx.log形式命名
                  FILEINFO对应info级别，文件名以log-info-xxx.log形式命名
                  CONSOLE将日志信息输出到控制上，为方便开发测试使用
        -->
    
        <!-- %m输出的信息,%p日志级别,%t线程名,%d日期,%c类的全名,,,, --><!-- ConsoleAppender：把日志输出到控制台 -->
        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
                <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level</pattern>
                <charset>UTF-8</charset>
            </encoder>
        </appender>
    
        <!-- debug 日志记录器，日期滚动记录 -->
        <appender name="FILEDEBUG" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <!-- 日志分包策略，当发生滚动时，决定 RollingFileAppender 的行为，涉及文件移动和重命名。 -->
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <!-- 归档的日志文件的路径，例如今天是2013-12-21日志，当前写的日志文件路径为file节点指定，可以将此文件与file指定文件路径设置为不同路径，从而将当前日志文件或归档日志文件置不同的目录。
                而2013-12-21的日志文件在由fileNamePattern指定。%d{yyyy-MM-dd}指定日期格式，%i指定索引 -->
                <fileNamePattern>${LOG_PATH}/${APPDIR}/debug/log-debug-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
                <!--日志文件保留天数-->
                <MaxHistory>50</MaxHistory>
                <!-- 除按日志记录之外，还配置了日志文件不能超过2M，若超过2M，日志文件会以索引0开始，
                命名日志文件，例如log-debug-2013-12-21.0.log -->
                <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                    <maxFileSize>2MB</maxFileSize>
                </timeBasedFileNamingAndTriggeringPolicy>
            </rollingPolicy>
            <!-- 如果是 true，日志被追加到文件结尾，如果是 false，清空现存文件，默认是true -->
            <append>true</append>
            <!-- 日志文件的格式 -->
            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
                <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger Line:%-3L - %msg%n</pattern>
                <charset>utf-8</charset>
            </encoder>
            <!-- 此日志文件只记录info级别的 -->
            <filter class="ch.qos.logback.classic.filter.LevelFilter">
                <level>debug</level>
                <onMatch>ACCEPT</onMatch>
                <onMismatch>DENY</onMismatch>
            </filter>
        </appender>
    
        <!-- error 日志记录器，日期滚动记录 -->
        <appender name="FILEERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <!-- 日志分包策略，当发生滚动时，决定 RollingFileAppender 的行为，涉及文件移动和重命名。 -->
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <!-- 归档的日志文件的路径，例如今天是2013-12-21日志，当前写的日志文件路径为file节点指定，可以将此文件与file指定文件路径设置为不同路径，从而将当前日志文件或归档日志文件置不同的目录。
                而2013-12-21的日志文件在由fileNamePattern指定。%d{yyyy-MM-dd}指定日期格式，%i指定索引 -->
                <fileNamePattern>${LOG_PATH}/${APPDIR}/error/log-error-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
                <!--日志文件保留天数-->
                <MaxHistory>50</MaxHistory>
                <!-- 除按日志记录之外，还配置了日志文件不能超过2M，若超过2M，日志文件会以索引0开始，
                命名日志文件，例如log-error-2013-12-21.0.log -->
                <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                    <maxFileSize>2MB</maxFileSize>
                </timeBasedFileNamingAndTriggeringPolicy>
            </rollingPolicy>
            <!-- 如果是 true，日志被追加到文件结尾，如果是 false，清空现存文件，默认是true -->
            <append>true</append>
            <!-- 日志文件的格式 -->
            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger Line:%-3L - %msg%n</pattern>
                <charset>utf-8</charset>
            </encoder>
            <!-- 此日志文件只记录info级别的 -->
            <filter class="ch.qos.logback.classic.filter.LevelFilter">
                <level>error</level>
                <onMatch>ACCEPT</onMatch>
                <onMismatch>DENY</onMismatch>
            </filter>
        </appender>
    
        <!-- warn 日志记录器，日期滚动记录 -->
        <appender name="FILEWARN" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <!-- 归档的日志文件的路径，例如今天是2013-12-21日志，当前写的日志文件路径为file节点指定，可以将此文件与file指定文件路径设置为不同路径，从而将当前日志文件或归档日志文件置不同的目录。
                而2013-12-21的日志文件在由fileNamePattern指定。%d{yyyy-MM-dd}指定日期格式，%i指定索引 -->
                <fileNamePattern>${LOG_PATH}/${APPDIR}/warn/log-warn-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
                <!--日志文件保留天数-->
                <MaxHistory>50</MaxHistory>
                <!-- 除按日志记录之外，还配置了日志文件不能超过2M，若超过2M，日志文件会以索引0开始，
                命名日志文件，例如log-error-2013-12-21.0.log -->
                <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                    <maxFileSize>2MB</maxFileSize>
                </timeBasedFileNamingAndTriggeringPolicy>
            </rollingPolicy>
            <!-- 追加方式记录日志 -->
            <append>true</append>
            <!-- 日志文件的格式 -->
            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger Line:%-3L - %msg%n</pattern>
                <charset>utf-8</charset>
            </encoder>
            <!-- 此日志文件只记录info级别的 -->
            <filter class="ch.qos.logback.classic.filter.LevelFilter">
                <level>warn</level>
                <onMatch>ACCEPT</onMatch>
                <onMismatch>DENY</onMismatch>
            </filter>
        </appender>
    
        <!-- info 日志记录器，日期滚动记录 -->
        <appender name="FILEINFO" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <!-- 日志记录器的滚动策略，按日期，按大小记录 -->
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <!-- 归档的日志文件的路径，例如今天是2013-12-21日志，当前写的日志文件路径为file节点指定，可以将此文件与file指定文件路径设置为不同路径，从而将当前日志文件或归档日志文件置不同的目录。
                而2013-12-21的日志文件在由fileNamePattern指定。%d{yyyy-MM-dd}指定日期格式，%i指定索引 -->
                <fileNamePattern>${LOG_PATH}/${APPDIR}/info/log-info-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
                <!--日志文件保留天数-->
                <MaxHistory>50</MaxHistory>
                <!-- 除按日志记录之外，还配置了日志文件不能超过2M，若超过2M，日志文件会以索引0开始，
                命名日志文件，例如log-error-2013-12-21.0.log -->
                <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                    <maxFileSize>2MB</maxFileSize>
                </timeBasedFileNamingAndTriggeringPolicy>
            </rollingPolicy>
            <!-- 追加方式记录日志 -->
            <append>true</append>
            <!-- 日志文件的格式 -->
            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger Line:%-3L - %msg%n</pattern>
                <charset>utf-8</charset>
            </encoder>
            <!-- 此日志文件只记录info级别的 -->
            <filter class="ch.qos.logback.classic.filter.LevelFilter">
                <level>info</level>
                <onMatch>ACCEPT</onMatch>
                <onMismatch>DENY</onMismatch>
            </filter>
        </appender>
    
        <!--日志异步到数据库  -->
        <appender name="DBAPPENDER" class="com.gadfly.spring_api.util.LogDBAppender">
            <connectionSource class="ch.qos.logback.core.db.DataSourceConnectionSource">
                <dataSource class="org.apache.commons.dbcp.BasicDataSource">
                    <driverClassName>com.mysql.cj.jdbc.Driver</driverClassName>
                    <url>jdbc:mysql://localhost:3306/spring_boot?characterEncoding=utf8&amp;useSSL=false</url>
                    <username>spring_boot</username>
                    <password>spring_boot</password>
                </dataSource>
            </connectionSource>
            <!--这里设置日志级别为error-->
            <filter class="com.gadfly.spring_api.util.LogbackMarkerFilter">
                <marker>DB</marker>
                <onMatch>ACCEPT</onMatch>
                <onMismatch>DENY</onMismatch>
            </filter>
        </appender>
    
        <!-- 打印到控制台 -->
        <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>CONSOLE -- ${CONSOLE_LOG_PATTERN}</pattern>
            </encoder>
        </appender>
    
        <logger name="com.gadfly.spring_api" level="DEBUG">
            <appender-ref ref="CONSOLE"/>
        </logger>
    
        <!-- show parameters for hibernate sql 专为 Hibernate 定制 -->
        <logger name="org.hibernate.type.descriptor.sql.BasicBinder"  level="TRACE"/>
        <logger name="org.hibernate.type.descriptor.sql.BasicExtractor"  level="DEBUG"/>
        <logger name="org.hibernate.SQL" level="DEBUG"/>
        <logger name="org.hibernate.engine.QueryParameters" level="DEBUG"/>
        <logger name="org.hibernate.engine.query.HQLQueryPlan" level="DEBUG" />
    
        <!--<logger name="org.springframework.data.mybatis" level="DEBUG"/>-->
        <logger name="org.springframework.aop.aspectj" level="ERROR"/>
    
        <logger name="javax.activation" level="WARN"/>
        <logger name="javax.mail" level="WARN"/>
        <logger name="javax.xml.bind" level="WARN"/>
        <logger name="ch.qos.logback" level="INFO"/>
        <logger name="com.codahale.metrics" level="WARN"/>
        <logger name="com.ryantenney" level="WARN"/>
        <logger name="com.sun" level="WARN"/>
        <logger name="com.zaxxer" level="WARN"/>
        <logger name="io.undertow" level="WARN"/>
        <logger name="net.sf.ehcache" level="WARN"/>
        <logger name="org.apache" level="WARN"/>
        <logger name="org.apache.catalina.startup.DigesterFactory" level="OFF"/>
        <logger name="org.bson" level="WARN"/>
        <logger name="org.hibernate.validator" level="WARN"/>
        <logger name="org.hibernate" level="WARN"/>
        <logger name="org.hibernate.ejb.HibernatePersistence" level="OFF"/>
        <logger name="org.springframework.web" level="INFO"/>
        <logger name="org.springframework.security" level="WARN"/>
        <logger name="org.springframework.cache" level="WARN"/>
        <logger name="org.thymeleaf" level="WARN"/>
        <logger name="org.xnio" level="WARN"/>
        <logger name="springfox" level="WARN"/>
        <logger name="sun.rmi" level="WARN"/>
        <logger name="liquibase" level="WARN"/>
        <logger name="sun.rmi.transport" level="WARN"/>
    
        <logger name="jdbc.connection" level="ERROR"/>
        <logger name="jdbc.resultset" level="ERROR"/>
        <logger name="jdbc.resultsettable" level="INFO"/>
        <logger name="jdbc.audit" level="ERROR"/>
        <logger name="jdbc.sqltiming" level="ERROR"/>
        <logger name="jdbc.sqlonly" level="INFO"/>
        
        <!-- 在日志中输出SQL -->
        <logger name="mapper所在的包名" level="DEBUG"></logger>
    
        <root level="INFO">
            <appender-ref ref="FILEERROR"/>
            <appender-ref ref="FILEWARN"/>
            <appender-ref ref="FILEINFO"/>
            <appender-ref ref="FILEDEBUG"/>
            <appender-ref ref="CONSOLE"/>
            <appender-ref ref="DBAPPENDER"/>
        </root>
    
    </configuration>
    ```
  * #### 自定义 LogDBAppender (Appender是logback框架中最重要的组件之一)
    ```java
    import ch.qos.logback.classic.db.DBHelper;
    import ch.qos.logback.classic.db.names.ColumnName;
    import ch.qos.logback.classic.db.names.DBNameResolver;
    import ch.qos.logback.classic.db.names.DefaultDBNameResolver;
    import ch.qos.logback.classic.db.names.TableName;
    import ch.qos.logback.classic.spi.CallerData;
    import ch.qos.logback.classic.spi.ILoggingEvent;
    import ch.qos.logback.core.db.DBAppenderBase;
    
    import java.lang.reflect.Method;
    import java.sql.Connection;
    import java.sql.PreparedStatement;
    import java.sql.SQLException;
    import java.util.HashMap;
    import java.util.Map;
    
    /**
     * @Title: LogDBAppender.java
     * @Description: TODO(日志持久化配置)
     * @Author: GadflyBSD  上午9:43
     */
    public class LogDBAppender extends DBAppenderBase<ILoggingEvent> {
    
    	protected String insertSQL;
    	protected static final Method GET_GENERATED_KEYS_METHOD;
    
    	private DBNameResolver dbNameResolver;
    
    	static final int TIMESTMP_INDEX = 1;
    	static final int FORMATTED_MESSAGE_INDEX = 2;
    	static final int LOGGER_NAME_INDEX = 3;
    	static final int LEVEL_STRING_INDEX = 4;
    	static final int THREAD_NAME_INDEX = 5;
    	static final int REFERENCE_FLAG_INDEX = 6;
    	static final int ARG0_INDEX = 7;
    	static final int ARG1_INDEX = 8;
    	static final int ARG2_INDEX = 9;
    	static final int ARG3_INDEX = 10;
    	static final int CALLER_FILENAME_INDEX = 11;
    	static final int CALLER_CLASS_INDEX = 12;
    	static final int CALLER_METHOD_INDEX = 13;
    	static final int CALLER_LINE_INDEX = 14;
    	static final int EVENT_ID_INDEX = 15;
    
    	static final StackTraceElement EMPTY_CALLER_DATA = CallerData.naInstance();
    
    	static {
    		// PreparedStatement.getGeneratedKeys() method was added in JDK 1.4
    		Method getGeneratedKeysMethod;
    		try {
    			// the
    			getGeneratedKeysMethod = PreparedStatement.class.getMethod("getGeneratedKeys", (Class[]) null);
    		} catch (Exception ex) {
    			getGeneratedKeysMethod = null;
    		}
    		GET_GENERATED_KEYS_METHOD = getGeneratedKeysMethod;
    	}
    
    	public void setDbNameResolver(DBNameResolver dbNameResolver) {
    		this.dbNameResolver = dbNameResolver;
    	}
    
    	@Override
    	public void start() {
    		if (dbNameResolver == null)
    			dbNameResolver = new DefaultDBNameResolver();
    		insertSQL = buildInsertSQL(dbNameResolver);
    		super.start();
    	}
    
    	@Override
    	protected void subAppend(ILoggingEvent event, Connection connection, PreparedStatement insertStatement) throws Throwable {
    
    		bindLoggingEventWithInsertStatement(insertStatement, event);
    		bindLoggingEventArgumentsWithPreparedStatement(insertStatement, event.getArgumentArray());
    
    		// This is expensive... should we do it every time?
    		bindCallerDataWithPreparedStatement(insertStatement, event.getCallerData());
    
    		int updateCount = insertStatement.executeUpdate();
    		if (updateCount != 1) {
    			addWarn("Failed to insert loggingEvent");
    		}
    	}
    
    	@Override
    	protected void secondarySubAppend(ILoggingEvent event, Connection connection, long eventId) throws Throwable {
    		Map<String, String> mergedMap = mergePropertyMaps(event);
    		//insertProperties(mergedMap, connection, eventId);
    
    //        if (event.getThrowableProxy() != null) {
    //            insertThrowable(event.getThrowableProxy(), connection, eventId);
    //        }
    	}
    
    	void bindLoggingEventWithInsertStatement(PreparedStatement stmt, ILoggingEvent event) throws SQLException {
    		stmt.setLong(TIMESTMP_INDEX, event.getTimeStamp());
    		stmt.setString(FORMATTED_MESSAGE_INDEX, event.getFormattedMessage());
    		stmt.setString(LOGGER_NAME_INDEX, event.getLoggerName());
    		stmt.setString(LEVEL_STRING_INDEX, event.getLevel().toString());
    		stmt.setString(THREAD_NAME_INDEX, event.getThreadName());
    		stmt.setShort(REFERENCE_FLAG_INDEX, DBHelper.computeReferenceMask(event));
    	}
    
    	void bindLoggingEventArgumentsWithPreparedStatement(PreparedStatement stmt, Object[] argArray) throws SQLException {
    
    		int arrayLen = argArray != null ? argArray.length : 0;
    
    		for (int i = 0; i < arrayLen && i < 4; i++) {
    			stmt.setString(ARG0_INDEX + i, asStringTruncatedTo254(argArray[i]));
    		}
    		if (arrayLen < 4) {
    			for (int i = arrayLen; i < 4; i++) {
    				stmt.setString(ARG0_INDEX + i, null);
    			}
    		}
    	}
    
    	String asStringTruncatedTo254(Object o) {
    		String s = null;
    		if (o != null) {
    			s = o.toString();
    		}
    
    		if (s == null) {
    			return null;
    		}
    		if (s.length() <= 254) {
    			return s;
    		} else {
    			return s.substring(0, 254);
    		}
    	}
    
    	void bindCallerDataWithPreparedStatement(PreparedStatement stmt, StackTraceElement[] callerDataArray) throws SQLException {
    
    		StackTraceElement caller = extractFirstCaller(callerDataArray);
    
    		stmt.setString(CALLER_FILENAME_INDEX, caller.getFileName());
    		stmt.setString(CALLER_CLASS_INDEX, caller.getClassName());
    		stmt.setString(CALLER_METHOD_INDEX, caller.getMethodName());
    		stmt.setString(CALLER_LINE_INDEX, Integer.toString(caller.getLineNumber()));
    	}
    
    	private StackTraceElement extractFirstCaller(StackTraceElement[] callerDataArray) {
    		StackTraceElement caller = EMPTY_CALLER_DATA;
    		if (hasAtLeastOneNonNullElement(callerDataArray))
    			caller = callerDataArray[0];
    		return caller;
    	}
    
    	private boolean hasAtLeastOneNonNullElement(StackTraceElement[] callerDataArray) {
    		return callerDataArray != null && callerDataArray.length > 0 && callerDataArray[0] != null;
    	}
    
    	Map<String, String> mergePropertyMaps(ILoggingEvent event) {
    		Map<String, String> mergedMap = new HashMap<String, String>();
    		// we add the context properties first, then the event properties, since
    		// we consider that event-specific properties should have priority over
    		// context-wide properties.
    		Map<String, String> loggerContextMap = event.getLoggerContextVO().getPropertyMap();
    		Map<String, String> mdcMap = event.getMDCPropertyMap();
    		if (loggerContextMap != null) {
    			mergedMap.putAll(loggerContextMap);
    		}
    		if (mdcMap != null) {
    			mergedMap.putAll(mdcMap);
    		}
    
    		return mergedMap;
    	}
    
    	@Override
    	protected Method getGeneratedKeysMethod() {
    		return GET_GENERATED_KEYS_METHOD;
    	}
    
    	@Override
    	protected String getInsertSQL() {
    		return insertSQL;
    	}
    
    
    	static String buildInsertSQL(DBNameResolver dbNameResolver) {
    		StringBuilder sqlBuilder = new StringBuilder("INSERT INTO ");
    		sqlBuilder.append(dbNameResolver.getTableName(TableName.LOGGING_EVENT)).append(" (");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.TIMESTMP)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.FORMATTED_MESSAGE)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.LOGGER_NAME)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.LEVEL_STRING)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.THREAD_NAME)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.REFERENCE_FLAG)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.ARG0)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.ARG1)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.ARG2)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.ARG3)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.CALLER_FILENAME)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.CALLER_CLASS)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.CALLER_METHOD)).append(", ");
    		sqlBuilder.append(dbNameResolver.getColumnName(ColumnName.CALLER_LINE)).append(") ");
    		sqlBuilder.append("VALUES (?, ?, ? ,?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)");
    		return sqlBuilder.toString();
    	}
    
    }
    ```
  * #### 日志拦截 新建LogbackMarkerFilter类
    ```java
    import ch.qos.logback.classic.spi.ILoggingEvent;
    import ch.qos.logback.core.filter.AbstractMatcherFilter;
    import ch.qos.logback.core.spi.FilterReply;
    import org.slf4j.Marker;
    import org.slf4j.MarkerFactory;
    
    /**
     * @Title: LogbackMarkerFilter.java
     * @Description: TODO(日志拦截)
     * @Author: GadflyBSD  上午10:15
     */
    public class LogbackMarkerFilter extends AbstractMatcherFilter<ILoggingEvent> {
    
    	private Marker markerToMatch = null;
    
    	@Override
    	public void start() {
    		if (null != this.markerToMatch) {
    			super.start();
    		} else {
    			addError(" no MARKER yet !");
    		}
    	}
    
    	@Override
    	public FilterReply decide(ILoggingEvent event) {
    		Marker marker = event.getMarker();
    		if (!isStarted()) {
    			return FilterReply.NEUTRAL;
    		}
    		if (null == marker) {
    			return onMismatch;
    		}
    		if (markerToMatch.contains(marker)) {
    			return onMatch;
    		}
    		return onMismatch;
    	}
    
    	public void setMarker(String markerStr) {
    		if (null != markerStr) {
    			markerToMatch = MarkerFactory.getMarker(markerStr);
    		}
    	}
    }
    ```
  * #### 在本地新建库auge_log，在该库中新建表error_log
    ```mysql
    BEGIN;
    DROP TABLE IF EXISTS logging_event_property;
    DROP TABLE IF EXISTS logging_event_exception;
    DROP TABLE IF EXISTS logging_event;
    COMMIT;
    
    BEGIN;
    CREATE TABLE logging_event(
      timestmp         BIGINT NOT NULL,
      formatted_message  TEXT NOT NULL,
      logger_name       VARCHAR(254) NOT NULL,
      level_string      VARCHAR(254) NOT NULL,
      thread_name       VARCHAR(254),
      reference_flag    SMALLINT,
      arg0              VARCHAR(254),
      arg1              VARCHAR(254),
      arg2              VARCHAR(254),
      arg3              VARCHAR(254),
      caller_filename   VARCHAR(254) NOT NULL,
      caller_class      VARCHAR(254) NOT NULL,
      caller_method     VARCHAR(254) NOT NULL,
      caller_line       CHAR(4) NOT NULL,
      event_id          BIGINT NOT NULL AUTO_INCREMENT PRIMARY KEY
    );
    COMMIT;
    
    BEGIN;
    CREATE TABLE logging_event_property(
      event_id          BIGINT NOT NULL,
      mapped_key        VARCHAR(254) NOT NULL,
      mapped_value      TEXT,
      PRIMARY KEY(event_id, mapped_key),
      FOREIGN KEY (event_id) REFERENCES logging_event(event_id)
    );
    COMMIT;
    
    BEGIN;
    CREATE TABLE logging_event_exception(
      event_id         BIGINT NOT NULL,
      i                SMALLINT NOT NULL,
      trace_line       VARCHAR(254) NOT NULL,
      PRIMARY KEY(event_id, i),
      FOREIGN KEY (event_id) REFERENCES logging_event(event_id)
    );
    COMMIT;
    ```
  * #### 通过`@Slf4j`标签去声明式注解日志对象的使用
    ```java
    @Slf4j
    @RestController
    public class HfwController {
      //指定配置的Marker 
      log.info(MarkerFactory.getMarker("DB"), "hello,logback!");
    }
    ```
* ### `Swagger2`
  * #### 修改`pom.xml`引入`Swagger2`依赖 
    ```xml
    <!-- SpringBoot整合Swagger2 -->
    <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.9.2</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.9.2</version>
    </dependency>
    ```
  * #### 创建 `Swagger2` 配置类
    > * 用 `@Configuration` 注解该类，等价于 `XML` 中配置 `beans`；
    > * 用 `@Bean` 标注方法等价于 `XML` 中配置 `bean`。
    ```java
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import springfox.documentation.builders.ApiInfoBuilder;
    import springfox.documentation.builders.PathSelectors;
    import springfox.documentation.builders.RequestHandlerSelectors;
    import springfox.documentation.service.ApiInfo;
    import springfox.documentation.spi.DocumentationType;
    import springfox.documentation.spring.web.plugins.Docket;
    
    /**
     * @author GadflyBSD
     * @ClassName com.gadfly.spring_api.Swgger2
     * @Description
     */
    @Configuration
    public class Swagger2 {
    
    	@Bean
    	public Docket createRestApi() {
    		return new Docket(DocumentationType.SWAGGER_2)
    				.apiInfo(apiInfo())
    				.select()
    				.apis(RequestHandlerSelectors.basePackage("com.gadfly.spring_api"))
    				.paths(PathSelectors.any())
    				.build();
    	}
    
    	private ApiInfo apiInfo() {
    		return new ApiInfoBuilder()
    				.title("SpringBoot 利用 Swagger2 构建 API 文档")
    				.description("简单优雅的restfun风格，https://github.com/GadflyBSD")
    				.termsOfServiceUrl("https://github.com/GadflyBSD/spring_api")
    				.version("1.0")
    				.build();
    	}
    }
    ```
  * #### `Application.class` 加上注解 `@EnableSwagger2` 表示开启 `Swagger2`
  * #### `Swagger2` 注解
    > swagger通过注解表明该接口会生成文档，包括接口名、请求方法、参数、返回信息的等等。
    * @Api：修饰整个类，描述Controller的作用
    * @ApiOperation：描述一个类的一个方法，或者说一个接口
    * @ApiParam：单个参数描述
    * @ApiModel：用对象来接收参数
    * @ApiProperty：用对象接收参数时，描述对象的一个字段
    * @ApiResponse：HTTP响应其中1个描述
    * @ApiResponses：HTTP响应整体描述
    * @ApiIgnore：使用该注解忽略这个API
    * @ApiError ：发生错误返回的信息
    * @ApiImplicitParam：一个请求参数
    * @ApiImplicitParams：多个请求参数
  * #### 修改控制器类，对类和方法增加 `Swagger2` 注解
    ```java
    import cn.saytime.bean.JsonResult;
    import cn.saytime.bean.User;
    import io.swagger.annotations.Api;
    import io.swagger.annotations.ApiImplicitParam;
    import io.swagger.annotations.ApiImplicitParams;
    import io.swagger.annotations.ApiOperation;
    import org.springframework.http.ResponseEntity;
    import org.springframework.web.bind.annotation.PathVariable;
    import org.springframework.web.bind.annotation.RequestBody;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;
    import org.springframework.web.bind.annotation.RestController;
    import springfox.documentation.annotations.ApiIgnore;
    
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    
    /**
     * @author GadflyBSD
     * @ClassName cn.saytime.web.UserController
     * @Description
     */
    @Api(description = "用户接口")
    @RestController
    @RequestMapping("/api")
    public class UserController {
    
    	// 创建线程安全的Map
    	static Map<Integer, User> users = Collections.synchronizedMap(new HashMap<Integer, User>());
    
    	/**
    	 * 根据ID查询用户
    	 * @param id
    	 * @return
    	 */
    	@ApiOperation(value="获取用户详细信息", notes="根据url的id来获取用户详细信息")
    	@ApiImplicitParam(name = "id", value = "用户ID", required = true, dataType = "Integer", paramType = "path")
    	@RequestMapping(value = "user/{id}", method = RequestMethod.GET)
    	public ResponseEntity<JsonResult> getUserById (@PathVariable(value = "id") Integer id){
    		JsonResult r = new JsonResult();
    		try {
    			User user = users.get(id);
    			r.setResult(user);
    			r.setStatus("ok");
    		} catch (Exception e) {
    			r.setResult(e.getClass().getName() + ":" + e.getMessage());
    			r.setStatus("error");
    			e.printStackTrace();
    		}
    		return ResponseEntity.ok(r);
    	}
    
    	/**
    	 * 查询用户列表
    	 * @return
    	 */
    	@ApiOperation(value="获取用户列表", notes="获取用户列表")
    	@RequestMapping(value = "users", method = RequestMethod.GET)
    	public ResponseEntity<JsonResult> getUserList (){
    		JsonResult r = new JsonResult();
    		try {
    			List<User> userList = new ArrayList<User>(users.values());
    			r.setResult(userList);
    			r.setStatus("ok");
    		} catch (Exception e) {
    			r.setResult(e.getClass().getName() + ":" + e.getMessage());
    			r.setStatus("error");
    			e.printStackTrace();
    		}
    		return ResponseEntity.ok(r);
    	}
    
    	/**
    	 * 添加用户
    	 * @param user
    	 * @return
    	 */
    	@ApiOperation(value="创建用户", notes="根据User对象创建用户")
    	@ApiImplicitParam(name = "user", value = "用户详细实体user", required = true, dataType = "User")
    	@RequestMapping(value = "user", method = RequestMethod.POST)
    	public ResponseEntity<JsonResult> add (@RequestBody User user){
    		JsonResult r = new JsonResult();
    		try {
    			users.put(user.getId(), user);
    			r.setResult(user.getId());
    			r.setStatus("ok");
    		} catch (Exception e) {
    			r.setResult(e.getClass().getName() + ":" + e.getMessage());
    			r.setStatus("error");
    
    			e.printStackTrace();
    		}
    		return ResponseEntity.ok(r);
    	}
    
    	/**
    	 * 根据id删除用户
    	 * @param id
    	 * @return
    	 */
    	@ApiOperation(value="删除用户", notes="根据url的id来指定删除用户")
    	@ApiImplicitParam(name = "id", value = "用户ID", required = true, dataType = "Long", paramType = "path")
    	@RequestMapping(value = "user/{id}", method = RequestMethod.DELETE)
    	public ResponseEntity<JsonResult> delete (@PathVariable(value = "id") Integer id){
    		JsonResult r = new JsonResult();
    		try {
    			users.remove(id);
    			r.setResult(id);
    			r.setStatus("ok");
    		} catch (Exception e) {
    			r.setResult(e.getClass().getName() + ":" + e.getMessage());
    			r.setStatus("error");
    
    			e.printStackTrace();
    		}
    		return ResponseEntity.ok(r);
    	}
    
    	/**
    	 * 根据id修改用户信息
    	 * @param user
    	 * @return
    	 */
    	@ApiOperation(value="更新信息", notes="根据url的id来指定更新用户信息")
    	@ApiImplicitParams({
    			@ApiImplicitParam(name = "id", value = "用户ID", required = true, dataType = "Long",paramType = "path"),
    			@ApiImplicitParam(name = "user", value = "用户实体user", required = true, dataType = "User")
    	})
    	@RequestMapping(value = "user/{id}", method = RequestMethod.PUT)
    	public ResponseEntity<JsonResult> update (@PathVariable("id") Integer id, @RequestBody User user){
    		JsonResult r = new JsonResult();
    		try {
    			User u = users.get(id);
    			u.setUsername(user.getUsername());
    			u.setAge(user.getAge());
    			users.put(id, u);
    			r.setResult(u);
    			r.setStatus("ok");
    		} catch (Exception e) {
    			r.setResult(e.getClass().getName() + ":" + e.getMessage());
    			r.setStatus("error");
    
    			e.printStackTrace();
    		}
    		return ResponseEntity.ok(r);
    	}
    
    	@ApiIgnore//使用该注解忽略这个API
    	@RequestMapping(value = "/hi", method = RequestMethod.GET)
    	public String  jsonTest() {
    		return " hi you!";
    	}
    }
    ```
  * #### 启动 `Spring Boot` 项目，访问项目的 `Swagger2` 文档
    > 启动 `SpringBoot` 项目，访问 `http://localhost:8080/swagger-ui.html`