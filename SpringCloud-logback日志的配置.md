>引入的依赖：其实logback的依赖不用特意的添加，只需要添加
 <br>web依赖，自然就会有logback的依赖；下面是web依赖:
```xml
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

>SpringCloud 默认的文件名是logback.xml,所以我们自定义的日志文件可以使用默认的，但最好定义成logback-xxxx.xml；
 ##### 1.xml头部，既然是xml文件，基础的字符集标签肯定少不了：
 ```xml
 <?xml version="1.0" encoding="UTF-8"?>
 ```
 ##### 2.logback根标签:
      <br>&emsp; &emsp;在logBack中根标签是。此标签包含有三个属性：debug、scan、scanPeriod：
      <br>&emsp; &emsp;2.1>:debug:即是否打印出logback内部的日志信息，默认为false；
      <br>&emsp; &emsp;2.2>:scan:是否扫描配置文件,与spring boot 热部署结合使用，当配置文件更改时，会自动启动；
      <br>&emsp; &emsp;2.3>:scanPeriod：扫描配置文件是否有修改的时间间隔；该属性必须与scan属性结合使用，当scan属性为true时，会已自定义的时间扫描配置文件；默认为1分钟。
 ##### 3.自定义标签：
      <br> &emsp; &emsp;在该标签中可以集中定义一些变量的名字以及值，然后再其他的地方通过${}引用；该用法类似于pom文件中标签的用法；比如：我在使用时将日志文件的路基以及名称定义在文档的头部；以及控制台或者文件中日志输出的格式，都定义在该标签中；
```xml
//格式化输出：%d表示日期，%thread表示线程名，%-5level：级别 从左显示5个字符宽度  %msg：日志消息，%n是换行符
//<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
<property name="CONSOLE_LOG_PATTERN"
          value="%d{yyyy-MM-dd HH:mm:ss.SSSS} - [thread:%thread] - [level:%-5level] - [%class : %method : %line] - %msg %n"/>
<property name="FILE_LOG_PATTERN"
          value="%d{yyyy-MM-dd HH:mm:ss.SSS} - [thread:%thread] - [level:%-5level] - [%class : %method : %line] - %msg %n"/>
//路径不能使用相对路径  路劲上的分隔符 windows 上和linux上不一样 
<property name="LOG_FILE_PATH" value="D:/Log/Test-Cluster-Master-Log.log"/>
<property name="APPENFER_NAME_CONSOLE" value="CONSOLE"/>
<property name="APPENDER_NAME_FILE" value="FILE"/>
<property name="SPRING_PROFILE_TESR" value="test"/>
<property name="ROOT_LOG_LEVEL" value="debug"/>
```
##### 4.appender标签：
       <br>&emsp; &emsp;该标签集中配置一种日志的格式；比如：控制台输出日志的配置，输出到日志文件中的日志的配置；
       <br>&emsp; &emsp;该标签包含有两个属性：
       <br>&emsp; &emsp;name 属性：用于定义此配置的名称，也方便其他地方的引用该名称（比如 与root标签同时使用时会引用这个name属性）；
       <br>&emsp; &emsp;class属性：用于定义该appender标签引用的类：一般有 ConsoleAppender（用于控制台输出日志配置） 和 RollingFileAppender（用于文件中输出日志的配置）两个；
控制台输出日志的配置：
```xml
<appender name="${APPENFER_NAME_CONSOLE}" class="ch.qos.logback.core.ConsoleAppender">
 #输出到控制台的日志的 格式 以及 使用的 字符集
  <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
    <pattern>${CONSOLE_LOG_PATTERN}</pattern>
    <charset>UTF-8</charset>
  </encoder>
</appender>
```
###### 4.1 日志文件的配置：
```xml
<appender name="${APPENDER_NAME_FILE}" class="ch.qos.logback.core.rolling.RollingFileAppender">
 <!--<File>${LOG_FILE_PATH}</File>-->
  #日志文件 分隔保存的策略  基于 大小和日期的方式 分隔
  <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
    #日志文件 输出的文件名的格式 以及归档的分隔方式
    #<FileNamePattern>${LOG_FILE_PATH}.%d{yyyy-MM-dd HH HH:mm:ss}.%i.log</FileNamePattern>有敏感符号 创建不成功
    <FileNamePattern>D:\Log\Test_Cluster_Master_Log-%d{yyyy-MM-dd}-%i.log</FileNamePattern>                                        <MaxHistory>1</MaxHistory> ----  日志文件保留的最大时长  只能是整数 
     <maxFileSize>1MB</maxFileSize> ------ 单个日志的最大容量 ，超出的话 会重新创建日志文件
     <totalSizeCap>2MB</totalSizeCap>----日志的总的大小
  </rollingPolicy>

   <encoder><!--class属性 可以省去-->
       <pattern>${FILE_LOG_PATTERN}</pattern>
       <charset>UTF-8</charset>
   </encoder>
</appender>
```

```xml
 # 以日期为基础 分隔日志文件
  <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      &lt;!&ndash;<FileNamePattern>${LOG_FILE_PATH}.%d{yyyy-MM-dd HH:mm:ss}.%i.log</FileNamePattern>&ndash;&gt;
      <FileNamePattern>D:\Log\Test_Cluster_Slave_Log-%d{yyyy-MM-dd}-%i.log</FileNamePattern>
      <MaxHistory>1</MaxHistory>
  </rollingPolicy>
```

```xml
 # 按照大小的方式 分隔日志文件
  <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
       <maxFileSize>1MB</maxFileSize>
  </triggeringPolicy>
```
###### 4.2注意事项：
```txt
1 已上分隔日志保存的方式，尽量使用一种使用基于Size和Time就既可以根据时间来分隔又可以根据大小来分隔日志；
2 在windows上不要使用File标签， 否则下面定义的FileNamePattern不会起作用。如果使用的话，在控制台会有日志显示，修改名字失败的记录，因为在windows上先通过File标签创建了文件并定义了文件名，但是通过FIleNamePattern修改名字的时候，会因为文件被占用中而修改名称失败。
3 如果使用基于SizeAndTimeBasedRollingPolicy的方式分隔日志的话，需要使用%i的参数来生成同一个时间段内，由于大小的不同产生的多个日志文件的自增的数，比如：Test_log-1.log;Test_log-2.log; Test_log-3.log;…….;
4：在windows上日志文件的命名不能有多个。号，否则会报日志文件名错误之类的异常；尽量使用下划线分隔或者破折号分隔名字；
```
##### 5.root标签：
<br>&emsp; &emsp;整个xml文件中只能有一个此标签，用于定义appender的日志级别；
<br>&emsp; &emsp;日志的级别从低到高依次是：Trace < debug < info < waen < error 五个级别；所以该标签会有一个 level属性用于定义记录的级别；
```xml
<root level="${ROOT_LOG_LEVEL}">
    <appender-ref ref="${APPENFER_NAME_CONSOLE}"/>
    <appender-ref ref="${APPENDER_NAME_FILE}"/>
</root>
```



       
       
       
       
