# 善用Mybatis Plus，提高你的开发效率吧！

<font color = gray>参考：https://baomidou.com/</font>

> MyBatis Plus是MyBatis的增强工具，只增强不改变。核心功能有：代码生成器、CRUD接口、条件构造器、主键策略、自定义ID生成器

<h2>1、代码生成器</h2>

<h3>1.1、example</h3>

<h4>1.1.1、依赖</h4>

```xml
         <!-- mybatis-plus-boot-starter 这里用这个，自动配置mybatis -->
         <!-- 事实上想要实现自动生成代码，仅仅使用mybatis-plus就够了 -->
        <dependency>
            <groupId>com.baomidou</groupId>
           <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.3.1</version>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.5.3.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.3</version>
        </dependency>
```

<h4>1.1.2、实现代码生成</h4>

>各种配置类：会发现这些类的编写和Stream.Builder有些相似。<br>配置参考：https://baomidou.com/pages/981406/
```java
package org.arvin.generator;

import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.rules.DateType;

import java.util.Collections;

public class BaseGenerator {
    /**
     * 策略配置
     */
    protected static StrategyConfig.Builder strategyConfig() {
        return new StrategyConfig.Builder()
                .addTablePrefix("ums_");
    }

    /**
     * 全局配置
     */
    protected static GlobalConfig.Builder globalConfig() {
        return new GlobalConfig.Builder()
                .outputDir("E:\\others project\\iot\\iot-mgb\\src\\main\\java")
                .author("arvin")
                .dateType(DateType.TIME_PACK)
                .commentDate("yyyy-MM-dd");
    }

    /**
     * 包配置
     */
    protected static PackageConfig.Builder packageConfig() {
        return new PackageConfig.Builder()
//                .parent("org.arvin")
//                .pathInfo(Collections.singletonMap(OutputFile.mapper,"E:\\others project\\iot\\iot-mgb\\src\\main\\java"))
//                .pathInfo(Collections.singletonMap(OutputFile.entity,"E:\\others project\\iot\\iot-mgb\\src\\main\\java"))
//                .pathInfo(Collections.singletonMap(OutputFile.service,"E:\\others project\\iot\\iot-admin\\src\\main\\java"))
//                .pathInfo(Collections.singletonMap(OutputFile.serviceImpl,"E:\\others project\\iot\\iot-admin\\src\\main\\java"))
//                .pathInfo(Collections.singletonMap(OutputFile.controller,"E:\\others project\\iot\\iot-admin\\src\\main\\java"))
                .pathInfo(Collections.singletonMap(OutputFile.xml,"E:\\others project\\iot\\iot-mgb\\src\\main\\resources\\mapper"))
                .entity("pojo")
                .service("service")
                .serviceImpl("service.impl")
                .mapper("mapper")
                .controller("controller")
                ;
    }

    /**
     * 模板配置
     */
    protected static TemplateConfig.Builder templateConfig() {
        return new TemplateConfig.Builder();
    }

    /**
     * 注入配置
     */
    protected static InjectionConfig.Builder injectionConfig() {
        // 测试自定义输出文件之前注入操作，该操作再执行生成代码前 debug 查看
        return new InjectionConfig.Builder().beforeOutputFile((tableInfo, objectMap) -> {
            System.out.println("tableInfo: " + tableInfo.getEntityName() + " objectMap: " + objectMap.size());
        });
    }
}
```

>生成代码程序

```java
package org.arvin.generator;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;

public class MySQLGenerator extends BaseGenerator{
    /**
     * 数据源配置
     */
    private static final DataSourceConfig DATA_SOURCE_CONFIG = new DataSourceConfig
            .Builder("jdbc:mysql://localhost:3306/iot?useUnicode=true&useSSL=false&characterEncoding=utf8",
            "root", "")
            .schema("arvin")
            .build();
    
    public static void main(String[] args) {
        AutoGenerator generator = new AutoGenerator(DATA_SOURCE_CONFIG);
        generator.strategy(strategyConfig().build());
        generator.global(globalConfig().build());
        generator.packageInfo(packageConfig().build());
        generator.injection(injectionConfig().build());
        generator.execute();
    }
}
```

<h4>1.1.3、使用</h4>

>启动main方法便会按照配置自动生成entity、mapper、xml、service、controller，是不是很方便？

<h3>1.2、它是如何实现？</h3>

> 我们打开AutoGenerator类会发现，代码生成核心功能是Velocity引擎赋予的，那mybatis-plus做了什么？其实它在这里充当“翻译”的角色，作为我们与Velocity沟通的桥梁以及读取数据库中内容。

>**Velocity介绍**<br>Velocity是一个基于Java的模板引擎，基于MVC模型实现，其提供了一个Context容器(相当于Spring的Model)，在java代码里面我们可以往容器中存值，然后在vm文件中使用特定的语法获取(相当于Spring页面中取值如freemarker、thymeleaf)。<br>官网：http://velocity.apache.org/

```java
 //
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package com.baomidou.mybatisplus.generator;

import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.TemplateConfig;
import com.baomidou.mybatisplus.generator.config.builder.ConfigBuilder;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.engine.AbstractTemplateEngine;
import com.baomidou.mybatisplus.generator.engine.VelocityTemplateEngine;
import java.util.List;
import org.jetbrains.annotations.NotNull;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class AutoGenerator {
    private static final Logger logger = LoggerFactory.getLogger(AutoGenerator.class);
    protected ConfigBuilder config;
    protected InjectionConfig injection;
    private DataSourceConfig dataSource;
    private StrategyConfig strategy;
    private PackageConfig packageInfo;
    private TemplateConfig template;
    private GlobalConfig globalConfig;

    private AutoGenerator() {
    }

    public AutoGenerator(@NotNull DataSourceConfig dataSourceConfig) {
        this.dataSource = dataSourceConfig;
    }

    public AutoGenerator injection(@NotNull InjectionConfig injectionConfig) {
        this.injection = injectionConfig;
        return this;
    }

    public AutoGenerator strategy(@NotNull StrategyConfig strategyConfig) {
        this.strategy = strategyConfig;
        return this;
    }

    public AutoGenerator packageInfo(@NotNull PackageConfig packageConfig) {
        this.packageInfo = packageConfig;
        return this;
    }

    public AutoGenerator template(@NotNull TemplateConfig templateConfig) {
        this.template = templateConfig;
        return this;
    }

    public AutoGenerator global(@NotNull GlobalConfig globalConfig) {
        this.globalConfig = globalConfig;
        return this;
    }

    public AutoGenerator config(@NotNull ConfigBuilder configBuilder) {
        this.config = configBuilder;
        return this;
    }

    public void execute() {
        this.execute((AbstractTemplateEngine)null);
    }

    public void execute(AbstractTemplateEngine templateEngine) {
        logger.debug("==========================准备生成文件...==========================");
        if (null == this.config) {
            this.config = new ConfigBuilder(this.packageInfo, this.dataSource, this.strategy, this.template, this.globalConfig, this.injection);
        }

        if (null == templateEngine) {
            templateEngine = new VelocityTemplateEngine();
        }

        ((AbstractTemplateEngine)templateEngine).setConfigBuilder(this.config);
        ((AbstractTemplateEngine)templateEngine).init(this.config).batchOutput().open();
        logger.debug("==========================文件生成完成！！！==========================");
    }

    protected @NotNull List<TableInfo> getAllTableInfoList(@NotNull ConfigBuilder config) {
        return config.getTableInfoList();
    }

    public ConfigBuilder getConfig() {
        return this.config;
    }

    public InjectionConfig getInjectionConfig() {
        return this.injection;
    }

    public DataSourceConfig getDataSource() {
        return this.dataSource;
    }

    public StrategyConfig getStrategy() {
        return this.strategy;
    }

    public PackageConfig getPackageInfo() {
        return this.packageInfo;
    }

    public TemplateConfig getTemplate() {
        return this.template;
    }

    public GlobalConfig getGlobalConfig() {
        return this.globalConfig;
    }
}
```

>详情待续……

<h2>2、CRUD接口</h2>

>基本封装好了对单表的增删改查。<br>参考：https://baomidou.com/pages/49cc81/