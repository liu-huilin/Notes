### 基础知识

##### 引用

引用是变量的“别名”，因此在声明引用的时候必须赋值。



指针类型的引用：类型  *&指针引用名 = 指针；

```c++
#include <iostream>
using namespace std;

int main()
{
	int a=10;
	int *p=&a;
	int *&q=p;
	*q=20;
	cout<<a<<endl;
	
	return 0;
}
```

```
运行结果：
20
```

#### 函数

##### 函数参数默认值

C++中函数的参数可以有默认值，但是有默认值的参数必须在参数表的最右边，如：

```c++
void fun(int i,int j=5,int k=10); //正确，参数j、k的默认值分别为5、10
void fun(int i,int j=5,int k);  //错误！
```

*注：最好是声明函数时写默认值，而实现的时候不写。*

##### 函数重载

在相同作用域内，用同一函数名定义多个函数，其中每个函数的参数个数或参数类型不同。

##### 内联函数

关键字：inline

作用：编译时将函数体代码和实参代替函数调用语句

*注意：内联编译是建议性的，由编译器决定。*

​			*逻辑简单，调用频繁的函数建议使用内联。*

​			*递归函数无法使用内联方式。*

##### 静态成员函数

所有的对象共享同一个静态成员函数，静态成员函数只能访问静态成员变量。对象的成员函数通常有一个隐藏的参数this指针，用于指向具体的对象。但是静态函数是所有对象共有的，不存在这个参数，因此只能访问静态成员变量。

```c++
class Person
{
public:
	static void func()
	{
		cout << "static func" << endl;
	}
};

// 调用方式1 通过对象调用
Person p;
p.func();

// 调用方式2 通过类名访问
Person::func();
```

##### Lambda函数

基本语法

```c++
[capture](parameters) -> return_type { /* ... */ }
/*
capture：捕获外部变量的方式
    []表示不捕获任何变量
  	[capture]表示以值传递方式捕获capture
  	[=]表示以值传递方式捕获所有父作用域的变量（包括this）
  	[&capture]表示以引用方式捕获capture
  	[&]表示以引用方式捕获所有父作用域的变量（包括this）
  	&和=可以混用，如[x, &y]
parameters：形参
return_type：返回值类型，Lambda表达式的返回类型会自动推导，通常可以不写
*/
```

> 详细参考：https://blog.csdn.net/qq_37085158/article/details/124626913

##### C++对象模型和this指针

在C++中，类的成员变量和成员函数分开存储，只有非静态成员变量才属于类的具体对象。

非静态成员变量占用对象的空间，静态成员变量不占用对象的空间（因为他不属于某个对象），成员函数也不属于类的具体对象，只有一份，通过隐藏的this指针参数区分是哪个对象调用的函数。

空对象占用内存空间为1字节，C++编译器会给每个空对象也分配一个自己的内存空间，是为了区分空对象占用的内存位置。

this指针是隐含的每个非静态成员函数内的一种指针。

#### const

##### const关键字与指针类型

```c++
const int *p=NULL;
int const *p=NULL;
```

上面两条语句完全等价，const修饰的都是*p，则p指向的内容不可变，但p可以改变指向。

```c++
int *const p=NULL;
```

上面语句的const修饰的是p，则p不可变，即p的指向不可变，但p所指向的内容可变。

```C++
const int * const p=NULL;
int const * const P=NULL;
```

上面两条语句完全等价，const既修饰*p又修饰p，则p的指向和p所指向的内容都不可变。

```c++
/* 以下代码错误， 因为将不可变的变量x的地址复制给了可变指针y，则有通过*y改变x的风险，故会报错*/
const int x=3;
int *y=&x;
/* 以下代码正确，将权限大（可变）的赋值给权限小（不可变）的 */
int x=3;
const int *y=&y;
```

##### const关键字与引用

```c++
int x=3;
const int &y=x;
x=10;//正确
y=20;//错误，const修饰了&y，则y不可变
```



##### const修饰成员函数

- 常函数

  成员函数后加了const的函数叫常函数，常函数不可以修改成员属性，但是加了关键字mutable的成员属性可以在常函数中修改。

- 常对象

  声明对象前加const的对象叫常对象，常对象只能调用常函数（可以通过常函数修改加了关键字mutable的成员属性）。

##### 友元

- 全局函数做友元

```c++
friend void functionName(); // 全局函数functionName()为当前类的友元
```

- 类做友元

```c++
friend class className; // 类className为当前类的友元
```

- 成员函数做友元

```c++
friend void className::functionName() // 类className的成员函数functionName()为当前类的友元
```

##### 运算符重载

```c++
class Person {
public:
	Person() {};
	Person(int a, int b)
	{
		this->m_A = a;
		this->m_B = b;
	}
	//成员函数实现 + 号运算符重载
	Person operator+(const Person& p) 
    {
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}

public:
	int m_A;
	int m_B;
};

//全局函数实现 + 号运算符重载
Person operator+(const Person& p1, const Person& p2) {
	Person temp(0, 0);
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;
}

//运算符重载 可以发生函数重载 
Person operator+(const Person& p2, int val)  
{
	Person temp;
	temp.m_A = p2.m_A + val;
	temp.m_B = p2.m_B + val;
	return temp;
}

void test() {

	Person p1(10, 10);
	Person p2(20, 20);

	//成员函数方式
	Person p3 = p2 + p1;  //相当于 p2.operaor+(p1)
	cout << "mA:" << p3.m_A << " mB:" << p3.m_B << endl;


	Person p4 = p3 + 10; //相当于 operator+(p3,10)
	cout << "mA:" << p4.m_A << " mB:" << p4.m_B << endl;

}

int main() {

	test();

	system("pause");

	return 0;
}
```

对于内置的数据类型的表达式的的运算符是不可能改变的

### 面向对象

#### 实例化对象

从栈中实例化对象，无需手动释放内存，系统自动回收。

```
A a;
A a[20];
```

从堆中实例化对象，使用完后需要通过delete关键字释放内存。

```
A *p=new A();
A *p=new A[20];
```

注：构造函数没有返回值，void也不用。

#### 构造函数初始化列表

1、初始化列表先于构造函数执行。

2、初始化列表只能用于构造函数。

3、初始化列表可以同时初始化多个数据成员。

```c++
Class Student
{
public:
	Student():m_strName("Jim"),m_iAge(10){}//使用初始化列表初始化成员m_strName、m_iAge
private:
	string m_strName;
	int m_iAge;
};
```

可以用初始化列表的方式初始化常量类型的数据成员。

#### 拷贝构造函数

1、若无自定义的拷贝构造函数，则系统自动生成一个默认的拷贝构造函数。

2、当采用直接初始化或复制初始化实例对象时，系统自动调用拷贝构造函数。

分**深拷贝**和**浅拷贝**。

#### 析构函数

格式：**~ClassNmae（）**

作用：对象销毁时自动调用，用于释放资源，需要注意的是析构函数无参数、无返回值，若无自定义析构函数，则系统自动生成。

```mermaid
graph LR
    A[申请内存]-->B[初始化列表]-->C[构造函数]-->D[使用]-->E[析构函数]-->F[释放内存]-.->A
    
```

<center>对象的生命历程<center>



### 继承

##### 基本语法

```c++
class newClass : public baseClass
{

}
```

##### 继承方式

无论是哪种继承方式，子类都无法访问父类的private成员

- public继承：父类的public继承为public，protected继承为protected
- protected继承：父类的public和protected都继承为protected
- private继承：父类的public和protected都继承为private

父类中的所有非静态成员都会被继承给子类，但是子类无法访问private成员，子类占用的内存大小是父类所有成员属性的空间加子类特有的成员属性的空间。

查看类布局：调用VS的开发人员命令提示符，调用如下命令

```
cl /d1 reportSingleClassLayoutClassName "filename.cpp"
```

 当子类与父类出现同名的成员时（包括静态成员）：

- 访问子类成员：直接访问
- 访问父类成员：加上作用域

子类中只要出现父类的同名成员，就会隐藏父类所有同名成员，包括所有的重载函数，都需要加作用域访问。

##### 多继承

```c++
class newClass : public baseClass1, private baseClass2, ...
{

}
```

多继承可能会引发各个父类中存在同名成员，因此子类访问需要加作用域区分。

> 在实际开发中不建议使用多继承

##### 菱形继承（钻石继承）

多个类A、B、C...继承了同一个父类，又有一个新类继承了A、B、C...这些类。

利用**虚继承**可以解决菱形继承中多次继承同一属性的问题。

```c++
class ClassA : virtual public baseClass
{
public:
	int age;
}
class ClassB : virtual public baseClass
{
public:
	int age;
}
class newClass : public baseClassA, private baseClassB
{

}
```

使用虚继承后，同名的属性age只继承一份，可以节约内存空间，也不会造成二义性。

### 多态

多态是C++面向对象三大特性之一

多态分为两类

* **静态多态**: 函数**重载**和运算符重载属于静态多态，复用函数名
* **动态多态**: 派生类和虚函数实现运行时多态

静态多态和动态多态区别：

* 静态多态的函数地址早绑定  -  编译阶段确定函数地址
* 动态多态的函数地址晚绑定  -  运行阶段确定函数地址



动态多态示例

```c++
class Animal
{
public:
	virtual void speak()
	{
		cout << "动物在说话" << endl;
	}
};

class Cat :public Animal
{
public:
	void speak()
	{
		cout << "猫在说话" << endl;
	}
};

void doSpeak(Animal& animal)
{
	animal.speak();
}


int main()
{
	Cat cat;
	doSpeak(cat);


	return 0;
}
```

多态满足条件

* 有继承关系
* 子类重写父类中的虚函数

多态使用条件

* 父类指针或引用指向子类对象

重写：函数返回值类型  函数名 参数列表 完全一致称为重写

##### 纯虚函数和抽象类

- 纯虚函数

```c++
virtual void func() = 0;
```

- 抽象类：含有纯虚函数的类

抽象类的子类必须要重写父类中的纯虚函数，否则也属于抽象类，**抽象类无法实例化**。

##### 虚析构和纯虚析构

使用多态时，如果子类在堆区申请了内存，父类指针无法释放这些内存，此时可以将父类中的析构函数改为**虚析构**或**纯虚析构**

- 虚析构

```c++
virtual ~className(){}
```

- 纯虚析构

```C++
virtual ~className()=0;
// 类外重写具体实现，必须写
className::~className(){}
```

若析构函数为纯虚析构，则该类属于抽象类，无法实例化该类。

### 文件操作

##### 头文件

```c++
#include<fstream>
```

##### 文件类型

- **文本文件**：文件以文本的**ASCII码**形式存储在计算机中

- **二进制文件**：文件以文本的**二进制**形式存储在计算机中，用户一般不能直接读懂它们

##### 相关类

- ofstream：写操作
- ifstream： 读操作
- fstream ： 读写操作

##### 文件打开方式

| 打开方式    | 解释                       |
| ----------- | -------------------------- |
| ios::in     | 为读文件而打开文件         |
| ios::out    | 为写文件而打开文件         |
| ios::ate    | 初始位置：文件尾           |
| ios::app    | 追加方式写文件             |
| ios::trunc  | 如果文件存在先删除，再创建 |
| ios::binary | 二进制方式                 |

**注意：** 文件打开方式可以配合使用，利用|操作符

**例如：**用二进制方式写文件 `ios::binary |  ios:: out



### 模板

##### 函数模板

**作用**：建立一个通用函数，返回值和形参类型可以不具体制定，用一个虚拟的类型来代表。

**语法**：

```c++
template<typename T>
T add(T a, T b)
{
    T sum = a + b;
    return sum;
}
```

template：声明创建模板

typename：表面其后面的符号表示一种数据类型，也可以用class代替

T：通用的数据类型，名称可以替换，通常为大写字母

**调用方式**：

```c++
int a =10, b= 20, sum = 0;
// 1、自动类型推导，必须要推出一致的数据类型T才可以使用
sum = add(a, b)
// 2、显示指定类型
add<int>(a, b)
```

**注意**：

- 模板必须确定出T的数据类型才可以使用，若使用自动类型推导，必须要推出一致的数据类型T。
- 函数模板在使用自动类型推导的时候，无法发生隐式类型转换。
- 函数模板指定类型时可以发生隐式类型转换。

**重载**

```c++
template<> int functionName(className a, className b)
```

这样如果在className类使用这个函数模板的时候，就会优先调用重载的函数模板



##### 类模板

**语法**

```c++
// 用typename和class区别不大
template<class NameType, class AgeType>
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->name = name;
		this->age = age;
	}
	
	NameType name;
	AgeType age;
}
```

**调用方式**

```c++
Person<string, int> P1("zhangsan", 18)
```

**注意**

- 类模板没有自动类型推导，只能用指定类型的方式
- 类模板可以有默认参数，如：

```c++
template<typename NameType, typename AgeType = int>
class Person
{
    ...
};

// 调用
Person<string> p1("zhangsan", 18);
```

- 类模板的成员函数在调用时才被创建

**类模板的继承**

如果父类是类模板，子类需要指定出父类中T的数据类型

```c++
template<class T>
class Base
{
	T m;
};

//类模板继承类模板 ,可以用T2指定父类中的T类型
template<class T1, class T2>
class Son2 :public Base<T2>
{
public:
	T1 t;
};

```



### STL

Standard Template Library

##### STL六大组件

- 容器：各种数据结构，如vector、list、set等，用于存放数据
- 算法：各种常用的算法，如sort、find
- 迭代器：容器和算法之间的桥梁
- 仿函数：行为类似函数，可作为算法的某种策略
- 适配器（配接器）：一种用于修饰容器、仿函数或迭代器接口的东西
- 空间配置器：负责空间的配置与管理

 

#### 容器

##### string容器

用一个类管理字符数组

##### vector容器

与数组很类似，但是vector可以动态扩展，这个动态扩展不是在原有的空间后面续接空间，而是找更大的空间，然后将原数据拷贝到新空间，释放原空间

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/clip_image002.jpg)

vector的迭代器是支持随机访问的迭代器

##### deque容器

双端数组，头尾都可以进行插入和删除

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/clip_image002-1547547642923.jpg)

**deque与vector的区别**：

- vector对于头部插入删除效率低，因为需要将数据依次后移
- deque同时支持尾部和头部的插入删除，头部的操作比vector效率高
- vector访问元素的速度比deque快

**deque内部工作原理**：

deque内部存在一个中控器，维护每段缓冲区的内容，缓冲区内存放数据，中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间，如图

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/clip_image002-1547547896341.jpg)

deque容器的迭代器也支持随机访问

##### stack容器

栈，后进先出

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/clip_image002-1547604555425.jpg)

##### queue容器

队列，先进先出。

##### list容器

链表

链表的迭代器不可随机访问，因此不可用全局函数sort对其进行排序，可通过其成员函数sort进行排序，排序规则可通过回调函数指定。

```c++
list<int> lt;
lt.sort()  // 默认为升序排列
```

```c++
bool my_compare(int val1, int val2)
{
    return val1 > val2;
}
list<int> lt;
lt.sort(my_compare)  // 使用回调函数，变成降序排列
```

##### set/multiset容器

集合容器，所有的元素都会在插入时自动被排序。

set与multiset属于**关联式容器**，底层结构是使用二叉树实现的。

**区别**：

- set中元素不可重复
- multiset中可存在重复的元素



##### map/multimap容器

- map容器中所有的元素都是pair，pair为对组容器，存放的是成对出现的数据，第一个元素（first）为key，第二天个元素（second）为value
- map容器中所有的元素都会根据元素的键值自动排序
- map中不可含重复的key，multimap可含重复的key

**本质**：

map/multimap属于关联式容器，底层结构是用二叉树实现。

```c++
map<int, int> m;
m.insert(pair<int, int>(1, 10)); // map中所有的数据都是成对出现的，插入要用对组pair
```



#### 函数对象（仿函数）

##### 概念

重载函数调用操作符的类，其对象称为函数对象，在使用重载的()时其行为类似函数调用，因此也叫**仿函数**

```c++
class my_add
{
public:
	int operator() (int val1, val2)
	{
		return val1 + val2;
	}
};
```

##### 本质

函数对象（仿函数）是一个类，不是函数

##### 特点

- 函数对象在使用时，与普通函数无异
- 函数对象可以有自己的状态，因为其本质是个类



##### 谓词

返回bool类型的仿函数称为谓词，如果该仿函数接受一个参数，称为一元谓词，接受两个参数则叫二元谓词

####  常用算法

