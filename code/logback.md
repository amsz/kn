# Logback

### 实践

- 时间精确到毫秒
- 生成环境不输出行号，行号需要通过 `stacktrace` 代价昂贵
- 记录用户的标识符(user_id/session_id)
- log.error表示系统级错误（不可预测）; log.warn表示应用级错误（可预测)

### 使用

```java
final Logger logger = LoggerFactory.getLogger(XX.class);

if(logger.isDebugEnabled()) { ... }   // 通常不推荐
if(logger.isDebugEnabled()) {         // 此种情况推荐
    logger.debug("receive request: {}", toJson(request)); 
}

logger.debug("receive request: " + url);   //错误
logger.debug("receive request: {}", url);  //正确

logger.error("xxxx", e.getMessage()); //错误
logger.error("xxxx", e;               //正确
```

### 配置

#### 控制台输出

```xml
<appender name="LOGFILE" class="ch.qos.logback.core.ConsoleAppender">
  <encoder>
    <pattern>%d{HH:mm:ss.SSS} [%thread] %highlight(%-5level) %cyan(%logger{36}):%line - %msg%n</pattern>
  </encoder>
</appender>
```

#### 文件输出

```xml
<appender name="LOGFILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
  <File>logs/server.log</File>
  <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
    <FileNamePattern>logs/server_%d{yyyy-MM-dd}.%i.log</FileNamePattern>
    <maxFileSize>100MB</maxFileSize>
    <!--<maxHistory>30</maxHistory>-->
    <!--<totalSizeCap>10GB</totalSizeCap>-->
  </rollingPolicy>
  <layout class="ch.qos.logback.classic.PatternLayout">
    <pattern>%date [%thread] %-5level %logger{36} - %msg%n</pattern>
  </layout>
</appender>
```

#### additivity

`MONITER_FILE` 的 additivity 设为 false 后，日志就不会跑到 `FILE` 中了。

```xml
<logger name="MONITER_FILE" additivity="false">
  <level value="INFO" />
  <appender-ref ref="MONITER_FILE" />
 </logger>
 
<root level="error">
  <appender-ref ref="FILE" />
</root>
```



#### Spring 中的使用

配置 `logging.config=classpath:logback-spring.xml`

使用 profile 配置的不同环境

```xml
<springProfile name="qa, prod">
  <appender> ... </appender>
</springProfile>
```



### MDC

`entities` 在多个日志/事件中的关联性，特别是在多线程的场景中使用，例如一个用户在登录过程中的行为。

```java
org.slf4j.MDC.put("username", "admin");
org.slf4j.MDC.put("sessionID", "1234");
```

```xml
<pattern>%X{username} %X{sessionID} %-5p - %m%n</pattern>
```

### Marker

主要用于过滤一些特殊类型的日志，也可用于某些 `Appender` 行为触发。

```java
Marker marker = MarkerFactory.getMarker("time");
log.info(marker, "Request processing took {} ms", (endTime - startTime)/1e6);
```

#### Filtering

```xml
<appender>
  <file name="file" fileName="/tmp/logger.out" append="true">
    <patternlayout pattern="%d{HH:mm:ss} [%t] %-5p %c{1}:%L - %m%n"/>
    <filters>
      <markerfilter marker="time" onMatch="ACCEPT" onMismatch="DENY"/>
    </filters>
  </file>
</appender>
```

#### Triggering

```xml
<appender name="EMAIL" class="ch.qos.logback.classic.net.SMTPAppender">
  <evaluator class="ch.qos.logback.classic.boolex.OnMarkerEvaluator">
    <!-- 这边可以添加多个 -->
    <marker>NOTIFY_ADMIN</marker>
    <marker>TRANSACTION_FAILURE</marker>
  </evaluator>
  <smtpHost>${smtpHost}</smtpHost>
  <to>${to}</to>
  <from>${from}</from>
  <layout class="ch.qos.logback.classic.html.HTMLLayout"/>
</appender>
```



## 参考

http://stackoverflow.com/questions/4165558/best-practices-for-using-markers-in-slf4j-logback