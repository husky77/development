# 实体类规范

### 主键策略

#### **IdType**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x503C;</th>
      <th style="text-align:left">&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">AUTO</td>
      <td style="text-align:left">&#x6570;&#x636E;&#x5E93;ID&#x81EA;&#x589E;</td>
    </tr>
    <tr>
      <td style="text-align:left">NONE</td>
      <td style="text-align:left">&#x65E0;&#x72B6;&#x6001;,&#x8BE5;&#x7C7B;&#x578B;&#x4E3A;&#x672A;&#x8BBE;&#x7F6E;&#x4E3B;&#x952E;&#x7C7B;&#x578B;(&#x6CE8;&#x89E3;&#x91CC;&#x7B49;&#x4E8E;&#x8DDF;&#x968F;&#x5168;&#x5C40;,&#x5168;&#x5C40;&#x91CC;&#x7EA6;&#x7B49;&#x4E8E;
        INPUT)</td>
    </tr>
    <tr>
      <td style="text-align:left">INPUT</td>
      <td style="text-align:left">insert&#x524D;&#x81EA;&#x884C;set&#x4E3B;&#x952E;&#x503C;</td>
    </tr>
    <tr>
      <td style="text-align:left">ASSIGN_ID</td>
      <td style="text-align:left">
        <p>&#x5206;&#x914D;ID(&#x4E3B;&#x952E;&#x7C7B;&#x578B;&#x4E3A;Number(Long&#x548C;Integer)&#x6216;String),&#x4F7F;&#x7528;&#x63A5;&#x53E3;<code>IdentifierGenerator</code>&#x7684;&#x65B9;&#x6CD5;<code>nextId</code>
        </p>
        <p>(&#x9ED8;&#x8BA4;&#x5B9E;&#x73B0;&#x7C7B;&#x4E3A;<code>DefaultIdentifierGenerator</code>&#x96EA;&#x82B1;&#x7B97;&#x6CD5;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ASSIGN_UUID</td>
      <td style="text-align:left">&#x5206;&#x914D;UUID,&#x4E3B;&#x952E;&#x7C7B;&#x578B;&#x4E3A;String,&#x4F7F;&#x7528;&#x63A5;&#x53E3;<code>IdentifierGenerator</code>&#x7684;&#x65B9;&#x6CD5;<code>nextUUID</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ID_WORKER</td>
      <td style="text-align:left">&#x5206;&#x5E03;&#x5F0F;&#x5168;&#x5C40;&#x552F;&#x4E00;ID &#x957F;&#x6574;&#x578B;&#x7C7B;&#x578B;</td>
    </tr>
    <tr>
      <td style="text-align:left">UUID</td>
      <td style="text-align:left">32&#x4F4D;UUID&#x5B57;&#x7B26;&#x4E32;</td>
    </tr>
    <tr>
      <td style="text-align:left">ID_WORKER_STR</td>
      <td style="text-align:left">&#x5206;&#x5E03;&#x5F0F;&#x5168;&#x5C40;&#x552F;&#x4E00;ID &#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;</td>
    </tr>
  </tbody>
</table>

  
**FieldFill**

| 值 | 描述 |
| :--- | :--- |
| DEFAULT | 默认不处理 |
| INSERT | 插入时填充字段 |
| UPDATE | 更新时填充字段 |
| INSERT\_UPDATE | 插入和更新时填充字段 |

#### TableLogic

* 描述：表字段逻辑处理注解（逻辑删除）

| 属性 | 类型 | 描述 |
| :--- | :--- | :--- |
| value | String | 逻辑未删除值 |
| delval | String | 逻辑删除值 |

#### TableField

| 属性 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| value | String | "" | 数据库字段名 |
| el | String | "" | 映射为原生 `#{ ... }` 逻辑,相当于写在 xml 里的 `#{ ... }` 部分 |
| exist | boolean | true | 是否为数据库表字段 |
| condition | String | "" | 字段 `where` 实体查询比较条件,有值设置则按设置的值为准,没有则为默认全局的 `%s=#{%s}`,[参考](https://github.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-annotation/src/main/java/com/baomidou/mybatisplus/annotation/SqlCondition.java) |
| update | String | "" | 字段 `update set` 部分注入, 例如：update="%s+1"：表示更新时会set version=version+1\(该属性优先级高于 `el` 属性\) |
| insertStrategy | Enum | DEFAULT | 举例：NOT\_NULL: `insert into table_a(<if test="columnProperty != null">column</if>) values (<if test="columnProperty != null">#{columnProperty}</if>)` |
| updateStrategy | Enum | DEFAULT | 举例：IGNORED: `update table_a set column=#{columnProperty}` |
| whereStrategy | Enum | DEFAULT | 举例：NOT\_EMPTY: `where <if test="columnProperty != null and columnProperty!=''">column=#{columnProperty}</if>` |
| fill | Enum | FieldFill.DEFAULT | 字段自动填充策略 |
| select | boolean | true | 是否进行 select 查询 |
| keepGlobalFormat | boolean | false | 是否保持使用全局的 format 进行处理 |
| jdbcType | JdbcType | JdbcType.UNDEFINED | JDBC类型 \(该默认值不代表会按照该值生效\) |
| typeHandler | Class&lt;? extends TypeHandler&gt; | UnknownTypeHandler.class | 类型处理器 \(该默认值不代表会按照该值生效\) |
| numericScale | String | "" | 指定小数点后保留的位数 |



