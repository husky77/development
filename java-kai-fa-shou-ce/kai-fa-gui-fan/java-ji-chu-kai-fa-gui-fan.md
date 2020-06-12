# Java基础开发规范

### 细节补充

1. 使用为底层以数组方式实现的集合、工具类时，如果能估计待添加的内容长度，请指定初始长度，避免引起多次扩容操作，这样可以明显地提升性能。比如`ArrayList、StringBuilder、StringBuffer、HashMap、HashSet`等等
   * 但是，注意像`HashMap`这种是以数组+链表/树实现的集合，不要把初始大小和你估计的大小设置得一样，因为一个table上只连接一个对象的可能性几乎为0。初始大小建议设置为2的N次幂，如估计有2000个元素，可设置成`new HashMap(128)、new HashMap(256)`
2. 当复制大量数据时，使用`System.arraycopy`命令
3. 乘法和除法使用移位操作，用移位操作可以极大地提高性能，因为在计算机底层，对位的操作是最方便、最快的
   * 移位操作虽然快，但是可能会使代码不太好理解，因此最好加上相应的注释。例如：

```text
a = val * 8;
b = val / 2;
// 替换为 注释略
a = val << 3;
b = val >> 1;
```

1. 尽量使用`HashMap、ArrayList、StringBuilder`，除非线程安全需要，否则不推荐使用`Hashtable、Vector、StringBuffer`
2. 程序运行过程中避免使用反射
   * 不建议在程序运行过程中使用尤其是频繁使用反射机制，特别是Method的`invoke`方法，如果确实有必要，一种建议的做法是将那些需要通过反射加载的类在项目启动的时候通过反射实例化出一个对象并放入内存—用户只关心和对端交互的时候获取最快的响应速度，并不关心对端的项目启动花多久时间。
3. 使用带缓冲的输入输出流进行IO操作
   * 带缓冲的输入输出流，即`BufferedReader、BufferedWriter、BufferedInputStream、BufferedOutputStream`，这可以极大地提升IO效率。
4. 公用集合类中不使用的数据一定要及时清除掉
   * 如果一个集合类是公用的（非方法里面的属性），那么这个集合里面的元素是不会自动释放的，因为始终有引用指向它们。所以如果公用集合里面的某些数据不使用而不去remove掉它们，那么将会造成这个公用集合不断增大，使得系统有内存泄露的隐患。
5. 把一个基本数据类型转为字符串，基本数据类型`.toString()`是最快的方式、`String.valueOf()`次之、`数据+""`最慢
6. 使用最有效率的方式去遍历Map
   * 遍历Map的方式有很多，通常场景下我们需要的是遍历Map中的Key和Value，那么推荐使用的、效率最高的方式是使用`Iterator`

```text
Iterator it = map.entrySet().iterator();
while (it.hasNext()){
    Map.Entry entry = (Map.Entry) it.next();
    // 获得key与value
    Object k = entry.getKey();
    Object v = entry.getValue();
}
```

