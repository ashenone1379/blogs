---
title: "OOP: 面向对象编程"
draft: false
tags:
---
# 概述
...

# 封装/Encapsulation
**封装**是一种接口技术. 把一个数据结构的内部实现隐藏起来, 在外部使用封装好的类通过公开的方法,如`getter`和`setter`实现.
- 对于类的设计者, 当你把类中的一个字段公开时, 你就要对这一部分负责. 在后续的代码维护中, 应当始终保持这个字段是可用的.
- 对于类的用户, i.e. 另一个程序员(甚至很可能就是**你自己**!), 不需要考虑类的实现细节, 只需要通过公开的方法来操作类即可.

>[!example] 破坏封装的后果
>假设小明设计了一个`Person`类, 其中的`birthday`为`int`型变量, 以`yyyymmdd`格式保存了生日的公开的字段
>```java
>public class Person {
>	public final int birthday;
>	//other attributes	
>}
>```
>为了得到生日的年份, 要在代码的多处使用`birthday/1000`的方式来获取年份信息.
>
>这显然是一种很笨拙的方法. 小明也意识到了这点, 于是他修改了`Person`类中的定义, 并使用`getter`获得生日年份:
>```java
>private final Date birthday;
>public int getYearOfBirth() {
>	//...
>}
>```
>不出意外的, 所有使用到裸露的`birthday`的代码都报错了. 于是还需要到每一个原先使用裸露的`birthday`属性的地方修改为使用新的`getter`方法. 忙的焦头烂额.
>
>可见, 破坏**封装**会使外部代码直接依赖于公开字段的具体实现, 一旦这个字段修改, 其它地方也要修改.
>
>如果一开始就**封装**好了类, 将实例字段隐藏起来, 使用`getter`获得年份, 那样就算修改了`birthday`的数据类型, 也不会影响到类之外其它地方的代码.

>[!success] **封装**的益处
>实现好类的**封装**, 既便于代码维护, 又便于使用.

# 继承/Inheritance
如果一个类**继承**了另一个类, 那么他们之间即为*is-a*关系.
>[!example]
>假设小明是一个大三学生, 那么他的身份可以是:
>- 本科生
>- 学生
>- 人类
>
>可以看到这三个身份明显有着一种层级关系: *is-a*  
>i.e.
>- An **Undergraduate** *is-a* **Student** 
>- A **Student** *is-a* **Person** 
>
>反映到Java代码上, 就是使用`extends`关键字的**继承**关系
>```java
>public class Person {
 >   //Attributes...
>}
>
>class Student extends Person {
>    //Attributes...
>}
>
>class Undergraduate extends Person {
>    //Attributes...
>}
>
>```

>[!hint]- Fun Fact: `Object`类
>所有的类在定义时, 就算没有指定继承某一个类, 这个类也隐式地继承了`Object`类.
>
>```java
>public class Person {
> //Attributes... 
> }
> 
>/*Same as
> public class Person extends Obeject {
>      ...
>  }
> */
>
>```
>其实是非常显然的, 毕竟任何的类都 *是*(*is-a*) 对象(Object).




# 多态/Polymorphism


# 参考/Reference
[CS 61A Spring 2024](https://www.learncs.site/docs/curriculum-resource/cs61a/syllabus)

[Hug61B Spring 2019 Edition](https://joshhug.gitbooks.io/hug61b/content/)

[《Java核心技术·卷 I(原书第11版)》](https://reader5.z-library.sk/?source=68900553ec925b7014f2fbf26b679077dac62c4af5cb9d953be0a27e4e750b15&download_location=https%3A%2F%2Fzh.z-lib.gs%2Fdl%2F16271542%2F41e8fe)