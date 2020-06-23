# 模块引用

#### pom.xml

```text
<dependency>
   <groupId>com.gc.plat</groupId>
   <artifactId>base-app</artifactId>
   <version>${base.version}</version>
 </dependency>
```

#### 启动类中增加

```text
@MapperScan({"com.gcxygrp.base.modules.*.mapper","com.gcxygrp.base.domain.modules.*.mapper"})
@ComponentScan({"com.gcxygrp.base.*"})


  /**
     * 使用@Bean注入fastJsonHttpMessageConvert
     *
     * @return
     */
    @Bean
    public HttpMessageConverters fastJsonHttpMessageConverters() {
        //1.需要定义一个Convert转换消息的对象
        FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();
        //2.添加fastjson的配置信息，比如是否要格式化返回的json数据
        FastJsonConfig fastJsonConfig = new FastJsonConfig();
        fastJsonConfig.setSerializerFeatures(
                SerializerFeature.PrettyFormat,
                //字符类型字段如果为null,输出为"",而非null
                SerializerFeature.WriteNullStringAsEmpty,
                //List字段如果为null,输出为[],而非null
                SerializerFeature.WriteNullListAsEmpty,
                //避免循环引用
                SerializerFeature.DisableCircularReferenceDetect
        );
        fastJsonConfig.setDateFormat("yyyy-MM-dd HH:mm:ss");
        //3.在convert中添加配置信息
        fastConverter.setFastJsonConfig(fastJsonConfig);

        //处理中文乱码问题(不然出现中文乱码)
        List<MediaType> fastMediaTypes = new ArrayList<MediaType>();
        fastMediaTypes.add(MediaType.APPLICATION_JSON_UTF8);
        fastConverter.setSupportedMediaTypes(fastMediaTypes);

        HttpMessageConverter<?> converter = fastConverter;
        return new HttpMessageConverters(converter);
    }


    /**
     * 创建一个bean
     *
     * @return
     */
    @Bean
    public Filter requestDecryptFilter() {
        return new RequestDecryptFilter();
    }


    public static void main(String[] args) {
        SpringApplication.run(FundsBackApplication.class, args);
    }

```

#### 版本更新,增加以下配置后通过更换版本号或使用—U进行强制更新

#### pom.xml

```text
  <distributionManagement>
        <repository>
            <id>nexus-releases</id>
            <name>Nexus Releases Repository</name>
            <url>http://127.0.0.1:8081/nexus/content/repositories/releases/</url>
        </repository>

        <snapshotRepository>
            <id>nexus-snapshots</id>
            <name>Nexus Snapshots Repository</name>
            <url>http://127.0.0.1:8081/nexus/content/repositories/snapshots/</url>
        </snapshotRepository>
    </distributionManagement>
```

#### maven settings.xml

```text

<!--此处设置的用户名和密码都是nexus的登陆配置-->
 <servers>
     <server>
      <id>nexus-releases</id>  
      <username>username</username>
      <password>password</password>
    </server>
     <server>
      <id>nexus-snapshots</id> <!--对应pom.xml中id=snapshots的仓库-->
      <username>username</username>
      <password>password</password>
  </server>

```

