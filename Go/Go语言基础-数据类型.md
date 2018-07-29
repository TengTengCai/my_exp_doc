# Go语言基础-数据类型

[TOC]

## 变量

Go语言是静态语言，不能在运行期间改变变量类型。

在使用变量之前需要对变量进行定义，自动初始初始化为零值。可省略变量类型，由编译器自动判断。

```go
package main

var x int					//定义变量x 整形
var f float32 = 1.6			//定义变量f 32位浮点数
var s  =  "abc"				//定义变量s	自动判断为字符串类型
var s1, n = "abcd", 123		//一次定义两个变量 ，自动判断变量类型
var (
	a int					//定义变量a 整形
    b float32				//定义变量b	32位浮点数
)

func main() {
	a := 0					//在函数内部，可使用：=来更简略的方式定义变量
    b1, b2, b3 := 10, 010, 0x100  //不同进制的表示
    data, i := [3]int{0, 1, 2}, 0	//多变量赋值时，先计算相关值，然后再从左到右依次赋值
    i, data[i] = 2, 100
	println("Hello World!")	//控制台输出
	println(a)				//输出变量
    println(b1, b2, b3) 	//在函数内部，如果声明的局部变量未被使用，编译器会报错
    println(i, data[0])		
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note00.go 
Hello World!
0
10 8 256
2 100

```

在Go语言中，`package main`中的`main`方法为程序的入口。（类似C语言和Java中的main方法）

”\_“为特殊只写变量，用于忽略值占位（对"\_"进行写赋值，无法读取“\_”的值）

```go
package main

func main() {
	_, s := test()			//test方法返回了两个参数， 1的整形和abc的字符串，整形未被引用所以会引发编译错误，使用_来占位
	println(&s)				//输出指针指向的地址

	s, y := "qwe", 20		//由于有新的变量所以使用了：=进行了定义和赋值
	println(&s, y)			//输出指针指向的地址
	{						//在新的代码块下编写
		s, z := 100, 30		//新的代码块下，变量位于新的内存区域，变量是属于另外的一个新的变量，与前面的变量s并不冲突
		println(&s, z)		//输出指针指向的地址
	}
	s = "asd"				//重新赋值
	println(&s)				//原地址不变
}

func test()(int, string)  {
	return 1, "abc"
}

```

运行结果

```bash
[tianjun@TengTengCai GoNotes]$ go run note01.go 
0xc42005bf68
0xc42005bf68 20
0xc42005bf60 30
0xc42005bf68
```

> **在开发中尽量避免使用全局变量**

## 常量

常量值必须是编译期可确定的数字、字符串、布尔值。

```go
package main

const x2, y2 int = 1, 2		//定义多个常量
const s2 string = "asdas"
const (
	a2, b2 = 10, 100		//批量定义
	c2 bool = false
	d2 = "asd"
	f2				// 如果不提供类型和初始化值，那么视作与上一常量相同 f2 = "asd"
)


func main(){
	const x = "xxx"		//未使用局部常量不会引发编译错误
	println(f2)			
}
```

```go
package main

import "unsafe"

const (
	// 常量值还可以是len、cap、unsafe.Sizeof
	a3 = "abc"				// 初始化字符串
	b3 = len(a3)			// 字符串长度
	c3 = unsafe.Sizeof(b3)	// b3变量的大小

	//如果常量类型足以存储初始化值，那么不会引发溢出错误
	d3 byte = 100
	//e3 int = 1e20    //该常量为float64位，int无法存储，发生溢出
)

func main(){
	println(b3, c3)
}
```

### 枚举

关键字`iota`	定义常量组中从0开始按行计数的自增枚举值。

```go
package main

const (
	Sunday = iota	// 0
	Monday			// 1
	Tuesday         // 2
	Wednesday		// 3
	Thursday		// 4
	Friday			// 5
	Saturday		// 6
)

const (
	// << 二进制左移位运算符， 剩余位用0填补
	_		 = iota				// iota = 0
	KB int64 = 1 << (10 * iota) // iota = 1
	MB							// 与KB表达式相同， 但iota = 2
	GB
	TB
)

const (
    // 在同一常量组中，可以提供多个iota，它们各自增长
	X, Y = iota, iota << 10		// 0, 0
	Z, W						// 1, 1 << 10 = 1024
)

const (
    // 如果iota自增被打断，需显式恢复。
	A = iota					// 0
	B							// 1
	C = "C"						// "C"
	D = "D"						// "D"
	E = iota					// 4
	F							// 5
)

func main() {
	println(Saturday, KB, Z, W)
	println(A, C, D, E, F)
}
```

运行结果

```bash
[tianjun@TengTengCai GoNotes]$ go run note04.go
6 1024 1 1024
0 C D 4 5
```

通过自定义类型来实现枚举类型限制。

```go
package main

type Color int

const (
	Black Color = iota
	Red
	Blue
)

func test5(c Color){
	println(c)
}

func main() {
	c5 := Black
	test5(c5)

	x5 := 1
	//test5(x5)			// 编译出错，x5的类型为int，test5接受的参数是Color
	test5(Color(x5))  	// 类型强转为Color

	test5(1)			// 常量会被编译器自动转换
}
```

## 基本类型

明确的数字类型命名，支持Unicode，支持常用数据结构。

| 类型          | 长度 | 默认值 | 说明                             |
| ------------- | ---- | ------ | -------------------------------- |
| bool          | 1    | false  |                                  |
| byte          | 1    | 0      | uint8                            |
| rune          | 4    | 0      | Unicode Code Point, int32        |
| int, uint     | 4或8 | 0      | 32位或64位                       |
| int8, uint8   | 1    | 0      | -128 ~ 127, 0 ~ 255              |
| int16, uint16 | 2    | 0      | -32768 ~ 32767, 0 ~ 65535        |
| int32, uint32 | 4    | 0      | -21亿 ～ 21亿， 0 ～42亿         |
| int64, uint64 | 8    | 0      |                                  |
| float32       | 4    | 0.0    |                                  |
| float64       | 8    | 0.0    |                                  |
| complex64     | 8    |        |                                  |
| complex128    | 16   |        |                                  |
| uintptr       | 4或8 |        | 足以存储指针的uint32或uint64整数 |
| array         |      |        | 值类型                           |
| struct        |      |        | 值类型                           |
| string        |      | “”     | UTF-8 字符串                     |
| slice         |      | nil    | 引用类型                         |
| map           |      | nil    | 引用类型                         |
| channel       |      | nil    | 引用类型                         |
| interface     |      | nil    | 接口                             |
| func          |      | nil    | 函数                             |

支持八进制、十六进制，以及科学计数法。标准库math定义了各数字类型取值范围。

```go
package main

import "math"

func main() {
	a6, b6, c6 := 071, 0x1F, 1e9
	println(a6, b6, c6)
	println(math.MinInt8)			// int8最小值
	println(math.MaxInt8)			// int8最大值
	println(math.MinInt16)			// int16最小值
	println(math.MaxInt16)			// int16最大值
	println(math.MinInt32)			// int32最小值
	println(math.MaxInt32)			// int32最大值
	println(math.MinInt64)			// int64最小值
	println(math.MaxInt64)			// int64最大值
	//println(math.MaxUint64)		// 内存溢出，int64
	println(math.MaxFloat32)		// float32最大值
	println(math.MaxFloat64)		// float64最大值
}

```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note06.go 
57 31 +1.000000e+009
-128
127
-32768
32767
-2147483648
2147483647
-9223372036854775808
9223372036854775807
+3.402823e+038
+1.797693e+308
```

## 引用类型

引用类型包括slice、map、和channel。它们有复杂的内部结构，除了申请内存外，还需要初始化相关属性。

内置函数new计算类型大小，为其分配零值内存，返回指针。而make会被编译器翻译成具体的创建函数，由其分配内存和初始化成员结构，返回对象而非指针。

```go
package main

func main() {
	a7 := []int{0, 0, 0}		// 初始化表达式
	a7[1] = 10
	println(a7)

	b7 := make([]int, 3)		// make slice
	b7[1] = 10
	println(b7)

	c7 := new([]int)			// new slice point
	println(c7)
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note07.go 
[3/3]0xc42005df48
[3/3]0xc42005df30
0xc42005df60
```

## 类型转换

不支持隐式类型转换，即便是从窄向宽转换也不行。

```go
package main

func main() {
	var b byte = 100
	// var a int = b		// 不能将byte类型的b赋值给int类型的a
	var n = int(b)
	var c = string(67)		// int转String字节
	var s = 'B'
	var d = int(s)			// String字节转int
	var e = float32(n)		//  int转float32
	var e1 = float64(n)		// int转float64
	println(n, c, d, e, e1)

    // 所有语言多会出现的问题，浮点数相加，并不会完全等于日常中的小数相加
    //(计算机是通过二进制进行小数相加的)
	a1 := 0.1
	b1 := 0.2
	// 这里总会是false
	if (a1 + b1) == 0.3 {
		println(true)
	}else {
		println(false)
	}
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note08.go 
100 C 66 +1.000000e+002
false
```

同样的不能将其他类型当bool值使用。

```go
a := 100
if a {				// non-bool x (type int) used as if condition
    println("true")
}
```

## 字符串

字符串是不可变值类型，内部用指针指向UTF-8字节数组。

- 默认值是空字符串“”。
- 用索引号访问某字节， 如s[i]。
- 不能用序号获取字节元素指针，&s[i]非法。
- 不可变类型， 无法修改字节数组。
- 字节数组尾部不包含NULL。

```go
package main

import "fmt"

func main() {
	// 使用索引号访问字符（byte）
	s := "abc"
	println(s[0] == '\x61', s[1] == 'b', s[2] == 0x63)

	// 使用“`”定义不做转义处理的原始字符串，支持跨行。
	s1 := `a
b\r\n\x00
c`
	println(s1)

	// 连接跨行字符串时， "+" 必须在上一行末尾，否则导致编译错误。
	s2 := "Hello, " +
		  "World!"

	// 支持用两个索引号返回子串。子串依然指向原字节数组，仅修改了指针和长度属性。
	s3 := s2[:5]
	s4 := s2[7:]
	s5 := s2[1:5]
	// 可以看出这是一个连续的地址，属于浅拷贝
	println(s2, &s2)
	println(s3, &s3)
	println(s4, &s4)
	println(s5, &s5)

	// 单引号字符常量表示Unicode Code Point， 支持\uFFFF、\U7FFFFFFF、\xFF格式。
	// 对应rune类型
	fmt.Printf("%T\n", 'a')
	fmt.Println("asd")

	var c1, c2 rune = '\u6211', '们'
	println(c1 == '我', string(c2) == "\xe4\xbb\xac")
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note09.go 
true true true
a
b\r\n\x00
c
Hello, World! 0xc42005df48
Hello 0xc42005df38
World! 0xc42005df28
ello 0xc42005df18
int32
asd
true true
```

要修改字符串，可先将其转换成`[]rune`或`[]byte`， 完成后再转换为`string`。无论哪种转换，都会重新分配内存，并复制字节数组。

```go
package main

func main() {
	s := "abcd"
	bs := []byte(s)

	bs[1] = 'B'
	println(string(bs))

	u := "电脑"
	us := []rune(u)
	us[1] = '话'

	println(string(us))
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note10.go 
aBcd
电话
```

用for循环遍历字符串时，也有`byte`和`rune`两种方式。

```go
package main

import "fmt"

func main(){
	s := "abc汉字"

	// byte
	for i := 0; i < len(s); i++ {
		fmt.Printf("%c,", s[i])
	}

	fmt.Println()

	// rune
	for _, r := range s  {
		fmt.Printf("%c,", r)
	}
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note11.go
a,b,c,æ,±, ,å,­, ,
a,b,c,汉,字,
```

## 指针

支持的指针类型`*T`， 指针的指针`**T`，以及包含包名前缀的`*<package>.T`。

- 默认值为`nil`, 没有`NULL`常量。
- 操作符“&” 取变量地址， “*”透过指针访问目标对象。
- 不支持指针运算，不支持“->”运算符，直接使用“.”访问目标成员。

```go
package main

import "fmt"

func main() {
    // 结构化数据，自定义数据类型
	type data struct {
		a int
	}
	// 定义赋值
	var d = data{1234}
	
    // 定义变量
	var p *data
	// 赋值指针
	p = &d
	// 调用输出
	fmt.Printf("%p, %v\n",p,p.a)
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note12.go
0xc4200180d0, 1234
```

可以在 unsafe.Pointer 和任意类型指针间进行行转换。

```go
package main

import (
	"unsafe"
	"fmt"
)

func main(){
	x := 0x12345678

	p := unsafe.Pointer(&x)
	n := (*[6]byte)(p) 		// 指针类型转换

	// 遍历数组指针
	for i := 0; i < len(n); i++ {
		fmt.Printf("%X ", n[i])
	}
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note13.go 
78 56 34 12 0 0 
```

返回局部变量指针是安全的，编译器会根据需要将其分配在GC Heap上。

```go
func test() *int {
    x := 100
    return &x			// 在堆上分配x内存。但在內联时，也可能直接分配自爱目标栈。
}
```

将Pointer转换成uintptr，可变相实现指针运算。

```go
package main

import (
	"unsafe"
	"fmt"
)

func main() {
	d := struct {
		s string
		x int
	}{"abc", 100}
	
	// 将指针转换为uint，然后再操作指针
	p := uintptr(unsafe.Pointer(&d))  	// 获取d的地址，偏移为0
	fmt.Println(p)
	p += unsafe.Offsetof(d.x)  		// 偏移字段到x， p指向d中的x
	fmt.Println(p)

	p2 := unsafe.Pointer(p)		// 转换为中间类型指针
	fmt.Println(p2)
	px := (*int)(p2)		// 转换指针类型为int

	*px = 200			// 在d.x上进行赋值

	fmt.Printf("%#v\n", d)	// 格式化输出d

}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note14.go 
842350845768
842350845784
0xc42005df58
struct { s string; x int }{s:"abc", x:200}
```

> **注意** : GC把uintptr当成普通整数对象，它无法阻止“关联”对象被回收。

## 自定义类型

可将类型分为命名和未命名两大类，命名包括`bool`、`int`、`string`等，而`array`、`slice`、`map`等和具体元素类型、长度等有关，属于未命名类型。

具有相同声明的未命名类型被视为同一类型。

- 具有相同类型的指针。
- 具有相同元素类型和长度的array。
- 具有相同元素类型的slice。
- 具有相同键值类型的map。
- 具有相同元素类型和传送方向的channel。
- 具有相同字段系列（字段名、类型、标签、顺序）的匿名struct。
- 签名相同（参数和返回值，不包括参数名称）的function。
- 方法集相同（方法名、方法前面相同，和次序无关）的interface。

可用type在全局或函数内定义新类型

```go
package main

func main() {
	type bigint int64
	var x bigint  = 100
	println(x)
}
```

新类型不是原类型的别名，除拥有相同数据存储结构外，它们之间没有任何关系，不会持有原类型任何信息。除非目标类型是未命名类型，否则必须显式转换。

```go
package main

import "fmt"

type myslice []int

func main() {
	type bigint int64
	//var x bigint  = 100
	//println(x)
	x := 1234
	var b bigint = bigint(x)
	var b2 int64 = int64(b)
	fmt.Println(b2)

	var s myslice = []int{1, 2, 3}
	var s2 []int = s
	fmt.Printf("%T %#v", s, s2)
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note15.go 
1234
main.myslice []int{1, 2, 3}
```

参考文献：[雨痕 Go 学习笔记 第四版](https://github.com/qyuhen/book/blob/master/Go%20%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%20%E7%AC%AC%E5%9B%9B%E7%89%88.pdf)