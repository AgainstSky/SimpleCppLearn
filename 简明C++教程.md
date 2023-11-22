# 简明C++教程

> 基于c++11标准和《Primer C++》一书



## 第一部分：C++基础



### 第一章 先来个hello world

```c++
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```







### 第二章 基本类型

​	c++提供了一些列的基本类型



#### C++支持的基本类型

| 类型        | 含义                            | 最小尺寸     |
| ----------- | ------------------------------- | ------------ |
| bool        | 布尔值（true，false）           | 为定义       |
| char        | 字符，用单引号框起来的          | 8            |
| wchar_t     | 宽字符                          | 16           |
| char_16t    | Unicode字符                     | 16           |
| char_32t    | Unicode字符                     | 32           |
| short       | 短整形                          | 16           |
| int         | 整形                            | 32           |
| long        | 长整形                          | 64           |
| long long   | 长整形                          | 64           |
| float       | 单精度浮点数，（3.14 这种小数） | 6位有效数字  |
| double      | 双精度浮点数                    | 10位有效数字 |
| long double | 扩展精度浮点数                  | 10位有效数字 |

关于上面整形的大小尺寸并不是统一的，但是他们至少存在如下关系

`long long >= long >= int >= short`

而浮点数的大小也不是统一的，大部分编译器实现的精度都比上表中展示的更高，且会受到具体平台硬件的影响

> 就我个人经验而言，当使用整形是要注意你代码的目标平台，使用浮点数做计算时要考虑你能接受的误差，特别是在做金额等计算时要更加小心，一般会选取其他更高精度的C++工具来完成。当然在我们初学者阶段可以暂时不管这些，只按照上表中的来使用就行（当然要是你使用的是16位，32位的老古董电脑的话请当我在放屁）

#### 有符号类型和无符号类型

​	上节中展示的类型里，除了`bool`，浮点数和`扩展的char类型`（`char_16t`，`char_32t`）外都支持带符号和无符号两种。默认情况下他们都是带符号的，要想使用他们的无符号版本只需在类型前加上  `unsigned `例如 `unsigned int`

​	这其中有个例外，就是字符类型`char`，不同于整形只分带符号和无符号两种，`char`会分为`char`,`signed char`,`unsigned char`

三种, 注意其中`char`,`signed char`, 是不同的，后者是带符号的，而前者具体是什么类型是由编译器决定，也就是说当你写一个`char`时，他最终可能是`signed char`的，也可能是`unsigned char`。这点在使用的过程中需要注意，感兴趣的可以自行深入探查里面的奥秘







#### 变量

##### 初始化

> 初始化不是赋值，初始化的含义是创建变量时赋予其一个初始值，而赋值的含义是把对象的当前值擦除，以一个新的值来替代。

1. = 初始化

   ``` c++
   int num = 0;
   ```

   使用 = 来初始化一个变量

2.列表初始化

```c++
int num = {0};
int b{0};
```

​	当用于内置类型的基本变量时列表初始化有一个特点：如果我们使用列表初始化且初始值存在丢失风险，编译器将报错。例如我门用一个double值来初始化int变量时`int a{3.14}` 编译器将会报错。

3.默认初始化

​	当定义变量时没有指定初始值，变量就会被默认初始化。如果是内置类型，它的值由定义它的位置决定。定义于任何函数体之外的变量被初始化为0，定义在函数体内部的将不被初始化。

​	其他类型的初始化将由每个类各自决定，而且是否允许不经初始化就定义对象也由类决定。

##### 声明和定义

​	为了支持分离式编译，C++将声明和定义区分开来，声明使得名字为程序所知，定义负责创建与名字关联的实体。

```c++
extern int i;//声明
int j;//定义
int m = 0;//定义并初始化
extern int n = 0; //定义并初始化

void test(){
  extern int k = 0;//错误，在函数内部试图初始化一个由extern关键字标记的变量
}
```

使用extern关键字且不给予初始化值来声明一个变量



#### 引用和指针

​	引用为对象齐了另外一个名字，引用类型引用另外一种类型。可以将引用理解为别名，相当于给某个变量起了一个别名，引用本身并不是一个对象，所以不能定义应用的引用

```c++
int ival =1024;//声明并定义了一个int类型
int &refVal = ival;//refVal是一个引用类型，他指向ival
int &refVal2; //报错，引用必须被初始化
int i = refVal;//此时i为1024，用refVal指向的ival的值来初始化i
int &refi = i;
refi = 2;//此时i为2.对refi赋值，其实是对refi指向的i进行赋值
int num = 3;
refVal = &num;//报错 引用无法被重新绑定到另外一个对象


int &refVal4 = 10;//错误，引用初始值必须是一个对象，而不能是字面值或者某个表达式的计算结果
double dval = 3.14;
int &refVal5= dval;//错误，引用的类型和绑定的对象的类型必须完全匹配
```

 	指针 是 *指向* 另外一种类型的复合类型。和引用类似，指针也实现了对其他对象的间接访问。与引用不同的是：指针本身也是一个对象，可以对其赋值和拷贝，也能重新指向其他对象，并且可以在定义时不赋初始值，和其他内置类型一样，如果在块作用域内定义且没被初始化，他将拥有一个不确定的值。

> 指针 其实是内部存储了它指向对象的地址

```c++
int i = 0;
int *pI = &i;
double *pN,*pM;
int &refI = i;
int *pRefI = &refI;//错误，引用不是对象，没有地址，指针存储的是地址，所以无法定义指向引用的指针

cout<<*pI;//输出0，利用解引用运算符 * 访问pI指向的对象i
*pI = 3;//i 的值等于3，利用解引用运算符 * 访问pI指向的对象i并且修改i的值
```

​	`void *` 指针是一种特殊的指针类型，可以存放任意对象的地址（也就是说可以指向任意对象）。他可以和别的指针做比较，作为函数的输入或者输出，赋值给另外一个 void* 指针，但是不能直接操作void* 指针所指的对象。

```c++
int i = 10;
int *p = &i;
int *&r= p; //定义一个绑定到指针的引用
*r = 0;
```

​	无法将一个指针指向一个引用，但是可以将一个引用绑定到指针。

> 当读到一个复杂的声明语句时，为了弄清楚它的真实含义，可以从右向走读



#### const常量

```c++
const double PI = 3.14;//定义一个常量
```

​	常量是一个一但定义就不允许改变其值的类型，因此常量在定义的时候必须进行初始化。一般而言，常量能支持大部分变量所支持的操作，只有当进行的操作会改变常量的值时才会被拒绝。

​	同时不同于变量，常量的可见性是在文件内的，也就是要在多个文件内访问上面定义的PI，则这多个文件都必须有PI的定义。如果想像变量一样在多个文件的访问，可以在定义常量时增加 `extern`

```c++
//file.cpp
extern const int M = 10;//定义了常量M

//file.h
extern const int M; //声明了常量M，并且和file.cpp中的M是同一个
```



#### const 的引用

> 很多人把对const的引用简称为 常量引用  。记住这种称法只是一个简称，引用并不是一个对象，所以它没有所谓的常量说法，但同时C++不允许引用重新绑定新的对象，这里面又含有一部分常量的意思。不过记住他只是一个简称。

​	一般而言，const的引用有如下几个特性：

- const 的引用可以绑定到非const对象，但是非const 引用不能绑定到const对象
- const的引用可以绑定到字面值，但是非const引用不能
- const的引用不允许通过引用修改绑定对象的值，无论绑定的对象是不是const的
- const的引用允许用任意表达式初始化，只要该表达式的结果能被转换成 引用 的类型

```c++
double ival = 0.01; //定义一个double对象
int &refIval = ival;//普通的int引用无法绑定到double对象上
const int &cRefIval = ival;//const int的引用可以绑定到double对象上（后面会有详细解释）
cRefIval = 3.14;//错误 不允许通过const 引用修改值
const int &r43 = 43;//允许直接用字面量对const引用初始化
int &r4 = r43 *2;//错误 r4不是 const 引用，无法用表达式初始化
    
const double PI = 3.14;
const double &cRefPI = PI;
double &refPI = PI; //错误 非const引用无法绑定到const对象
```

​	上面例子里有个特殊情况，就是const int 引用可以绑定到double对象上。其实`cRefIval`并不是绑定到真正的 `ival`上，他实际的效果类似于下面的代码

```c++
double ival = 0.01; 
const int temp = dval;
const int &cRefIval = temp;//cRefIval实际绑定到了一个temp上，这是一个编译器创建的临时量，我们无法直接访问这个temp，并且它的名字并不是temp，对程序员而言，这个临时量是未知的
```



#### const 和 指针

​	const和指针分两种情况：指向const 对象的指针，const的指针。

```c++
int num =10;
const int *p = &num;//指向const 对象的指针
int * const p = &num;//const的指针
```

​	和const 引用一样，指向const对象的指针也可以指向非 const对象，只不过不能通过该指针修改指向对象的值。

##### 指向const对象的指针

​	这代表着指针指向的对象是一个常量（或者说通过这个指针到底的对象的方式只能是const的，因为指向const对象的指针依然可以指向非常量），意味着你不能使用这个指针改变指向对象的值。但是指针本身并不是const的，所以指针可以重新指向新的对象

##### const的指针

​	和引用不同，指针本身就是一个对象，所以它本身可以是常量和非常量的，而const的指针是指这个指针本身就是const的，这与他指向的对象无关。和其他常量相同，const的指针本身的值不能修改（即不能使他重新指向其他对象），且必须初始化。



```c++
const int * const p;
```

上面代码是一个指向常量的常量指针，它指向的对象是常量，并且指针本身也是一个常量。这里简单介绍下遇到这种声明定义怎么去理解它的意思。首先我们从变量名开始从右向左读，首先我们得到一个const， 代表我们的p是一个常量，继续读得到一个指针，现在我们知道 p 是一个常量指针，接着读到一个int，我们可知 p 是一个指向int的常量指针，继续读到一个const，可知 p 是一个指向int常量的常量指针。



#### 顶层和底层const

​	从前文我们知道指针本身是一个对象，它又可以指向另外一个对象。因此指针本身是不是常量 和 指针所指的是不是一个常量 就是两个独立的问题。用名词 顶层const（top-level const） 表示指针本身是个常量，而用名称 底层const (low-level const) 表示指针所指的对象是常量。

​	更一般的，顶层const可以表示任意的对象是常量，对任何数据类型都适用。而底层cosnt则与指针和引用等复合类型的基本类型部分有关。指针可以即是顶层也是底层const的，引用只能是底层const。

​	 

#### constexpr 和常量表达式

​	常量表达式 是指不会改变并且在编译过程中就能得到计算结果的表达式，显然字面值属于常量表达式。用常量表达式初始化的const 对象也是常量表达式

​	constexpr 用来使编译器检查我们所定义的是否是一个合法的常量表达式

```c++
constexpr int mf = 20;
constexpr int limit = mf +1 ;
constexpr int sz = size();//只有到size是一个constexpr函数时才能正确通过编译
constexpr int *p = nullptr;// 正确 p是一个指向int的常量指针
constexpr int *p1=0x5566;// 错误
constexpr int *p2;//错误 
```



#### 类型别名 

```c++
typedef double wages;//wages是double的同义词，可以 wages pi = 3.14；
typedef wages base,*p;//base是double的同义词，p是double * 的同义词，可以 p m = &pi；

using wages = double;//等价于第一行
using p = double *;//等价于第二行对p的别名定义
```

​	当类型别名指代的是某个复杂类型的变量或者常量时，往往有出人意料的结果

```c++
    typedef char * pstr;
    const char AChar = 'a';
    char bChar = 'b';
    const char* ASTR = "abc";

    const pstr str = &bChar;//本质上等于 char * const str; 是一个指向char的常量指针
    char * const str2 = &bChar;//
    char * const str3 = str;//这三行对变量的定义本质上相同

    const char * str4 = &AChar;//和const pstr str定义并不相同
```

​	如上所示，如果单纯的想把 pstr 拆成 `char *`那对于str的定义理解就会是错误的，pstr是类型别名，要记住他本身就是一个别名。对于 str 的定义，我们从右向左读到pstr的时候就可以得到str是一个指向char的指针，再读到const时得到str本身是常量，所以他是一个指向char的常量指针。str是顶层const的。



#### auto

​	对于简单的变量，我们能很容易的写出它的定义，但对于复杂类型的变量，写出它的定义往往不是一件很简单的事情，为此可以使用 auto 类型说明符来让编译器帮助我们推断出它的定义。

```c++
using UnReadType = double * (int * (*)(int *,double *));
UnReadType * fun();//假设我们有这么一个函数，返回的是一个难以阅读的复合类型，且我们没有使用 UnReadType
auto m = fun(); // 此时要想正常写出m变量的定义是一件困难的事情，而我们可以使用auto让编译器推导出来

auto val =10;
auto num = 100,temp = 10;//也可以一个auto 定义两个变量，但是这两个变量的类型必须相同
auto err;//显然auto定义变量的时候必须初始化，否则编译器无从推导
auto sum = val+num;//显然我们也可以这么用auto

int &refVal = val;
auto valCopy = refVal;//这个时候valcopy的类型是int而不是int引用

//对于指针或者引用而言 auto 一般会忽略顶层const，但底层const会被保留下来
const int ci =10,&cr = ci;
auto b = ci; //b是一个 int
auto c = cr;//c 是一个 int
auto d = &cr;// d 是一个指向 const int的指针
auto e = &ci;// e 也是一个指向const int的指针

//如果希望明确保留顶层 const可以这么做
const auto j = ci;//j 是一个const int
auto &g =ci;//g 是一个绑定到ci的 int常量引用
auto &k = 42;//错误，无法给非常量引用赋予字面值
const auto &m = 42; //可以， m 是常量引用

```

ps：虽说auto用来简化推导定义，但是整体auto的使用规则看起来反而更加麻烦了，类似的复杂规则还有很多，后面我们会继续看到。我觉得这也可以说是c++越来越复杂，没人敢说精通的原因之一了。



#### decltype 类型说明符

​	decltype 可以从 表达式推断出类型来，与auto在用处上明显不同的一点就是 auto必须给予初始化，否则无从推断类型，而decltype可以不初始化而推断出类型，当然如果推断出来的类型是引用，那必须得初始化，这是引用的规则。

​	此外，decltype处理顶层const 和引用 的规则和auto有所不同，如果decltype使用的表达式是一个变量，则会返回该变量的类型，包括顶层const和引用。

```c++
decltype(fun()) m = fun();

decltype(10+10) x; // x是int
decltype(x) y;//y 是int
const int a=1,&b=a; 
decltype(b) c; //错误，c是一个const int 引用，必须初始化
decltype(b) d = a;//d是一个const int引用，绑定到a
decltype(a) e = 10; // e是一个const int

int s =1;
int *p = &s;
decltype(*p) m = s;//m其实是一个int 引用
decltype(m+0) n;//n不用初始化，因为表达式m+0的结果是个int，所以n也是int，不强制要求初始化
decltype(m) o;//错误，o时一个int引用，必须初始化

decltype(s) t;//t时一个int
decltype((s)) u;//错误 u是一个int引用，必须初始化
```

​	decltype使用一个变量时，如果不加括号，返回的就是这个变量的类型，但是如果给该变量再加一个括号，他返回的将是引用类型，就如上面最后两行代码所示。

​	记住，当``decltype((vari))`` 时，它返回的永远是一个引用。



#### 自定义类型

​	自定义类是一个复杂的主题，统一放到后面讲，这里先介绍下定义简单的类型或者说是一个数据结构

```c++
/**
 * \brief 定义一个Book的类型
 */
struct Book {
    std::string name; //会被默认初始化，对于string是空字符串
    double price=1; //类内初始化price 为 1
    int age;//默认初始化，对于int是0
} b;//这里定义Book的同时还声明了一个Book类型的变量b，一般不建议这么使用，类的定义和变量的定义应该分开

//使用book的一些代码，通过 . 来访问Book类的成员
int main() {
    Book book;
    book.age = 20;
    book.price = 13.88;
    auto sumPrice = book.price * 10;
    book.name="Simple C++ Learn";
    std::cout<<book.name<<std::flush;
    return 0;
}
```





### 第三章 字符串string，向量vector和数组[]



//todo:: 命名空间和using



#### 标准库类型 string

​	标准库 string 类型表示可变长字符序列，要使用它，必须包含 string头文件

```c++
include <string>
using std::string;

string s1;//默认初始化 ，是个空字符串
string s2(s1);//s2 是s1的副本
string s3= s1;//等价于上一行
string s4("Abcd");//s4是该字符串字面值的副本，除了字面值后面的空字符
string s5 = "Abcd";//等价于上一条
string s6(10,'c');//s6 是10个c组成的字符串
```



##### 直接初始化和拷贝初始化

​	如果使用 = 号初始化一个对象，实际执行的是拷贝初始化，会把等号右侧的值拷贝到等号左侧的对象中。而不是用 = 号，执行的是 直接初始化 。

```c++
string s2(s1);//直接初始化
string s3= s1;//拷贝初始化
string s4 = string(10,'c');//创建一个直接初始化的临时变量来拷贝初始化s4
```

