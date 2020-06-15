# 单元测试教程

#### Spring Boot的测试类库

我们的java服务都是Spring框架搭建的，并且也会使用到Spring boot来进行快速搭建开发，在Spring Boot提供了许多实用工具和注解来帮助测试应用程序，主要包括以下两个模块：

* spring-boot-test：支持测试的核心内容。
* spring-boot-test-autoconfigure：支持测试的自动化配置。

开发进行只要使用 spring-boot-starter-test 启动器就能引入这些 Spring Boot 测试模块，还能引入一些像 JUnit, AssertJ, Hamcrest 及其他一些有用的类库，具体如下所示：

* JUnit：Java 应用程序单元测试标准类库。
* Spring Test & Spring Boot Test：Spring Boot 应用程序功能集成化测试支持。
* AssertJ：一个轻量级的断言类库。
* Mockito：一个Java Mock测试框架，默认支付 1.x，可以修改为 2.x。
* JsonPath：一个JSON操作类库。

#### 编写测试用例

**1 引入pom依赖**

再IDEA中创建一个普通的maven项目即可，然后导入pom依赖：

```text
<parent>
        <groupId> org.springframework.boot </groupId>
        <artifactId> spring-boot-starter-parent </artifactId>
        <version>2.1.7.RELEASE </version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.20</version>
        </dependency>
    </dependencies>
```

**3.2 MockMVC基于RESTful风格的测试**

> 对于前后端分离的项目而言，无法直接从前端静态代码中测试接口的正确性，因此可以通过MockMVC来模拟HTTP请求。基于RESTful风格的SpringMVC的测试，我们可以测试完整的Spring MVC流程，即从URL请求到控制器处理，再到视图渲染都可以测试。

首先创建一个超简单的controller

```text
@RestController
@RequestMapping(value = "/web")
public class WebController {

    @PostMapping(value = "/create")
    public WebResponse<String> ping(@RequestBody WebRequest webRequest){
        System.out.println(webRequest);
        WebResponse<String> response = new WebResponse<>();
        response.setBody("create 完成---");
        response.setCode("00000");
        response.setMessage("成功");
        return response;
    }
}
```

request和response

```text
@Data
@ToString
@EqualsAndHashCode
public class WebRequest {
    private String name;
    
    private String mobile;
}
```

```text
@Data
@ToString
@EqualsAndHashCode
public class WebResponse<T> {
    private String code;

    private String message;

    private T body;
}
```

然后创建一个测试用例类

```text
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class WebControllerIT {

    @Autowired
    private WebApplicationContext mac;

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void ping() throws Exception {
        //请求的json
        String json = "{\"name\":\"王五\",\"mobile\":\"12345678901\"}";

        //perform,执行一个RequestBuilders请求，会自动执行SpringMVC的流程并映射到相应的控制器执行处理
        mockMvc.perform(MockMvcRequestBuilders
                //构造一个post请求
                .post("/web/create")
                //json类型
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                //使用writeValueAsString()方法来获取对象的JSON字符串表示
                .content(json))
                //andExpect，添加ResultMathcers验证规则，验证控制器执行完成后结果是否正确，【这是一个断言】
                .andExpect(MockMvcResultMatchers.status().is(200))
                .andExpect(MockMvcResultMatchers.status().isOk())
                //使用jsonPaht验证返回的json中code字段的返回值
                .andExpect(MockMvcResultMatchers.jsonPath("$.code").value("00000"))
                .andExpect(MockMvcResultMatchers.jsonPath("$.message").value("成功"))
                //body属性不为空
                .andExpect(MockMvcResultMatchers.jsonPath("$.body").isNotEmpty())
                //添加ResultHandler结果处理器，比如调试时 打印结果(print方法)到控制台
                .andDo(MockMvcResultHandlers.print())
                //返回相应的MvcResult
                .andReturn();
    }

}
```

其中`MockMvcRequestBuilders` 写好后直接运行就可以了，从控制台就可以看到详细信息。  


![](../../.gitbook/assets/image%20%2817%29.png)

