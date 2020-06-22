# 日志规范

### 异常规范 <a id="h2-u5F02u5E38u89C4u8303"></a>

1.返回给前端接口统一抛出`CommonException`异常，这样返回给前端的状态码为`200`，前端通过`code`是否为`200`判断成功与否。

2.内部接口\(用于被其他服务通过feign或ribbon调用的\)，统一抛出`FeignException`异常，抛出异常时状态码为`500`，方便接口调用端“`感知`”异常。

3.手动抛异常时应该把`exception`一块抛出，可以保留异常堆栈。 try { String input = mapper.writeValueAsString\(projectEventMsg\); sagaClient.startSaga\(PROJECT\_UPDATE, new StartInstanceDTO\(input, “project”, “” + projectDO.getId\(\)\)\); } catch \(Exception e\) { throw new CommonException\(“error.projectService.update.event”, e\); }

4.不允许记录日志后又抛出异常，因为这样会多次记录日志，只允许记录一次日志，应尽量抛出异常，顶层打印一次日志。

5.使用SLF4J中的API进行日志打印, 在一个对象中通常只使用一个`Logger`对象，`Logger`应该是`static final`的。 private static final Logger LOGGER= LoggerFactory.getLogger\(Abc.class\)。

6.对`trace/debug/info`级别的日志输出，必须使用占位符的方式。 logger.debug\(“Processing trade with id: “ + id + ” symbol: “ + symbol\);

如果日志级别是 `warn`，上述日志不会打印，但是会执行字符串拼接操作，如果`symbol`是对象，会执行 toString\(\)方法，浪费了系统资源，执行了上述操作，最终日志却没有打印。所以应该使用：logger.debug\(“Processing trade with id:{} and symbol : {} “, id, symbol\);

7.输出的`POJO`类必须重写`toString`方法，否则只输出此对象的`hashCode`值（地址值），没啥参考意义。

8.输出`Exceptions`的全部`Throwable`信息。

* LOGGER.error\(e.getMessage\(\)\); 错误,失掉StackTrace信息
* LOGGER.error\(“Bad things : {}“,e.getMessage\(\)\); 错误,失掉StackTrace信息
* LOGGER.error\(“Bad things : {}“,e\); 正确

9.不允许出现`System print`\(包括System.out.println和System.error.println\)语句。

10.不允许出现`printStackTrace`。堆栈打印应该`LOGGER.error`\(“Bad things : {}“,e\)。

### 日志格式 <a id="h2-u65E5u5FD7u683Cu5F0F"></a>

1.将附件中的`logback-spring.xml`放入`src/main/resources/`中。

2.系统抛出的异常不会附带`traceid`，如果需要输出`traceid`应该使用如下方式输出异常信息`logger.error`\(“internal server error”, e\)。

