### mybatis-generator可以自动生成mapper.xml/entity实体类/Dao层
   - mapper.xml  存放sql语句
   - entity 实体类 对应数据库表的字段
   - Dao层 中的方法名对应 mapper.xml中标签的id名
   
### IDEA中使用mybatis-generator
-   1.在pom文件的<build>下的<plugins>添加以下配置
    
    [*] 记住mysql连接一定添加在plugins中
```xml
<plugin>
       <groupId>org.mybatis.generator</groupId>
       <artifactId>mybatis-generator-maven-plugin</artifactId>
       <version>1.3.2</version>
       <configuration>
             <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
             <!--在控制台打印执行日志-->
             <verbose>true</verbose>
             <!--重复生成时会覆盖之前的文件-->
             <overwrite>true</overwrite>
       </configuration>
</plugin>
```
    
-   2.新建generatorConfig.xml配置文件,这个文件的存放位置要对应上边Plugin标签中<configurationFile>中配置的路径
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <classPathEntry location="D:\software\repository\mysql\mysql-connector-java\8.0.19\mysql-connector-java-8.0.19.jar"/>
    <!-- context 是逆向工程的主要配置信息 -->
    <!-- id：起个名字 -->
    <!-- targetRuntime：设置生成的文件适用于那个 mybatis 版本 -->
    <context id="Mysql" targetRuntime="MyBatis3">

        <property name="autoDelimitKeywords" value="true" />
        <property name="beginningDelimiter" value="`" />
        <property name="endingDelimiter" value="`" />
        <property name="javaFileEncoding" value="utf-8" />
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin" />

        <plugin type="org.mybatis.generator.plugins.ToStringPlugin" />

        <!-- 注释 -->
        <commentGenerator >
            <property name="suppressAllComments" value="true"/><!-- 是否取消注释 -->
            <property name="suppressDate" value="true" /> <!-- 是否生成注释代时间戳-->
        </commentGenerator>

        <!--数据库链接地址账号密码-->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306/jtdb?characterEncoding=utf-8&amp;serverTimezone=GMT"
                        userId="root"
                        password="root">
            <property name="nullCatalogMeansCurrent" value="true" />
        </jdbcConnection>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!-- 是否使用bigDecimal， false可自动转化以下类型（Long, Integer, Short, etc.） -->
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!--生成Model类存放位置-->
        <javaModelGenerator targetPackage="com.cn.entity" targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- 生成mapxml文件 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources" >
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- 生成mapxml对应client，也就是接口dao -->
        <javaClientGenerator targetPackage="com.cn.mapper" targetProject="src/main/java" type="XMLMAPPER" >
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>

        <table tableName="user" enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false">
            <generatedKey column="id" sqlStatement="Mysql" identity="true" />
        </table>
        <table tableName="tb_item" enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false">
            <generatedKey column="id" sqlStatement="Mysql" identity="true" />
        </table>

    </context>
</generatorConfiguration>
```
   [*]在generatorConfig.xml中'<table>'标签可以有多个
   
   [*]这时双击在右侧Maven标签栏中的Plugins文件夹下的mybatis-generator下的mybatis-generator:generate
