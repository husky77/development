# 数据库设计规范

## 建表规约

#### 默认字段-所有表建表时默认添加以下字段

| 字段 | 类型 | 注释 |
| :--- | :--- | :--- |
| is\_valid | tinyint | 是否删除 |
| createBy | varchar | 创建者 |
| create\_dt | datetime | 创建时间 |
| modify\_by | varchar | 修改者 |
| modify\_dt | datetime | 修改时间 |
| last\_ver | tinyint | 版本号 |
| memo\_field\_1 | varchar | 备注字段—1 |
| memo\_field\_2 | varchar | 备注字段—2 |
| memo\_field\_3 | varchar | 备注字段—3 |



