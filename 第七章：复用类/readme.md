## 第七章：复用类

java在继承上相对于cpp对继承规则进行了大幅简化，因此在熟悉cpp的基础上，java的继承规则很容易掌握。

### 1. 继承的基本概念
通过在声明类Expired时使用extends Base关键字，可以使得当前所声明的类Expired获得其所继承的Base类的所有成员属性和方法，这就是继承的基本概念。其中Base称为基类，Expired称为派生类。

### 2. java类的继承方式
继承方式的基本概念是，继承方式决定了派生类从基类中所获得的成员属性或方法的访问权限。

回顾一下，在cpp中存在的三种主要继承方式包括：
+ public继承：基类的public和protected成员在派生类中的访问权限分别为public，protected。
+ protected继承：基类的public和protected成员在派生类中的访问权限均为protected。
+ private继承：基类的public和protected成员在派生类中的访问权限均为private。
需要注意的是，不论是哪种继承方式，基类的private成员在派生类中都是private的。

而在java中对继承规则进行了很大的简化。
在java中仅有一种继承方式，就是public继承，换言之，在java的继承中：基类的public和protected成员在派生类中的访问权限仍然分别是public，protected。

### 3. java派生类的构造
和cpp类似，java派生类的构造器中，同样先使用super引用调用其基类的构造器，然后再构造派生类对象自身；也即首先调用基类构造器构造继承自基类的成员属性，然后再初始化派生类自己的新的成员属性。

### 4. java虚函数和非虚函数
和cpp中需要将虚函数声明为virtual不同，java中默认所有方法都是虚函数，对于不想设置为虚函数方法，使用final关键字用于修饰。
特别的，final关键字修饰成员属性时，表示该属性是一个常量；而final关键字修饰成员方法时，表示该方法不作为虚函数，派生类不能重写该方法。

### 5. 向上转型规则
java的继承机制和cpp类似，允许将派生类的对象绑定到基类的引用上，这一过程是自动进行的，被称为向上转型。例如：

```
Base a=Derived();    // 将派生类Derived的对象绑定在基类引用a上
```

附注：在cpp中更进一步，不仅允许将基类引用绑定到派生类对象上，还允许将基类指针指向派生类对象，在java中没有指针的概念，因此向上转型规则就体现在将派生类对象绑定到基类的引用变量上。

### 6. 重载和重写方法，@Override关键字
在派生类中定义和基类中名字相同，但是参数列表不同的方法，称为重载。这时基类的同名方法不会被覆盖。调用该名字的成员方法时，根据参数类型列表的不同进行静态绑定，以确定调用哪个方法。

在基类中定义和基类中名字相同，且参数列表也相同的方法，称为重写。这时基类的同名方法会被覆盖。使用派生类引用调用该名字的成员方法时，只会调用派生类中定义的同名方法（基类中同名且参数类型列表相同的方法被隐藏）；使用基类引用调用成员方法时，将会发生动态绑定，根据引用实际绑定的对象类型来决定调用哪个版本的方法，这部分内容可以参考后面多态的介绍。

> @Override注解：@Override注解类似于cpp11中的override关键字，推荐在具体需要重写的方法前加上@Override注解，基于两点原因：第一，该注解清楚地标识了当前方法是一个重写方法，重写了基类中的同名方法。第二，当不小心声明方法错误，没有重写基类的同名方法时，在编译期就会进行报错。

### 7. final关键字
final关键字出现在不同的位置，修饰不同的对象时，具有不同的含义，如下所示：

+ final修饰成员属性/普通属性：类似于cpp中的const关键字，表示声明的属性为一个常量。
+ final修饰成员方法：类似于cpp中的final关键字，表示该成员方法不能被派生类进行重写。

### 8. java继承的构造和析构
  （1）构造规则：
  java中对于继承的构造方式和cpp相同，简言之：先调用基类的构造函数构造基类对象，然后再构造自身成员。区别在于cpp中调用基类对象的写法是直接使用基类名的构造函数，而java的用super表示基类，因此使用super来调用基类的构造函数。

  （2）析构规则：
  因为java中不存在真正的析构函数，因此派生类若使用了某种资源，需要自己实现一个方法dispose()，在该方法中首先释放自身的资源，然后再调用基类的dispose()方法释放基类的资源，示例代码如下：

```
class BaseClass
{
    public void dispose()
    {
        System.out.println("基类释放资源")
    }
}

class ExtendClass
{
    public void dispose()
    {
        System.out.println("派生类释放资源");
        super.dispose();
    }
}
```
  
  特别的，在使用需要使用资源的类的对象时，必须使用try...finally语句来进行，将需要申请资源的方法放在try语句块中，然后在finally语句中调用对应的dispose()方法来进行资源的释放。由于finally语句在执行了try语句后，不论出现何种情况都必定会被执行，因此在finally语句中调用dispose()能够确保所申请的资源一定会被释放。
  一个简单的示例如下，Worker类对象在work方法中使用资源，dispose中释放资源。
  ```
  Worker worker=new Worker();
  try
  {
    worker.work();    // 在try语句中调用需要使用资源或者抛出异常的方法
  }
  catch ...
  catch ...
  finally
  {
    worker.dispose();  // 在finally语句中调用释放资源的dispose方法，以确保所使用的资源确实得到了释放
  }
  ```

  
### 9. 静态代理模式
java中的代理模式分为静态代理，动态代理。其中动态代理将在RTTI一章讲解，本处主要介绍静态代理模式，静态代理模式的核心是：
+ 首先创建一个接口，代理类和被代理类均实现该接口，被代理类对接口需要给出具体实现，而代理类只需要在对应接口中调用被代理类对象的同名接口，从而实现静态代理。
下面举一个静态代理的经典实例。

```
// 1.公共接口：Flayable是一个公共接口，表示会飞的对象
public interface Flyable
{
  public void fly();
}

// 2. 被代理类：Bird是一种可以飞的动物，实现Flyable接口
public class Bird implements Flyable
{
  public void fly()
  {
    System.out.println("Birds can fly");
  }
}

// 3. 代理类：BirdProxy同样实现与代理类相同的接口，在对应接口只需要调用被代理类对象的同名接口
// 这里的代理类不仅能够代理Bird，还可以代理实现了Flyable接口的其他类，例如会飞的昆虫等。
public class BirdProxy implements Flyable
{
  private Flyable obj;
  public BirdProxy(Flyable obj) 
  {
    this.obj=obj;
  }
  public void fly()
  {
    this.obj.fly();
  }
}

// 4. 代理类的使用：当需要调用被代理类的某个方法时，只需要将被代理类对象传入代理类进行初始化，然后调用代理类中的方法即可，代理类可以多次代理不同的被代理类对象
class Test
{
  public static void main()
  {
    Bird A=new Bird();
    BirdProxy proxy=new Bird(A);
    proxy.fly();
  }
}
```


