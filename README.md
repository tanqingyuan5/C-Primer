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
练习3.20:读入一组整数并把它们存入一个vector对象，将每对相邻的整数和输出出来。改写你的程序，这次要求先输出第1个和最后一个元素的和，接着输出第2个和倒数第2个元素的和，以此类推。
```c++
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    vector<int> nums;
    for(int num;cin >> num;nums.push_back(num));
    for(decltype(nums.size()) i = 0;i!=nums.size()-1;i++)
        cout << nums[i]+nums[i+1] << endl;
    return 0;
}

//改写后：
#include <iostream>
#include <vector>

using std::vector;
using std::cout;
using std::endl;
using std::cin;

int main()
{
    vector<int> ivec;
    for (int i; cin >> i; ivec.push_back(i));

    if (ivec.empty())
    {
        cout << "input at least one integer." << endl;
        return -1;
    }

    auto size = ivec.size();       //或者decltype(ivec.size()) size = ivec.size();  其实有点多此一举了，看下面Note分析
    if (size % 2 != 0) size = size / 2 + 1;
    else size /= 2;

    for (int i = 0; i != size; ++i)
        cout << ivec[i] + ivec[ivec.size() - i - 1] << " ";
    cout << endl;

    return 0;
}
```
**Note**:编程的时候，常常需要把表达式的值赋给变量，但是很多时候，我们往往并不能清楚的知道表达式的类型，这一棘手的问题催生了C++11新标准提出了`auto`这个概念。有时候我们仅仅需要知道表达式表示的数据类型，而不需要用该表达式的值来初始化变量，这时我们就可以使用C++提供的`decltype`标识符。

练习3.22:修改之前那个输出text第一段的程序，首先把text的第一段全部都改成大写形式，然后输出它。
```c++
#include <iostream>
#include <vector>
#include <string>
#include <cctype>      //改变某个字符特性的头文件，貌似不加也可以改变字符特性

using std::vector; using std::string; using std::cout; using std::cin; using std::isalpha;

int main()
{
    vector<string> text;
    for (string line; cin >> line; text.push_back(line));   //cin >> line的话，省略了空格，可以对比getline(cin,line)时输出结果
    
    for (auto& word : text)                   //遍历每个单词的每个字母
    {
        for (auto& ch : word)
            if (isalpha(ch))                 //当c是字母时为真
                ch = toupper(ch);
        cout << word << " ";                 //每一个词之间添加一个空格
    }
    
    return 0;
}
```
练习3.23:编写一段程序，创建一个含有10个整数的vector对象，然后使用迭代器将所有元素的值都变成原来的两倍。输出vector对象的内容，检验程序是否正确。
```c++
#include<iostream>
#include<vector>
#include<iterator>       //使用迭代器不要忘记此头文件,还有cctype,但是不加xcode也可以编译
using namespace std;

int main()
{
    vector<int> nums;
    for(int num;cin >> num;nums.push_back(num));
    for(auto it = nums.begin();it != nums.end();++it)  //一段字符的话，判断结束还要加上&& !isspace(*it),意思是直至处理全部字符或遇到空白
        *it = *it * 2;
    for(auto i : nums)     //范围for语句遍历给定序列（或者容器）中的每个元素（字母/单词），并对其执行某种操作。
        cout << i << " ";
    cout << endl;
    return 0 ;
}
```
* 使用迭代器运算
```c++
//text 必须是有序的
//beg和end表示我们搜索的范围
auto beg = text.begin(),end = text.end();
auto mid = beg + (beg - end)/2;           //初始状态下的中间点
//当还有元素尚未检查并且我们还没有找到sought时执行循环
while(mid != end && *mid != sought){
    if(sought < *mid)         //我们要找的元素在前半部分吗？
        end = mid;            //如果是，调整搜索范围使得忽略到后半部分
    else                      //我们要找的元素在后半部分
        beg = mid + 1;        //在mid之后寻找
    mid = beg + (end - beg)/2;    //新的中间点
}
```
beg指向搜索范围内的第一个元素、end指向尾元素的下一个位置、mid指向中间的那个元素。

练习3.24:**请使用迭代器**读入一组整数并把它们存入一个vector对象，将每对相邻的整数和输出出来。改写你的程序，这次要求先输出第1个和最后一个元素的和，接着输出第2个和倒数第2个元素的和，以此类推。
```c++
#include <iostream>
#include <vector>

using std::vector; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<int> v;
    for (int buffer; cin >> buffer; v.push_back(buffer));

    if (v.size() < 2)
    {
        cout << " please enter at least two integers";
        return -1;
    }

    for (auto it = v.cbegin(); it + 1 != v.cend(); ++it)   //体会循环条件，最后一个值的index要比容器中最大index少1，因为循环里是i+1
        cout << *it + *(it + 1) << " ";         //是*(it + 1)不是*it + 1
    cout << endl;

    for (auto lft = v.cbegin(), rht = v.cend() - 1; lft <= rht; ++lft, --rht)    //仔细看这一句
        cout << *lft + *rht << " ";
    cout << endl;

    return 0;
}
```
**数组与vector**

与vector相似的地方是，数组也是存放类型相同的对象的容器，这些对象本身没有名字，需要通过其所在位置访问。

与vector不同的地方是，数组的大小确定不变，不能随意向数组中增加元素。

因为数组的大小固定，因此对某些特殊的应用来说程序的运行时性能较好，但是相应地也损失了一些灵活性。

**如果不清楚元素确切个数，请使用vector**

定义数组的时候必须指定数组的类型，不允许用auto关键字由初始值的列表推断类型。另外和vector一样，数组的元素应为对象，因此不存在引用的数组。

不能将数组的内容拷贝给其他数组作为初始值，也不能用数组为其他数组赋值。

**理解复杂的数组声明**

```c++
int *ptrs[10];                    //ptrs是含有10个整型指针的数组
int &refs[10] = /* ? */;          //错误：不存在引用的数组
int (*Parray)[10] = &arr;         //Parray指向一个含有10个整数的数组
int (&arrRef)[10] = arr;          //arrRef引用一个含有10个整数的数组
int *(&arry)[10] = ptrs;          //arry是数组的引用，该数组含有10个指针
```
1.默认情况下，类型修饰符**从右向左**依次绑定。对于ptrs来说，从右向左理解其含义比较简单：首先知道我们定义的是一个大小为10的数组，它的名字是ptrs，然后知道数组中存放的是指向int的指针。

2.对于Parray来说，从右向左理解就不太合理了。因为数组的维度是紧跟着被声明的名字的，所以就数组而言，由内向外阅读要比从右向左好多了。由内向外的顺序可帮助我们更好的理解Parray的含义：首先圆括号括起来的部分，\*Parray意味着Parray是个指针，接下来观察右边，可知道Parray是个指向大小为10的数组的指针，最后观察左边，知道数组中的元素是int。这样最终的含义就明白无误了，Parray是一个指针，它指向一个int数组，数组中包含10个元素。

3.同理，（&arrRef）表示arrRef是一个引用，它引用的对象是一个大小为10的数组，数组中元素的类型是int。

4.按照**由内到外**的顺序，首先知道arry是一个引用，然后观察右边知道，arry引用的对象是一个大小为10的数组，最后观察左边知道，数组的元素类型是指向int的指针。这样，***arry就是一个含有10个int型指针的数组的引用***

**要想理解数组声明的含义，最好的办法是从数组的名字开始按照由内向外的顺序阅读**

练习3.28:下列数组中元素的值是什么？
```c++
string sa[10];             //all elements are empty strings
int ia[10];                //内置函数在函数体外会被设置为0
int main(){
    string sa2[10];        //all elements are empty strings
    int ia2[10];           //all elements are undefined 函数体内，内置函数不被设置为0
}
```
与vector和string一样，当需要遍历数组的所有元素时，最好的办法也是使用范围for语句。**在使用数组下标的时候，通常将其定义为`size_t`类型。**

练习3.31:编写一段程序，定义一个含有10个int的数组，令每个元素的值就是其下标值。
```c++
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int arr[10];
    for (auto i = 0; i < 10; ++i) arr[i] = i;
    for (auto i : arr) cout << i << " ";
    cout << endl;

    return 0;
}
```
练习3.32:将上一题刚刚创建的数组拷贝给另外一个数组。利用vector重写程序，实现类似的功能。
```c++
#include <iostream>
#include <vector>
using std::cout; using std::endl; using std::vector;

int main()
{
    // array
    int arr[10];
    for (int i = 0; i < 10; ++i) arr[i] = i;
    int arr2[10];
    for (int i = 0; i < 10; ++i) arr2[i] = arr[i];

    // vector
    vector<int> v(10);
    for (int i = 0; i != 10; ++i) v[i] = arr[i];
    vector<int> v2(v);
    for (auto i : v2) cout << i << " ";
    cout << endl;

    return 0;
}
```
数组由一个特性：在很多用到数组名字的地方，编译器都会自动地将其替换为一个指向数组首元素的指针：
```c++
string *p2 = nums; //等价于p2 = &num[0]
```
**大多数表达式中，使用数组类型的对象其实是使用一个指向该数组首元素的指针。**由上可知，在一些情况下数组的操作实际上是指针的操作，这一结论有很多隐含的意思。其中一层意思是当使用数组作为一个auto变量的初始值时，推断得到的类型是指针而非数组：
```c++
int ia[] = {0,1,2,3,4,5,6,7,8,9};  //ia是一个含有10个整数的数组
auto ia2(ia);   //ia2是一个整型指针，指向ia的第一个元素
ia2 = 42;       //错误：ia2是一个指针，不能用int值给指针赋值
```
尽管ia是由10个整数构成的数组，但当使用ia作为初始值时，编译器实际执行的初始化过程类似于下面的形式：
```c++
auto ia2(&ia[0]);  //显然ia2的类型是int*
```
必须指出的是，当使用`decltype`关键字时上述转换不会发生，decltype(ia)返回的类型是由10个整数构成的数组：
```c++
// ia3是一个含有10个整数的数组
decltype(ia) ia3 = {0,1,2,3,4,5,6,7,8,9};
ia3 = p;    //错误：不能用整型指针给数组赋值
ia3[4] = i; //正确：把i的值赋给ia3的一个元素
```
**指针也是迭代器**
```c++
int arr[] = {0,1,2,3,4,5,6,7,8,9}
int *p = arr;   //p指向arr的第一个元素
++p;        //p指向arr[1]

//利用上面得到的指针能重写之前的循环，令其输出arr的全部元素：
int *e = &arr[10];  //指向arr尾元素的下一位置的指针，没什么卵用，后面引入begin和end函数
for (int *b = arr;b! = e; ++b)
    cout << *b << endl; //输出arr的元素
```
**标准库函数begin和end**，这两个函数定义在iterator头文件中
```c++
int ia[] = {0,1,2,3,4,5,6,7,8,9};  //ia是一个含有10个整数的数组
int *beg = begin(ia);            //指向ia首元素的指针
int *last = end(ia);             //指向arr尾元素的下一位置的指针
```
两个指针相减的结果的类型是一种名为`ptrdiff_t`的标准库类型，和size_t一样，ptrdiff_t也是一种定义在cstddef头文件中的机器相关的类型。因为差值可能为负值，所以ptrdiff_t是一种带符号类型。

**内置的下标运算符所用的索引值不是无符号类型，这一点与vector和string不一样**
```c++
int *p = &ia[2];      //p指向索引为2的元素
int j = p[1];         //p[1]等价于*(p+1),就是ia[3]表示的那个元素
int k = p[-2];        //p[-2]是ia[0]表示的那个元素
```
练习3.35:编写一段程序，利用指针将数组中的元素置为0.
```c++
#include<iostream>
using namespace std;

int main()
{
    const int size = 10;
    int arr[size];
  
    for (auto ptr = arr; ptr!= arr + size ; ++ptr)
        *ptr = 0;
    for(auto i:arr)
        cout << i << " ";
    cout << endl;
    
    return 0;
}
```
练习3.36:编写一段程序，比较两个数组是否相等。再写一段程序，比较两个vector对象是否相等
```c++
#include<iostream>
#include<vector>
#include<iterator>

using namespace std;
bool compare(int* const pb1, int* const pe1, int* const pb2, int* const pe2)
{
    if ((pe1 - pb1) != (pe2 - pb2)) // have different size.
        return false;
    else
    {
        for (int* i = pb1, *j = pb2; (i != pe1) && (j != pe2); ++i, ++j)
            if (*i != *j) return false;
    }
    
    return true;
}

int main()
{
    int arr1[3] = { 0, 1, 2 };
    int arr2[3] = { 0, 2, 4 };
    
    if (compare(begin(arr1), end(arr1), begin(arr2), end(arr2)))
        cout << "The two arrays are equal." << endl;
    else
        cout << "The two arrays are not equal." << endl;
    
    cout << "==========" << endl;
    
    vector<int> vec1 = { 0, 1, 2 };
    vector<int> vec2 = { 0, 1, 2 };
    
    if (vec1 == vec2)
        cout << "The two vectors are equal." << endl;
    else
        cout << "The two vectors are not equal." << endl;
    
    return 0;
}
```

| 命令 | 表3.8:C风格字符串的函数|
| :---------- | :---------- | 
| strlen(p)  | 返回p的长度，空字符不计算在内  |
| strcmp(p1,p2)  | 比较p1和p2的相等性。如果p1==p2，返回0；如果p1>p2,返回一个正值；如果p1<p2，返回一个负值  |
| strcat(p1,p2)  | 将p2附加到p1之后，返回p1  |
| strcpy(p1,p2)  | 将p2拷贝给p1，返回p1  |

传入此类函数的指针必须指向以空字符作为结束的数组
```c++
char ca[] = {'C', '+', '+'};    //不以空字符结束
cout << strlen(ca) << endl;     //严重错误：ca没有以空字符结束
```
**Tip：对于大多数应用来说，使用标准库string要比使用C风格字符串更安全、更高效。**

练习3.39:编写一段程序，比较两个string对象。再编写一段程序，比较两个C风格字符串内容。
```c++
#include <iostream>
#include <string>
#include <cstring>
using std::cout; using std::endl; using std::string;

int main()
{
    // use string.
    string s1("Mooophy"), s2("Pezy");
    if (s1 == s2)
        cout << "same string." << endl;
    else if (s1 > s2)
        cout << "Mooophy > Pezy" << endl;
    else
        cout << "Mooophy < Pezy" << endl;

    cout << "=========" << endl;

    // use C-Style character strings.
    const char* cs1 = "Wangyue";
    const char* cs2 = "Pezy";
    auto result = strcmp(cs1, cs2);
    if (result == 0)
        cout << "same string." << endl;
    else if (result < 0)
        cout << "Wangyue < Pezy" << endl;
    else
        cout << "Wangyue > Pezy" << endl;

    return 0;
}
```
练习3.40:编写一段程序，定义两个字符数组并用字符串字面值初始化它们；接着再定义一个字符数组存放前两个数组连接后的结果。使用strcpy和strcat把前两个数组的内容拷贝到第三个数组中。
```c++


#include <iostream>
#include <cstring>

const char cstr1[]="Hello";
const char cstr2[]="world!";

int main()
{
    constexpr size_t new_size = strlen(cstr1) + strlen(" ") + strlen(cstr2) +1;  \\这里+1
    char cstr3[new_size];
    
    strcpy(cstr3, cstr1);
    strcat(cstr3, " ");
    strcat(cstr3, cstr2);
    
    std::cout << cstr3 << std::endl;
}
```
任何出现字符串字面值的地方都可以用以空字符结束的字符数组来替代：

1.允许使用以空字符结束的字符数组来初始化string对象或为string对象赋值。

2.在string对象的加法运算中允许使用以空字符结束的字符数组作为其中一个运算对象（不能两个运算对象都是）；在string对象的复合赋值运算中允许使用以空字符结束的字符数组作为其中一个运算对象（不能两个运算对象都是）；在string对象的复合赋值运算中允许使用以空字符结束的数组作为右侧的运算对象。

**不能用string对象直接初始化指向字符的指针**。为了完成该功能，string专门提供了一个名为c_str的成员函数：
```c++
char *str = s; //错误：不能用string对象初始化char*
const char *str = s.c_str();  //正确，
```
**c_str函数的返回值是一个C风格的字符串**。也就是说，函数的返回结果是一个指针，该指针指向一个以空字符结束的字符数组，而这个数组所存的数据恰好与那个string对象一样。结果指针的类型是const char\*，从而确保我们不会改变字符数组的内容。

**warning：如果执行完c_str函数后程序想一直都能使用其返回的数组，最好将该数组重新拷贝一份。**

不允许使用一个数组为另一个内置类型的数组赋初值，也不允许使用vector对象初始化数组。相反的，允许使用数组来初始化vector对象。要实现这一目的，只需指明要拷贝区域的首元素地址和**尾后地址**就可以了。
```c++
int int_arr[] = {0 ,1, 2, 3, 4, 5};
//ivec有6个元素，分别是int_arr中对应元素的副本
vector<int> ivec(begin(int_arr), end(int_arr));
```
用于初始化vector对象的值也可能仅是数组的一部分：
```c++
//拷贝三个元素：int_arr[1]、int_arr[2]、int_arr[3]
vector<int> subVec(int_arr + 1, int_arr + 4);
```
这条初始化语句用3个元素创建了对象subVec，3个元素的值分别来自int_arr[1]、int_arr[2]、int_arr[3]。

**建议：尽量使用标准库类型而非数组，现代的C++程序应当尽量使用vector和迭代器，避免使用内置数组和指针；应该尽量使用string，避免使用C风格的基于数组的字符串。**

练习3.41：编写一段程序，用整型数组初始化一个vector对象。
```c++
#include<iostream>
#include<vector>
using namespace std;

int main()
{
    int arr[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    vector<int> v(begin(arr),end(arr));
    
    for(auto i:v) cout << i << " ";
    cout << endl;
    
    return 0;
}
```

练习3.42:编写一段程序，将含有整数元素的数组的vector对象拷贝给一个整型数组。
```c++
#include<iostream>
#include<vector>
using namespace std;

int main()
{
    vector<int> v{0, 1, 2, 3, 4, 5 ,6 ,7 ,8 ,9 };
    int arr[10];
    for(int i = 0;i != v.size(); ++i) arr[i] = v[i];
    
    for(auto i:arr) cout << i << " ";
    cout << endl;
    
    return 0;
}
```
严格来说，C++语言中没有多维数组，通常所说的多维数组其实是数组的数组。
```c++
int ia[3][4] = {         //三个元素，每个元素都是大小为4的数组
    {0, 1, 2, 3},        //第1行的初始值
    {4, 5, 6, 7},        //第2行的初始值
    {8, 9 ,10 ,11}       //第3行的初始值
};
//没有标识每行的花括号，与之前的初始化语句是等价的
int ia[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8 , 9, 10, 11} 
```

如果表达式含有的下标运算符数量和数组的维度一样多，该表达式的结果将是给定类型的元素；反之，如果表达式含有的下标运算符数量比数组的维度小，则表达式的结果将是给定索引处的一个内层数组：
```c++
//用arr的首元素为ia最后一行的最后一个元素赋值
ia[2][3] = arr[0][0][0];
int (&row)[4] = ia[1];    //把row绑定到ia的第二个4元素数组上
```

程序中经常会用到两层嵌套的for循环来处理多维数组的元素：
```c++
constexpr size_t rowCnt = 3, colCnt = 4;
int ia[rowCnt][colCnt];  //12个未初始化的元素
//对于每一行
for (size_t i = 0; i != rowCnt; ++i) {
    //对于行内每一列
    for (size_t j = 0; j != colCnt; ++j) {
        //将元素的位置索引作为它的值
        ia[i][j] = i * colCnt + j;
    }
}
```
由于在C++11新标准中新增了范围for语句，所以前一个程序可以简化为如下形式：
```c++
size_t cnt = 0;
for(auto &row:ia)           //对于外层数组的每一个元素
    for(auto &col:row){     //对于内层数组的每一个元素
        col = cnt;          //将下一值赋给该元素
        ++cnt;              //将cnt加1
}
```
因为要改变元素的值，所以得把控制变量row和col声明成引用类型。第一个for循环遍历ia的所有元素，这些元素是大小为4的数组，因此**row的类型就应该是含有4个整数的数组的引用。**

即使循环中没有任何写操作，我们还是将外层循环的控制变量声明成了引用类型，这是为了避免数组被自动转成指针。

**Note：要使用范围for语句处理多维数组，除了最内层的循环外，其他所有循环的控制变量都应该是引用类型。**

当程序使用多维数组的名字时，也会西东将其转换成指向数组首元素的指针。

***指针和多维数组***

***Note：定义指向多维数组的指针时，千万别忘了这个多维数组实际上是数组的数组。***

***因为多维数组实际上是数组的数组，所以由多维数组名转换得来的指针实际上是指向第一个内层数组的指针：***

```c++
int ia[3][4];      //大小为3的数组，每个元素是含有4个整数的数组
int (*p)[4] = ia;  //p指向含有4个整数的数组
p = &ia[2];        //p指向ia的尾元素
```
***我们首先明确（\*p）意味着p是一个指针。接着观察右边发现，指针p所指的是一个维度为4的数组；再观察左边知道，数组中的元素是整数。因此，p就是指向含有4个整数的数组的指针。***
```c++
//在上述声明中，圆括号必不可少。
int *ip[4];    //整型指针的数组
int (*ip)[4];  //指向含有4个整数的数组
```
***
随着C++11新标准的提出，通过使用auto或者decltype就能尽可能地避免在数组前面加上一个指针类型了：***
```c++
//输出ia中每个元素的值，每个内层数组各占一行
//p指向含有4个整数的数组
for (auto p = ia; q != ia + 3; ++p){
    // q指向4个整数数组的首元素，也就是说，q指向一个整数
    for(auto q = *p;q != *p + 4; ++Q)
        cout << *q << ' ';
    cout << endl;
}
```
***外层的for循环首先声明一个指针p并令其指向ia的第一个内层数组，然后依次迭代直到ia的全部3行都处理完为止。其中递增运算++p负责将指针p移动到ia的下一行。

***内层的for循环负责输出内层数组所包含的值。它首先令指针q指向p当前所在行的第一个元素。\*p是一个含有4个整数的数组，像往常一样，数组名被自动地转换成指向该数组首元素的指针。内层for循环不断迭代直到我们处理完了当前内层数组的所有元素为止。为了获取内层for循环的终止条件，再一次解引用p得到指向内层数组首元素的指针，给它加上4就得到了终止条件。***

***当然，使用标准库函数begin和end也能实现同样的功能，而且看起来更简洁一些：***
```c++
//p指向ia的第一个数组
for(auto p = begin(ia); p!=end(ia); ++p) {
    //q指向内层数组的首元素
    for(auto q = begin(*p); q!=end(*p); ++q)
        cout << *q << ' '; //输出q所指的整数值
cout << endl;
}
```
***在这一版本的程序中，循环终止条件由end函数负责判断。虽然我们也能推断出p的类型是指向含有4个整数的数组的指针，q的类型是指向整数的指针，但是使用auto关键字我们就不必再烦心这些类型到底是什么了。***

**类型别名简化多维数组的指针**

    读、写和理解一个指向多维数组的指针是一个让人不胜其烦的工作，使用的类型别名能让这项工作变得简单一点儿，例如：
```c++
using int_array = int[4];    //新标准下类型别名的声明
typedef int int_array[4];    //等价的typedef声明

//输出ia中每个元素的值，每个内层数组各占一行
for(int_array *p = ia;p != ia + 3; ++p) {
    for( int *q = *p; q != *p + 4; ++q)
        cout << *q << ' ';
    cout << endl;
}
```
程序将类型“4个整数组成的数组”命名为int_array，用类型名int_array定义外层循环的控制变量让程序显得简洁明了。

练习3.43:编写3个不同版本的程序，令其均能输出ia的元素。版本1使用范围for语句管理迭代过程；版本2和版本3使用普通的for语句，其中版本2要求用下标运算符，版本3要求用指针。此外，在所有的3个版本的程序中都要直接写出数据类型，而不能使用类型别名、auto关键字或decltype关键字。
```c++
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int arr[3][4] = 
    { 
        { 0, 1, 2, 3 },
        { 4, 5, 6, 7 },
        { 8, 9, 10, 11 }
    };

    // range for
    for (const int(&row)[4] : arr)
        for (int col : row) cout << col << " ";
    cout << endl;

    // for loop
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j) cout << arr[i][j] << " ";
    cout << endl;

    // using pointers.
    for (int(*row)[4] = arr; row != arr + 3; ++row)
        for (int *col = *row; col != *row + 4; ++col) cout << *col << " ";
    cout << endl;

    return 0;
}
```
练习3.44:改写上一个练习中的程序，使用类型别名来代替循环控制变量的类型。
```c++
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int ia[3][4] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };

    // a range for to manage the iteration
    // use type alias
    using int_array = int[4];
    for (int_array& p : ia)
        for (int q : p)
            cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j)
            cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    // use type alias
    for (int_array* p = ia; p != ia + 3; ++p)
        for (int *q = *p; q != *p + 4; ++q)
            cout << *q << " ";
    cout << endl;

    return 0;
}
```
练习3.45:再一次改写程序，这次使用auto关键字。
```c++

#include <iostream>
using std::cout; using std::endl;

int main()
{
    int ia[3][4] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };

    // a range for to manage the iteration
    for (auto& p : ia)
        for (int q : p)
            cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j)
            cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    for (auto p = ia; p != ia + 3; ++p)
        for (int *q = *p; q != *p + 4; ++q)
            cout << *q << " ";
    cout << endl;

    return 0;
}
```

string对象是一个可变长的字符序列，vector对象是一组同类型对象的容器。一般来说，应该优先选用标准库提供的类型，之后再考虑C++语言内置的底层的替代品数组或指针。

C风格字符串：以空字符结束的字符数组。字符串字面值是C风格字符串，C风格字符串容易出错。

empty：是string和vector的成员，返回一个布尔值。当对象的大小为0时返回真，否则返回假。

getline：在string头文件中定义的一个函数，以一个istream对象和一个string对象为输入参数。该函数首先读取输入流的内容直到遇到换行符为止，然后将读入的数据存入string对象麻醉后返回istream对象。其中换行符读入但是不保留。

迭代器：是一种类型，用于访问容器中的元素或者在元素之间移动。

尾后迭代器：end函数返回的迭代器，指向一个并不存在的元素，该元素位于容器尾元素的下一位置。

prtdiff_t：是cstddef头文件中定义的一种与机器实现有关的带符号整数类型，它的空间足够大，能够表示数组中任意两个指针之间的距离。

push_back：是vector的成员，向vector对象的末尾添加元素。

size：是string和vector的成员，分别返回字符的数量或元素的数量。返回值的类型是size_type。

size_t:是cstddef头文件中定义的一种与机器实现有关的无符号整数类型，它的空间足够大，能够表示任意数组的大小。

->运算符：箭头运算符，该运算符综合了解引用操作和点操作。a->b等价与（\*a).b。



























































