

# 让自己习惯C++
## 条款1：视C++为一个语言联邦
C++是包含许多特性的语言，不要把它理解成一个语言，理解C++你需要学习这4个部分

1）C语言

2）<font style="color:rgba(0, 0, 0, 0.75);">objected-oriented</font> C++。类、封装、继承、多态

3）template  C++

4）stl

## 条款2：用const替换#define
+ <font style="color:rgba(0, 0, 0, 0.75);">对于单纯常量，最好以const或者enums替换#define</font>
+ <font style="color:rgba(0, 0, 0, 0.75);">对于形似函数的宏（macros），最好改用inline函数替换</font>

---

<font style="background:#F8CED3;color:#70000D">原因</font>

+ 用define在编译前会有多份拷贝，浪费空间
+ 用define不会把宏名记录到符号表中，编译错误只会显示具体的值，不容易定位
+ 用define容易出错，如括号问题题题<u></u>

## 条款3：尽可能用const
1. const是什么
+  类型修饰符
2. const作用
+ 保护变量以防被修改
+ 节省空间，const修饰的变量在程序中只有一份拷贝
+ 提高程序运行效率，编译器不为普通的const常量分配空间，而是保存在符号表中
3. const使用

1）修饰变量

```cpp
const int a = 1;  a是常量不可修改
int b = 2;
const int* p1 = &a; 指针非常量，指向内容为常量
p1 = &b;

```

必须初始化，默认只有本文件的作用域，除非加extern，普通变量有默认加extern

修饰数组

要注意不能用于下标，有个疑问普通const变量能做下标吗

修饰引用

<font style="color:rgb(34, 34, 34);">普通引用不能绑定到const 对象，但const 引用可以绑定到非const 对象。</font>

1.const int &r = 100; // 绑定到字面值常量

2.int i = 50; const int &r2 = r + i; // 引用r绑定到右值 

3.double dVal = 3.1415; const int &ri = dVal; // 整型引用绑定到double 类型

<font style="color:rgb(34, 34, 34);">const 引用则可以绑定到不同但相关的类型的对象或绑定到右值。</font>

<font style="color:rgb(34, 34, 34);">例如：</font>

1.const int &r = 100; // 绑定到字面值常量 2.int i = 50; const int &r2 = r + i; // 引用r绑定到右值 3.double dVal = 3.1415; const int &ri = dVal; // 整型引用绑定到double 类型 

<font style="color:rgb(34, 34, 34);">编译器会把以上代码转换成如下形式的编码：</font>

int temp = dVal; // create temporary int from double const int &ri = temp; // bind ri to that temporary

const指针

const函数



<font style="color:rgb(34, 34, 34);">用形式： 类型 函数名 const； </font>  
<font style="color:rgb(34, 34, 34);">2.修饰类成员函数</font>

void func() const; // const成员函数中不允许对数据成员进行修改，如果修改，编译器将报错。如果某成员函数不需要对数据成员进行修改，最好将其声明为const 成员函数，这将大大提高程序的健壮性。

  <font style="color:rgb(34, 34, 34);">修饰成员变量</font>

const size_t size; // 对于const的成员变量， [1]必须在构造函数里面进行初始化； [2]只能通过初始化成员列表来初始化； [3]试图在构造函数体内对const成员变量进行初始化会引起编译错误。



<font style="color:rgb(34, 34, 34);">3.修饰类对象</font>

const A a; // 类对象a 只能调用const 成员函数，否则编译器报错。

## 条款10：<font style="color:rgb(18, 18, 18);">令operator=返回一个reference to </font>**<font style="color:rgb(18, 18, 18);">this</font>**
    等号运算符重载返回左对象的*this，这样就可以连锁赋值，因为连锁赋值功能大家都会共同遵守

### <font style="color:rgb(18, 18, 18);">条款11：在operator=中处理自我赋值</font>
<font style="color:rgb(18, 18, 18);">自我赋值很容易出现，比如用两个下标二维数组遍历对象时，主对角线上的元素很可能就被自我赋值。这一条条款提到了两个安全性：自我复制安全性和异常安全性。</font>

<font style="color:rgb(18, 18, 18);">先看自我赋值安全性，有如下代码：</font>

**<font style="color:rgb(18, 18, 18);">class</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(23, 81, 153);">Bitmap</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">{</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">...</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">};</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">class</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(23, 81, 153);">Widget</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">{</font><font style="color:rgb(18, 18, 18);">     </font><font style="color:rgb(18, 18, 18);">...</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">private</font>****<font style="color:rgb(18, 18, 18);">:</font>**<font style="color:rgb(18, 18, 18);">     </font><font style="color:rgb(18, 18, 18);">Bitmap</font>**<font style="color:rgb(18, 18, 18);">*</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">};</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">operator</font>****<font style="color:rgb(18, 18, 18);">=</font>**<font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">const</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">)</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">{</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">delete</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);">     </font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">=</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">new</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">Bitmap</font><font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">*</font>**<font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">.</font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);">);</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">return</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">*</font>****<font style="color:rgb(18, 18, 18);">this</font>**<font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">}</font><font style="color:rgb(18, 18, 18);"> </font>

<font style="color:rgb(18, 18, 18);">如果this和rhs指向同一个对象，即自我复制，那delete pb析构了相当于析构了rhs.pb，后面一行对*rhs.pb的访问可能直接就coredump了。</font>

<font style="color:rgb(18, 18, 18);">其中一个解决办法时加入等同测试（identity test），代码如下：</font>

<font style="color:rgb(18, 18, 18);">Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">operator</font>****<font style="color:rgb(18, 18, 18);">=</font>**<font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">const</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">)</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">{</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">if</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">this</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">==</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">)</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">return</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">*</font>****<font style="color:rgb(18, 18, 18);">this</font>**<font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">delete</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);">     </font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">=</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">new</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">Bitmap</font><font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">*</font>**<font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">.</font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);">);</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">return</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">*</font>****<font style="color:rgb(18, 18, 18);">this</font>**<font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">}</font><font style="color:rgb(18, 18, 18);"> </font>

<font style="color:rgb(18, 18, 18);">但是这个代码不是异常安全的，如果new出了问题（可能是malloc内存失败或者Bitmap的构造异常）,当前Widget对象就不能再安全地使用了。</font>

<font style="color:rgb(18, 18, 18);">如果使用临时变量来分配内存，等确认成功了再进行释放操作呢？考虑如下解决代码：</font>

<font style="color:rgb(18, 18, 18);">Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">operator</font>****<font style="color:rgb(18, 18, 18);">=</font>**<font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">const</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">)</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">{</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">if</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">this</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">==</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">)</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">return</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">*</font>****<font style="color:rgb(18, 18, 18);">this</font>**<font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);">  </font>_<font style="color:rgb(153, 153, 153);">// 这一行可以删掉了 </font>_<font style="color:rgb(18, 18, 18);">    </font><font style="color:rgb(18, 18, 18);">Bitmap</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">*</font>**<font style="color:rgb(18, 18, 18);">temp</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">=</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">new</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">Bitmap</font><font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">*</font>**<font style="color:rgb(18, 18, 18);">rhs</font><font style="color:rgb(18, 18, 18);">.</font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);">);</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">delete</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);">     </font><font style="color:rgb(18, 18, 18);">pb</font><font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">=</font>**<font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">temp</font><font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);">     </font>**<font style="color:rgb(18, 18, 18);">return</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">*</font>****<font style="color:rgb(18, 18, 18);">this</font>**<font style="color:rgb(18, 18, 18);">;</font><font style="color:rgb(18, 18, 18);"> </font><font style="color:rgb(18, 18, 18);">}</font><font style="color:rgb(18, 18, 18);"> </font>

<font style="color:rgb(18, 18, 18);">这样虽然解决了问题，但是多多少少看起来有点痛苦。使用所谓的copy and swap技术可以让代码逻辑简洁不少：</font>

<font style="color:rgb(18, 18, 18);">Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">operator=</font>**<font style="color:rgb(18, 18, 18);">(</font>**<font style="color:rgb(18, 18, 18);">const</font>**<font style="color:rgb(18, 18, 18);"> Widget</font>**<font style="color:rgb(18, 18, 18);">&</font>**<font style="color:rgb(18, 18, 18);"> rhs) {     Widget </font>**<font style="color:rgb(241, 64, 60);">temp</font>**<font style="color:rgb(18, 18, 18);">(rhs);     swap(rhs);     </font>**<font style="color:rgb(18, 18, 18);">return</font>**<font style="color:rgb(18, 18, 18);"> </font>**<font style="color:rgb(18, 18, 18);">*this</font>**<font style="color:rgb(18, 18, 18);">; }</font>

