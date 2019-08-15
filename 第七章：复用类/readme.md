## 第七章：复用类

java在继承上相对于cpp对继承规则进行了大幅简化，因此在熟悉cpp的基础上，java的继承规则很容易掌握。

### 1. java类的继承方式
回顾一下，在cpp中存在的三种主要继承方式包括：
+ public继承：基类的public和protected成员在派生类中的访问权限分别为public，protected。
+ protected继承：基类的public和protected成员在派生类中的访问权限均为protected。
+ private继承：基类的public和protected成员在派生类中的访问权限均为private。
需要注意的是，不论是哪种继承方式，基类的private成员在派生类中都是private的。

而在java中对继承规则进行了很大的简化。在java中仅有一种继承方式，就是public继承，换言之，在java的继承中：基类的public和protected成员在派生类中的访问权限仍然分别是public，protected。

### 2. java继承的构造函数
java继承中派生类的构造函数遵守和cpp相同的规则，即：派生类的构造函数需要首先调用基类的构造函数以初始化基类对象成员，然后再初始化派生类的成员。

### 3. java虚函数规则和final关键字
和cpp中需要将虚函数声明为virtual不同，java中默认所有方法都是虚函数，对于不想设置为虚函数方法，使用final关键字用于修饰。
特别的，final关键字修饰成员属性时，表示该属性是一个常量；而final关键字修饰成员方法时，表示该方法不作为虚函数，派生类不能重写该方法。

### 4. 向上转型规则
java的继承机制和cpp相同，允许将基类引用绑定到派生类的对象上。在cpp中更进一步，不仅允许将基类引用绑定到派生类对象上，还允许将基类指针指向派生类对象。

### 5. 静态代理模式
java中的代理模式分为静态代理，动态代理。其中动态代理将在RTTI一章讲解，本处主要介绍静态代理模式，静态代理模式的核心是：
+ 首先创建一个接口，代理类和被代理类均实现该接口，被代理类对接口需要给出具体实现，而代理类只需要在对应接口中调用被代理类对象的同名接口，从而实现静态代理。
下面举一个静态代理的经典实例。

```
// 1.Flayable是一个公共接口，表示会飞的对象
public interface Flyable
{
  public void fly();
}

// 2. 被代理类，Bird是一种可以飞的动物，实现Flyable接口
public class Bird implements Flyable
{
  public void fly()
  {
    System.out.println("Birds can fly");
  }
}

// 3. 代理类，BirdProxy同样实现与代理类相同的接口，在对应接口只需要调用被代理类对象的同名接口
public class BirdProxy implements Flyable
{
  
}
```


