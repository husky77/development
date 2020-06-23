# 权限控制

### 接口权限

#### 开启/关闭

本地测试时可设置为false,不允许提交。\(注:无登录状态下去用户信息失败错误\)

```text
jwt:
  enabled: true #是否启用jwt
```

#### 指定接口或类不接近权限控制

默认加载所有接口,单独接口单独开放

```text
通过注解： com.gcxygrp.base.common.annotation.Auth
default ""; //访问权限
name() default "";
moduleName() default "默认";  //模块
isAuth() default true; //需要鉴定是否包含权限
```

### 数据权限

#### 权限范围

```text
数据范围（1：全部数据权限 2：自定数据权限 3：本部门数据权限 4：本部门及以下数据权限）
```

#### 开启/关闭

```text
dataScope:
  enabled: true #开启数据权限过滤(true:开启，false：关闭)
```

#### 使用方式

```text
//标注在mapper上
注解：com.gcxygrp.base.domain.datascope.DataScope
```



