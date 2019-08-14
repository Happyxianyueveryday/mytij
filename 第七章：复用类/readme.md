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
和cpp中需要将虚函数声明为

### 4. 向上转型规则
