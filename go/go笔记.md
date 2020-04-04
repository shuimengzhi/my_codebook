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
#接口实现
```
    mike := Student{Human{"Mike", 25, "222-222-XXX"}, "MIT", 0.00}
  // 定义 Men 类型的变量i
    var i Men

    // i 能存储 Student
    i = mike
    i.SayHi()
```
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

#反射
```
kk := Person{name: "sj", age: 2}
t := reflect.TypeOf(kk.name)
v := reflect.ValueOf(kk.name)
```
获取值的类型，以及值
#信道声明
```
var a chan int
a = make(chan int)
简洁声明:
a:=make(chan int)
```
#信道读取
```
data := <- a // 读取信道 a  
a <- data // 写入信道 a
```
#信道与go并发工作机制
当信道写入时，才可能往下执行，如:
```
package main

import (  
    "fmt"
)

func hello(done chan bool) {  
    fmt.Println("Hello world goroutine")
    done <- true
}
func main() {  
    done := make(chan bool)
    go hello(done)
    <-done
    fmt.Println("main function")
}
```
只有当true数据传入done时，<-done才会往下执行
#切片返回
```
func producer() []int {
	sum := make([]int, 10)
	for i := 0; i < 10; i++ {
		sum[i] = i
	}
	return sum
}
func main() {

	a := producer()
	godump.Dump(a)
}
```
#数组返回
```
func producer() [10]int {
	var sum [10]int
	for i := 0; i < 10; i++ {
		sum[i] = i
	}
	return sum
}
```
#缓冲信道
```
func main() {  
    ch := make(chan string, 2)
    ch <- "naveen"
    ch <- "paul"
    fmt.Println(<- ch)
    fmt.Println(<- ch)
}
```
信道设置缓冲容量为2，因此执行两个信道写入不会堵塞
#工作池
```
func Good(i int, wg *sync.WaitGroup) {
	fmt.Println("this is ", i)
	wg.Done()
}
func main() {
	var wg sync.WaitGroup
	n := 4
	for i := 0; i <= n; i++ {
		wg.Add(1)
		Good(i, &wg)
	}
	wg.Wait()
	fmt.Println("Over")

}
```
工作池wg，计数器为零时才能执行到Wait()方法，不然会卡住。计数器增加通过Add()方法增加，减少通过Done()方法。必须传递工作池的地址，不然Good方法每次执行的都是新的工作池。
#select信道的switch
```
 select {
    case s1 := <-output1:
        fmt.Println(s1)
    case s2 := <-output2:
        fmt.Println(s2)
    }
```
#Mutex信道锁（互斥锁）
```
mutex.Lock()  
x = x + 1  
mutex.Unlock()
```
在这个方法之间的所有计算，只能有一个信道执行，相当于锁。数据不会被同时修改
#sync.RWMutex信道锁（读锁和写锁）
写锁权限高于读锁，有写锁时优先进行写锁定

写锁
```
sync.RWMutex.Lock()
fmt.Println("Change with rwmutex lock")
time.Sleep(3 * time.Second)
c.password = pass
sync.RWMutex.Unlock()
```
读锁
```
 sync.RWMutex.RLock()
    fmt.Println("show with rwmutex",time.Now().Second())
    time.Sleep(1 * time.Second)
    defer sync.RWMutex.RUnlock()
```
Mutex with RWMutex:仅针对读的性能来说，RWMutex要高于Mutex，因为rwmutex的多个读可以并存。Mutex适用于读写不确定场景

#理解指针的*与&
```
a := "A"   // a的类型为 string
	b := &a  // b的类型为*string *代表指针，这里b是一个指针变量
	res := *b
	fmt.Println(b) // 0xc0420381d0  这里a是被取地址的变量，b变量进行接收。
	fmt.Println(res) // "A" 
```

#go的...
‘…’ 其实是go的一种语法糖。
它的第一个用法主要是用于函数有多个不定参数的情况，可以接受多个不确定数量的参数。
第二个用法是slice可以被打散进行传递。
```
func test1(args ...string) { //可以接受任意个string参数
    for _, v:= range args{
        fmt.Println(v)
    }
}

func main(){
var strss= []string{
        "qwr",
        "234",
        "yui",
        "cvbc",
    }
    test1(strss...) //切片被打散传入
}
```

#type func
函数套娃
```
package main

import "fmt"

type First func(int) int
type Second func(int) First

func squareSum(x int) Second {
	return func(y int) First {
		return func(z int) int {
			return x*x + y*y + z*z
		}
	}
}

func main() {
	// 5*5 + 6*6 + 7*7
	fmt.Println(squareSum(5)(6)(7))
}
```