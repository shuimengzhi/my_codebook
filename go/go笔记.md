[TOC]

#断点方法

```
go get -u -v github.com/liudng/godump
```

```
godump.Dump(k)
os.Exit(1)
```

# 编译命令

```
测试命令：
go run 
编译命令:
go build
应用生成:
go install
```

# 声明变量

```
var a int8 =1
var a = 1
a:=1(这个方法只能在函数内存在)
```

#数组(map)

数组key是string则使用map来定义,两种声明方法

```
第一种声明
var number map[string]int
number = make(map[string]int)
第二种
number := make(map[string]int)
赋值：
number := map[string]int{"a": 1, "b": 2}
```

# 读取数组(类似foreach)

```
for k,v:=range map {
    fmt.Println("map's key:",k)
    fmt.Println("map's val:",v)
}
for _, v := range map{
    fmt.Println("map's val:", v)
}
```

# defer

```
package main

import (
	"fmt"
)

func printA(a int) {
	fmt.Println("value of a in deferred function", a)
}
func printB(a int) {
	fmt.Println("value of a in deferred function b", a)
}
func main() {
	a := 5
	defer printB(a)
	defer printA(a)

	a = 10
	fmt.Println("value of a before deferred function call", a)

}
结果：
value of a before deferred function call 10
value of a in deferred function 5
value of a in deferred function b 5
```

defer从下往上运行
#结构的理解
```
type Human struct {
    name string
    age int
    phone string
}
```
更像声明class

用结构（类）去在方法中应用
```
// Human 对象实现 Sayhi 方法
func (h *Human) SayHi() {
    fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}
```
#接口的理解
```
// 定义 interface
type Men interface {
    SayHi()
    Sing(lyrics string)
    Guzzle(beerStein string)
}
```
接口里面都是待填写参数的方法
#变量类型判断(comma-ok)
```
value, ok := element.(int);
```
value = element ,ok = true或者false（依据括号内的类型判断）,以上面为例,element是int类型，则ok=true，如果不是，则ok=false。
#结构体打印
```
type Person struct {
	name string
	age  int
}
a := Person{"Dennis", 70}
fmt.Println(a)
```
打印结果:
```
{Dennis 70}
```
如果添加方法重写string:
```
// 定义了 String 方法，实现了 fmt.Stringer
func (p Person) String() string {
	return "(name: " + p.name + " - age: " + strconv.Itoa(p.age) + " years)"
}
```
打印结果:
```
(name: Dennis - age: 70 years)
```

#类型定义别名
```
type a int
```
int类型别名叫a

