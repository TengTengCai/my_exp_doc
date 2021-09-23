# Go语言基础之map

Go语言中提供的映射关系容器为`map`，其内部使用`散列表(hash)`实现。

## map

map是一种无序的基于`key-value`的数据结构，Go语言中的map是引用类型，必须初始化才能使用。

### map定义

Go中`map`定义语法如下：

```go
map [keyType] ValueType
```

其中，

- KeyType: 表示建类型
- ValueType: 表示键对应的值的类型

map类型的变量默认值为nil，需要使用make()函数来分配内存。

```go
make(map[KeyType]ValueType, [cap])
```

其中cap表示map的容量，该参数虽然不是必须的，但是我们应该在初始化map的时候就为其指定一个合适的容量。

### map基本使用

map中的数据都是成对出现的，如：

```go
package main

import "fmt"

func main() {
    scoreMap := make(map[string]int, 8)
    scoreMap["李华"] = 89
    scoreMap["小红"] = 99
    fmt.Println(scoreMap)
    fmt.Println(scoreMap["李华"])
    fmt.Printf("type of scoreMap:%T \n", scoreMap)
}
```

输出：

```
map[小红:99 李华:89]
89
type of scoreMap:map[string]int 
```

 map也可以在声明的时候填充元素，如：

```go
package main

import "fmt"

func main() {
    userInfo := map[string]string{
        "name": "小明",
        "age": "18",
    }
    fmt.Println(userInfo)
}
```

输出：

```
map[age:18 name:小明]
```

### 判断某个键是否存在

Go语言中有个判断map中键是否存在的特殊写法，如：

```go
value, ok := map[key]
```

如：

```go
package main

import "fmt"

func main() {
    scoreMap := make(map[string]int)
    scoreMap["李华"] = 89
    scoreMap["小红"] = 99
    v, ok := scoreMap["小红"]
    if ok {
        fmt.Println(v)
    }else {
        fmt.Println("不存在")
    }
}
```

### map的遍历

Go语言中使用`for range`遍历map。

```go
package main

import "fmt"

func main() {
    scoreMap := make(map[string]int)
    scoreMap["李华"] = 89
    scoreMap["小红"] = 99
    scoreMap["小明"] = 78
    for k, v := range scoreMap{
        fmt.Println(k, v)
    }
}
```

但是只想遍历Key的时候，可以如下写：

```go
package main

import "fmt"

func main() {
	scoreMap := make(map[string]int)
    scoreMap["李华"] = 89
    scoreMap["小红"] = 99
    scoreMap["小明"] = 78
    for k := range scoreMap{
        fmt.Println(k)
    }
}
```

> 注意：遍历map时的元素顺序与添加键值对的顺序无关。

### 使用delete()函数删除键值对

使用`delete`内建函数从map中删除一组键值对，`delete()`函数的格式如下：

```go
delete(map, key)
```

其中：

- map: 表示要删除键值对的map
- key: 表示要删除的键值对的键

如：

```go
package main

import "fmt"

func main() {
    scoreMap := make(map[string]int)
    scoreMap["李华"] = 89
    scoreMap["小红"] = 99
    scoreMap["小明"] = 78
    delete(scoreMap, "小明")
    for k,v := range scoreMap {
        fmt.Println(k, v)
    }
}
```

输出：

```
李华 89
小红 99
```

### 按照指定顺序遍历map

```go
package main

import (
	"fmt"
	"math/rand"
	"sort"
	"time"
)

func main() {
	rand.Seed(time.Now().UnixNano())

	var scoreMap = make(map[string]int, 200)

	for i := 0; i < 100; i++ {
		key := fmt.Sprintf("Num%02d", i) //生成Num开头的字符串
		value := rand.Intn(100)          //生成0~99的随机整数
		scoreMap[key] = value
	}

	//取出map中的所有key存入切片keys
	var keys = make([]string, 0, 200)
	for key := range scoreMap {
		keys = append(keys, key)
	}

	//对切片进行排序
	sort.Strings(keys)
	//按照排序后的key遍历map
	for _, key := range keys {
		fmt.Println(key, scoreMap[key])
	}
}
```

### 元素为map类型的切片

切片中的元素为map类型：

```go
package main

import "fmt"

func main() {
	var mapSlice = make([]map[string]string, 3)
	for index, value := range mapSlice {
		fmt.Printf("index:%d value:%v\n", index, value)
	}
	fmt.Println("初始化第一个map元素")
	// 对切片中的map元素进行初始化
	mapSlice[0] = make(map[string]string, 10)
	mapSlice[0]["name"] = "TengTengCai"
	mapSlice[0]["age"] = "18"
	mapSlice[0]["address"] = "北京"
	for index, value := range mapSlice {
		fmt.Printf("index:%d value:%v\n", index, value)
	}
}
```

输出：

```
index:0 value:map[]
index:1 value:map[]
index:2 value:map[]
初始化第一个map元素
index:0 value:map[address:北京 age:18 name:TengTengCai]
index:1 value:map[]
index:2 value:map[]
```

### 值为切片类型的map

```go
package main

import "fmt"

func main() {
	var sliceMap = make(map[string][]string, 3)
	fmt.Println(sliceMap)
	fmt.Println("初始化第一个键值")
	key := "中国"
	value, ok := sliceMap[key]
	if !ok {
		value = make([]string, 0, 2)
	}
	value = append(value, "重庆", "成都")
	sliceMap[key] = value
	fmt.Println(sliceMap)
}
```

输出：

```
map[]
初始化第一个键值
map[中国:[重庆 成都]]
```

