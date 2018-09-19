# Go 语言基础 数据

[TOC]

## Array

和以往认知的数组有很大的不同

- 数组是值类型，赋值和传参会复制整个数组，而不是指针。
- 数组长度必须是常量，且类型的组成部分。[2]int 和[3]int 是不同类型。
- 支持“==”、“！=”操作符，因为内存总是被初始化过的。
- 指针数组[n]\*T、数组指针\*[n]T。

可用符合语句进行初始化。

```go
package main

import "fmt"

func foo40_1(x1 [2]int) {
	fmt.Printf("x1: %p\n", &x1)
	x1[1] = 1000
}

func foo40_2(x2 *[2]int) {
	fmt.Printf("x2: %p\n", x2)
	x2[1] = 1000
}

func main() {
	a := [3]int{1, 2}  				// 未初始化元素值为0
	b := [...] int{1, 2, 3, 4}		// 通过初始化值确定数组长度。
	c := [5]int{2: 100, 4: 200}		// 使用索引号初始化元素

	d := [...]struct{				// 数组对象可以是struct
		name string
		age uint8
	}{
		{"user1", 10},
		{"user2", 20},
	}

	// 支持多维数组
	e := [2][3]int{{1, 2, 3}, {4, 5, 6}}
	f := [...][2]int{{1, 1}, {2, 2}, {3, 3}}  	// 第2维度不能用“...”

	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)
	fmt.Println(d)
	fmt.Println(e)
	fmt.Println(f)

	// 值拷贝行为会造成性能问题，通常会建议使用slice，或数组指针
	g := [2]int{}
	fmt.Printf("g: %p\n", &g)
	foo40_1(g)
	fmt.Println(g)
	foo40_2(&g)
	fmt.Println(g)

	// 如果需要使用方法对原数组进行修改，可以使用指针来进行操作，
	// 如果不使用指针，在参数的传递过程中会发生值拷贝，影响性能，
	// 如果需要修改原数组，请修改后返回再赋值
}
```

运行结果：

```bash
[tianjun@TengTengCai GoNotes]$ go run note40.go 
[1 2 0]
[1 2 3 4]
[0 0 100 0 200]
[{user1 10} {user2 20}]
[[1 2 3] [4 5 6]]
[[1 1] [2 2] [3 3]]
g: 0xc4200180f0
x1: 0xc420018100
[0 0]
x2: 0xc4200180f0
[0 1000]
```

内置函数`len`和`cap`都返回数组长度（元素数量）。

```go
a := [2]in{}
println(len(a), cap(a))		// 2, 2
```

len数组中元素个素，cap数组长度，由于数组长度不可变一般cap=len。

## Slice

需要说明，slice并不是数组或数组指针。它通过内部指针和相关属性引用数组片段，一实现变长方案。

runtime.h

```c
struct Slice
{
    byte* 	array;
    uintgo 	len;
    uintgo 	cap;
};
```

- 引用类型。但自身是结构体，值拷贝传递。
- 属性`len`表示可用元素数量，读写操作不能超出该限制。
- 属性`cap`表示最大扩容容量，不能超出数组限制。
- 如果`slice == nil`那么`len`、`cap`结果都等于0。

```go
package main

import "fmt"

func main(){
	data := [...]int{0, 1, 2, 3, 4, 5, 6}
	slice := data[1:4:5]

	fmt.Println(slice)
	fmt.Println(data)
	slice[2] = 23

	fmt.Println(slice)
	fmt.Println(data)

	// 可直接创建slice对象，自动分配底层数组
	s1 := []int{0, 1, 2, 3, 8: 100}		// 通过初始化表达式构造，使用索引进行初始化
	fmt.Println(s1, len(s1), cap(s1))

	// 使用make动态创建slice，避免了数组必须用常量做长度的麻烦。
	// 还可用指针直接访问底层数组，退化成普通数组操作
	s2 := make([]int, 6, 8)				// 使用make创建，制定了len 和cap值。
	fmt.Println(s2, len(s2), cap(s2))

	s3 := make([]int, 6)				// 省略cap，相当于 cap = len
	fmt.Println(s3, len(s3), cap(s3))

	s3[1] = 1000			// 直接对slice元素赋值

	fmt.Println(s3)

	p := &s3[2]			// 获取底层数组元素指针
	*p += 100

	fmt.Println(s3)
}
```

### reslice

所谓`reslice`是基于已有slice创建新slice对象，以便在cap允许范围内调整属性。

```go
s := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

s1 := s[2:5]			// [2 3 4]
s2 := s1[2:6:7] 		// [4 5 6 7]
//s3 := s2[3:6]			// Error

fmt.Println(s)
fmt.Println(s1)
fmt.Println(s2)
//fmt.Println(s3)
```

### append

向slice尾部添加数据，返回新的slice对象。

```go
s := make([]int, 0, 5)
fmt.Printf("%p\n", &s)

s2 := append(s, 1)
fmt.Printf("%p\n", &s2)

fmt.Println(s, s2)
```

简单点说，就是在array[slice.high]写数据

```go
data ：= [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
s := data[:3]
s2 := append(s, 100, 200)

fmt.Println(data)
fmt.Println(s)
fmt.Println(s2)
```

一旦超出原slice.cap限制，就会重新分配底层数组，即使原数组并未填满。

```go
data ：= [...]int{0, 1, 2, 3, 4, 10:0}
s := data[:2:3]

s = append(s, 100, 200)		// 一次append两个值，超出s.cap限制

fmt.Println(s, data)		// 重新分配底层数组，与原数组无关
fmt.Println(&s[0], &data[0])	// 比对底层数组起始指针
```

append后的s重新分配了底层数组，并复制数据。如果只追加一个值，则不会超过s.cap限制，也就不会重新分配内存了。

通常是以2倍容量重新分配底层数组。在大批量添加数据时，建议一次性分配足够打的空间，以减少内存分配和数据复制开销。或初始化足够长的len属性，改用索引号进行操作。及时释放不再使用的slice对象，避免持有过期数组，造成GC无法回收。

### copy

函数copy在两个slice间复制数据，复制长度以len小的为准。两个slice可指向同一底层数组，允许元素区间重叠。

```go
data := [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

s := data[8:]
s2 := data[:5]

copy(s2, s)

fmt.Println(s2)
fmt.Println(data)
```

运行结果：

```bash
[8 9 2 3 4]
[8 9 2 3 4 5 6 7 8 9]
```

## Map

引用类型，哈希表。键必须是支持相等运算符（==， ！=）类型，比如`number` `string` `pointer` `array`  `struct`，以及对应的interface。值可以是任意类型，没有限制。

```go
m := map[int]struct{
    name string
    age int
}{
    1: {"user1", 10},
    2: {"user2", 20},
}

fmt.Println(m[1].name)
```

预先给make函数一个合理元素数量参数，有助于提升性能。因为事先申请一块大内存，可避免后续操作频繁扩张。

```go
m := make(map[string]int, 1000)
```

常见操作：

```go
m := map[string]int {
    "a":1,
}
if v, ok := m["a"]; ok{			// 判断key是否存在
    fmt.Println(v)
}

fmt.Println(m["c"])				// 对于不存在的key， 直接返回0

m["b"] = 2						// 新增或修改。

delete(m, "c")					// 删除。如果key不存在，不会出错

fmt.Println(len(m))				// 获取键值对数量。cap无效

for k, v := range m {			// 迭代，可以返回key。 随机顺序返回每次都不相同。
    fmt.Println(k, v)
}
```

