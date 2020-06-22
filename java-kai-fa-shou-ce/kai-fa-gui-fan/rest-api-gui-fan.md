# Rest API规范

### HTTP Verbs

* GET（SELECT）：从服务器取出资源（一项或多项）
* POST（CREATE）：在服务器新建一个资源
* PUT（UPDATE）：在服务器更新完整的资源（客户端提供改变后的完整资源）
* DELETE（DELETE）：从服务器删除资源

#### 基本规范

使用”/“表示层级关系url 不能以”/“结尾url 中不能包含空格””url 中不能以文件后缀结尾url 中字母小写，单词间加下划线不要再url中添加CRUD

#### 类别

| Description | Action Name | HTTP Mapping | HTTP Request Body | HTTP Response Body |
| :--- | :--- | :--- | :--- | :--- |
| 查询所有 | / | GET | N/A | Resource\*list |
| 获取单个资源 | / | GET | N/A | Resource\* |
| 创建单个资源 | / | POST | Resource | Resource\* |
| 更新单个资源 | / | PUT | Resource | Resource\* |
| 删除单个资源 | / | DELETE | N/A | Empty |

#### 获取单个资源

方法接受一个 Collection id ,一个资源,和0或多个参数。它创建一个新的资源在指定的父资源下,并返回新创建的资源。

必须使用 POST 方法。应该包含父资源名用于指定资源创建的位置。创建的资源应该对应在request body。响应体应该返回整个资源对象

#### 更新单个资源

方法接受一个资源和0或多个参数。更新指定的资源和其属性,并返回更新的资源。

#### 删除单个资源

方法接受一个Resource Name 和0或多个参数,并删除指定的资源。

### 批量添加

| Description | Action Name | HTTP Mapping | HTTP Request Body | HTTP Response Body |
| :--- | :--- | :--- | :--- | :--- |
| 批量添加 | batch | POST/batch | Resource\*list | Resource IDS |

### 批量删除

| Description | Action Name | HTTP Mapping | HTTP Request Body | HTTP Response Body |
| :--- | :--- | :--- | :--- | :--- |
| 批量删除 | batch | POST /batch | Resource IDS | Empty |

### 对资源执行某一动作

比如发送消息，启用什么功能。如果是针对资源，则Action Name为动词。如果是针对资源的属性，则Action Name为动词+属性名。请求以动词结尾，属性作为参数。

| Description | Action Name | HTTP Mapping | HTTP Request Body | HTTP Response Body |
| :--- | :--- | :--- | :--- | :--- |
| 对资源执行某一动作 | customVerb | POST /custom\_verb | N/A | \* |
| 取消某种操作 | cancel | POST /cancel | N/A | Boolean |
| 从回收站中恢复一个资源 | undelete | POST /v1/projects/1/undelete | N/A | Boolean |
| 检查项目是否重名 | checkName | POST /v1/projects/1/check?name= | N/A |  |

### 查询某一资源的单个属性

对于单个资源的所有的查询Action Name，都需要以query开头。Action Name以query+属性名结尾。

| Description | Action Name | HTTP Mapping | HTTP Request Body | HTTP Response Body |
| :--- | :--- | :--- | :--- | :--- |
| 查询资源的某属性 | queryAttribute | GET /:attribute | N/A | {“key”:“”,“value”:“”} |
| 查询用户的年龄 | queryAge | GET /v1/users/1/age | N/A | {“key”:“age”,“value”:“25”} |
| 查询用户下的项目 | queryProjects | GET /v1/users/1/projects | N/A | {“key”:“projects”,“value”:\[\]} |

### 查询collection 的数量

计算集合自身的数量，使用count作为Action Name计算资源中子集合的数量，使用count+集合名作为Action Name。

| Description | Action Name | HTTP Mapping | HTTP Request Body | HTTP Response Body |
| :--- | :--- | :--- | :--- | :--- |
| 查询Collection 的数量 | count | GET /count | N/A | {“key”:“”,“count”:“”} |
| 查询组织的数目 | count | GET /v1/organizations/count | N/A | {“key”:“organizations”,“count”:“100”} |
| 查询用户下的所有项目数量 | countProjects | GET /v1/users/1/projects/count | N/A | {“key”:“projects”,“count”:“100”} |

### 复杂条件查询

对于collection的所有查询Action Name，都需要以list开头。查询的条件中，如果条件为一到两个，使用By和And。

eg.: listByUserIdAndName如果查询条件大于3个，则使用ByOptions，查询条件作为请求体传入。

eg.: listByOptions

### 版本控制

主版本号必须作为包名的最后一个字符。如：cn.exrick.controller.v1。

版本兼容的修改：

添加一个服务接口添加一个api方法添加一个请求字段添加一个相应字段添加一个字段的枚举值

版本不兼容的修改：

删除或重命名一个服务、接口、方法，枚举值改变一种HTTP method改变字段的类型改变一个resource name

### Demo示例

```text
@RestController("/v1/users")
public class UserController {

    @GetMapping("/")
    public ResponseEntity<User> list() {
        return null;
    }
    
    @GetMapping("/{id}")
    public User query(@PathVariable("id") String id) {
        return null;
    }
    
    @PostMapping("/")
    public Result<User> create(@RequestBody User user) {
        return null;
    }
    
    @PutMapping("/")
    public Result<User> update(@RequestBody User user) {
        return null;
    }
    
    @DeleteMapping("/{id}")
    public Result<User> delete(String id) {
        return null;
    }
    
    @PostMapping("/batch_create")
    public Result<User> batchCreate(@RequestBody List<User> users) {
        return null;
    }
    
    @PostMapping("/batch_delete")
    public Result<User> batchDelete(@RequestBody List<User> users) {
        return null;
    }
    
    @PostMapping("/age")
    public Result<User> updateAge(@RequestParam("value") Integer age) {
        return null;
    }
    
    @PostMapping("/{:id}/undelete")
    public Result<User> undelete(@PathVariable("id") String id) {
        return null;
    }
    
    @PostMapping("/check")
    public Result<User> checkName(@RequestParam("name") String name) {
        return null;
    }
    
    @GetMapping("/{:id}/age")
    public Result<User> queryAge(@PathVariable("id") String id) {
        return null;
    }
    
    @GetMapping("/{:id}/name")
    public Result<User> queryByUserIdAndName(@PathVariable("id") String id, @RequestParam("name") String name) {
        return null;
    }
    
    @GetMapping("/{:id}/projects/count")
    public Result<User> countProjects(@PathVariable("id") String id, @RequestParam("name") String name) {
        return null;
    }
    
    @GetMapping("/")
    public Result<User> listByOptions(@RequestBody Map<String, Object> options) {
        return null;
    }
}
```

