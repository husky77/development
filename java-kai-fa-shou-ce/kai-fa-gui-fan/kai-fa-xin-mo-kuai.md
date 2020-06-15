# 开发新模块

### 编写`entity`

* 每一个`entity`类即为一个实体类，对应数据库中的一个具体表。
* 名称=`表具体名称`，表名中`_`替换为驼峰命名法，首字母大写。如：`Student`对应表为`t_student`。
* 创建在 项目模块 的`xxx.entity.[模块名称]`包下。如：`xxx.entity.Student`

#### 属性规范

* 所有属性均为`private`属性。
* 每一个属性需要生成对应的`getter`和`setter`方法。
* 字段名称应根据驼峰命名规则从数据库列名转换过来。例如：数据库列名为`USER_NAME`，则字段名为`UserName`，特殊字段名称，可以在字段在添加`@Column(name = "xxx")`注解，指定数据库列名。
* 每个属性需加上Swagger注解，相当于注释，且前端代码生成器会用到该注释。
* 建议可添加`Long`类型属性`objectVersionNumber`，用以更新数据时的版本控制。

#### 属性的的类型与字段的`type`对应

* 不使用基本类型，全部使用基本类型的包装类，如`Long`对应数据库中的`INTEGER`，而不是使用`long`。
* 数字类型主键统一采用`Long`。
* 金额、数量 等精度严格浮点类型采用`BigDecimal`

> 注意：BigDecimal 在计算、比较方面的特殊性

#### 所有的主键字段都需要用`@Id`标注

\*　对于自增张、序列（SEQUENCE）类型的主键，需要添加注解`@GeneratedValue`。  
\*　序列命名规范：`表名_S`。例如：表`SYS_USER`对应的序列为`SYS_USER_S`。

#### 非数据库字段

* 需要用`@Transient`标注`javax.persistence.Transient`

#### `entity`类相关注解

* `@Data`，Lombok自动生成get和set方法
* `@Entity`，JPA声明实体类，并且使用默认的ORM规则，即类名即数据库表中表名
* `@Table`，JPA改变类名与数据库中表名的默认ORM映射规则
* `@TableName`，Mybatis-Plus注解
* `@ApiModel`，Swagger实体类注解
* `@ApiModelProperty`，Swagger实体类字段注解

#### `Student.java`代码

```text
// 省略 import

/**
 * @author Exrick
 */
@Data
@Entity
@Table(name = "t_student")
@TableName("t_student")
@ApiModel(value = "测试")
public class Student extends BaseEntity {

    private static final long serialVersionUID = 1L;

    @ApiModelProperty(value = "用户名")
    private String username;

    @ApiModelProperty(value = "密码")
    private String password;

    @ApiModelProperty(value = "版本")
    private Long objectVersionNumber;
}
```

### 编写Dao

#### `Dao`接口类

* `Dao`接口类定义了数据操作的一系列接口，并不提供实现，具体实现需要通过`Dao`实现层提供。创建在项目模块的`xxx.dao`包下。
* 每一个`Dao`对应一个`entity`，所以命名为`entity`类名尾缀替换为`Dao`。如：`StudentDao`对应`Student`。

#### `StudentDao.java`代码

```text
// 省略 import

/**
 * 测试数据处理层
 * @author 
 */
public interface StudentDao extends BaseMapper<Student,String> {

}
```

### 编写应用层`Service`

`service`调用领域对象或服务来解决问题，应用层Service主要有以下特性：

1. 负责事务处理，所以事务的注解可以在这一层的`service`中使用。
2. 只处理非业务逻辑，重点是调度业务处理流程。业务逻辑处理一定要放在领域层处理。
3. 不做单元测试，只做验收测试。
4. 可能会有比较多的依赖组件\(领域实体数据层\)。

### `Service`接口类

* `Service`接口类定义了业务操作的一系列接口，并不提供实现，具体实现需要通过服务实现层提供，所以属于供应方的服务接口层。创建在项目模块的`xxx.service`包下。

#### `StudentService.java`代码

```text
// 省略 import

/**
 * 测试接口
 * @author 
 */
public interface StudentService extends IService<Student,String> {

    /**
    * 多条件分页获取
    * @param student
    * @param searchVo
    * @param pageable
    * @return
    */
    Page<Student> findByCondition(Student student, SearchVo searchVo, Pageable pageable);
}
```

### `Service`实现类

* `Service`接口的具体实现通过服务实现层提供，所以属于供应方的服务实现层。创建在项目模块的`xxx.serviceimpl`包下。
* 实现类，需要用`@Service`标注

#### `StudentServiceImpl.java`代码

```text
// 省略 import

@Slf4j
@Service
@Transactional
public class StudentServiceImpl implements ServiceImpl{

    @Autowired
    private StudentDao studentDao;

    @Override
    public StudentDao getRepository() {
        return studentDao;
    }
}
```

### 编写`Controller`

* `Controller`负责对`Model`和`View`的处理，创建在项目模块的`xxx.api.controller.v1`包下。如`xxx.api.controller.v1`。
* 每一个`Controller`是对一个具体的`DTO`或`VO`资源进行处理的，所以命名为`dto`或`vo`类名尾缀替换为`Controler`。如：`StudentController`对应`StudentDTO`或`StudentVO`类。
* 需要通过`@RestController`指定该类为一个`Controller`类。

#### `Controller`类相关注解

* `@PreAuthorize`，设置API访问权限，Spring Security官方推荐该权限管理

  * @PreAuthorize\("authenticated"\)
  * @PreAuthorize\("hasAuthority\('SCOPE\_add'\)"\)
  * @PreAuthorize\("hasRole\('ADMIN'\)"\)
  * @PreAuthorize\("hasRole\('ADMIN'\) AND hasRole\('USER'\)"\)
  * @PreAuthorize\("hasRole\('ADMIN'\) OR hasRole\('USER'\)"\)

  ```text
     @PreAuthorize("authentication.name == #username")
     public User find(String username) {
        return null;
     }
  ```

* `@ApiOperation`，显示在Swagger上的接口注释
* `@GetMapping`，是一个组合注解，是`@RequestMapping(mathod = RequestMethod.GET)`的缩写，`@PostMapping`等同理。建议使用**组合注解**。

#### `StudentController.java`代码

```text
// 省略 import

/**
 * @author Exrick
 */
@Slf4j
@RestController
@Api(description = "测试管理接口")
@RequestMapping("/student")
@Transactional
public class StudentController extends SuperController<Student, String> {

    @Autowired
    private StudentService studentService;

    @Override
    public StudentService getService() {
        return studentService;
    }
}
```

