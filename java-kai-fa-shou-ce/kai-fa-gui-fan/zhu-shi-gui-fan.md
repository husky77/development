# 注释规范

* 文件注释：从Java文件第一行开始，为避免被JavaDoc收集，以 /\* 开始，以 \*/ 结束（JavaDoc收集的注释内容以 /\*\* 开始），中间每一行前面加一个“\*”。一般是一些版权信息、描述信息及License。

```text
/*    
* Copyright (c) 2020, smuyyh@gmail.com All Rights Reserved.   
 */
```

* 类注释：以 /\*\* 开头， 在Class、Interface、Enum之前。一般是用一句话描述类用途，也可有作者及时间等信息，这类信息不宜过多。
* 属性注释：以 /\*\* 开头。有时也可以直接写在一行

```text
/** 
* 注释信息 
*/
private static final int TEXT_COLOR = Color.RED;
/** 
注释信息 
*/
private static final int BG_COLOR = Color.RED;
```

* 方法注释：@since表明从那个版本开始就有这个方法；@exception或throws可能的异常；@deprecated表示不建议使用该方法。异常注释用@exception或@throws表示，在JavaDoc中二者等价，但推荐用@exception标注Runtime异常，@throws标注非Runtime异常。异常的注释必须说明该异常的含义及什么条件下抛出该异常。

```text
/**
* 方法描述
* @param   [参数1]   [参数1说明]
* @param   [参数2]   [参数2说明]
* @return  [返回类型说明]
* @exception/throws    [违例类型]   [违例说明]
* @see     [类、类#方法、类#成员]
* @deprecated   [说明及原因]
*/
public int method(String arg1, String agr2) throws Exception{}
```

* 对变量的定义和分支语句（条件分支、循环语句等），稍微复杂一点的最好编写注释，因为这些语句往往是程序实现某一特定功能的关键，对于维护人员来说，良好的注释帮助更好的理解程序，有时甚至优于看设计文档。
* 对于switch语句下的case语句，如果因为特殊情况需要处理完一个case后需进入下一个case处理，最好在该case语句处理完、下一个case语句前加上明确的注释，这样比较清楚程序编写者的意图，有效防止无辜遗漏break语句
* 边写代码边注释，修改代码同时修改相应注释，以保证注释与代码的一致性。更新代码须和更新注释保持同步。
* 注释的内容要清晰、明了，含义明确，防止注释二义性，错误的注释不但无益反而有害。
* 尽量避免在注释中使用缩写，特别是不常用缩写。

  


