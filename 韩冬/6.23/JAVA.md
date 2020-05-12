# 6.23学习总结

## 学习心得

  





## 学习笔记

### 异常

异常指不期而至的各种状况，它在程序运行的过程中发生。作为开发者，我们都希望自己写的代码永远都不会出现bug，然而现实告诉我们并没有这样的情景。如果用户在程序的使用过程中因为一些原因造成他的数据丢失，这个用户就可能不会再使用该程序了。所以，对于程序的错误以及外部环境能够对用户造成的影响，我们应当及时报告并且以适当的方式来处理这个错误。之所以要处理异常，也是为了增强程序的[鲁棒性](http://baike.baidu.com/view/45520.htm)

异常通常有四类：

- Error：系统内部错误，这类错误由系统进行处理，程序本身无需捕获处理
- Exception：可以处理的异常
- RuntimeException：可以捕获，也可以不捕获的异常
- 继承 Exception 的其他类：必须捕获，通常在 API 文档中会说明这些方法抛出哪些异常

RuntimeExcption 异常(运行时异常)通常有以下几种：

- 错误的类型转换
- 数组访问越界
- 访问 null 指针
- 算术异常

#### throw 抛出异常

当程序运行时数据出现错误或者我们不希望发生的情况出现的话，可以通过抛出异常来处理。

异常抛出语法：

```
throw new 异常名();
```

![1561190267791](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561190267791.png)

#### hrows 声明异常

throws 用于声明异常，表示该方法可能会抛出的异常。如果声明的异常中包括 checked 异常（受查异常），那么调用者必须处理该异常或者使用 throws 继续向上抛出。throws 位于方法体前，多个异常使用`,`分割。

![1561190352340](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561190352340.png)

**捕获异常**

通常抛出异常后，还需要将异常捕获。使用`try`和`catch`语句块来捕获异常，有时候还会用到`finally`。

对于上述三个关键词所构成的语句块，`try`语句块是必不可少的，`catch`和`finally`语句块可以根据情况选择其一或者全选。你可以把可能发生错误或出现问题的语句放到`try`语句块中，将异常发生后要执行的语句放到`catch`语句块中，而`finally`语句块里面放置的语句，不管异常是否发生，它们都会被执行。

你可能想说，那我把所有有关的代码都放到`try`语句块中不就妥当了吗？可是你需要知道，捕获异常对于系统而言，其开销非常大，所以应尽量减少该语句块中放置的语句

![1561190582284](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561190582284.png)

![1561190665008](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561190665008.png)

自定义一个异常类非常简单，只需要让它继承 Exception 或其子类就行。在自定义异常类的时候，建议同时提供无参构造方法和带字符串参数的构造方法，后者可以为你在调试时提供更加详细的信息。

![1561190859375](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561190859375.png)

当异常抛出后，我们可以通过异常堆栈追踪程序的运行轨迹，以便我们更好的DEBUG

![1561191113007](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561191113007.png)

