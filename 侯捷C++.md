# C++面向对象开发

### 2、头文件与类的声明

C++中class与struct几乎没什么区别

class的两种经典分类

（1）Object Based：不带指针的类

（2）Object Oriented： 带指针的类

模板

3、

内联函数 inline ，内联函数更快，但是是否能变成内联函数由编译器决定，不是在class内定义的函数都是内联函数

构造函数也可以重载（overloading），重载实际上是编译器给函数取了不同的名字 

如果把构造函数放在private，则该类不可被构造。

对于不会改变数据的函数，后面加const，如：

```c++
double real() const { return re; }
double imag() const { return im; }
```

函数传参与返回都尽量传引用

同一个class的各个对象互为友元

临时对象，class（）

堆（stack）栈（heap）

```
直接构造，对象放在栈中，生命周期为其作用域
使用new构造，对象放在堆中，生命周期为全局，需要手动delete释放内存
```

静态函数无this指针，只能处理静态数据

类模板，函数模板 template<>



##### 类之间的关系

**构造由内而外，析构由外而内**。

复合：对象中包含对象

委托（Delegation）：对象中包含对象指针

继承：数据被继承，函数的调用权被继承



非虚函数 ：不希望子类重载此函数

虚函数（virtual）：希望子类能重载此函数，并且此函数有默认定义

纯虚函数：希望子类一定要重载此函数，此函数无默认定义

```c++
class Shape
{
public:
	virtual void draw() const = 0;  //纯虚函数
	virtual void error(const std::string& msg); //虚函数
	int objectID() const; //非虚函数
}
```

