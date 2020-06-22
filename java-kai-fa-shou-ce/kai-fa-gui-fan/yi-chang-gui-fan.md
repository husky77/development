# 异常规范

### 后端异常规范 <a id="h2-u540Eu7AEFu5F02u5E38u89C4u8303"></a>

### 用户自定义异常 <a id="h2-u7528u6237u81EAu5B9Au4E49u5F02u5E38"></a>

1.当应用程序出现问题时，直接抛出自定义异常。

```text
 throw new DaoObjectNotFoundException("Couldn't find dao with id " + id);
```

2.将自定义异常中的原始异常包装并抛出。

```text
catch (NoSuchMethodException e) { 
    throw new DaoObjectNotFoundException("Couldn't find dao with id " + id, e);
 }
```

错误的做法：

1.不要吞下`catch`的异常。

```text
try {   
     System.out.println("Never do that!");
 } catch (AnyException exception) 
 {  
     // Do nothing
 }
```

* 这样的捕获毫无意义。我们应该使用一定的日志输出来定位到问题。 2.方法上应该抛出具体的异常。而不是`Exception`。

```text
public void foo() throws Exception {
 //错误方式
 }
 public void foo() throws SQLException { 
 //正确方式
 }
```

3.要捕获异常的子类，而不是直接捕获`Exception`。

```text
catch (Exception e) { //错误方式}
```

4.永远不要捕获`Throwable`类。

5.不要只是抛出一个新的异常，而应该包含堆栈信息。错误的做法：

```text
try {    
} catch (BankAccountNotFoundException exception)
{
     throw new BusinessException();    
     // or   
      throw new BusinessException("Some information: " + e.getMessage());
 }
```

```text
try {  
  } catch (BankAccountNotFoundException exception) 
  {   
   throw new BusinessException(exception);   
 // or   
  throw new BusinessException("Some information: " ,exception);
 }
```

6.要么记录异常要么抛出异常，但不要一起执行。

```text
catch (NoSuchMethodException e) {
  //错误方式  
    LOGGER.error("Some information", e);   
    throw e;
}
```

7.不要在`finally`中再抛出异常。

```text
try { 
   someMethod();
   } finally {
     cleanUp();    //如果finally还抛出异常，那么exceptionOne将永远丢失
}
```

* 如果`someMethod` 和 `cleanUp` 都抛出异常，那么程序只会把第二个异常抛出来，原来的第一个异常（正确的原因）将永远丢失。 8.始终只捕获实际可处理的异常。

```text
catch (NoSuchMethodException e) { 
  throw e; //避免这种情况，因为它没有任何帮助
  }
```

* 不要为了捕捉异常而捕捉，只有在想要处理异常时才捕捉异常。 9.不要使用`printStackTrace()`语句或类似的方法。

10.如果你不打算处理异常，请使用`finally`块而不是`catch`块。

11.应该尽快抛出\(throw\)异常，并尽可能晚地捕获\(catch\)它。你应该做两件事：分装你的异常在最外层进行捕获，并且处理异常。

12.在捕获异常之后，需要通过finally 进行收尾。在使用io或者数据库连接等，最终需要去关闭并释放它。

13.不要使用`if-else` 来控制异常的捕获。

14.一个异常只能包含在一个日志中。

```text
// 错误
LOGGER.debug("Using cache sector A");
LOGGER.debug("Using retry sector B");
// 正确
LOGGER.debug("Using cache sector A, using retry sector B");
```

15.将所有相关信息尽可能地传递给异常。有用且信息丰富的异常消息和堆栈跟踪也非常重要。

16.在`JavaDoc`中记录应用程序中的所有异常。应该用`javadoc`来记录为什么定义这样一个异常。

17.异常应该有具体的层次结构。如果异常没有层次的话，则很难管理系统中异常的依赖关系。

类似这样

```text
class Exception {}class BusinessException extends Exception {}
class AccountingException extends BusinessException {}
class BillingCodeNotFoundException extends AccountingException {}
class HumanResourcesException extends BusinessException {}
class EmployeeNotFoundException extends HumanResourcesException {}
```

