# Go



## 01_准备工作

### 1.1 下载包

1. 谷歌下载https://golang.org/dl/

2. 国内一些镜像下载

### 1.2 环境变量

1. 下载解压  例如C:\go
2. 新建环境变量 GOROOT: C:\go
3. PATH添加  %GOROOT%\bin
4. 测试 go version

### 1.3 IDE选择

1. vscode+Go插件
2. Goland(Jetbrains)

## 02_HelloWord

``` go
package main //表示当前程序所在的包
import "fmt"
// 主函数
func main() { // 左花括号必须和函数名同一行
	// 可加分号也可以不加 推荐不加
	fmt.Println("Hello World")
}
```



## 03_变量常量声明

> fmt是go的标准化I/O   
>
> 1. print,println区别是回车 当然前者也可以使用\n     
> 2. printf 格式化字符串  可以将后面参数的值赋给第一个参数



### 3.1 变量类型

| 序号 | 类型和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **布尔型** 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。 |
| 2    | **数字类型** 整型 int 和浮点型 float，Go 语言支持整型和浮点型数字，并且原生支持复数，其中位的运算采用补码。 |
| 3    | **字符串类型:** 字符串就是一串固定长度的字符连接起来的字符序列。Go的字符串是由单个字节连接起来的。Go语言的字符串的字节使用UTF-8编码标识Unicode文本。 |
| 4    | **派生类型:** 包括：(a) 指针类型（Pointer）(b) 数组类型(c) 结构化类型(struct)(d) 联合体类型 (union)(e) 函数类型(f) 切片类型(g) 接口类型（interface）(h) Map 类型(i) Channel 类型 |



### 3.2 局部变量

1. var a int   //默认0
2. var a int = 10
3. var a = 20
4. a:= 30



### 3.3 全局变量

在方法外使用

上述1,2,3可以用  4不行

也可以使用多行声明

### 3.4 声明多个变量

1. 直接声明

   + var aa, bb int = 100, 200
   + var aa, bb = 100 ,200
   + var aa, bb = true, "aaa"

2. 多行声明

   ```go
   var(
      a1 int = 200
      a2 bool = true
   )
   ```



### 3.5 常量

关键字: const  ,  iota(iota只能在const中使用)

```go
// const定义常量   一般用于枚举
// 1. 定义常量   即不可改变
const aaa = 1
// 2. 定义枚举
const (
   A1= 1
   A2 = 2
   A3 =3
)
// 3. 使用iota关键字实现自动增加
const (
   B1 = iota     // iota从0开始 iota = 0
   B2    // iota = 1
   B3    // iota = 2
   B4 = 10*iota
   B5
   B6
   B7 = iota+1
   B8
)
```





### 3.5 示例

```go
package main
import "fmt"
// 声明全局变量 方法1.2.3都可以 4不行
var hhh = 1000
// const定义常量   一般用于枚举
// 1. 定义常量   即不可改变
const aaa = 1
// 2. 定义枚举
const (
   A1= 1
   A2 = 2
   A3 =3
)
// 3. 使用iota关键字实现自动增加
const (
   B1 = iota     // iota从0开始 iota = 0
   B2    // iota = 1
   B3    // iota = 2
   B4 = 10*iota
   B5
   B6
   B7 = iota+1
   B8
)
// := 只能声明局部变量
func main() {
   // 1. var声明 a变量名  int数据类型 不初始化默认是0
   var a int
   fmt.Println(a)
   fmt.Printf("类型是 %T\n",a)
   // 2. 声明一个变量直接初始化
   var b int= 10
   fmt.Println(b)
   fmt.Printf("类型是 %T\n",b)
   // 3. 省去类型
   var c = 20
   fmt.Println(c)
   fmt.Printf("类型是 %T\n",c)
   // 4. 省去var   常用
   // %v自动匹配类型 其实也有f float s string .....
   e := 30
   fmt.Printf("e = %v , type = %T\n",e,e)
   f := 3.14
   fmt.Printf("f = %v , type = %T\n",f,f)
   g := "3.14"
   fmt.Printf("g = %v , type = %T\n",g,g)
   // 声明多个变量
   var aa, bb int = 100, 200
   fmt.Printf("aa = %v , type = %T\n",aa, aa)
   fmt.Printf("bb = %v , type = %T\n",bb, bb)
   var cc ,dd = 111, "ddd"
   fmt.Printf("cc = %v , type = %T\n",cc, cc)
   fmt.Printf("dd = %v , type = %T\n",dd, dd)
   // 多行变量声明
   var(
      a1 int = 200
      a2 bool = true
   )
   fmt.Printf("a1 = %v , type = %T\n",a1, a1)
   fmt.Printf("a2 = %v , type = %T\n",a2, a2)
}
```



## 04_函数



### 4.1 写法

func 函数名(参数名 参数类型, ....)  返回值{

.......

}

### 4.2 调用+放回值

```go
package main
import "fmt"
// 单返回值
func test1(a string, b int) string {
   fmt.Println("a = ", a)
   fmt.Println("b = ", b)
    c := "返回值"
   return c
}
// 多返回值 匿名
func test2(a string, b int) (int,int) {
   fmt.Println("a = ", a)
   fmt.Println("b = ", b)
   return 11, 22
}
// 多返回值 有名称
func test3(a string, b int) (a1 int,a2 int) {   // 也可以a1, a2  int
   fmt.Println("a = ", a)
   fmt.Println("b = ", b)
   a1 = 11
   a2 =22
   return 11, 22
}
func main()  {
   c := test1("aaa", 2)
   fmt.Println("c = ",c)

   d, e := test2("aaa", 2)
   fmt.Println("d = ",d, "e = ",e)

   f, g := test2("aaa", 2)
   fmt.Println("f = ",f, "g = ",g)

}
```



### 4.3 函数执行过程

1. go语言通过包名区分文件
   + 通过包名的不同文件被视为一个文件(不能使用相同的变量名,方法名)
2. go语言也有init()函数,一定程度上相当于构造
3. <font color="red">多包调用过程(注意和其他语言不一样)</font>
   1. 从main包入口进入
   2. 检查是否有import其他包
   3. 如果有先执行其他包的import->初始化init()函数->const变量->var变量
   4. 最后执行main的init()函数
   5. 最最后开始main方法中的逻辑片段
4. 值得一提
   + 变量或者包,如果导入了或创建了不使用会报错
     + 如果非要这么做可以加入匿名的关键字  在"他们"前加下划线和空格即"_ "但是不能再使用包的方法了只能自动的执行init()
   + 调用其他包的方法可以有两个有意思的关键字
     + `字符串 包路径` 此时这个字符串就是这个包的别名,方法调用可以使用别名.方法名
     + `. 包路径` 此时会将包导入到当前包中,使用起来就像自己包一样,方法可以直接调用































