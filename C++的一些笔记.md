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
* 



