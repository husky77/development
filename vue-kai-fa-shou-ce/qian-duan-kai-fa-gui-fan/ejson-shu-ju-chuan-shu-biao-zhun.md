# 数据传输标准

### JSON数据类型

JSON（JavaScript Object Notation）是一种轻量级，基于文本，语言无关的数据交换格式。其包括了基本数据类型4种和复合数据类型2种，共6种数据类型。在下面章节中，JSON数据类型的表示法为JSON+空格+数据类型，如：JSON Array。

传输的数据，包括对象属性以及数组成员， _必须_是6种JSON数据类型之一。 杜绝使用function、Date等js对象类型。

#### 基本数据类型 <a id="h3-u57FAu672Cu6570u636Eu7C7Bu578B"></a>

* Number可以表示整数和浮点数。
* Boolean可以表示真假，值为true或false。
* String表示一个字符串。
* Null通常用于表示空对象。

“true”和true，这两个数据代表的是不同的数据类型。非字符串类型数据输出时一定 _不要_为两端加上双引号，否则可能产生不希望的后果（如if中判断”false”的结果是true）。其他容易产生错误的例子如：0和”0”等。

#### 复合数据类型 <a id="h3-u590Du5408u6570u636Eu7C7Bu578B"></a>

Object是无序的集合，以键值对的方式保持数据。一个Object中包含零到多个name/value的数据，数据间以逗号\(,\)分隔。name为String类型，value可以是任意类型的数据。

Object的最后一个元素之后一定 _不要_加上分隔符的逗号，否则可能导致解析出错。

Array\(数组\)为多个值的有序集合，数组元素间以逗号\(,\)分隔。

### http响应头 <a id="h2-http-"></a>

#### code <a id="h3-status"></a>

http响应的status _必须_ 为200。通常JSON数据被用于通过XMLHttpRequest对象访问，通过javascript进行处理。返回错误的状态码可能导致错误不被响应，数据不被处理。

#### Content-Type <a id="h3-content-type"></a>

Content-Type字段定义了响应体的类型。一般情况下，浏览器会根据该类型对内容进行正确的处理。对于传输JSON数据的响应，Content-Type _推荐_设置为”application/json”或”text/plain”。 _避免_将Context-Type设置为text/html，否则可能导致安全问题。

Content-Type中可以指定字符集。通常 _需要_ 明确指定一个字符集。如果是通过XMLHTTPRequest请求的数据，并且字符编码为UTF-8时，可以不指定字符集。

**Context-Type示例**

```text
application/json;charset=UTF-8
```

### 数据字段 <a id="h2-u6570u636Eu5B57u6BB5"></a>

返回的数据包含在http响应体中。数据 _必须_ 是一个JSON Object

#### code <a id="h3-status"></a>

字段 _必须_是一个不小于0的JSON Number整数

200：表示server端理解了请求，成功处理并返回。

非200：表示发生错误。 _可以_根据错误类型扩展错误码。

**一个成功请求的status字段**

```text
{    
    "code": 200,    
    "data": "hello world!"
}
```

#### msg <a id="h3-statusinfo"></a>

msg字段 _通常_是一个JSON String或JSON Object，表示除了请求状态外server端想要对code做出的说明，使client端能够获取更多信息进行后续处理。

**client端参数错误的**msg

简单说明的msg：

```text
{    
 "code": 200,   
 "msg": "参数错误"
 }
```

具有更多信息的msg：

```text
{    
"code": 1,   
"msg": {   
    "text": "参数错误",     
    "parameters": {           
     "email": "电子邮件格式不正确"  
   }    
  }
}
```

#### data <a id="h3-data"></a>

data字段可以是除JSON Null之外的任意JSON类型，表示请求返回的数据主体。数据主体data包含了在请求成功时有意义的数据。

**一个查询姓名请求的返回数据**

```text
{   
  "code": 200, 
   "data": "Lily"
}
```

**数据场景：日期**

```text
{    
    "code": 0,    
    "data": "2010-10-10 00:00:00"
}
```

#### 记录 <a id="h3-u8BB0u5F55"></a>

记录代表二维表中的一行，通常用于表示某个具体事务抽象的属性。标准记录数据 _必须\(MUST\)_ 为一个JSON Object，记录的主键命名。单条记录数据不包含变通数据格式。

**数据场景：记录**

```text
{    
    "id": 250,    
    "name": "erik",    
    "sex": 1,    
    "age": 18
}
```

#### 数据页 <a id="h3-u6570u636Eu9875"></a>

数据页是列表数据常用的数据方式，可能通过查询或翻页获得数据。数据页是二维表数据的包装，包含列表数据本身更多的信息。

#### 数据页可选属性 <a id="h3-u6570u636Eu9875u53EFu9009u5C5Eu6027"></a>

* {Number} pageNum - 当前页码，计数 为不小于0的整数，从0开始。
* {Number} pageSize - 每页显示条数,大于0。
* {Number} total - 列表总记录数， _必须_为不小于0的整数。表示当前条件下所有记录的数目，非本页的记录数。
* {String} orderBy - 列表排序规则。多个排序规则之间以逗号分割（,）；正序或倒序以asc或desc表示，与字段名之间以一个空格间隔。例：”id desc,name asc”
* {String} keyword - 列表所属的搜索关键字。
* {Object} condition - 列表所属的搜索条件集合。属性中可以包含或不包含keyword字段，如果不包含， _建议_在解析的时候附加搜索关键字keyword条件。

**数据场景：数据页**

```text
{    
 "page": 0,    
 "pageSize": 30,    
 "keyword": "",    
 "condition":        
  {            
    "id": 250,            
    "name": "erik",            
    "sex": 1,            
    "age": 18        
    }   
 }
```

#### 键/值对象 <a id="h3--"></a>

对于在一个JSON Object中表示键/值：

* 键的属性名必须为name， 杜绝使用key或k
* 值的属性名必须为value， 杜绝使用v。

**数据场景：键/值对象**

```text
{    
    "name": "BMW",    
    "value": 1
}
```

#### 键/值有序集合 <a id="h3--"></a>

键/值有序集合表示对事务或逻辑类型的抽象与分类。常见的应用场景有单选复选框集合，下拉菜单等。

标准的键/值有序集合是一个JSON Array，集合中的每一项是一个JSON Object。项 _必须\(MUST\)_ 包含name和value属性。 _可以\(MAY\)_ 通过其他的属性修饰每一项的特殊信息，如selected。

**数据场景：键/值有序集合**

```text
[    
    {        
        "name": "BMW",        
        "value": 1    
        },   
         {        
         "name": "Benz",        
         "value": 2,        
         "selected": true    
     }
 ]
```

#### 树 <a id="h3-u6811"></a>

树形数据用于表示层叠的数据结构。树型数据必须是一个JSON Object，代表树型数据的根节点。下面是标准定义的可选节点列表，不在列表中的属性 _可以_自行扩展。

#### 树型数据结构的可选节点属性 <a id="h3-u6811u578Bu6570u636Eu7ED3u6784u7684u53EFu9009u8282u70B9u5C5Eu6027"></a>

* {Number\|String} id - 节点的唯一标识。
* {String} name/title - 名称或用于显示的字符串。
* {Array} children       - 子节点列表。

**数据场景：树型数据**

```text
{    
    "id": 1,    
    "name": "中国",    
    "children": [        
        {            
            "id": 10,            
            "name": "北京",            
            "children": [                
                {                    
                    "id": 100,                    
                    "name": "东城区"                
                },                
                {                    
                    "id": 101,                    
                    "name": "西城区"                
                },                
                {                    
                    "id": 102,                    
                    "name": "海淀区"                
                }               
                     ......            
               ]    
           },        
           {            
               "id": 31,            
               "name": "海南",            
               "children": [                
                   {                    
                       "id": 600,                    
                       "name": "海口"                
                   },                
                   {                    
                       "id": 601,                    
                       "name": "三亚"                
                   },                
                   {                    
                       "id": 602,                    
                       "name": "五指山"                
                   }                
                       ......            
                   ]        
               }        
               ......    
       ]}
```



