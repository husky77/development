# Lombok环境配置

### IDEA安装Lombok插件

![](../.gitbook/assets/image%20%284%29.png)

### 开启 AnnocationProcessors

![&#x5F00;&#x542F;&#x8BE5;&#x9879;&#x662F;&#x4E3A;&#x4E86;&#x8BA9;Lombok&#x6CE8;&#x89E3;&#x5728;&#x7F16;&#x8BD1;&#x9636;&#x6BB5;&#x8D77;&#x5230;&#x4F5C;&#x7528;&#x3002;](../.gitbook/assets/image%20%285%29.png)

#### 引入相应的maven包

```text
   <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.16.18</version>
          <scope>provided</scope>
    </dependency>

```

###  Lombok注解的使用

*  `@Getter/@Setter: 作用类上，生成所有成员变量的getter/setter方法；作用于成员变量上，生成该成员变量的getter/setter方法。`
*  `@ToString：作用于类，覆盖默认的toString()方法，可以通过of属性限定显示某些字段，通过exclude属性排除某些字段。`
* `@EqualsAndHashCode：作用于类，覆盖默认的equals和hashCode。`
* `@NonNull：主要作用于成员变量和参数中，标识不能为空，否则抛出空指针异常。`
* `@NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor：作用于类上，用于生成构造函数。有staticName、access等属性。`
* `@NoArgsConstructor：生成无参构造器；`
* `@RequiredArgsConstructor：生成包含final和@NonNull注解的成员变量的构造器；`
* `@AllArgsConstructor：生成全参构造器。`
* `@Data：作用于类上，是以下注解的集合：@ToString @EqualsAndHashCode @Getter @Setter` 
* `@RequiredArgsConstructor`
* `@Builder：作用于类上，将类转变为建造者模式`
* `@Log：作用于类上，生成日志变量。针对不同的日志实现产品，有不同的注解：`





