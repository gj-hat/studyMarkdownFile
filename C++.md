[TOC]



#  C++基础



## 01_C++ 简介



## 02_基本知识



#### 2.1 你好世界

``` c++
#include <iostream>
using namespace std;
void main()
{
   cout << "Hello World!\n";
}
```



#### 2.2 注释

+ 单行注释: 	//
+ 多行注释: /*......*/



#### 2.3 变量

就是可以定义之后可以随意修改的量(只可以修改值,不可以修改数据类型)





#### 2.4 常量

定义之后就不可以修改了  

+ #define:  #define 常量名 常量值  
+ const:   const 数据类型 常量名 = 常量值

``` c++
#include <iostream>
using namespace std;
#define Day 7
const string IP = "127.0.0.1";
int main()
{
    cout << "Hello World!\n";
    cout << "一周有" << Day << "天 " << endl;
    cout << "本地IP是" << IP;
}
```



#### 2.5 关键字

 <img src="https://www.runoob.com/wp-content/uploads/2018/06/20130806104900234.jpg">

## 03_数据类型

常见数据类型答题可分为: 字符型、宽字符型、整型、浮点型、双浮点型、布尔型等

值得一提: 不同操作系统的内存空间也不同

| 类型                            | 位            | 范围                                                         |
| :------------------------------ | :------------ | :----------------------------------------------------------- |
| <font color="red">char</font>   | 1 个字节      | -128 到 127 或者 0 到 255                                    |
| unsigned char                   | 1 个字节      | 0 到 255                                                     |
| signed char                     | 1 个字节      | -128 到 127                                                  |
| <font color="red">int</font>    | 4 个字节      | -2147483648 到 2147483647                                    |
| unsigned int                    | 4 个字节      | 0 到 4294967295                                              |
| signed int                      | 4 个字节      | -2147483648 到 2147483647                                    |
| short int                       | 2 个字节      | -32768 到 32767                                              |
| unsigned short int              | 2 个字节      | 0 到 65,535                                                  |
| signed short int                | 2 个字节      | -32768 到 32767                                              |
| long int                        | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| signed long int                 | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| unsigned long int               | 8 个字节      | 0 到 18,446,744,073,709,551,615                              |
| <font color="red">float</font>  | 4 个字节      | 精度型占4个字节（32位）内存空间，+/- 3.4e +/- 38 (~7 个数字) |
| <font color="red">double</font> | 8 个字节      | 双精度型占8 个字节（64位）内存空间，+/- 1.7e +/- 308 (~15 个数字) |
| long double                     | 16 个字节     | 长双精度型 16 个字节（128位）内存空间，可提供18-19位有效数字。 |
| wchar_t                         | 2 或 4 个字节 | 1 个宽字符                                                   |



C++ 允许在 **char、int 和 double** 数据类型前放置修饰符。修饰符用于改变基本类型的含义，所以它更能满足各种情境的需求。

下面列出了数据类型修饰符：

- signed
- unsigned
- long
- short

修饰符 **signed、unsigned、long 和 short** 可应用于整型，**signed** 和 **unsigned** 可应用于字符型，**long** 可应用于双精度型。

修饰符 **signed** 和 **unsigned** 也可以作为 **long** 或 **short** 修饰符的前缀。例如：**unsigned long int**。

#### 3.1 sizeof()关键字

sizeof(数据类型/变量名)返回其所占内存空间大小

``` c++
#include <iostream>
using namespace std;
#define Day 7
const string IP = "127.0.0.1";
int main()
{
    cout << "一周有" << Day << "天 " << endl;
    cout << "本地IP是" << IP;
    cout << "Day的大小:" << sizeof(Day) << endl;
    cout << "IP:" << sizeof(IP) << endl;
    cout << "Int:" << sizeof(int) << endl;
}
```

#### 3.2 Input/Output

+ cout输出: 语法 `cout << 需要输出的东西`
+ cin输入: 语法`cin >> 变量名`

示例:

``` c++
#include <iostream>
using namespace std;
int main()
{
	int a;
	cout << "请输入a的值:";
	cin >> a;
	cout << "a=" << a;
}
```



## 04_ 运算符

#### 4.1 算术运算符

| 运算符 | 描述                                                         | 实例             |
| :----- | :----------------------------------------------------------- | :--------------- |
| +      | 把两个操作数相加                                             | A + B 将得到 30  |
| -      | 从第一个操作数中减去第二个操作数                             | A - B 将得到 -10 |
| *      | 把两个操作数相乘                                             | A * B 将得到 200 |
| /      | 分子除以分母                                                 | B / A 将得到 2   |
| %      | 取模运算符，整除后的余数                                     | B % A 将得到 0   |
| ++     | [自增运算符](https://www.runoob.com/cplusplus/cpp-increment-decrement-operators.html)，整数值增加 1 | A++ 将得到 11    |
| --     | [自减运算符](https://www.runoob.com/cplusplus/cpp-increment-decrement-operators.html)，整数值减少 1 | A-- 将得到 9     |

#### 4.2 关系运算符

| ==   | 检查两个操作数的值是否相等，如果相等则条件为真。             | (A == B) 不为真。 |
| ---- | ------------------------------------------------------------ | ----------------- |
| !=   | 检查两个操作数的值是否相等，如果不相等则条件为真。           | (A != B) 为真。   |
| >    | 检查左操作数的值是否大于右操作数的值，如果是则条件为真。     | (A > B) 不为真。  |
| <    | 检查左操作数的值是否小于右操作数的值，如果是则条件为真。     | (A < B) 为真。    |
| >=   | 检查左操作数的值是否大于或等于右操作数的值，如果是则条件为真。 | (A >= B) 不为真。 |
| <=   | 检查左操作数的值是否小于或等于右操作数的值，如果是则条件为真。 | (A <= B) 为真。   |

#### 4.3 逻辑运算符

| 运算符 | 描述                                                         | 实例                 |
| :----- | :----------------------------------------------------------- | :------------------- |
| &&     | 称为逻辑与运算符。如果两个操作数都 true，则条件为 true。     | (A && B) 为 false。  |
| \|\|   | 称为逻辑或运算符。如果两个操作数中有任意一个 true，则条件为 true。 | (A \|\| B) 为 true。 |
| !      | 称为逻辑非运算符。用来逆转操作数的逻辑状态，如果条件为 true 则逻辑非运算符将使其为 false。 | !(A && B) 为 true。  |

#### 4.4 位运算符

下表显示了 C++ 支持的位运算符。假设变量 A 的值为 60，变量 B 的值为 13，则：

| 运算符 | 描述                                                         | 实例                                                         |
| :----- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| &      | 按位与操作，按二进制位进行"与"运算。运算规则：`0&0=0;    0&1=0;     1&0=0;      1&1=1;` | (A & B) 将得到 12，即为 0000 1100                            |
| \|     | 按位或运算符，按二进制位进行"或"运算。运算规则：`0|0=0;    0|1=1;    1|0=1;     1|1=1;` | (A \| B) 将得到 61，即为 0011 1101                           |
| ^      | 异或运算符，按二进制位进行"异或"运算。运算规则：`0^0=0;    0^1=1;    1^0=1;   1^1=0;` | (A ^ B) 将得到 49，即为 0011 0001                            |
| ~      | 取反运算符，按二进制位进行"取反"运算。运算规则：`~1=-2;    ~0=-1;` | (~A ) 将得到 -61，即为 1100 0011，一个有符号二进制数的补码形式。 |
| <<     | 二进制左移运算符。将一个运算对象的各二进制位全部左移若干位（左边的二进制位丢弃，右边补0）。 | A << 2 将得到 240，即为 1111 0000                            |
| >>     | 二进制右移运算符。将一个数的各二进制位全部右移若干位，正数左补0，负数左补1，右边丢弃。 |                                                              |

```c++
#include <iostream>
using namespace std;
 
int main()
{
   unsigned int a = 60;      // 60 = 0011 1100  
   unsigned int b = 13;      // 13 = 0000 1101
   int c = 0;           
   c = a & b;             // 12 = 0000 1100
   cout << "Line 1 - c 的值是 " << c << endl ;
   c = a | b;             // 61 = 0011 1101
   cout << "Line 2 - c 的值是 " << c << endl ;
   c = a ^ b;             // 49 = 0011 0001
   cout << "Line 3 - c 的值是 " << c << endl ;
   c = ~a;                // -61 = 1100 0011
   cout << "Line 4 - c 的值是 " << c << endl ;
   c = a << 2;            // 240 = 1111 0000
   cout << "Line 5 - c 的值是 " << c << endl ;
   c = a >> 2;            // 15 = 0000 1111
   cout << "Line 6 - c 的值是 " << c << endl ;
   return 0;
}


// 值
Line 1 - c 的值是 12
Line 2 - c 的值是 61
Line 3 - c 的值是 49
Line 4 - c 的值是 -61
Line 5 - c 的值是 240
Line 6 - c 的值是 15
```



#### 4.5 赋值运算符

| 运算符 | 描述                                                         | 实例                            |
| :----- | :----------------------------------------------------------- | :------------------------------ |
| =      | 简单的赋值运算符，把右边操作数的值赋给左边操作数             | C = A + B 将把 A + B 的值赋给 C |
| +=     | 加且赋值运算符，把右边操作数加上左边操作数的结果赋值给左边操作数 | C += A 相当于 C = C + A         |
| -=     | 减且赋值运算符，把左边操作数减去右边操作数的结果赋值给左边操作数 | C -= A 相当于 C = C - A         |
| *=     | 乘且赋值运算符，把右边操作数乘以左边操作数的结果赋值给左边操作数 | C *= A 相当于 C = C * A         |
| /=     | 除且赋值运算符，把左边操作数除以右边操作数的结果赋值给左边操作数 | C /= A 相当于 C = C / A         |
| %=     | 求模且赋值运算符，求两个操作数的模赋值给左边操作数           | C %= A 相当于 C = C % A         |
| <<=    | 左移且赋值运算符                                             | C <<= 2 等同于 C = C << 2       |
| >>=    | 右移且赋值运算符                                             | C >>= 2 等同于 C = C >> 2       |
| &=     | 按位与且赋值运算符                                           | C &= 2 等同于 C = C & 2         |
| ^=     | 按位异或且赋值运算符                                         | C ^= 2 等同于 C = C ^ 2         |
| \|=    | 按位或且赋值运算符                                           | C \|= 2 等同于 C = C \| 2       |

#### 4.6 杂项运算符

| 运算符               | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| sizeof               | sizeof 运算符返回变量的大小。例如，sizeof(a) 将返回 4，其中 a 是整数。 |
| Condition ? X : Y    | 条件运算符如果 Condition 为真 ? 则值为 X : 否则值为 Y。      |
| ,                    | 逗号运算符会顺序执行一系列运算。整个逗号表达式的值是以逗号分隔的列表中的最后一个表达式的值。 |
| .（点）和 ->（箭头） | 成员运算符)用于引用类、结构和共用体的成员。                  |
| Cast                 | 强制转换运算符把一种数据类型转换为另一种数据类型。例如，int(2.2000) 将返回 2。 |
| &                    | 指针运算符 & 返回变量的地址。例如 &a; 将给出变量的实际地址。 |
| *                    | 指针运算符 * 指向一个变量。例如，*var; 将指向变量 var。      |

#### 4.7 C++ 中的运算符优先级

| 类别       | 运算符                            | 结合性   |
| :--------- | :-------------------------------- | :------- |
| 后缀       | () [] -> . ++ - -                 | 从左到右 |
| 一元       | + - ! ~ ++ - - (type)* & sizeof   | 从右到左 |
| 乘除       | * / %                             | 从左到右 |
| 加减       | + -                               | 从左到右 |
| 移位       | << >>                             | 从左到右 |
| 关系       | < <= > >=                         | 从左到右 |
| 相等       | == !=                             | 从左到右 |
| 位与 AND   | &                                 | 从左到右 |
| 位异或 XOR | ^                                 | 从左到右 |
| 位或 OR    | \|                                | 从左到右 |
| 逻辑与 AND | &&                                | 从左到右 |
| 逻辑或 OR  | \|\|                              | 从左到右 |
| 条件       | ?:                                | 从右到左 |
| 赋值       | = += -= *= /= %=>>= <<= &= ^= \|= | 从右到左 |
| 逗号       | ,                                 | 从左到右 |



## 05_程序流程结构

C++支持三种基本流程结构:  顺序结构, 选择结构, 循环结构

+ 顺序结构: 按顺序执行不发生跳转
+ 选择结构: 判断符合条件的执行
+ 循环结构: 符合条件循环执行



#### 5.1 选择结构

+ if语句

  单行,多行,嵌套

+ 三目运算

  判断条件 ? 真执行的语句 :  假执行的语句

+ switch语句

####  5.2 循环结构

+ while语句
+ do...while语句
+ for语句

#### 5.3 跳转语句

###### 5.3.1 break

> break: 跳出选择结构或循环结构

+ 在switch中: 作用终止case并跳出switch
+ 在循环语句中出现: 作用跳出当前循环
+ 出现在在嵌套中: 跳出最近的内层循环



###### 5.3.2 continue

> continue: 跳出当前循环中剩余部分，并进入下次循环



###### 5.3.3 goto关键字

> goto： 无条件跳转，跳转到标记处

语法：`goto 标记`

如果标记名称存在执行到goto语句时，跳转到标记位置

``` c++
#include <iostream>
using namespace std;
int main()
{
	// 顺序执行1->2 然后goto到FLAG下 继续顺序执行 5 -> 6
	cout << 1 << endl;
	cout << 2 << endl;
	goto FLAG;
	cout << 3 << endl;
	cout << 4 << endl;
	FLAG:
	cout << 5 << endl;
	cout << 6 << endl;
}
```



#### 5.3 示例

``` c++
#include <iostream>
using namespace std;
int main()
{
	for (int i = 1; i < 100; i++)
	{
		if (i % 4 == 0) {
			cout << i << endl;
		}
		else if (i % 3)
		{
			continue;
		}
		else if (i % 51 == 0)
		{
			break;
		}
	}
}
```



## 06_ 数组

数组就是存放相同数据元素的一个集合

特点：

+ 数组中每个元素都是相同的数据类型
+ 数组在内存中是连续的

#### 6.1 一维数组

###### 6.1.1 声明定义



``` c++
// 定义
int a[10];
int b[2*5];
int c[n*2];  // 之前定义过n

// 初始化
int d[5] = {1,2,3,4,5};
int e[100] = {2,2,3,4,5}  // 只给一部分值定义
int f[] = {1,2,3,4,5,6,7}  //如果给全部元素复制可以不需要定义长度
```



###### 6.1.2 一维数组名的用途

+ 可以统计整个数组在内存中的长度
+ 可以获取数组在内存中的首地址



**示例:**数组示例, 遍历, 逆置, 冒泡

``` c++
#include <iostream>
using namespace std;

int main()
{

	int a[] = { 1,2,3,7,4,5 };

	cout << "数组a的大小是:" << sizeof(a) << endl;
	cout << "数组a的长度是:" << size(a) << endl;

	cout << "数组a的首地址是:" << a << endl;

	cout << "数组a的遍历:";
	for (int b : a) {
		cout << b;
	}
	// 数组逆置
	int arrStart = 0;
	int arrEnd = size(a) - 1;
	int temp;
	cout << "\n-------------";
	while (arrStart < arrEnd)
	{
		temp = a[arrStart];
		a[arrStart] = a[arrEnd];
		a[arrEnd] = temp;
		arrStart++;
		arrEnd--;
	}
	cout << "\n数组a的遍历:";
	for (int b : a) {
		cout << b;
	}
    
    cout << "\n数组冒泡 小->大";
	for (int j = 0; j < size(a) - 1; j++)
	{
		for (int i = 0; i < size(a) - 1-j; i++)
		{
			if (a[i + 1] < a[i])
			{
				temp = a[i + 1];
				a[i + 1] = a[i];
				a[i] = temp;
			}
		}
	}
	cout << "\n数组a的遍历:";
	for (int b : a) {
		cout << b;
	}
}
```

#### 6.2 二维数组

###### 6.2.1 声明定义

> 建议使用第二种

1. 数据类型 数组名`[行数][列数]`;
2. 数据类型 数组名`[行数][列数]` ={ {数据1，数据2}，{数据3，数据4}}
3. 数据类型 数组名`[行数][列数]` ={ 数据1，数据2，数据3，数据4}
4. 数据类型 数组名`[][列数]` ={ 数据1，数据2，数据3，数据4}

###### 6.2.2 二维数组名的用途

+ 可以统计整个数组在内存中的长度
+ 可以获取数组在内存中的首地址



## 07_函数



#### 7.1 函数的写法

不写了  学了那么多语言要是还不知道函数怎么写就自己找原因



#### 7.2 函数调用

语法:  函数名(参数)

值得一提:  c++的函数必须在main之前,或者在main之前进行声明



#### 7.3 函数分文件

步骤:

1. 在头文件的文件夹里创建一个`.h`结尾的文件(类似于java中的接口)
   + 里面写必要的头(iostream,useing namespace std等等),和函数的声明
2. 新建一个`.cpp`文件
   + 里面包含刚刚写的`.h`头文件
   + 写刚刚`.h`内声明的函数
3. 在主`.cpp`文件中包含`.h`头文件
4. 在main方法中直接写方法名



文件结构:

1. functions.h

   ``` c++
   #include <iostream>
   using namespace std;
   int add(int a, int b);
   ```

2. change.cpp

   ``` c++
   #include "functions.h"
   int add(int a, int b) {
   	return a + b;
   }
   ```

3. main.cpp

   ``` c++
   #include <iostream>
   #include "functions.h"
   using namespace std;
   int main()
   {
   	int a = add(1, 2);
   	cout << a;
   }
   ```

   

## 08_指针



#### 8.1 指针的基本概念

> 指针关键字*      取地址&

指针也是一种特殊的数据类型:  值是十六进制的地址

作用: 通过指针直接访问内存

特点:

+ 内存编号从0开始记录,一般使用十六进制
+ 可以利用指针变量保存地址



#### 8.2 定义

``` c++
#include <iostream>
using namespace std;

int main()
{
	int a = 1;
    // 指针保存的是地址
	int* p = &a;
	// 第一个和第二个都是a的地址   第三个是p的地址
	cout << p << endl;
	cout << &a << endl;
	cout << &p << endl;


}
```

#### 8.3 使用

> 可以通过在指针前加解引用关键字* 来找到指针指向的内存中的数据

``` c++
#include <iostream>
using namespace std;
int main()
{
	int a = 1;
	int* p ;
	p = &a;
	// 第一个和第二个都是a的地址   第三个是p的地址
	cout << p << endl;
	cout << &a << endl;
	cout << &p << endl;

	// 打印的都是1000
	*p = 1000;
	cout << "a=" << a << endl;
	cout << "*p=" << *p << endl;
}
```









#### 8.4 空指针

空指针: 指针变量指向内存编号为0的空间

用途: 初始化指针变量

注意: 空指针指向的内存变量是不能访问的



#### 8.5 野指针

​	野指针: 指针初始化或者复制时候直接手动写了一个十六进制数

``` c++
#include <iostream>
using namespace std;
int main()
{
	int* p = (int *)0x0011;
}
```



#### 8.6 const修饰指针

const修饰指针有三种情况:

+ const修饰指针:  常量指针 
+ const修饰常量:   指针常量
+ const既修饰指针,又修饰常量



示例: 

1. const修饰指针:

   ``` c++
   #include <iostream>
   using namespace std;
   int main()
   {
   	int a = 10;
   	int b = 20;
   	const int* p = &a;    // const修饰指针
   	p = &b;   //  通过  指针的指向可以修改
   	*p = 30;    // 报错  指针指向的常量不可以修改
   }
   ```

2. const修饰常量:

   ``` c++
   #include <iostream>
   using namespace std;
   int main()
   {
   	int a = 10;
   	int b = 20;
   	int* const p = &a;		// const修饰常量
   	*p = 30;     // 通过  指针指向的变量可以修改
   	p = &b;		// 报错    指针的指向不可以更改
   }
   ```

3. const既修饰指针又修饰常量:

   ``` c++
   #include <iostream>
   using namespace std;
   int main()
   {
   	int a = 10;
   	int b = 20;
   	const int* const p = &a;		// const修饰常量
   	*p = 30;     // 报错  指针指向的变量不可以修改
   	p = &b;		// 报错    指针的指向不可以更改
   }
   ```



#### 8.7 指针和数组

作用: 利用指针访问数组

``` c++
#include <iostream>
using namespace std;
int main()
{
	int a[] = { 1,2,3,4,545,6,3,1,3 };
	int* p = a;		// 数组名的代表的就是首地址
	int* pp = (int *)&a;   // 取地址测试
	cout << p << endl;
	cout << pp << endl;
	// 打印第一个数据
	cout << a[0] << endl;
	cout << *p << endl;
	// 遍历
	for (int i = 0; i < size(a); i++)
	{
		cout << *p << endl;
		p++;   // 因为p的int类型所以+1就是加一个int的大小   同理char,float一样
	}
}
```



#### 8.8 指针和函数

主要讨论函数参数值的改变情况:  

+ 值传递: 形参是值;    不改变原函数的变量值
+ 地址传递: 形参是地址;    改变原函数的变量值



示例:

``` c++
#include <iostream>
using namespace std;
// 值传递
void change1(int a, int b) {
	int temp;
	temp = b;
	b = a;
	a = temp;
	cout << "change1 a =" << a << endl;
	cout << "change1 b = " << b << endl;
}
// 地址传递
void change2(int* a, int* b) {
	int temp;
	temp = *b;
	*b = *a;
	*a = temp;
	cout << "change2 a =" << *a << endl;
	cout << "change2 b = " << *b << endl;
}

int main()
{
	int a = 1;
	int b = 3;
	change1(a, b);
	cout << "值传递后的ab值" << endl;
	cout << "a =" << a << endl;
	cout << "b = " << b << endl;

	cout << "-------------" << endl;
	change2(&a, &b);
	cout << "地址传递后的ab值" << endl;
	cout << "a =" << a << endl;
	cout << "b = " << b << endl;
}
```



#### 8.9 指针,数组,函数

要求: 传入一个数组实现对原数组的冒泡排序;

``` c++
#include <iostream>
using namespace std;
void change1(int* p, int count) {
	int temp = 0;
	for (int i = 0; i < count-1; i++)
	{
		for (int j = 0; j < count-i-1; j++)
		{
			if (*(p+j+1)> *(p+j))
			{
				temp = *(p + j + 1);
				*(p + j + 1) = *(p + j);
				*(p + j) = temp;
			}

		}
	}
}
int main()
{
	int arr[] = { 2,5,9,6,3,1,0 };
	cout << "----------" << endl;
	for (int i : arr) {
		cout << i;
	}
	cout << endl;
	change1(arr, size(arr));
	cout << "----------排序后" << endl;
	for (int i : arr) {
		cout << i;
	}
}
```



## 09_结构体

> 结构体是用户自定义的数据类型, 	允许用户存储不同的数据类型
>
> 类似于java的..类(名字一时间想不起来了)

#### 9.1 定义

语法: struct 结构体名 {结构体成员列表};

通过结构体创建变量三种方式:

+ struct 结构体名 变量名
+ struct 结构体名 变量名 = {成员1值, 成员2值}
+ 定义结构体时唇边创建变量

示例:

``` c++
#include <iostream>
using namespace std;
// 创建结构体
struct student  {
	string name;
	int age;
	int score;
};
// 方式三  在结构体后直接声明
struct student {
	string name;
	int age;
	int score;
}s3, s4, s5;
int main()
{
	// 创建结构体变量 
	// 方式一
	struct student s1 = { "张三",12,99 };
	// 方式二
	struct Student s2;
	s2.name = "李四";
	s2.age = 14;
	s2.score = 98;
	cout << s1.name;
}
```

#### 9.2 结构体数组

作用: 将自定义的结构体放入数组中

语法: struct 结构体名 数组名[个数] = {{}, {}, {}, ....}

``` c++
#include <iostream>
using namespace std;
struct Student  {
	string name;
	int age;
	int score;
};
int main()
{
	struct student studentarr[3] = { {"张三",22,44}, {"李四",55,88}, {"王五",99,67} };
	for (student s : studentarr) {
		cout << s.name << s.age << s.score << endl;
	}
}
```



#### 9.3 结构体指针

作用: 通过指针方式结构体中的成员

使用: 利用操作符`->` 通过结构体指针访问结构体属性

``` c++
#include <iostream>
using namespace std;
struct student  {
	string name;
	int age;
	int score;
};
int main()
{
	struct student s = { "张三",22,33 };
	struct student* p;
	p = &s;
	cout << p->name << p->age << p->score << endl;
}
```



#### 9.4 结构体嵌套结构体

在结构体内写结构体

```` c++
#include <iostream>
using namespace std;
struct student  {
	string name;
	int age;
	int score;
};
struct teacher {
	string name;
	int age;
	int score;
	struct student st;
};
int main()
{
	struct teacher s = { "张三",22,33 , {"李四",33,66}};
}
````



#### 9.5 结构体当作函数参数

> 这就不说了 和java太像了  



#### 9.6 结构体中const

> 作用防止结构体内的数据更改

``` c++
#include <iostream>
using namespace std;
struct student  {
	string name;
	int age;
	int score;
};
void ptintStruct(const struct student* p) {
	//p->name = "dd";   //报错不能更改
	cout << p->name << p->age << p->score << endl;
}
int main()
{
	struct student s = { "张三",22,33};
	ptintStruct(&s);
}
```





# C++核心编程

> 和C不同的地方,   面向对象



## 01_内存分区模型

c++程序运行时,将内存大方向划分为四个区域

+ 代码区: 存放函数体的二进制代码, 由操作系统进行管理
+ 全局区: 存放全局变量和静态变量以及常量
+ 栈区: 由编译器自动分配释放,存放函数的参数值, 局部变量等
+ 堆区: 由程序员分配和释放, 若程序员不释放,则程序结束时由操作系统回收

内存四区的意义: 不同的区域存放的数据, 赋予不同的生命周期, 给我们更大的灵活编程



### 1.1 程序运行前 

> 程序运行前没有堆栈区

在程序编译后, 生成exe可执行程序, 未执行该程序前分为两个区域

**代码区:** 

+ 存放CPU执行的机器指令
+ 代码区的共享的, 共享的目的是对于频繁被执行的程序只需要在内存中有一份代码即可
+ 代码区是只读的, 使其只读的原因是防止程序意外的修改了他的指令

**全局区:**

+ 全局变量和静态变量存放在这里
+ 全局区还包含 了常量区, 字符串常量和其他常量也存放在这里
+ 该区域的数据在程序结束后由操作系统释放





### 1.2 程序运行后

**栈区:** 

+ 由编译器自动分配释放, 存放函数的参数值,局部变量等
+ 注意事项: 不要返回局部变量(形参也属于)的地址, 栈区开辟的数据由编译器自动释放
+ 2你可以想想 可以想得通的  加油;

示例:

``` c++
#include <iostream>
using namespace std;

int func1() {
	int a = 10;      // 局部变量
	return (int)&a;		// 局部变量的地址
}
int* func3() {
	int a = 10;      // 局部变量
	return &a;		// 局部变量的地址
}
int func2() {
	int a = 10;		// 局部变量
	return a;			// 返回局部变量
}
int main()
{
	int* p = (int*)func1();
	cout << *p << endl;			// 第一次可以打印
	cout << *p << endl;			// 第二次编译器自动释放了 打印的乱码
	cout << "------------"<< endl;
	int* p = func3();
	cout << *p << endl;			// 第一次可以打印
	cout << *p << endl;			// 第二次编译器自动释放了 打印的乱码
	cout << "------------" << endl;
	int pp = func2();
	cout << pp << endl;			// 可以打印
	cout << pp << endl;			// 可以打印
}
```

**堆区:**

+ 由程序员分配释放, 由程序员分配和释放, 若程序员不释放,则程序结束时由操作系统回收
+ 在C++中主要使用new在堆区开辟内存
+ new关键字返回的是地址    可以将数据开辟到堆区
+ 指针本质也是局部变量, 放在栈上, 指针报错的数据在堆区

示例:

``` c++
#include <iostream>
using namespace std;
int* func4() {
	// new关键字返回的是地址    可以将数据开辟到堆区
	// 指针本质也是局部变量, 放在栈上, 指针报错的数据在堆区
	int* a = new int(10);
	return a;
}
int main()
{
	int* ppp = func4();
	cout << *ppp << endl;			// 第一次可以打印
	cout << *ppp << endl;			// 第二次编译器自动释放了 打印的乱码
	cout << "------------" << endl;
}
```

### 1.3 new操作符

+ c++中利用new操作符在堆区开辟数据
+ 堆区开辟的数据, 由程序员手动开辟, 手动释放, 释放利用操作符`delete`
+ 语法: `new 数据类型`
+ 利用new创建的数据, 会返回该数据对应的类型的指针(返回的是指针)



## 02_引用



### 2.1 引用的基本使用

**作用**: 给变量起别名

**语法**: `数据类型 &别名 = 原名`

**注意:** 引用必须初始化, 初始化后就不可以更改;

**示例:**

``` c++
int a = 10;
int &b = a;
cout << a ;  // 10
cout << b ;  // 10
b = 20;
cout << a ;  // 20
cout << b ;  // 20
```



### 2.2 引用与函数



#### 2.2.1 引用做函数参数

**作用:** 函数传参时, 可以利用引用的技术让形参修饰实参

**优点:** 可以简化指针修改实参

**示例:**

``` c++
#include <iostream>
using namespace std;
// 地址传递
void func(int* a, int* b) {

	int temp = *a;
	*a = *b;
	*b = temp;
}
// 引用传递
void func2(int& a, int& b) {
	int temp = a;
	a = b;
	b = temp;
}
int main()
{
	int a = 10;
	int b = 20;
	func(&a, &b);    // 会改变
	cout << a << endl;
	cout << b << endl;
	func2(a, b);   //会改变
	cout << a << endl;
	cout << b << endl;
}
```

#### 2.2.2 引用做函数返回值

**作用:** 引用时可以作为函数的返回值存在的

**注意:** 不要返回局部变量的引用

**用法:** 函数调用作为左值

**示例:**

``` c++
#include <iostream>
using namespace std;
int& func1() {
	int a = 10;  // 局部变量  在堆区
	return a;
}
int& func2() {
	static	int a = 10;  // 静态变量  全局区   程序结束后系统释放
	return a;
}
int main()
{
	int& a = func1();
	cout << a << endl;   // 打印10
	cout << a << endl;  // 乱码 原因参考指针作为返回值
	int& a1 = func2();
	cout << a1 << endl;   // 打印10
	cout << a1 << endl;  // 打印10
}
```



### 2.3 引用本质

本质: 引用的本质在C++内部实现是一个指针常量(指向不可以修改的指针)

示例:

``` c++
#include <iostream>
using namespace std;
// 发现是引用 自动转化为 int* const ref = &a
void func(int& ref) {
	ref = 100;    // ref是引用 转化为	*ref = 100
}
int main()
{
	int a = 1;
	// 自动转化为 int* const a1 = &a    指针常量是指针指向不可更改, 也说明为什么引用不可更改
	int& a1 = a;
	a1 = 20;      // 内部发现a1是引用  自动转化为 *a1 = 20
	cout << a << endl;
	cout << a1 << endl;
	func(a);
}
```

### 2.4 常量引用

作用: 常量引用主要用来修饰形参, 防止误操作

示例: `void showValue(const int& a)`



## 03_函数



### 3.1 函数默认参数

**写法:**`void int fun(int a = 0, string b = "define"){.....}`



### 3.2 函数的默认参数

**写法:**`void void fun(int a =0 , int){...}`

**示例:**

``` c++
#include <iostream>
using namespace std;
void func(int a, int) {
	cout << "a =" << a << "b =" << endl;
}
int main()
{
	// 占位符必须写
	func(10, 20);
}
```



### 3.3 函数重载

**作用:** 和java一样  可以使用相同的方法名但是参数不同的方式调用不同的函数   提高复用性

**注意:** 

+ 同一个作用域下
+ 函数名相同
+ 参数类型, 个数, 顺序 不同区分
+ 函数的返回值不能作为函数重写的条件

**示例:**

``` c++
#include <iostream>
using namespace std;
void func(int a) {
	cout <<"我是一" << endl;
}
void func(string a) {
	cout << "我是二" << endl;
}
int main()
{
	// 占位符必须写
	func(100);
	func("www");
}
```



## 04_类和对象

C++是面向对象的 三大特征: 封装 继承  多态



### 4.1 封装

封装最好理解了。封装是面向对象的特征之一，是对象和类概念的主要特性。

封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。

语法: `class 类名 {访问权限: 属性  /  行为};`

访问权限: 

+ public
+ private
+ protected

**struct和class区别:**

两者唯一的区别在于默认的访问权限不同:

+ struct:  默认 公有
+ class:    默认 私有



**示例:**

``` c++
#include <iostream>
using namespace std;
const float PI = 3.14;
class Circle {
private:
	float r;
public:
	void setR(float r) {
		this->r = r;
	}
	float getR() {
		return this->r;
	}
	double calculateZC() {
		return 2 * PI * r;
	}
};
int main()
{
	Circle c1;
	c1.setR(2);
	cout << "周长 =" << c1.calculateZC();
	
}
```



### 4.2 对象的初始化和清理



#### 4.2.1 构造和析构函数

和java一样构造和析构是编译器自动调用的,如果程序员不去设置系统会调用默认的构造和析构

1. 构造函数:   同java

   构造函数是一种特殊的成员函数,它主要用于为对象分配空间,进行初始化。

2. 析构函数:  destory清理函数    java中的对象清理是jvm自动的

   析构函数也是一种特殊的成员函数。它执行与构造函数相反的操作,通常用于撤消对象时的一些清理任务,如释放分配给对象的内存空间等。





#### 4.2.2构造函数的分类和调用

两种分类方式:

+ 按参数类型分: 有参构造和无参构造
+ 按类型分:  普通构造和拷贝构造

三种调用方式:

+ 括号法
+ 显示法
+ 隐式转换法

示例:

``` c++
#include <iostream>
using namespace std;
class TestClass {
private:
	int age;
public:
	// 普通构造函数
	TestClass() {
		cout << "构造函数" << endl;
	}
	TestClass(int a) {
		this->age = a;
		cout << "构造函数age=" << this->age << endl;
	}
	// 拷贝构造函数
	TestClass(const TestClass& p) {
		this->age = p.age;				// 拷贝属性
		cout << "拷贝构造函数 age=" << p.age << endl;
	}
	int getAge() {
		return age;
	}
	~TestClass() {
		cout << "析构函数" << endl;
	}
};
int main()
{
	//调用
	// 1. 括号法
	TestClass t1;				//无参构造调用
	TestClass t2(20);			//有参构造调用
	TestClass t3(t2);			//拷贝构造调用
	// 2. 显示法
	TestClass c1;
	TestClass c2 = TestClass(30);
	TestClass c3 = TestClass(c2);
	TestClass(40);   // 匿名对象    特点: 当前执行结束后,系统会立即收回匿名对象
	TestClass(p2);   //不要利用拷贝构造函数初始化匿名对象  编译器会认为 TestClass(p2) === TestClass p2;  对象的声明 提示重复
	// 3. 隐式法
	TestClass d1 = 40;    // === TestClass d1(40)
	TestClass d2 = d1;   //  === TestClass d2(d1)
}
```



#### 4.2.3 拷贝构造函数调用时机

C++中拷贝构造函数调用时机有三种情况:

+ 使用一个已经创建完毕的对象来初始化一个新对象    纯复制
+ 值传递的方式给函数参数传递
+ 以值方式返回局部对象

示例:

``` c++
#include <iostream>
using namespace std;
class TestClass {
private:
	int age;
public:
	// 普通构造函数
	TestClass() {
		cout << "构造函数" << endl;
	}
	TestClass(int a) {
		this->age = a;
		cout << "构造函数age=" << this->age << endl;
	}
	// 拷贝构造函数
	TestClass(const TestClass& p) {
		this->age = p.age;				// 拷贝属性
		cout << "拷贝构造函数 age=" << p.age << endl;
	}
	int getAge() {
		return age;
	}
	~TestClass() {
		cout << "析构函数" << endl;
	}
};
// 使用一个已经创建完毕的对象来初始化一个新对象    纯复制
void fun1() {
	TestClass p1;
	TestClass p2(p1);
}
//值传递的方式给函数参数传递
void testFun2(TestClass p) {
	cout << "值传递的方式给函数参数传递" << endl;
}
void fun2() {
	TestClass p1;
	testFun2(p1);
}
// 返回值形式
TestClass testFun3() {
	TestClass t1;
	return t1;		//返回的是t1的拷贝构造函数  
}
void fun3() {
	testFun3;
}
int main()
{
	testFun3();
}
```



#### 4.2.4 构造函数调用规则

默认情况下,编译器会给一个类添加至少三个函数:

+ 无参构造  函数体为空
+ 无参析构  函数体为空
+ 拷贝构造  对属性值进行拷贝



调用规则:

+ 如果用户定义了有参构造    编译器不会再提供无参构造  但是会提供拷贝
+ 如果用户定义了拷贝构造    编译器不会再提供其他所有的构造



#### 4.2.5 深拷贝与浅拷贝

+ 浅拷贝: 简单的复制拷贝操作    带来的问题:
  + 如果以传进来的是一个new的地址(在堆区申请一个空间) 浅拷贝会单纯的复制地址
  + 然而析构函数会释放new出来的地址 
  + 堆区释放规则时 后进先出
  + 所以第一次调用函数时   拷贝地址 然后析构释放(new出的地址)
  + 第二次调用时  拷贝地址  但是new的地址已经被释放了  导致重复释放编译器报错

+ 深拷贝: 在堆区重新申请空间  进行拷贝操作
  

#### 4.2.6 初始化列表

语法: `构造函数():属性1(值1), 属性2(值2).....`{}

``` c++
#include <iostream>
using namespace std;
class Test {
public:
	int num;
	string name;
	int age;
	Test(int num, string name, int age) :num(202), name("haha"), age(32) {
	}
};
int main()
{
	Test t1(10,"hahah",30);
	t1.num;
	t1.name;
	t1.age;
}
```



#### 4.2.7 类对象作为类成员

和java一样



#### 4.2.8 静态成员

使用关键字`static`修饰的变量或者方法

**静态成员变量:**

+ 所有对象共享同一份数据
+ 在编译阶段分配内存
+ 类内声明, 类外初始化

**静态成员函数:**

+ 所有对象共享同一个函数
+ 静态函数只能访问静态成员变量

**示例:**

``` c++
#include <iostream>
using namespace std;
int Test::a = 10;      // 类外初始化
class Test {
public: 
	static void fun1() {
		a = 30;
		cout << "静态函数测试" << endl;
	}
	static int a;  // 静态成员变量
};
int main()
{
	// 1. 通过对象访问
	Test t1;
	t1.fun1;
	// 2. 通过类名访问
	Test::fun1();
}
```



### 4.3 对象模型和this指针



#### 4.3.1 成员变量和成员函数分开存储

c++的成员变量和成员函数分开存储

只有非静态成员变量才属于类的对象上





#### 4.3.2 this指针

this指针的用途:

+ 当形参和成员变量同名时候, 可以使用this指针区分
+ 在类的非静态成员函数中返回对象本身,  可以使用`return *this`



#### 4.3.3 空指针访问成员函数

C++中空指针时可以调用成员函数的

``` c++
class A{
public:
    int a = 1;
    void fun(){
        cout << "aaa";
    }
    void fun2(){
        cout << a;    // 报错 因为 a实质时this->a     类没有实例化所以this是空
    } 
}
int main(){
    A* p = NULL;
    p-> fun();
    p-> fun2();   //报错  原因如上
}
```



#### 4.3.4 const修饰成员函数

常函数:

+ 成员函数后加const后我们称之为这个函数是常函数
+ 常函数不可以修改成员属性
+ 成员属性声明`mutable`后,  常函数内就可以修改了

常对象:

+ 声明对象前加const称该对象是常对象
+ 常对象只能调用常函数



### 4.4 友元

友元的目的是让一个函数或者类 访问另一个类中的私有成员

**关键字**:`friend`

**三种实现**:

+ 全局函数做友元
+ 类做友元
+ 成员函数做友元

**实现总结:**

+ 类或者方法 或者类作用域方法    在有私有成员的类中首行带有`friend`声明就可以了

**示例:**

``` c++
#include <iostream>
using namespace std;

class MyClass {
	friend void fun1();       //必须写在前面如此一来就可以访问私有成员变量了   不仅是函数 类也可以  
private:
	string b = "bbbbb";
public:
	string a = "aaaa";
};

// 全局函数
void fun1() {
	MyClass mc;
	mc.a;
	mc.b;
}
int main()
{
}
```



### 4.5运算符重载

运算符重载: 对已有的运算符重新进行定义, 赋予其另一种功能, 以适用不同的数据类型



#### 4.5.1 加号运算符重载

作用: 实现两个自定义数据类型相加的运算

实现过程: 

+ 先重写写一个函数实现两个对象相加的具体操作
+ 这个函数的参数必须传入一个对象, 函数类型必须是一个类的名字
+ 然后把函数名换成`operator+`即可

**示例:**

``` c++
#include <iostream>
using namespace std;
// 加法重载
class Persion {
public:
	// 成员变量
	int m_A;
	int m_B;

	// 1. 成员函数加法重载
	//Persion operator+(Persion& p) {
	//	Persion temp;
	//	temp.m_A = this->m_A + p.m_A;
	//	temp.m_B = this->m_B + p.m_B;
	//	return temp;
	//}

};
// 2. 成员函数的加法重载
Persion operator+(Persion& p1, Persion& p2) {
	Persion temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;

}
void test01() {
	Persion p1;
	p1.m_A = 10;
	p1.m_B = 20;

	Persion p2;
	p2.m_A = 30;
	p2.m_B = 40;
    
	// 本质
	//Persion p3 = p1.operator+(p2);       // 成员函数重载
	//Persion p3 = operator+(p1, p2);		//  全局函数重载
	Persion p3 = p1 + p2;
	cout << "成员函数加法重载" << "  p3.m_A=" << p3.m_A << "  p3.m_B=" << p3.m_B << endl;
}
int main() {
	test01();
}
```



#### 4.5.2 左移运算符重载

作用: 可以输出自定义数据类型   类似于java类的`tostring`

使用: 只能利用全局函数重载左移运算符   

**示例:**

``` c++
#include <iostream>
using namespace std;
class Persion {
public:
	int num;
	string name;
	int age;
	Persion(int num, string name, int age) :num(num), name(name), age(age) {}
};
void operator<<(ostream& cout, Persion& p) {
	cout << "num = " << p.num << "  name=" << p.name << "  age =" << p.age << endl;
}
int main() {
	Persion p1(1111, "张三", 18);
	cout << p1;
}
```



#### 4.5.3 递增运算符重载

作用: 通过重载递增运算符, 实现自己的整形数据

说人话:  更改累加累减的作用效果  相当与java的增强器   没什么用其实

示例:

``` c++
#include <iostream>
using namespace std;
class Persion {
public:
	int num;
	Persion(int num) :num(num) {}
	// 前置++重载
	Persion& operator++() {			// 返回引用数据类型  多次操作后 还是对原数据进行操作 不会新实例化对象
		num++;
		return *this;			// 返回自身   this是指向自身的  *是解引用 
	}
	// 后置++重载
	Persion operator++(int) {		// int是占位参数      用于区分前置和后置重载   而且只能写int
		//  先  记录当时的结果
		Persion temp = *this;
		// 后 递增
		this->num++;
		//最后将记录的结果返回
		return temp;
	}
};
ostream& operator<<(ostream& cout, Persion& p) {
	cout << "num = " << p.num << endl;
	return cout;
}
int main() {
	Persion p1(1111);
	cout << "++p=" << ++p1 << endl;
	cout << "p = " << p1 << endl;
	 p1++;
	cout << "p++ = " << p1 << endl;
}
```



#### 4.5.4 赋值运算符重载

c++一共会给一个类添加四个函数:

+ 构造函数
+ 拷贝构造函数
+ 析构函数
+ 赋值运算符`operator+`    对属性值进行拷贝

如果类中有属性指向堆区, 则会出现深浅拷贝问题

```c++
#include <iostream>
using namespace std;
class Persion {
public:
    int *m_Age;
    Persion(int age) {
        m_Age = new int(age);
    }
    ~Persion() {
        if (m_Age != NULL) {
            delete m_Age;
            m_Age = NULL;
        }
    }
    // 重载 赋值运算符
    Persion &operator=(Persion &persion) {
        // 编译器提供浅拷贝
//        m_Age = persion.m_Age;

        // 先判断是否有属性在堆区,   如果有就先释放干净  然后再深拷贝
        if (m_Age != NULL) {
            delete m_Age;
            m_Age = NULL;
        }
        // 深拷贝
        m_Age = new int(*persion.m_Age);
        return *this;
    }
};
void test1() {
    Persion p1(18);
    Persion p2(28);
    Persion p3(33);
    p3 = p2 = p1;
    cout << "p1.age = " << *p1.m_Age << endl;
    cout << "p2.age = " << *p2.m_Age << endl;
    cout << "p3.age = " << *p3.m_Age << endl;
}
int main() {
    test1();
}
```



#### 4.5.5 关系运算符重载

作用: 两个对象比较操作





#### 4.5.6 函数调用运算符重载

介绍:

+ 函数调用运算符()也可以重载
+ 由于重载后使用方式非常像函数的调用, 因此又称为仿函数
+ 仿函数没有固定的写法, 使用起来非常灵活

``` c++
#include <iostream>
using namespace std;
class Persion {
public:
    // 尝试做一个打印操作
    void operator()(string test) {
        cout << "test= " << test << endl;
    }
};
void test2(string test){
    cout << "test2 = "<< test <<endl;
}
void test1() {
    Persion p1;
    p1("HELLO WORD");           // 重载
    test2("HELLO WORD");        // 正常使用
}
int main() {
    test1();
}
```



### 4.6 继承

**继承执行顺序: **   子类继承父类

1. 构造子类对象时     先执行父类构造 再执行子类构造
2. 析构时候                 先执行子类析构   再执行父类析构











语法: `class 类名: 继承方式 父类名{.....}`

``` c++
#include <iostream>
using namespace std;
 class BaseClass{
 public:
     void fun1(){
         cout << "我是一"<< endl;
     }

     void fun2(){
         cout << "我是二"<< endl;
     }
     void fun3(){
         cout << "我是三"<< endl;
     }
     void fun4(){
         cout << "我是四"<< endl;
     }
 };
class Son1: public BaseClass {
public:
    void fun5(){
        cout << "我是五" << endl;
    }
};
class Son2: public BaseClass {
public:
    void fun4(){
        cout << "我是五" << endl;
    }
};
int main() {
    Son1 s1;
    s1.fun1();
    s1.fun2();
    s1.fun3();
    s1.fun4();
    s1.fun5();

    Son2 s2;
    s2.fun1();
    s2.fun2();
    s2.fun3();
    s2.fun4();
}
```



#### 4.6.1 继承方式

三种继承方式:

+ 公共继承 :   public
+ 保护继承:    protected
+ 私有继承:    private



#### 4.6.2 多继承

语法: `class 类名: 继承方式 父类名, 继承方式 父类名{.....}`

注意:

+  多继承时可能会出现同名情况 需要加作用域区分
+ 但是不建议使用多继承



#### 4.6.3 菱形继承

解决方式:  使用虚继承  在继承方式前加`virtual`



### 4.7 多态

**多态分为两类:** 

+ 静态多态:    函数重载和运算符重载属于静态多态, 复用函数名
+ 动态多态:    派生类和虚函数实现运行时的多态

**区别:**

+ 静态多态的函数地址早绑定:      编译阶段确定函数地址
+ 动态多态的函数地址晚绑定:      运行阶段确定函数地址

**动态绑定要求:**

+ 有继承关系
+ 子类重写父类的虚函数 

**示例:**

1. 早绑定:  出现的问题是无论等号右边如何 左边已经固定了   `Animal animal = new Cat();`

   ``` c++
   #include <iostream>
   using namespace std;
   class Animal {
   public:
       void speak() {
           cout << "动物在说话" << endl;
       }
   };
   class Cat : public Animal {
   public:
       void speak() {
           cout << "猫在说话" << endl;
       }
   };
   class Dog : public Animal {
   public:
       void speak() {
           cout << "狗在说话" << endl;
       }
   };
   //地址早绑定   在编译阶段确定函数地址    如此一来 相等于java的 Animal animal = new Cat();    无论如何都是Animal类型的对象
   void doSpeak(Animal &animal){
       animal.speak();
   }
   void test1() {
       Cat cat;
       doSpeak(cat);
   }
   int main() {
       test1();
   }
   ```

2. 晚绑定:  在父类的方法前加`virtual`    只有在运行的时候才执行   相等于等号的左边按照右边而决定   Cat animal = new Cat();`

   ``` c++
   #include <iostream>
   using namespace std;
   class Animal {
   public:
       // 虚函数
       virtual void speak() {
           cout << "动物在说话" << endl;
       }
   };
   class Cat : public Animal {
   public:
       void speak() {
           cout << "猫在说话" << endl;
       }
   };
   class Dog : public Animal {
   public:
       void speak() {
           cout << "狗在说话" << endl;
       }
   };
   //地址早绑定   在编译阶段确定函数地址    如此一来 相等于java的 Animal animal = new Cat();    无论如何都是Animal类型的对象
   void doSpeak(Animal &animal){
       animal.speak();
   }
   void test1() {
       Cat cat;
       doSpeak(cat);
   }
   int main() {
       test1();
   }
   ```



#### 4.7.1 纯虚函数和抽象类

> 等同于java的抽象类和抽象方法

**纯虚函数:**

+  语法: `virtual 返回值类型 函数名 (参数列表) = 0;`
+ 当类中有纯虚函数,  这个类也就称为抽象类

**抽象类:**

+ 无法实例化
+ 子类必须重写抽象类中的纯虚函数,  否则子类也时抽象类





#### 4.7.2 虚析构和纯虚析构

**注意:** 多态使用时, 如果子类中有属性开辟到堆区, 那么父类指针在释放时无法调用子类的析构代码

**解决方式:** 将父类中的析构函数改为虚析构或者纯虚析构

**语法:**

+ 虚析构:  `virtual ~类名(){}`
+ 纯虚析构:  `virtual ~类名() = 0;`

**虚析构和纯虚析构共同点:**

+ 解决父类指针释放子类对象
+ 都需要具体的函数实现

**虚析构和纯虚析构不同点:**

+ 如果是纯虚析构    则该类属于抽象类  无法实例化对象

**示例:**

```c++
#include <iostream>
using namespace std;
class Animal{
public:
    Animal(){
        cout << "Animal的构造函数" <<endl;
    }
    virtual void speak() = 0;
    ~Animal(){
        cout << "Animal的析构函数" <<endl;
    }
};
class Cat:public Animal{
public:
    string *m_Name;
    Cat(string name) {
        cout << "CAT的构造函数调用"  << endl;
        this->m_Name =  new string(std::move(name));
    }
    // 重写父类纯虚函数
    virtual void speak(){
        cout  << *m_Name  << "小猫在说话" << endl;
    }
    virtual ~Cat() {                // 利用虚析构 可以通过父类释放子类的堆内存的指针对象
        if (m_Name != NULL) {
            cout << "CAT的析构函数"  << endl;
            delete m_Name;     //delete后跟一个指针
            m_Name = NULL;
        }
    }
};
void test1(){
    Animal *animal = new Cat("Tom");
    animal -> speak();
    delete animal;          //  堆内存的指针对象需要手动释放  不然不会调用析构
}
int main(){
    test1();
}
```



## 05_文件

通过文件可以将数据进行持久化

**c++文件操作的头文件:** `include <fstream>`

**文件操作三种情况:**

+ ofstream:    写操作 
+ ifstream:      读操作
+ fstream:       读写操作

### 5.1 文本文件

#### 5.1.1 写操作

步骤:

1. 包含头:   `include <fstream>`
2. 创建流对象:   `ofstream ofs`
3. 打开文件:    `ofs.open("文件路径", 打开方式) `        打开方式可以写多个用 `|`间隔

4. 写数据:       `ofs << "写入数据";`

5. 关闭文件:    `ofs.close();`

文件打开方式:
| 打开方式     | 解释         |
| ----------- | ---------------------------- |
| ios::in     | 为输入(读)而打开文件         |
| ios::out    | 为输出(写)而打开文件         |
| ios::ate    | 初始位置：文件尾             |
| ios::app    | 所有输出附加在文件末尾       |
| ios::trunc  | 如果文件已存在则先删除该文件 |
| ios::binary | 二进制方式                   |

#### 5.1.2 读操作

步骤:

1. 包含头:   `include <fstream>`
2. 创建流对象:   `ifstream ifs`
3. 打开文件:    `ifs.open("文件路径", 打开方式) `        f返回打开是否成功

4. 读文件:   四种读取方式

5. 关闭文件:    `ifs.close();`

读取方式:

1. 使用数组 

   ``` C++
   char buf[2048] = {0};
   while(ifs >> buf){     // ifs>>buf会一行一行的读取  没有数据返回false
   	cout << buf <<endl;    // 每次返回一行的数据
   }
   ```

2. 使用数据  ifs的成员函数

   ``` c++
   char buf[2048] = {0};
   while(ifs.getline(buf, sizeof(buf))){   // 以行的方式读  第一个参数读取行数据存放哪里(指针)  第二个参数一行读多少个字符
       cout << buf <<endl;    // 每次返回一行的数据
   }
   ```

3. 使用string

   ``` c++
   string str;
   while(getline(ifs, str)){  // 以行的方式读    第一个参数 输入流对象 第二个参数 数据放哪里
       cout << str << endl;
   }
   ```

4. 一个一个字符读

   ``` c++
   char c;
   while((c = ifs.get()) != EOF){  //EOF = End Of File 是否读尾了 
       cout << c;
   } 
   ```

   





### 5.2 二进制文件

打开方式指定为: `ios::binary`

#### 5.2.1 写文件

二进制写文件主要利用流对象调用成员函数write

**函数原型:** `ostream& write(const char* buffer , int len);`

**参数:** buffer指针指向内存中一段存储空间    len是读写的字节数

```c++
#include <iostream>
#include <fstream>
using namespace std;

class Person{
private:
    char *name = new char[64];
    int age;

public:
    Person(char *name, int age) : name(name), age(age) {}

    const char *getName() const {
        return name;
    }

    int getAge() const {
        return age;
    }

    void setAge(int age) {
        Person::age = age;
    }

    virtual ~Person() {
        if(name != nullptr) {
            cout << "析构了" << endl;
            delete[] name;
            name = nullptr;
        }
    }
};
void operator<<(ostream& cout, Person& p){
    cout << "p.name=" << p.getName() << "  p.age=" << p.getAge() << endl;
}


void test1(){
    Person p1("张三", 17);
    ofstream ofs("1.txt", ios::out);
    ofs.write((const char*)&p1, sizeof(p1));

    ofs.close();
    cout << p1;
    delete &p1;

}

int main(){
    test1();
}
```



#### 5.2.2 读文件

二进制写文件主要利用流对象调用成员函数read

**函数原型:** `ostream& read(char* buffer , int len);`

**参数:** buffer指针指向内存中一段存储空间    len是读写的字节数

**示例:**

``` c++
#include <iostream>
#include <fstream>
using namespace std;
class Person {
private:
    char *name = new char[64];
    int age;
public:
    Person() {}

    Person(char *name, int age) : name(name), age(age) {}

    const char *getName() const {
        return name;
    }
    int getAge() const {
        return age;
    }
    void setAge(int age) {
        Person::age = age;
    }
    virtual ~Person() {
        if (name != nullptr) {
            cout << "析构了" << endl;
            delete[] name;
            name = nullptr;
        }
    }
};
void operator<<(ostream &cout, Person &p) {
    cout << "p.name=" << p.getName() << "  p.age=" << p.getAge() << endl;
}
void test1() {
    Person p1;
    ifstream ifs("1.txt", ios::in | ios::binary);
    ifs.read((char *) &p1, sizeof(p1));             // 把内容写回p对象中
    ifs.close();
    cout << p1;
    delete &p1;
}
int main() {
    test1();
}
```

