# C语言头文件

## 类的定义

* 分离的 .h 文件和 .cpp 文件来定义一个类 ；
* 会把类的声名（declaration）和这个类里所有的函数的原型（这个本来就在类的declaration里面）都放在头文件里面（ .h ）；
* 所有的函数的实现（body of function）都放在对应的 source file （ .cpp）里面。

我们要把这两个分开，声名（ declaration）和定义（ definition）在C++中是非常不同的东西。

## 头文件

* 头文件中声明函数后，之后所有要用到这个函数的地方以及唯一定义他的地方都该 include这个头文件
* 同样所有要用到这个类的地方，以及要用到这个类的实体的地方 include这个头文件

## 头文件 = 接口 （Header = interface）

* 头文件是一个你（设计这个类的人）和你的代码的使用者（使用这个类的人）之间的一份合同（contract）
* 编译器会强化（enforce）这份合同，在用这些结构、这些函数、这些类之前都必须要声明，不声明是无法使用他们的

## C++程序的结构

include做的是文本的插入

C++最早只有翻译器，C++的所有的东西都能翻译为 C 的代码，而C++编译器在翻译的过程中会改名字（你的函数名字前面会加一个一个下划线），C不会

### 声名（declarations）和 定义（ definitions）

* 一个 .cpp 文件是一个编译单元
* 只有声名能往 .h 里面放 ，
  * extern variables（变量前加 extern是声名）
  * function prototypes （函数后面没有大括号的是原型，有大括号的是定义）
  * class/struct declaration

### \#include

* \#include 把 include的文件的内容插入到需要的地方，该放的地方就得放

  * #include "xx.h" ：先在文件目录里找

  * #include <xx.h> ：直接在系统文件里找（编译器认为的地方）

  * #include \<xx\> ：如同 #include <xx.h> （历史故事）

    历史上有版本用的的iostream.h，后面改版了由于某中原因保留了旧版本。为了区别新旧两个版本新版本中就把后缀.h去掉了

### 标准头文件结构

```c++
#ifndef HEader_FLAG
#define HEader_FLAG
// Typ declaration here...
#endif //HEAR_FLAG 
```

## Tips for header

1. 一个类的声名都要放在一个头文件里面

2. 对应的源代码文件用相同的前缀

3. 头文件要用标准头文件围绕起来

   绝对不能include一个 .cpp or .c文件，会重复

# 抽象

* 看待一件事情的时候，有意的不看一些细节

# 成员变量

## 本地变量

当本地变量和成员变量重名时，就近原则（和C一样）

## 成员变量（fields 字段），parameters，local variables

* fields 定义在一个类的所有的函数和构造函数之外

  ```c++
  class Student{
  public:
      //成员变量
      char *name;
      int age;
      float score;
      //成员函数
      void say();  //函数声明
  };
  ```

  

* fields 的值在一个对象的生存期内是永远存在的，和函数无关

* 成员变量的作用域是类的作用域，在整个类的所有成员函数里面都可以直接使用成员变量

  成员变量在哪里？他写在class声名里

  > parameters和local variable是一摸一样的，存储属性都是本地存储，进入这个函数，参数和本地变量都存在，出了函数，都不存在。他们放在堆栈里，具体位置有所不同

  我们既然说了声名，那我们就不知道他在哪里。但是类的成员变量我们是可以自如的在类的成员函数里面进行使用的。我们不需要考虑他在哪里，他们就会有，就能用。

  **类是虚的，类是概念，类是观点，类不是实体**

  类不是实体是虚的就不会有变量，对象才有变量

  > 而类中的函数是属于类的，函数不属于类的任何一个对象，所有的对象用的是同一个函数，如何做到函数知道是哪个函数调用的他？传类的指针当参数进去

## 调用类的函数

> Point a;
>
> a.print();

* 被调用的函数和调用他的变量之间是有联系的
* 这个函数知道要对这个变量做事情

### this：一个隐藏的参数

* **this** is a hidden parameter for all member functions, with the type of the class

  > void Point::print()
  >
  > ​	(**can be regarded as**)
  >
  > void Point::print(Point *p)

* 当我们调用一个函数的时候，必须指明一个变量，

  当我们调用成员函数时，实际上是替某个对象调用它。

  成员函数通过一个名为 this 的额外隐式参数来访问调用它的那个对象，当我们调用一个成员函数时，用请求该函数的对象地址初始化 this。

  > Point a;
  >
  > a.printf();
  >
  > ​	(**can be regarded as**)
  >
  > void Point::print(&a)

几点注意：

- this 是 const 指针，它的值是不能被修改的，一切企图修改该指针的操作，如赋值、递增、递减等都是不允许的。
- this 只能在成员函数内部使用，用在其他地方没有意义，也是非法的。
- 只有当对象被创建后 this 才有意义，因此不能在 static 成员函数中使用（后续会讲到 static 成员）。

#### this 到底是什么

this 实际上是成员函数的一个形参，在调用成员函数时将对象的地址作为实参传递给 this。

不过 this 这个形参是隐式的，它并不出现在代码中，而是在编译阶段由编译器默默地将它添加到参数列表中。

 this 作为隐式形参，本质上是成员函数的==局部变量==，所以只能用在成员函数的内部，并且只有在通过对象调用成员函数时才给 this 赋值。

 在《[C++函数编译原理和成员函数的实现](http://c.biancheng.net/cpp/biancheng/view/2996.html)》一节中讲到，成员函数最终被编译成与对象无关的普通函数，除了成员变量，会丢失所有信息，所以编译时要在成员函数中添加一个额外的参数，把当前对象的首地址传入，以此来关联成员函数和成员变量。这个额外的参数，实际上就是 this，它是成员函数和成员变量关联的桥梁。

# 构造与析构

```c++
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();   // 这是构造函数声明
      ~Line();  // 这是析构函数声明
 
   private:
      double length;
};
```

## 类的构造函数

类的**构造函数**是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。

构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。

构造函数可用于为某些成员变量设置初始值。

> Java 在类的声名的时候会要求初始化类所用的空间，C++不做这件事情

### 带参数的构造函数

默认的构造函数没有任何参数，但如果需要，构造函数也可以带有参数。这样在创建对象时就会给对象赋初始值，如下面的例子所示：

```c++
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line(double len);  // 这是构造函数
 
   private:
      double length;
};
```

### 使用初始化列表来初始化字段

```c++
Line::Line( double len): length(len)
{
    cout << "Object is being created, length = " << len << endl;
}
```

上面的语法等同于如下语法：

```c++
Line::Line( double len)
{    
  length = len;    
  cout << "Object is being created, length = " << len << endl; 
}
```

假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化，同理，您可以使用上面的语法，只需要在不同的字段使用逗号进行分隔，如下所示：

```c++
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}
```

## 类的析构函数

类的**析构函数**是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。

析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，==它不会返回任何值，也不能带有任何参数==。

析构函数有助于在跳出程序（比如关闭文件、释放内存等）==前==释放资源。

下面的实例有助于更好地理解析构函数的概念：

```c++
#include <iostream>
 
using namespace std;
 
class Line{
   public:
      void setLength( double len );
      double getLength( void );
      Line();   // 这是构造函数声明
      ~Line();  // 这是析构函数声明
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void){
    cout << "Object is being created" << endl;
}
Line::~Line(void){
    cout << "Object is being deleted" << endl;
}
 
void Line::setLength( double len ){
    length = len;
}
 
double Line::getLength( void ){
    return length;
}
// 程序的主函数
int main( ){
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```

# 对象的初始化

空间是进了大括号就分配了的，但构造函数是要运行到类构造对象的时候才会调用的

初始化列表的成员初始化顺序:

C++ 初始化类成员时，是按照声明的顺序初始化的，而不是按照出现在初始化列表中的顺序。

```c++
class CMyClass {
    CMyClass(int x, int y);
    int m_x;
    int m_y;
};

CMyClass::CMyClass(int x, int y) : m_y(y), m_x(m_y)
{
};
```

你可能以为上面的代码将会首先做 m_y=y，然后做 m_x=m_y，最后它们有相同的值。

但是编译器先初始化 m_x，然后是  m_y,，因为它们是按这样的顺序声明的。

结果是 m_x  将有一个不可预测的值。有两种方法避免它：

* ==一个是总是按照希望被初始化的顺序声明成员==，

* 如果你决定使用初始化列表，==总是按声明顺序罗列这些成员==。

  这将有助于消除混淆。

#   动态内存 （dynamic memory allocation）

> 了解动态内存在 C++ 中是如何工作的是成为一名合格的 C++ 程序员必不可少的。C++ 程序中的内存分为两个部分：
>
> - **栈：**在函数内部声明的所有变量都将占用栈内存。
> - **堆：**这是程序中未使用的内存，在程序运行时可用于动态分配内存。
>
> 很多时候，您无法提前预知需要多少内存来存储某个定义变量中的特定信息，所需内存的大小需要在运行时才能确定。

在 C++ 中， 可以使用特殊的运算符为给定类型的变量在运行时分配堆内的内存，这会返回所分配的空间地址。这种运算符即 **new** 运算符。

如果您不再需要动态分配的内存空间，可以使用 **delete** 运算符，删除之前由 new 运算符分配的内存。

## new 和 delete 运算符

new 运算符来为任意的数据类型动态分配内存的通用语法：

```c++
new data-type;
new int;
new Stash;
new int[10];


double* pvalue  = NULL; // 初始化为 null 的指针
pvalue  = new double;   // 为变量请求内存

```

如果自由存储区已被用完，可能无法成功分配内存。所以建议检查 new 运算符是否返回 NULL 指针，并采取以下适当的操作：

```c++
double* pvalue  = NULL; 
if( !(pvalue  = new double )) {  
  cout << "Error: out of memory." <<endl;   
  exit(1);
}
```

> **malloc()** 函数在 C 语言中就出现了，在 C++ 中仍然存在，但建议尽量不要使用 malloc() 函数。new 与 malloc() 函数相比，其主要的优点是，new 不只是分配了内存，它还创建了对象。

在任何时候，当您觉得某个已经动态分配内存的变量不再需要使用时，您可以使用 delete 操作符释放它所占用的内存，如下所示：

```c++
delete pvalue;        // 释放 pvalue 所指向的内存
```

如何使用 new 和 delete 运算符：

```c++
#include <iostream>
using namespace std;
 
int main ()
{
   double* pvalue  = NULL; // 初始化为 null 的指针
   pvalue  = new double;   // 为变量请求内存
 
   *pvalue = 29494.99;     // 在分配的地址存储值
   cout << "Value of pvalue : " << *pvalue << endl;
 
   delete pvalue;         // 释放内存
 
   return 0;
}
```

## 数组的动态内存分配

假设我们要为一个字符数组（一个有 20 个字符的字符串）分配内存，我们可以使用上面实例中的语法来为数组动态地分配内存，如下所示：

```c++
char* pvalue  = NULL;   // 初始化为 null 的指针
pvalue  = new char[20]; // 为变量请求内存
```

要删除我们刚才创建的数组，语句如下：

```c++
delete [] pvalue;        // 删除 pvalue 所指向的数组
```

下面是 new 操作符的通用语法，可以为多维数组分配内存，如下所示：

```c++
//一维数组
// 动态分配,数组长度为 m
int *array=new int [m]；
 
//释放内存
delete [] array;

```

```c++
//二维数组
int **array
// 假定数组第一维长度为 m， 第二维长度为 n
// 动态分配空间
array = new int *[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int [n]  ;
}
//释放
for( int i=0; i<m; i++ )
{
    delete [] arrary[i];
}
delete [] array;

```

```c++
//三维数组
int ***array;
// 假定数组第一维为 m， 第二维为 n， 第三维为h
// 动态分配空间
array = new int **[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int *[n];
    for( int j=0; j<n; j++ )
    {
        array[i][j] = new int [h];
    }
}
//释放
for( int i=0; i<m; i++ )
{
    for( int j=0; j<n; j++ )
    {
        delete[] array[i][j];
    }
    delete[] array[i];
}
delete[] array;

```

## 对象的动态内存分配

对象与简单的数据类型没有什么不同。例如，请看下面的代码，我们将使用一个对象数组来理清这一概念：

```c++
#include <iostream>
using namespace std;
 
class Box{
   public:
      Box() { 
         cout << "调用构造函数！" <<endl; 
      }
      ~Box() { 
         cout << "调用析构函数！" <<endl; 
      }
};
 
int main( ){
   Box* myBoxArray = new Box[4];
 
   delete [] myBoxArray; // 删除数组
   return 0;
}
```

如果要为一个包含四个 Box 对象的数组分配内存，构造函数将被调用 4 次，同样地，当删除这些对象时，析构函数也将被调用相同的次数（4次）。



## delete 与 delete[] 区别：

1、针对简单类型 使用 new 分配后的不管是数组还是非数组形式内存空间用两种方式均可 如：

```c++
int *a = new int[10];   
delete a;   
delete [] a; 
```

此种情况中的释放效果相同，原因在于：分配简单类型内存时，内存大小已经确定，系统可以记忆并且进行管理，在析构时，系统并不会调用析构函数，它直接通过指针可以获取实际分配的内存空间，哪怕是一个数组内存空间(在分配过程中  系统会记录分配内存的大小等信息，此信息保存在结构体_CrtMemBlockHeader中，具体情况可参看VC安装目录下CRT\SRC\DBGDEL.cpp)

2、针对类Class，两种方式体现出具体差异 

当你通过下列方式分配一个类对象数组：

```c++
class A
{
    private:
        char *m_cBuffer;
        int m_nLen;
    public:
        A(){ m_cBuffer = new char[m_nLen]; }
        ~A() { delete [] m_cBuffer; }
};
A *a = new A[10];

// 仅释放了a指针指向的全部内存空间 但是只调用了a[0]对象的析构函数 剩下的从a[1]到a[9]这9个用户自行分配的m_cBuffer对应内存空间将不能释放 从而造成内存泄漏
delete a;

// 调用使用类对象的析构函数释放用户自己分配内存空间并且   释放了a指针指向的全部内存空间
delete [] a;
```

所以总结下就是，如果ptr代表一个用new申请的内存返回的内存空间地址，即所谓的指针，那么：

- **delete ptr** -- 代表用来释放内存，且只用来释放ptr指向的内存。
- **delete[] rg** -- 用来释放rg指向的内存，！！==还逐一调用数组中每个对象的 destructor==！！

对于像 int/char/long/int*/struct 等等简单数据类型，由于对象没有 destructor，所以用 delete 和 delete [] 是一样的！但是如果是C++ 对象数组就不同了！

## Tips for new and delete

* 不要  **delete **不是  **new** 出来的空间

* 不要对同一块空间用两次 **delete **

* **new[] **用 **delete[]** 相匹配

* **delete **空指针（NULL point）是安全的

  留着这个口子可能是为了让代码好写一点

# 继承

## 组合

软件重用的一种方式——组合（composition），用两个对象放一起加点其他的东西组合为一个新的对象。

一个对象也可以是另一个对象的成员，可以通过对象本身访问（fully 是自己的一部分），也可以用指针访问（reference 外部访问）具体用哪种看语意

> Java 只有reference

## 继承

面向对象程序设计中最重要的一个概念是**继承**。

继承允许我们依据另一个类来定义一个类，这使得创建和维护一个应用程序变得更容易。这样做，也达到了重用代码功能和提高执行效率的效果。

当创建一个类时，不需要重新编写新的数据成员和成员函数，只需指定新建的类继承了一个已有的类的成员即可。这个已有的类称为**基类**，新建的类称为**派生类**。

继承代表了 **is a** 关系。例如，哺乳动物是动物，狗是哺乳动物，因此，狗是动物，等等。

组合玩实的，继承是玩虚的

### 基类 & 派生类

Class relationship：Is - A

父类 ：Base Class（基类）； Super； Parent

子类：Derived Class （派生类）；Sub ；Child

一个类可以派生自多个类， 它可以从==多个基类==继承数据和函数。

定义一个派生类，我们使用一个类派生列表来指定基类。

类派生列表以一个或多个基类命名，形式如下：

```c++
class derived-class: access-specifier base-class
```

其中，访问修饰符 access-specifier 是 **public、protected** 或 **private** 其中的一个，base-class 是之前定义过的某个类的名称。如果未使用访问修饰符 access-specifier，则默认为 private。

假设有一个基类 **Shape**，**Rectangle** 是它的派生类，如下所示：

```c++
#include <iostream>
 
using namespace std;
 
// 基类
class Shape {
   public:
      void setWidth(int w){
         width = w;				//宽
      }
      void setHeight(int h){
         height = h;				//高
      }
   protected:
      int width;
      int height;
};
 
// 派生类
class Rectangle: public Shape{
   public:
      int getArea(){ 
         return (width * height); 
      }
};
 
int main(void){
   Rectangle Rect;
 
   Rect.setWidth(5);				//基类里的宽函数
   Rect.setHeight(7);				//基类里的高函数
 
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;		//自己的面积函数
 
   return 0;
}
```

### 访问控制和继承

派生类可以访问基类中所有的非私有成员。因此基类成员如果不想被派生类的成员函数访问，则应在基类中声明为 private。

我们可以根据访问权限总结出不同的访问类型，如下所示：

| 访问     | public | protected | private |
| -------- | ------ | --------- | ------- |
| 同一个类 | yes    | yes       | yes     |
| 派生类   | yes    | yes       | no      |
| 外部的类 | yes    | no        | no      |

一个派生类继承了所有的基类方法，但下列情况除外：

- 基类的构造函数、析构函数和拷贝构造函数。
- 基类的重载运算符。
- 基类的友元函数。

### 继承类型

当一个类派生自基类，该基类可以被继承为 **public、protected** 或  **private** 几种类型。继承类型是通过上面讲解的访问修饰符 access-specifier 来指定的。

我们几乎不使用 **protected** 或  **private** 继承，==通常使用 **public** 继承==。当使用不同类型的继承时，遵循以下几个规则：

- **公有继承（public）：**当一个类派生自**公有**基类时，基类的**公有**成员也是派生类的**公有**成员，基类的**保护**成员也是派生类的**保护**成员，基类的**私有**成员不能直接被派生类访问，但是可以通过调用基类的**公有**和**保护**成员来访问。
- **保护继承（protected）：** 当一个类派生自**保护**基类时，基类的**公有**和**保护**成员将成为派生类的**保护**成员。
- **私有继承（private）：**当一个类派生自**私有**基类时，基类的**公有**和**保护**成员将成为派生类的**私有**成员。

### 多继承

多继承即一个子类可以有多个父类，它继承了多个父类的特性。

C++ 类可以从多个类继承成员，语法如下：

```c++
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
<派生类类体>
};
```

其中，访问修饰符继承方式是 **public、protected** 或 **private** 其中的一个，用来修饰每个基类，各个基类之间用逗号分隔 。

```c++
#include <iostream>
 
using namespace std;
 
// 基类 Shape
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 基类 PaintCost
class PaintCost 
{
   public:
      int getCost(int area)
      {
         return area * 70;
      }
};
 
// 派生类
class Rectangle: public Shape, public PaintCost
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
   int area;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   area = Rect.getArea();
   
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   // 输出总花费
   cout << "Total paint cost: $" << Rect.getCost(area) << endl;
 
   return 0;
}

```

### 多继承(环状继承),A->D, B->D, C->(A，B)

例如：

```c++
class D{......};
class B: public D{......};
class A: public D{......};
class C: public B, public A{.....};
```

这个继承会使D创建两个对象,要解决上面问题就要用虚拟继承格式

格式：class 类名: virtual 继承方式 父类名

```c++
class D{......};
class B: virtual public D{......};
class A: virtual public D{......};
class C: public B, public A{.....};
```

虚继承--（在创建对象的时候会创建一个虚表）在创建父类对象的时候

```c++
A:virtual public D
B:virtual public D
```

**实例：**

```c++
#include <iostream>

using namespace std;
//基类

class D
{
public:
    D(){cout<<"D()"<<endl;}
    ~D(){cout<<"~D()"<<endl;}
protected:
    int d;
};

class B:virtual public D
{
public:
    B(){cout<<"B()"<<endl;}
    ~B(){cout<<"~B()"<<endl;}
protected:
    int b;
};

class A:virtual public D
{
public:
    A(){cout<<"A()"<<endl;}
    ~A(){cout<<"~A()"<<endl;}
protected:
    int a;
};

class C:public B, public A
{
public:
    C(){cout<<"C()"<<endl;}
    ~C(){cout<<"~C()"<<endl;}
protected:
    int c;
};

int main()
{
    cout << "Hello World!" << endl;
    C c;   //D, B, A ,C
    cout<<sizeof(c)<<endl;
    return 0;
}
```

- 1、与类同名的函数是构造函数。
- 2、**~ 类名**的是类的析构函数。

