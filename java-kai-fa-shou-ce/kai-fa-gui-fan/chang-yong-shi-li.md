# 常用实例

异常

### 日志

项目中可以使用 `@Log`   注解来标记该方法需要写入日志,参数如下,标记下Controller 中

```text
/**模块*/
String moduleName() default "";

/**
 * 操作
 * @return
 */
String operation() default "";

/**
 * 操作描述
 * @return
 */
String remark() default "";

/**
 * 日志类型（0：操作日志，1：登录日志）
 */
int logType()  default 0 ;

/**
 * 是否保存请求的参数
 */
boolean isSaveReqData() default true;

/**
 * 是否保存响应数据
 */
boolean isSaveResData() default true;
```

### 缓存

#### 缓存注解

```text
@Cache(key = "'" + BizCacheConstants.SYS.CONFIG_ID + "' + #id")
```

### 清除缓存

```text
@CacheClear(key = "'" + BizCacheConstants.SYS.CONFIG_ID + "' + #sysConfig.id") //更新完成后清除缓存
```

