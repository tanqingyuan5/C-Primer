C++ Primer 学习笔记
==================

****
## 目录
* [第一章](#第一章)
* [第二章](#第二章)
* [第三章](#第三章)
* [第四章](#第四章)
* [第五章](#第五章)
* [第六章](#第六章)
* [第七章](#第七章)
-----------------
### 第一章
`cerr`输出警告和错误信息

`clog`输出程序运行时的一般性信息

* 执行前置递增和递减操作时，变量的值都是在语句被求值以前改变的         
* 执行后置递增和递减操作时，变量的值都是在语句被求值之后改变的        
``` C++
int num1 = 2;
int num2 = 20;
int num3 = --num1 + num2;//等于21
int num4 = num1 + num2;//等于21
```
``` C++
int num1 = 2;
int num2 = 20;
int num3 = num1-- + num2;//等于22
int num4 = num1 + num2;//等于21
```
事先知道循环次数用`for`，不知道用`while`。`for`循环头由三个部分组成：**一个初始化语句**、**一个循环条件**、**一个表达式**。    
* 统计在输入每个值连续出现了多少次
``` C++
#include<iostream>
using namespace std;

int main()
{
    //currVal时我们正在统计的数；我们将读入的新值存入val
    int currVal = 0, val = 0;
    if (cin >> currVal){
        int cnt = 1;                            //保存我们正在处理的当前值的个数
        while(cin >> val){                      //读取剩余的数
            if(val == currVal)                  //如果值相同
                ++cnt;                          //将cnt加1
            else{                               //否则打印前一个值的个数
                cout << currVal << " occurs "
                << cnt << " times" << endl;
                currVal = val;                  //记住新值
                cnt =1;                         //重置计数器
            }
        }//while循环在这里结束
        //记住打印文件中最后一个值的个数
        cout << currVal << " occurs "
        << cnt << " times" << endl;
    }//最外层if语句在这里结束
    return 0;
}

输入：
42 42 42 42 42 55 55 62 100 100 100
输出：
42 occurs 5 times
55 occurs 2 times
62 occurs 1 times
100 occurs 3 times
```
-------------------------------
### 第二章
当我们赋给无符号类型一个超出它表示范围的值时，结果是初始值对无符号类型表示数值总数取模后的余数。       
* 从数学角度：取模运算时，对于负数，应该加上被除数的整数倍，使结果大雨或等于0之后，再进行运算。       
* 从计算机存储角度： 计算机中负数是以补码形式存储的，`-1的补码11111111`，转换成无符号数即是255的二进制编码。        
``` C++
(-1)%256=(-1+256)%256=255
```
表达式中既有带符号类型又有无符号类型，则带符号数会自动低转换成无符号数。       
* 如果反斜杠线\后面跟着的八进制数字超过3个，只有前3个数字与\构成转义序列。
```C++
\1234表示2个字符，即八进制数123对应的字符以及字符4.
```
C++中程序员们在很多场合都会使用对象(object）这个名词。通常情况下，**对象是指一块能存储数据并具有某种类型的内存空间**。     
**声明**使得名字为程序所知，一个文件如果想使用别处定义的名字则必须包含对那个名字的声明。而**定义**负责创建与名字相关的实体。变量声明规定了变量的类型和名字，在这一点上定义与之相同。但是除此之外，定义还申请存储空间，也可能会为变量赋一个初始值。        
如果想声明一个变量而非定义它，就在变量名前添加关键字`extern`。     
```C++
extern int i;//声明i而非定义i
```
Note:变量只能被**定义一次**，但是可以被**多次声明**。       
`for`循环中的`i`是局部变量，出了循环就没用了。    
**引用**为对象起了另外一个名字。无法另引用重新绑定另外一个对象。引用的类型都要和与之绑定的对象严格匹配。           
```C++
int &refVal;        //报错：引用必须被初始化
int &refVal2 = 10;  //报错：引用类型的初始值必须是一个对象
```
**指针**是指向另外一种类型的复合类型。     
>指针和引用的不同点。
>其一，指针本身就是一个对象，允许对指针赋值和拷贝，而且在指针对声明周期内它可以先后指向几个不同的对象。
>其二，指针无须在定义时赋初值。和其他内置类型一样，在块作用域内定义的指针如果没有被初始化，也将拥有一个不确定的值。

>key difference:
>1.引用是已经存在的对象的另一个名称，指针本身就是一个对象。
>2.初始化后，引用仍然绑定到它的初始对象，无法重新绑定引用来引用不同的对象，一个指针可以被分配和复制。
>3.引用总是获取引用最初被绑定的对象，一个指针可以在它的生命周期中指向几个不同的对象。
>4.必须初始化引用，指针在定义的时候可以不初始化。
```C++
int *p = &ival; //p存放变量ival的地址，或者说p是指向变量ival的指针
```
```C++
int *p = nullptr; //等价于int *p = 0;
p = 0 ;//指向的地址变了，不指向任何对象了
*p = 0;//指向的地址内的值变了
```
`void*`是一种特殊的指针类型，可用于存放任意对象的地址。**不能直接操作void\*指针所指的对象，因为我们并不知道这个对象到底是个什么类型，也就无法确定能在这个对象上做哪些操作。**
* 指向指针的指针      
```C++
int *pi = &val;  //pi指向一个int型的数
int **ppi = &pi; //ppi指向一个int型的指针
```
* 指向指针的引用      
引用本身不是一个对象，因此不能定义指向引用的指针。但指针是对象，所以存在对指针的引用。     
```C++
int i = 42;
int *p;            //p是一个int型指针
int *&r = p;       //r是一个对指针p的引用

r = &i;            //r引用了一个指针，因此给r赋值&i就是令p指向i
*r = 0;            //解引用r得到i，也就是p指向的对象，将i的值改为0
```
要理解r的类型到底是什么，最简单的办法是从**右向左阅读**r的定义。离变量名最近的符号（此例中是&r的符号&）对变量的类型有最直接的影响，因此r是一个引用。声明符的其余部分用以确定r引用的类型是什么，此例中的符号\*说明r引用的是一个指针。最后，声明的基本数据类型部分指出r引用的是一个int指针。        

Note：面对一条比较复杂的指针或者引用的声明语句时，从右向左阅读有助于弄清楚它的真实含义。             

const对象一旦创建后其值就不能再改变，所以const对象**必须初始化**。     
```C++
const int i = get_size();  //正确：运行时初始化
const int j = 42;          //正确：编译时初始化
const int k;               //错误：k是一个未经初始化的常量
```
Note：如果想在多个文件之间共享const对象，必须在变量的定义之前添加extern关键字。只在一个文件定义const，而在其他多个文件中声明并使用它。     
```C++
//file_1.cc定义并初始化了一个常量，该常量能被其他文件访问
extern const int bufSize = fcn();
//file_1.h头文件
extern const int bufSize; //与file_1.cc中定义的bufSize是同一个
```
* const的引用          
与普通引用不同的是，对常量对引用不能被用作修改它所绑定的对象：
```C++
const int ci =1024;
const int &r1 = ci; //正确：引用及其对应的对象都是常量
r1 = 42;            //错误：r1是对常量对引用
int &r2 = ci；      //错误：试图让一个非常量引用指向一个常量对象
```
* 对const的引用可能引用一个并非const的对象                
必须认识到，常量引用仅对引用可参与的操作做出了限定，对于引用的对象本身是不是一个常量未作限定。因为对象也可能是个非常量，所以允许通过其他途径改变它的值：   
```C++
int i = 42;    
int &r1 = i;         //引用ri绑定对象i
const int &r2 = i;   //r2也绑定对象i,但是不允许通过r2修改i的值
r1 = 0;              //r1并非常量，i的值修改为0
r2 = 0;              //错误：r2是一个常量引用
```
r2绑定（非常量）整数i是合法的行为。然而，不允许通过r2修改i的值。      
* 指针和const           
类似于常量引用，指向常量的指针不能用于改变其所指对象的值。**要想存放常量对象的地址，只能使用指向常量的指针**。     
```C++
const double pi = 3.14;      //pi是个常量，它的值不能改变
double *ptr = &pi;           //错误：ptr是一个普通指针
const double *cptr = &pi;    //正确：cptr可以指向一个双精度常量
*cptr = 42;                  //错误：不能给*cptr赋值
```   
之前提到，指针的类型必须与其所指向的类型一致，**但是有两个例外**。第一种例外的情况是允许令一个指向常量的指针指向一个非常量对象：    
```C++
double dval = 3.14;  //dval是一个双精度浮点数，它的值可以改变
cptr = &dval;        //正确：但是不能通过\*cptr改变dval的值
```
和常量引用一样，指向常量的指针也没有规定其所指的对象必须是一个常量。所谓指向常量的指针仅仅要求不能通过改变对象的值，而没有规定那个对象的值不能通过其他途径改变。     
* const指针            
指针是对象而引用不是，因此就像其他对象类型一样，允许把指针本身定为常量。**常量指针必须初始化**，而且一旦初始化完成，则它的值（也就是存放在指针中的那个地址就不能再改变了。      
```C++
int errNumb = 0;
int *const curErr = &errNumb;       //curErr将一直指向errNumb
const double pi = 3.14159;
const double *const pip = &pi;      //pip是一个指向常量对象的常量指针
```   
**指针本身是一个常量并不意味着不能通过指针修改其所指对象的值，能否这样做完全依赖于所指对象的类型**。       
* 顶层const          
**顶层const**表示指针本身是个常量，而用名词**底层const**表示指针所指的对象是一个常量。更一般的，**顶层const**可以表示任意的对象是常量，这一点对任何数据类型都适用，如算数类型、类、指针等。**底层const**则与指针和引用等复合类型的基本类型部分有关。**比较特殊的是指针类型既可以是顶层const也可以是底层const，这一类型和其他相比区别明显：**       
```C++
int i = 0;
int *const p1 = &i;       //不能改变p1的值，这是一个顶层const
const int ci = 42;        //不能改变ci的值，这是一个顶层const
const int *p2 = &ci;      //允许改变p2的值，这是一个底层const
const int *const p3 = p2;  //靠右的const是顶层const，靠左的const是底层const
const int &r = ci;        //用于声明的引用const都是底层const
i = ci;         //正确：拷贝ci的值，ci是一个顶层const，对此操作无影响
p2 = p3;        //正确：p2和p3指向的对象类型相同，p3顶层const的部分不影响
int *p = p3;    //错误：p3包含底层const定义，而p没有。 当执行对象的拷贝操作时候，拷入和拷出的对象必须具有相同的底层const资格
p2 = p3;       //正确：p2和p3都是底层const
p2 = &i;        //正确：int*能够转换成const int*
int &r = ci;    //错误：普通的int&不能绑定到int常量上
const int &r2 = i;  //正确：const int&可以绑定到一个普通int上
```
const int \*p为什么可以不初始化？               

P53 写道：**const对象一旦创建后其值就不能再改变，所以const对象必须初始化**            

但是在P57中练习2.28第（e）题为什么判断合法呢？为什么可以不用初始化呢？               
```C++
const int *p;    //legal.a pointer to const int
```
-------解决思路-------      
```c++
const int *p; //p是指针，指向const int类型数据
int * const int p; //p是常指针，指向int类型数据
const int * const p; //p是常指针，指向const int类型数据
```
--------解决思路-------         

const对象一旦创建后，其值就不能再改变，所以const对象必须初始化。对于这句话中的const对象要弄清楚：`const int *p`中，const对象是\*p（即\*p是只读），而对于此句“**const对象必须初始化**”，一般用法中我们要给指针p初始化，而不是给\*p初始化。所以在此可以不初始化。即使在声明是进行初始化（`const int *p = 0x123456`），也是对指针p初始化，等价于如下一般用法：
```c++
const int *p;
p = 0x123456;  //此时是在给指针p赋值，而不是给*p赋值
```
`int *const p`中，const对象是p，此时在指针p声明时必须初始化。一般用法：    
```c++
int *const p = 0x123456;
```
----------解决思路------------     
* 遇到const修饰对指针，需要注意两种可能：         
1.指针变量指向的值无法改变            
2.指针变量本身无法改变           
```c++
int a = 10;
const int *p;
p = &a;
*p = 3;   //错误，通过此指针改变指向的值
int b = 5;
p = &b;   //正确
int c = 10;
int *const p = &c;
*p = 5;  //正确
int d = 5;
p = &d;   //错误，指针变量不能改变
```
-------------解决思路-----------------
>（1）int \*p ----- p是个非const指针，指向的对象是int;       
>（2）const int \*p ---- p是个非const指针，指向的对象是const int;       
>（3）int \*const p ---- p是个const指针，指向的对象的是int;    
>（4）const int \*const p ---- p是个const指针，指向的对象是const int。            
>以上的四个都是定义指针p，如果初始化是指初始化指针p，而不是它所指向的对象，由于（3）和（4）中指针p是const的，所以必须初始化。（1）和（2）中的指针p是非const的，可以不初始化。       
必须要明确的一点，在constexpr声明中如果定义了一个指针，限定符constexpr仅对指针有效，与指针所指的对象无关：    
```c++
const int *p = nullptr;     //p是一个指向整型常量的指针
constexpr int *q = nullptr; //q是一个指向整数的常量指针
```
q是一个常量指针，其中关键在于contexpr把它所定义的对象置为了顶层const。                   
**易混淆：**(如果某个类型别名指代的是复合类型或常量，那么把它用到声明语句里就会产生意想不到的后果。例： 。  
```c++
typedef char *pstring    //pstring是char *同义词
const pstring cstr = 0;  //注意：cstr是指向char的常量指针，不能错误理解为const char *cstr = 0     
                         //pstring实际上是指向char的指针，因此，const pstring就是指向char的常量指针
                         //前者声明了一个指向char的常量指针，错误理解是声明了一个指向const char的指针
const pstring *ps;       //ps是一个指针，它的对象是指向char的常量指针
```    
**易混淆** ：C++11新标准引入了`auto`类型操作符，用它能让编译器替我们去分析表达式所属的类型。`auto`让编译器通过初始值来推算变量的类型。显然，auto定义的变量必须有初始值。auto一般会忽略掉顶层const，同时底层const则会保留下来，比如当初始值是一个指向常量的指针时：    
```c++
int i = 0,&r = i;
auto a = r;     //a是一个整数（r是i的别名，而i是一个整数）
const int ci =i, &cr = ci;
auto b = ci;    //b是一个整数（ci的顶层const特性被忽略掉了）
auto c = cr;    //c是一个整数（cr是ci的别名，ci本身就是一个顶层const）
auto d = &i;    //d是一个整型指针（整数的地址就是指向整数的指针）
auto e = &ci;   //e是一个指向整数常量的指针（对常量对象取地址是一种底层const）
```
如果希望推断出的auto类型是一个顶层const，需要明确指出：    
```c++
const auto f = ci;    //ci推演类型是int，f是const int
```
还可以将引用的类型设为auto，此时原来的初始化规则仍然使用：      
```c++
auto &g = ci;   //g是一个整型常量引用，绑定到ci
auto &h = 42;   //错误：不能为非常量引用绑定字面值
const auto &j = 42; //正确：可以为常量引用绑定字面值 
```
要在一条语句中定义多个变量，切记，符号&和\*只从属于某个声明符，而非基本数据类型的一部分，因此初始值必须是同一种类型：    
```C++
auto k = ci, &l = i;   //k是整数（ci的顶层特性被忽略掉），l是整型引用
auto &m = ci, *p = &ci;   //m是对整型常量的引用，p是指向整型常量的指针
//错误：i的类型是int而&ci的类型是const int
auto &n = i, *p = &ci;
```
有时希望遇到这种情况，希望从表达式的类型推断出要定义的变量的类型，但是不想用该表达式初始化变量。C++11新标准引入了第二种类型说明符：`decltype`,它的作用是选择并返回操作数的数据类型。在此过程中，编译器分析表达式并得到它的类型，却不实际计算表达式的值：   
```c++
decltype(f()) sum = x;    //sum的类型就是函数f的返回类型
```
decltype处理顶层const和引用的方式与auto有些许不同。如果decltype使用的表达式是一个变量，则decltype返回该变量的类型（包括顶层const和引用在内）：   
```c++
const int ci = 0, &cj = ci;
decltype(ci) x = 0;  //x的类型是const int
decltype(cj) y = x;  //y的类型是const int&, y绑定到变量x
decltype(cj) z;      //错误：z是一个引用，必须初始化
```
decltype的结果可以是引用类型：     
```c++
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;    //正确：加法的结果是int，因此b是一个（为初始化的）int
decltype(*p) c;       //错误：c是int&，必须初始化
decltype((i)) d;      //错误：d是int&，必须初始化
```
r+0，显然这个表达式的结果将是一个具体值而非一个引用。另一方面，**如果表达式的内容是解引用，则decltype将得到引用类型**。正如我们熟悉的那样，解引用指针可以得到指针所指的对象，而且还能给这个对象赋值。因此**decltype(\*p)的结果类型就是int&，而非int**。`decltype((variable))`（注意是双层括号）的结果**永远**是引用。      
>The way decltype handles top-level const and references differ subtly from the way auto does.Another important difference between decltype and auto is that the deduction done by decltype depends on the form of its given expression.      
`struct`是一个关键字，用于定义类：     
```c++
struct Sales_data{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```
-------------------------------------------------
### 第三章                  
练习3.17：从cin读入一组词并把它们存入一个vector对象，然后设法把所有词都改写为大写形式。输出改变后的结果，每个词占一行。
```c++
#include<iostream>
#include<vector>
#include<string>
using namespace std;

int main(){
    vector<string> str;
    for(string word;cin >> word;str.push_back(word));
    for(auto &i : str)     //str中每个单词i
        for(auto &c : i)   //i中每个字母c
            c = toupper(c);
    for(decltype(str.size()) ix = 0;ix != str.size();ix++)
        cout << str[ix] << endl;
    return 0;
}
```
练习3.19:如果想定义一个含有10元素的vector对象，所有元素的值都是42，请列举出三种不同的实现方法。哪种方法更好呢？为什么？
```c++
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    vector<int> ivec1(10, 42);
    vector<int> ivec2{ 42, 42, 42, 42, 42, 42, 42, 42, 42, 42 };
    vector<int> ivec3;
    for (int i = 0; i != 10; ++i) ivec3.push_back(42);
    cout << "The first approach is better!" << endl;
    
    return 0;
}
```








































