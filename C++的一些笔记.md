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



# 类成员函数

类的成员函数是指那些把定义和原型写在类定义内部的函数，就像类定义中的其他变量一样。类成员函数是类的一个成员，它可以操作类的任意对象，可以访问对象中的所有成员。

让我们看看之前定义的类 Box，现在我们要使用成员函数来访问类的成员，而不是直接访问这些类的成员：

```c++
class Box
{
   public:
      double length;         // 长度
      double breadth;        // 宽度
      double height;         // 高度
      double getVolume(void);// 返回体积
};
```

成员函数可以定义在类定义内部，或者单独使用**范围解析运算符 ::** 来定义。在类定义中定义的成员函数把函数声明为**内联**的，即便没有使用 inline 标识符。所以您可以按照如下方式定义 **Volume()** 函数：

 

```c++
class Box
{
   public:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
   
      double getVolume(void)
      {
         return length * breadth * height;
      }
};
```

您也可以在类的外部使用**范围解析运算符 ::** 定义该函数，如下所示：

```c++
double Box::getVolume(void)
{
    return length * breadth * height;
}
```

在这里，需要强调一点，在 :: 运算符之前必须使用类名。调用成员函数是在对象上使用点运算符（**.**），这样它就能操作与该对象相关的数据，如下所示：

```c++
Box myBox;          // 创建一个对象
 
myBox.getVolume();  // 调用该对象的成
```

## 域区分符

:: 叫作用域区分符，指明一个函数属于哪个类或一个数据属于哪个类。

:: 可以不跟类名，表示全局数据或全局函数（即非成员函数）。

```c++
int month;//全局变量
int day;
int year;
void Set(int m,int d,int y)
{
    ::year=y; //给全局变量赋值，此处可省略
    ::day=d;
    ::month=m;
}

Class Tdate
{
    public:
        void Set(int m,int d,int y) //成员函数
        {
            ::Set(m,d,y); //非成员函数
        }
    private:
        int month;
        int day;
        int year;
}
```

#   内联函数

C++ **内联函数**是通常与类一起使用。

如果一个函数是内联的，那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方。

对内联函数进行任何修改，都需要重新编译函数的所有客户端，因为编译器需要重新更换一次所有的代码，否则将会继续使用旧的函数。

如果想把一个函数定义为内联函数，则需要在函数名前面放置关键字 **inline**，在调用函数之前需要对函数进行定义。如果已定义的函数多于一行，编译器会忽略 inline 限定符。

在==类定义中的定义的函数都是内联函数==，即使没有使用 **inline** 说明符。

下面是一个实例，使用内联函数来返回两个数中的最大值：

```c++
#include <iostream>
 
using namespace std;

inline int Max(int x, int y)
{
   return (x > y)? x : y;
}

// 程序的主函数
int main( )
{

   cout << "Max (20,10): " << Max(20,10) << endl;
   cout << "Max (0,200): " << Max(0,200) << endl;
   cout << "Max (100,1010): " << Max(100,1010) << endl;
   return 0;
}
```

引入内联函数的目的是为了解决程序中函数调用的效率问题，这么说吧，程序在编译器编译的时候，编译器将程序中出现的内联函数的调用表达式用内联函数的函数体进行替换，而对于其他的函数，都是在运行时候才被替代。这其实就是个空间代价换时间的节省。所以内联函数一般都是1-5行的小函数。在使用内联函数时要留神：

- 1.在内联函数内不允许使用循环语句和开关语句；
- 2.内联函数的定义必须出现在内联函数第一次调用之前；
- 3.类结构中所在的类说明内部定义的函数是内联函数。

有些函数即使声明为内联的也不一定会被编译器内联, 这点很重要; 比如虚函数和递归函数就不会被正常内联. 通常, 递归函数不应该声明成内联函数.(递归调用堆栈的展开并不像循环那么简单,  比如递归层数在编译时可能是未知的, 大多数编译器都不支持内联递归函数). 虚函数内联的主要原因则是想把它的函数体放在类定义内, 为了图个方便,  抑或是当作文档描述其行为, 比如精短的存取函数.

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

# 重载运算符和重载函数

C++ 允许在同一作用域中的某个**函数**和**运算符**指定多个定义，分别称为**函数重载**和**运算符重载**。

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的==参数列表和定义（实现）不相同==。

当您调用一个**重载函数**或**重载运算符**时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为**重载决策**。

## 函数重载

在同一个作用域内，可以声明几个功能类似的同名函数，但是这些同名函数的==形式参数（指参数的个数、类型或者顺序）必须不同==。

==**不能仅通过返回类型的不同来重载函数！**==

下面的实例中，同名函数 **print()** 被用于输出不同的数据类型：

```c++
#include <iostream>
using namespace std;
 
class printData
{
   public:
      void print(int i) {
        cout << "整数为: " << i << endl;
      }
 
      void print(double  f) {
        cout << "浮点数为: " << f << endl;
      }
 
      void print(char c[]) {
        cout << "字符串为: " << c << endl;
      }
};
 
int main(void)
{
   printData pd;
 
   // 输出整数
   pd.print(5);
   // 输出浮点数
   pd.print(500.263);
   // 输出字符串
   char c[] = "Hello C++";
   pd.print(c);
 
   return 0;
}

```

## 运算符重载

您可以重定义或重载大部分 C++ 内置的运算符。这样，您就能使用自定义类型的运算符。

重载的运算符是带有特殊名称的函数，函数名是由关键字 operator 和其后要重载的运算符符号构成的。与其他函数一样，重载运算符有一个返回类型和一个参数列表。

```c++
Box operator+(const Box&);
```

声明加法运算符用于把两个 Box 对象相加，返回最终的 Box 对象。大多数的重载运算符可被定义为普通的非成员函数或者被定义为类成员函数。如果我们定义上面的函数为类的非成员函数，那么我们需要为每次操作传递两个参数，如下所示：

```c++
Box operator+(const Box&, const Box&);
```

下面对象作为参数进行传递，对象的属性使用 **this** 运算符进行访问，如下所示：

```c++
#include <iostream>
using namespace std;
 
class Box
{
   public:
 
      double getVolume(void)//计算体积
      {
         return length * breadth * height;
      }
      void setLength( double len )
      {
          length = len;
      }
 
      void setBreadth( double bre )
      {
          breadth = bre;
      }
 
      void setHeight( double hei )
      {
          height = hei;
      }
      // 重载 + 运算符，用于把两个 Box 对象相加
      Box operator+(const Box& b)
      {
         Box box;
         box.length = this->length + b.length;
         box.breadth = this->breadth + b.breadth;
         box.height = this->height + b.height;
         return box;
      }
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   Box Box3;                // 声明 Box3，类型为 Box
   double volume = 0.0;     // 把体积存储在该变量中
 
   // Box1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
 
   // Box2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
 
   // Box1 的体积
   volume = Box1.getVolume();
   cout << "Volume of Box1 : " << volume <<endl;
 
   // Box2 的体积
   volume = Box2.getVolume();
   cout << "Volume of Box2 : " << volume <<endl;
 
   // 把两个对象相加，得到 Box3
   Box3 = Box1 + Box2;
 
   // Box3 的体积
   volume = Box3.getVolume();
   cout << "Volume of Box3 : " << volume <<endl;
 
   return 0;
}

```

> **this 指针的作用**
>
> this 指针是一个隐含于每一个非静态成员函数中的特殊指针。它指向正在被该成员函数操作的那个对象。当对一个对象调用成员函数时，编译器先将对象的地址赋给 this 指针，然后调用成员函数，每次成员函数存取数据成员时由隐含使用 this 指针。

### 可重载运算符

 可重载的运算符列表：

| 双目算术运算符 | + (加)，-(减)，*(乘)，/(除)，% (取模)                        |
| -------------- | ------------------------------------------------------------ |
| 关系运算符     | ==(等于)，!= (不等于)，< (小于)，> (大于>，<=(小于等于)，>=(大于等于) |
| 逻辑运算符     | \|\|(逻辑或)，&&(逻辑与)，!(逻辑非)                          |
| 单目运算符     | + (正)，-(负)，*(指针)，&(取地址)                            |
| 自增自减运算符 | ++(自增)，--(自减)                                           |
| 位运算符       | \| (按位或)，& (按位与)，~(按位取反)，^(按位异或),，<< (左移)，>>(右移) |
| 赋值运算符     | =, +=, -=, *=, /= , % = , &=, \|=, ^=, <<=, >>=              |
| 空间申请与释放 | new, delete, new[ ] , delete[]                               |
| 其他运算符     | ()(函数调用)，->(成员访问)，,(逗号)，[](下标)                |

### 不可重载的运算符列表

- 成员访问运算符：.
- 成员指针访问运算符：.    *,   ->*
- 域运算符：::
- 长度运算符: sizeof
- 条件运算符：?   :
- 预处理符号:   \# 

### 注意:

- 1、运算重载符不可以改变语法结构。
- 2、运算重载符不可以改变操作数的个数。
- 3、运算重载符不可以改变优先级。
- 4、运算重载符不可以改变结合性。

### 类重载、覆盖、重定义之间的区别：

重载指的是函数具有的不同的参数列表，而函数名相同的函数。重载要求参数列表必须不同，比如参数的类型不同、参数的个数不同、参数的顺序不同。如果仅仅是函数的返回值不同是没办法重载的，因为重载要求参数列表必须不同。==（发生在同一个类里）==

 覆盖是存在类中，子类重写从基类继承过来的函数。被重写的函数不能是static的。必须是virtual的。但是函数名、返回值、参数列表都必须和基类相同==（发生在基类和子类）==

 重定义也叫做隐藏，子类重新定义父类中有相同名称的非虚函数 ( 参数列表可以不同 ) 。==（发生在基类和子类）==

# 引用

引用变量是一个别名， 它是某个==已存在变量==的另一个名字。

一旦把引用初始化为某个变量，就可以使用该引用名称或变量名称来指向变量。

## 引用 vs 指针

引用很容易与指针混淆，它们之间有三个主要的不同：

- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化。

引用从一而终

## 创建引用

试想变量名称是变量附属在内存位置中的标签，您可以把引用当成是变量附属在内存位置中的第二个标签。因此，您可以通过原始变量名称或引用来访问变量的内容。例如：

```
int i = 17;
```

我们可以为 i 声明引用变量，如下所示：

```
int&  r = i;
double& s = d;
```

在这些声明中，& 读作**引用**。因此，第一个声明可以读作 "r 是一个初始化为 i 的整型引用"，第二个声明可以读作 "s 是一个初始化为 d 的 double 型引用"。下面的实例使用了 int 和 double 引用：

```c++
#include <iostream>
 
using namespace std;
 
int main ()
{
   // 声明简单的变量
   int    i;
   double d;
 
   // 声明引用变量
   int&    r = i;
   double& s = d;
   
   i = 5;
   cout << "Value of i : " << i << endl;
   cout << "Value of i reference : " << r  << endl;
 
   d = 11.7;
   cout << "Value of d : " << d << endl;
   cout << "Value of d reference : " << s  << endl;
   
   return 0;
}

```

引用更接近const指针，必须在创建时进行初始化，一旦引用和某个变量关联起来，该引用就会一直指向该变量。

```c++
int rats = 10;
int &rodents = rats;
```

上面代码实际上是下述代码的伪装表示：

```c++
int rats = 10;
int * const pr = &rats;
```

## 引用用法

### **引用作为函数参数**

C++之所以增加引用类型， 主要是把它作为函数参数，以扩充函数传递数据的功能。

> **C++ 函数传参：**
>
> (1)将变量名作为实参和形参。这时传给形参的是变量的值，传递是单向的。如果在执行函数期间形参的值发生变化，并不传回给实参。因为在调用函数时，形参和实参不是同一个存储单元。// 同 c
>
> (2) 传递变量的指针。形参是指针变量，实参是一个变量的地址，调用函数时，形参(指针变量)指向实参变量单元。这种通过形参指针可以改变实参的值。// 同 c
>
> (3) C++提供了  传递变量的引用。形参是引用变量，和实参是一个变量，调用函数时，形参(引用变量)指向实参变量单元。这种通过形参引用可以改变实参的值。

```c++
#include <iostream>
using namespace std;
 
// 函数声明
void swap(int& x, int& y);
 
int main ()
{
   // 局部变量声明
   int a = 100;
   int b = 200;
 
   cout << "交换前，a 的值：" << a << endl;
   cout << "交换前，b 的值：" << b << endl;
 
   /* 调用函数来交换值 */
   swap(a, b);
 
   cout << "交换后，a 的值：" << a << endl;
   cout << "交换后，b 的值：" << b << endl;
 
   return 0;
}
 
// 函数定义
void swap(int& x, int& y)
{
   int temp;
   temp = x; /* 保存地址 x 的值 */
   x = y;    /* 把 y 赋值给 x */
   y = temp; /* 把 x 赋值给 y  */
  
   return;
}
```

有这么一个结构体：

```c++
struct Demo 
{
  char name[10000];
  int  count;
};

int main()
{
  Demo my_demo = {0};
  Demo *my_demo_p = &my_demo;
  Demo &my_demo_ref = my_demo;
  
  // 方式1：传递实参  
  func1(my_demo);

  // 方式2：传递指针  
  func2(my_demo_p);

  // 方式3：传递引用  
  func3(my_demo_ref);

  return 0;
}
如上所示，参数传递可以有三种方式，则三种函数可以这样定义：

// 方式1：传递实参 
void func1(Demo demo)
{
  cout << demo.name;
}

// 方式2：传递指针  
void func2(const Demo *demo_p)
{
  cout << demo->name;
}

// 方式3：传递引用  
void func3(const Demo &demo_ref)
{
  cout << demo.name;
}
```

可以看出，引用和实参的使用方式很像，但注意以下几点：

- 从内存使用的角度来说，传递实参，则会将数据拷贝过去（创建了副本），即 func1 对传入的数据做任何修改，都不会影响 my_demo。
- 既然传递实参要创建副本，就意味着效率更低。例子中结构体的 name 成员长度为 10000，就是要突出这个效率问题。

引用和指针也很像，它们都不会创建副本，因此效率都很高。它们的主要区别在于：

- 选择成员的时候，引用使用点 . 来查找，而指针则使用 -> 来查找。
- 指针可能传递一个 NULL 过来，因此在使用前必须检查有效性；引用则必然代表某个对象，不需要做此检查。

**PS:** 在代码中加上 const 关键字的目的，是不希望 func2 和 func3 去修改原始数据，因为函数仅仅是为了打印出 name。

### 引用作为返回值

（1）以引用返回函数值，定义函数时需要在函数名前加 &

（2）用引用返回一个函数值的最大好处是，在内存中不产生被返回值的副本。

**引用作为返回值，必须遵守以下规则：**

- （1）不能返回局部变量的引用。主要原因是局部变量会在函数返回后被销毁，因此被返回的引用就成为了"无所指"的引用，程序会进入未知状态。
- （2）不能返回函数内部new分配的内存的引用。虽然不存在局部变量的被动销毁问题，可对于这种情况（返回函数内部new分配内存的引用），又面临其它尴尬局面。例如，被函数返回的引用只是作为一 个临时变量出现，而没有被赋予一个实际的变量，那么这个引用所指向的空间（由new分配）就无法释放，造成memory leak。
- （3）可以返回类成员的引用，但最好是const。主要原因是当对象的属性是与某种业务规则（business  rule）相关联的时候，其赋值常常与某些其它属性或者对象的状态有关，因此有必要将赋值操作封装在一个业务规则当中。如果其它对象可以获得该属性的非常 量引用（或指针），那么对该属性的单纯赋值就会破坏业务规则的完整性。 

通过使用引用来替代指针，会使 C++ 程序更容易阅读和维护。C++ 函数可以返回一个引用，方式与返回一个指针类似。

当函数返回一个引用时，则返回一个指向返回值的隐式指针。这样，函数就可以放在赋值语句的左边。

```c++
#include <iostream>
 
using namespace std;
 
double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};
 
double& setValues( int i )
{
  return vals[i];   // 返回第 i 个元素的引用
}
 
// 要调用上面定义函数的主函数
int main ()
{
 
   cout << "改变前的值" << endl;
   for ( int i = 0; i < 5; i++ )
   {
       cout << "vals[" << i << "] = ";
       cout << vals[i] << endl;
   }
 
   setValues(1) = 20.23; // 改变第 2 个元素
   setValues(3) = 70.8;  // 改变第 4 个元素
 
   cout << "改变后的值" << endl;
   for ( int i = 0; i < 5; i++ )
   {
       cout << "vals[" << i << "] = ";
       cout << vals[i] << endl;
   }
   return 0;
}

```

一个变量被返回其引用的意思是，给了返回变量的一个别名，如上面的例子显示函数给 val[i] 了一个别名。当调用这个函数 setValues(3) 时，返回了这个 val[3] 的别名， 那么我们就可以修改 val[3]，所以可以放到等号的左边。

当返回一个引用时，要注意被引用的对象不能超出作用域。所以返回一个对局部变量的引用是不合法的，但是，可以返回一个对静态变量的引用。

```c++
int& func() {
   int q;
   //! return q; // 在编译时发生错误
   static int x;
   return x;     // 安全，x 在函数作用域外依然是有效的
}
```

# 多态

**多态**按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。

多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数；

形成多态必须具备三个条件：

1、必须存在继承关系；

2、继承关系必须有同名虚函数（**其中虚函数是在基类中使用关键字Virtual声明的函数，在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数**）；

3、存在基类类型的指针或者引用，通过该指针或引用调用虚函数；

下面的实例中，基类 Shape 被派生为两个类，

```c++

#include <iostream> 
using namespace std;
 

class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      virtual int area()
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
};
class Rectangle: public Shape{
   public:
      Rectangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Rectangle class area :" <<endl;
         return (width * height); 
      }
};
class Triangle: public Shape{
   public:
      Triangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Triangle class area :" <<endl;
         return (width * height / 2); 
      }
};
// 程序的主函数
int main( )
{
   Shape *shape;
   Rectangle rec(10,7);
   Triangle  tri(10,5);
 
   // 存储矩形的地址
   shape = &rec;
   // 调用矩形的求面积函数 area
   shape->area();
 
   // 存储三角形的地址
   shape = &tri;
   // 调用三角形的求面积函数 area
   shape->area();
   
   return 0;
}

```

每个子类都有一个函数 area() 的独立实现。这就是**多态**的一般使用方式。有了多态，您可以有多个不同的类，都带有同一个名称但具有不同实现的函数，函数的参数甚至可以是相同的。

## 多态的实现放法

### 虚函数

**虚函数** 是在基类中使用关键字 **virtual** 声明的函数。在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。

我们想要的是在程序中任意点可以根据所调用的对象类型来选择调用的函数，这种操作被称为**动态链接**，或**后期绑定**。

一个类一旦有了虚函数他就会大一点点（在内存中头部回出现一个指针 VPTER，他指向一个地址 ——虚函数表）

### 纯虚函数

您可能想要在基类中定义虚函数，以便在派生类中重新定义该函数更好地适用于对象，但是您在基类中又不能对虚函数给出有意义的实现，这个时候就会用到纯虚函数。

我们可以把基类中的虚函数 area() 改写如下：

```c++
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      // pure virtual function
      virtual int area() = 0;
};
```

= 0 告诉编译器，函数没有主体，上面的虚函数是**纯虚函数**。

1、纯虚函数声明如下： virtual void funtion1()=0;  纯虚函数一定没有定义，纯虚函数用来规范派生类的行为，即接口。包含纯虚函数的类是抽象类，抽象类不能定义实例，但可以声明指向实现该抽象类的具体类的指针或引用。

2、虚函数声明如下：virtual ReturnType FunctionName(Parameter) 虚函数必须实现，如果不实现，编译器将报错，错误提示为：

```c++
error LNK****: unresolved external symbol "public: virtual void __thiscall ClassName::virtualFunctionName(void)"
```

3、对于虚函数来说，父类和子类都有各自的版本。由多态方式调用的时候动态绑定。

4、实现了纯虚函数的子类，该纯虚函数在子类中就编程了虚函数，子类的子类即孙子类可以覆盖该虚函数，由多态方式调用的时候动态绑定。

5、虚函数是C++中用于实现多态(polymorphism)的机制。核心理念就是通过基类访问派生类定义的函数。

6、在有动态分配堆上内存的时候，析构函数必须是虚函数，但没有必要是纯虚的。

7、友元不是成员函数，只有成员函数才可以是虚拟的，因此友元不能是虚拟函数。但可以通过让友元函数调用虚拟成员函数来解决友元的虚拟问题。

8、析构函数应当是虚函数，将调用相应对象类型的析构函数，因此，如果指针指向的是子类对象，将调用子类的析构函数，然后自动调用基类的析构函数。



# 拷贝构造函数

 **拷贝构造函数**是一种特殊的构造函数，它在创建对象时，是使用同一类中之前创建的对象来初始化新创建的对象。拷贝构造函数通常用于：

- 通过使用另一个同类型的对象来==初始化==新创建的对象。
- 复制对象把它作为参数传递给函数。
- 复制对象，并从函数返回这个对象。

如果在类中没有定义拷贝构造函数，编译器会自行定义一个。如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数。

拷贝构造函数的最常见形式如下：

```c++
classname (const classname &obj) {
   // 构造函数的主体
}
// 在这里，obj 是一个对象引用，该对象是用于初始化另一个对象的。
```

**关于为什么当类成员中含有指针类型成员且需要对其分配内存时，一定要有总定义拷贝构造函数？？**

默认的拷贝构造函数实现的只能是浅拷贝，即直接将原对象的数据成员值依次复制给新对象中对应的数据成员，并没有为新对象另外分配内存资源。

这样，如果对象的数据成员是指针，两个指针对象实际上指向的是==同一块内存空间==。

在某些情况下，浅拷贝回带来数据安全方面的隐患。

当类的数据成员中有指针类型时，我们就必须定义一个特定的拷贝构造函数，该拷贝构造函数不仅可以实现原对象和新对象之间数据成员的拷贝，而且可以为新的对象分配单独的内存资源，这就是深拷贝构造函数。

**如何防止默认拷贝发生**

声明一个==私有的拷贝构造函数==，这样因为拷贝构造函数是私有的，如果用户试图按值传递或函数返回该类的对象，编译器会报告错误，从而可以避免按值传递或返回对象。

**总结：**

当出现类的等号赋值时，会调用拷贝函数，在未定义显示拷贝构造函数的情况下，系统会调用默认的拷贝函数——即浅拷贝，它能够完成成员的一一复制。当数据成员中没有指针时，浅拷贝是可行的。但当数据成员中有指针时，如果采用简单的浅拷贝，则两类中的两个指针将指向同一个地址，当对象快结束时，会调用两次析构函数，而导致指针悬挂现象。所以，这时，必须采用深拷贝。

深拷贝与浅拷贝的区别就在于深拷贝会在堆内存中另外申请空间来储存数据，从而也就解决了指针悬挂的问题。简而言之，当数据成员中有指针时，必须要用深拷贝。

拷贝构造函数

**几个原则：**

C++ primer p406  ：拷贝构造函数是一种特殊的构造函数，具有单个形参，该形参（常用const修饰）是对该类类型的引用。当定义一个新对象并用一个同类型的对象对它进行初始化时，将显示使用拷贝构造函数。当该类型的对象传递给函数或从函数返回该类型的对象时，将隐式调用拷贝构造函数。

**C++支持两种初始化形式：**

拷贝初始化 int a = 5; 和直接初始化 int  a(5); 对于其他类型没有什么区别，对于类类型直接初始化直接调用实参匹配的构造函数，拷贝初始化总是调用拷贝构造函数，也就是说：

```c++
A x(2);　　//直接初始化，调用构造函数
A y = x;　　//拷贝初始化，调用拷贝构造函数
```

**必须定义拷贝构造函数的情况：**

只包含类类型成员或内置类型（但不是指针类型）成员的类，无须显式地定义拷贝构造函数也可以拷贝；有的类有一个数据成员是指针，或者是有成员表示在构造函数中分配的其他资源，这两种情况下都必须定义拷贝构造函数。

**什么情况使用拷贝构造函数：**

类的对象需要拷贝时，拷贝构造函数将会被调用。以下情况都会调用拷贝构造函数：

- （1）一个对象以值传递的方式传入函数体 
- （2）一个对象以值传递的方式从函数返回 
- （3）一个对象需要通过另外一个对象进行初

```c++
include <iostream>
 
using namespace std;
 
class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
 
   private:
      int *ptr;
};
 
// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
 
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
 
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}
 
void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}
 
// 程序的主函数
int main( )
{
   Line line(10);
 
   display(line);
 
   return 0;
}
```

使用已有的同类型的对象来初始化新创建的对象：

```c++

#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
 
   private:
      int *ptr;
};
 
// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
 
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
 
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}
 
void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}
 
// 程序的主函数
int main( )
{
   Line line1(10);
 
   Line line2 = line1; // 这里也调用了拷贝构造函数
 
   display(line1);
   display(line2);
 
   return 0;
}

```

==以后写了类，三个函数就要写全==  



# C++ 数据抽象

数据抽象是指，只向外界提供关键信息，并隐藏其后台的实现细节，即只表现必要的信息而不呈现细节。

数据抽象是一种依赖于接口和实现分离的编程（设计）技术。

让我们举一个现实生活中的真实例子，比如一台电视机，您可以打开和关闭、切换频道、调整音量、添加外部组件（如喇叭、录像机、DVD 播放器），但是您不知道它的内部实现细节，也就是说，您并不知道它是如何通过缆线接收信号，如何转换信号，并最终显示在屏幕上。

因此，我们可以说电视把它的内部实现和外部接口分离开了，您无需知道它的内部实现原理，直接通过它的外部接口（比如电源按钮、遥控器、声量控制器）就可以操控电视。

现在，让我们言归正传，就 C++ 编程而言，C++ 类为**数据抽象**提供了可能。它们向外界提供了大量用于操作对象数据的公共方法，也就是说，外界实际上并不清楚类的内部实现。

例如，您的程序可以调用 **sort()** 函数，而不需要知道函数中排序数据所用到的算法。实际上，函数排序的底层实现会因库的版本不同而有所差异，只要接口不变，函数调用就可以照常工作。

在 C++ 中，我们使用**类**来定义我们自己的抽象数据类型（ADT）。您可以使用类 **iostream** 的 **cout** 对象来输出数据到标准输出，如下所示：

## 实例

```c++
#include <iostream> 
using namespace std; 
int main( ) {  
  cout << "Hello C++" <<endl;  
  return 0;
}
```

在这里，您不需要理解 **cout** 是如何在用户的屏幕上显示文本。您只需要知道公共接口即可，cout 的底层实现可以自由改变。

## 访问标签强制抽象

在 C++ 中，我们使用访问标签来定义类的抽象接口。一个类可以包含零个或多个访问标签：

- 使用公共标签定义的成员都可以访问该程序的所有部分。一个类型的数据抽象视图是由它的公共成员来定义的。
- 使用私有标签定义的成员无法访问到使用类的代码。私有部分对使用类型的代码隐藏了实现细节。

访问标签出现的频率没有限制。每个访问标签指定了紧随其后的成员定义的访问级别。指定的访问级别会一直有效，直到遇到下一个访问标签或者遇到类主体的关闭右括号为止。

## 数据抽象的好处

数据抽象有两个重要的优势：

- 类的内部受到保护，不会因无意的用户级错误导致对象状态受损。
- 类实现可能随着时间的推移而发生变化，以便应对不断变化的需求，或者应对那些要求不改变用户级代码的错误报告。

如果只在类的私有部分定义数据成员，编写该类的作者就可以随意更改数据。如果实现发生改变，则只需要检查类的代码，看看这个改变会导致哪些影响。如果数据是公有的，则任何直接访问旧表示形式的数据成员的函数都可能受到影响。

## 数据抽象的实例

C++ 程序中，任何带有公有和私有成员的类都可以作为数据抽象的实例。请看下面的实例：

```c++
\#include <iostream>
  using namespace std;  
class Adder{   
  public:      // 构造函数      
    	Adder(int i = 0){       
        total = i;
      }      // 对外的接口   
      void addNum(int number){          
        total += number;      
      }      // 对外的接口      
      int getTotal(){          
        return total;      
      };  
  private:      // 对外隐藏的数据     
  int total; 
}; 

	int main( ) {  
    Adder a;      
    a.addNum(10);  
    a.addNum(20);  
    a.addNum(30);   
    cout << "Total " << a.getTotal() <<endl;   return 0;
  }
```

当上面的代码被编译和执行时，它会产生下列结果：

```
Total 60
```

上面的类把数字相加，并返回总和。公有成员 **addNum** 和 **getTotal** 是对外的接口，用户需要知道它们以便使用类。私有成员 **total** 是用户不需要了解的，但又是类能正常工作所必需的。

## 设计策略

抽象把代码分离为接口和实现。所以在设计组件时，必须保持接口独立于实现，这样，如果改变底层实现，接口也将保持不变。

在这种情况下，不管任何程序使用接口，接口都不会受到影响，只需要将最新的实现重新编译即可。

#   数据封装

所有的 C++ 程序都有以下两个基本要素： 

- **程序语句（代码）：**这是程序中执行动作的部分，它们被称为函数。
- **程序数据：**数据是程序的信息，会受到程序函数的影响。

封装是面向对象编程中的把数据和操作数据的函数绑定在一起的一个概念，这样能避免受到外界的干扰和误用，从而确保了安全。数据封装引申出了另一个重要的 OOP 概念，即**数据隐藏**。

**数据封装**是一种把数据和操作数据的函数捆绑在一起的机制，**数据抽象**是一种仅向用户暴露接口而把具体的实现细节隐藏起来的机制。

C++ 通过创建**类**来支持封装和数据隐藏（public、protected、private）。我们已经知道，类包含私有成员（private）、保护成员（protected）和公有成员（public）成员。默认情况下，在类中定义的所有项目都是私有的。例如：

```c++
class Box {   
  public:      
  double getVolume(void)      
  {       
    return length * breadth * height;     
  }   
  private:     
  double length;      // 长度     
  double breadth;     // 宽度     
  double height;      // 高度 };


```

变量 length、breadth 和 height 都是私有的（private）。这意味着它们只能被 Box 类中的其他成员访问，而不能被程序中其他部分访问。这是实现封装的一种方式。

为了使类中的成员变成公有的（即，程序中的其他部分也能访问），必须在这些成员前使用 **public** 关键字进行声明。所有定义在 public 标识符后边的变量或函数可以被程序中所有其他的函数访问。

把一个类定义为另一个类的友元类，会暴露实现细节，从而降低了封装性。理想的做法是尽可能地对外隐藏每个类的实现细节。

## 数据封装的实例

C++ 程序中，任何带有公有和私有成员的类都可以作为数据封装和数据抽象的实例。请看下面的实例：

## 实例

```c++
\#include <iostream>
  using namespace std;  
class Adder{   
  public:      // 构造函数      
  Adder(int i = 0)     
  {       
    total = i;      
  }      // 对外的接口     
  void addNum(int number)    
  {         
    total += number;   
  }      // 对外的接口    
  int getTotal()    
  {     
    return total; 
  };   
  private:      // 对外隐藏的数据     
  int total; 
}; 
int main( ) {  
  Adder a;     
  a.addNum(10);   
  a.addNum(20);
  a.addNum(30);   
  cout << "Total " << a.getTotal() <<endl; 
  return 0; 
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```c++
Total 60
```

上面的类把数字相加，并返回总和。公有成员 **addNum** 和 **getTotal** 是对外的接口，用户需要知道它们以便使用类。私有成员 **total** 是对外隐藏的，用户不需要了解它，但它又是类能正常工作所必需的。

## 设计策略

通常情况下，我们都会设置类成员状态为私有（private），除非我们真的需要将其暴露，这样才能保证良好的**封装性**。

这通常应用于数据成员，但它同样适用于所有成员，包括虚函数。

# 接口（抽象类）

接口描述了类的行为和功能，而不需要完成类的特定实现。

C++ 接口是使用**抽象类**来实现的，抽象类与数据抽象互不混淆，数据抽象是一个把实现细节与相关的数据分离开的概念。

如果类中至少有一个函数被声明为纯虚函数，则这个类就是抽象类。纯虚函数是通过在声明中使用 "= 0" 来指定的，如下所示：

```c++
class Box {   
  public:      // 纯虚函数     
  virtual double getVolume() = 0;  
  private:     
  double length;      // 长度   
  double breadth;     // 宽度    
  double height;      // 高度 
};
```

设计**抽象类**（通常称为 ABC）的目的，是为了给其他类提供一个可以继承的适当的基类。抽象类不能被用于实例化对象，它只能作为**接口**使用。如果试图实例化一个抽象类的对象，会导致编译错误。

因此，如果一个 ABC 的子类需要被实例化，则必须实现每个虚函数，这也意味着 C++ 支持使用 ABC 声明接口。如果没有在派生类中重写纯虚函数，就尝试实例化该类的对象，会导致编译错误。

可用于实例化对象的类被称为**具体类**。

## 抽象类的实例

请看下面的实例，基类 Shape 提供了一个接口 **getArea()**，在两个派生类 Rectangle 和 Triangle 中分别实现了 **getArea()**：

```c++

#include <iostream>
 
using namespace std;
 
// 基类
class Shape 
{
public:
   // 提供接口框架的纯虚函数
   virtual int getArea() = 0;
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
 
// 派生类
class Rectangle: public Shape
{
public:
   int getArea()
   { 
      return (width * height); 
   }
};
class Triangle: public Shape
{
public:
   int getArea()
   { 
      return (width * height)/2; 
   }
};
 
int main(void)
{
   Rectangle Rect;
   Triangle  Tri;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
   // 输出对象的面积
   cout << "Total Rectangle area: " << Rect.getArea() << endl;
 
   Tri.setWidth(5);
   Tri.setHeight(7);
   // 输出对象的面积
   cout << "Total Triangle area: " << Tri.getArea() << endl; 
 
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
Total Rectangle area: 35
Total Triangle area: 17
```

从上面的实例中，我们可以看到一个抽象类是如何定义一个接口 getArea()，两个派生类是如何通过不同的计算面积的算法来实现这个相同的函数。

## 设计策略

面向对象的系统可能会使用一个抽象基类为所有的外部应用程序提供一个适当的、通用的、标准化的接口。然后，派生类通过继承抽象基类，就把所有类似的操作都继承下来。

外部应用程序提供的功能（即公有函数）在抽象基类中是以纯虚函数的形式存在的。这些纯虚函数在相应的派生类中被实现。

这个架构也使得新的应用程序可以很容易地被添加到系统中，即使是在系统被定义之后依然可以如此。