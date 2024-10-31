---
title: "OOP: 面向对象编程"
draft: false
tags:
---
# 概述/Overview
**面向对象编程**(OOP, Object Oriented Programming)是一种程序开发方法, 能够将一系列相关的函数和数据打包封装为一个数据基本单位(类), 从而大大提高程序的灵活性和拓展性. 

OOP不仅是一种编程方法, 还是一种**抽象**的思想. 通过抽象简化问题, 能够让程序更便于学习与维护. 因此许多流行的编程语言都是面向对象的, 而Java正是其中的一种. 本文将简要地基于Java介绍OOP的特性及其蕴含的思想.

---
# 封装/Encapsulation
**封装**是一种抽象的方法. 把一个数据结构的内部实现隐藏起来, 在外部操作**封装**好的对象, 通常需要通过公开的方法(如`getter`和`setter`)实现. 

下面是一个封装好的类的简单示例:
```java
class Student {
	private String name;
	private int age;

	public String getName() {
		return this.name;
	}

	public int getAge() {
		return this.age;
	}

	public void setAge(int age) {
		this.age = age;
	}
}
```


封装可以使代码既易于维护, 又易于使用.
- 对于类的设计者, 当把类中的一个字段公开时, 就需要对这一部分负责. 在后续的代码维护中, 应当始终保持这个字段是可用的; 而对于私有的字段, 设计者则可以随意修改实现方法.
- 对于类的用户, i.e. 另一个程序员(甚至很可能就是**你自己**!), 不需要考虑类的实现细节, 只需要通过公开的方法来操作类即可.

>[!warning] 破坏封装的后果
>假设小明设计了一个`Person`类, 其中的`birthday`以`yyyymmdd`格式表示生日, 并保存为公开的`int`型变量.
>```java
>public class Person {
>	public final int birthday;
>	//other attributes	
>}
>```
>在代码的多处, 为了得到生日的年份, 需要要使用`birthday/1000`的方式来获取年份信息.
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
>如果一开始就**封装**好了类, 将实例字段隐藏起来, 使用`getter`获得年份, 那样就算修改了`birthday`的数据类型, 也不会影响到类之外其它地方的代码.

>[!bug] 结论
>可见, 破坏封装会使**外部代码直接依赖于类的公共字段的具体实现,** 一旦这个字段修改, 其它地方也要修改. 从而极大提高代码维护成本.
---


# 继承/Inheritance
继承是类之间最常见的一种关系. 通过继承, 可以基于已有的类创建新的类, 新的类可以继承已有的类的方法, 从而能够拥有和父类一样的属性和行为, 同时又能创建新方法, 或是重载旧方法, 具有特化的属性和行为, 实现代码的复用.
>[!hint]
> 如果一个类**继承**了另一个类, 那么他们之间即为*is-a*关系.

A *is-a* B关系意味着:
- A在逻辑上是B的子集
- A是B的子类(Subclass), B是A的超类(Superclass)

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
>	public void greeting() {
>		System.out.println("Hi");
>	}
 >   //other attributes...
>}
>
>class Student extends Person {
 >   //other attributes...
>}
>
>class Undergraduate extends Student {
>	private String name = "Xiaoming";
>	@override
>	public void greeting() {
>		System.out.printf("Hi, I am %s.\n", name);
>	}
 >   //other attributes...
>}
>
>```

>[!info]- Fun Fact: `Object`类
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
>毕竟任何的类都 *是*(*is-a*) 对象(Object).

---
# 多态/Polymorphism
**多态**允许不同类的对象以相同的方式被处理. 它使得程序可以通过统一的接口操作不同类型的对象，从而提高代码的灵活性和可扩展性. 

多态体现在运行时, 对象的类是多种形式的, 它可以被视为自己类的实例, 父类的实例, 父类的父类的实例..., 还是实现了某个接口的实例.
```java
public class Person {
    public void speak() {
        System.out.println("I'm a person");
    }
}

class Student extends Person implements Comparable{
    public void speak() {
        System.out.println("I'm a student");
    }
}

class teacher extends Person {
    public void speak() {
        System.out.println("I'm a teacher");
    }
}
```
对于一个`Student`实例`s`, `s instanceof Student`, `s instanceof Person`, `s instanceof Comparable`的结果都为`True`.

此外, 通过下面的输出可以看到, 结果`p`的类型是`Person`, 却也可以指向`Person`的子类对象, 同时根据不同实例的类, 调用了不同的方法.
```java
class Main {
    public static void main(String[] args) {
        Person p;
		p = new Person();
        p.speak();//I'm a person
        p = new Student();
        p.speak();//I'm a student
        p = new teacher();
        p.speak();//I'm a teacher
    }
}

```


---
# 接口/Interface
接口用来描述类应该做什么, 而不指定它们具体应该如何做. 
接口不是类, 但是在某些方面却和类有相似之处:
- 接口和类一样可以使用`instanceof`关键字
- 和类一样, 可以用接口声明一个变量

实现接口和继承父类有点相似, 但区别在于
- 一个类能实现多个接口, 却只能继承一个父类;
- 类实现接口, 需要实现接口要求的方法, 而类继承父类自动获得父类的方法.

>[!hint]
> 如果一个类实现了一个接口, 那么他们之间为*is-xx-able*的关系.

A is-B-able意味着A承诺能够完成B指定的操作, 由此一来, A就能享受因为自己是B-able的而带来的服务.

>[!example] `Iterable`接口
>所谓的"服务"指的是什么呢? `Iterable`接口就是个很好的例子:
>
>任何实现了`Iterable`接口的类都能够使用Java内置的`for-each`形式的for循环, 关于类的具体实现, Java完全不(需要)知情.
>```java
>myList<Integer> l = new myList<>();
>//Do sth...  
>for (Integer i : l) {  
>   //Do sth...  
>}
>```
>当然了, 为了实现这个功能, 你需要在类中实现一个返回迭代器的方法
>```java
>class myList<T> implements Iterable<T> {
>	public Iterator<T> iterator() {  
> 	   return new myIterator<T>();  
>	}
>	public static class myIterator<T> implements java.util.Iterator<T> {
>		//definition of myIterator...
>	}
>	//other attributes...
>}
>```
---

# 参考/Reference
[CS 61A Spring 2024](https://www.learncs.site/docs/curriculum-resource/cs61a/syllabus)

[Hug61B Spring 2019 Edition](https://joshhug.gitbooks.io/hug61b/content/)

[《Java核心技术·卷 I(原书第11版)》](https://reader5.z-library.sk/?source=68900553ec925b7014f2fbf26b679077dac62c4af5cb9d953be0a27e4e750b15&download_location=https%3A%2F%2Fzh.z-lib.gs%2Fdl%2F16271542%2F41e8fe)