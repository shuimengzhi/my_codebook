[TOC]
# Basic Type
`char`
`int`
`float`
`double`
# 编译&执行程序
gcc hello.c
./a.out
or 
gcc test1.c test2.c -o main.out
 ./main.out

# extern
变量在别的文件中定义
 extern int i; //声明，不是定义

# 定义常量
两种方法
`#define identifier value`
`const type variable = value;`
```
#include <stdio.h>
 
#define LENGTH 10   
#define WIDTH  5
#define NEWLINE '\n'
 
int main()
{
   const int  LENGTH = 10;
   const int  WIDTH  = 5;
   int area;  
  
   area = LENGTH * WIDTH;
   printf("value of area : %d", area);
   printf("%c", NEWLINE);
 
   return 0;
}
```

# register
register 存储类用于定义存储在寄存器中而不是 RAM 中的局部变量.regitster也可能存内存中，因为有可能寄存器容量不够。
```
{
   register int  miles;
}
```

# static
static 存储类指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。因此，使用 static 修饰局部变量可以在函数调用之间保持局部变量的值

```
#include <stdio.h>
 
/* 函数声明 */
void func1(void);
 
static int count=10;        /* 全局变量 - static 是默认的 */
 
int main()
{
  while (count--) {
      func1();
  }
  return 0;
}
 
void func1(void)
{
/* 'thingy' 是 'func1' 的局部变量 - 只初始化一次
 * 每次调用函数 'func1' 'thingy' 值不会被重置。
 */                
  static int thingy=5;
  thingy++;
  printf(" thingy 为 %d ， count 为 %d\n", thingy, count);
}
```
output:
```
 thingy 为 6 ， count 为 9
 thingy 为 7 ， count 为 8
 thingy 为 8 ， count 为 7
 thingy 为 9 ， count 为 6
 thingy 为 10 ， count 为 5
 thingy 为 11 ， count 为 4
 thingy 为 12 ， count 为 3
 thingy 为 13 ， count 为 2
 thingy 为 14 ， count 为 1
 thingy 为 15 ， count 为 0
 ```

 # ++
 a++先赋值再运算，++a先运算再赋值

 # 函数声明
  int sum(int, int);

# array
type arrayName [ arraySize ];
`double balance[10];`

# enum
```
#define MON  1
#define TUE  2
#define WED  3
#define THU  4
#define FRI  5
#define SAT  6
#define SUN  7
```
equal 
```
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
};
```

example:
```
#include <stdio.h>
 
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
};
 
int main()
{
    enum DAY day;
    day = WED;
    printf("%d",day);
    return 0;
}
```

# 函数指针
typedef int (*fun_ptr)(int,int); // 声明一个指向同样参数、返回值的函数指针类型

```
#include <stdio.h>

int max(int x, int y)
{
    return x > y ? x : y;
}
 
int main(void)
{
    /* p 是函数指针 */
    int (* p)(int, int) = & max; // &可以省略
    int a, b, c, d;
 
    printf("请输入三个数字:");
    scanf("%d %d %d", & a, & b, & c);
 
    /* 与直接调用函数等价，d = max(max(a, b), c) */
    d = p(p(a, b), c); 
 
    printf("最大的数字是: %d\n", d);
 
    return 0;
}
```
output:
```
请输入三个数字:1 2 3
最大的数字是: 3
```

# 回调函数
```
#include <stdlib.h>  
#include <stdio.h>
 
// 回调函数
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{
    for (size_t i=0; i<arraySize; i++)
        array[i] = getNextValue();
}
 
// 获取随机值
int getNextRandomValue(void)
{
    return rand();
}
 
int main(void)
{
    int myarray[10];
    /* getNextRandomValue 不能加括号，否则无法编译，因为加上括号之后相当于传入此参数时传入了 int , 而不是函数指针*/
    populate_array(myarray, 10, getNextRandomValue);
    for(int i = 0; i < 10; i++) {
        printf("%d ", myarray[i]);
    }
    printf("\n");
    return 0;
}
```

# 字符处理
strcpy(s1, s2);
复制字符串 s2 到字符串 s1。

strcat(s1, s2);
连接字符串 s2 到字符串 s1 的末尾。

strlen(s1);
返回字符串 s1 的长度。

strcmp(s1, s2);
如果 s1 和 s2 是相同的，则返回 0；如果 s1<s2 则返回小于 0；如果 s1>s2 则返回大于 0。

strchr(s1, ch);
返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置。

strstr(s1, s2);
返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置。

# struct
string in struct must use pointer or use strcpy 
char本质是一个数组，一个字符一个数组
```
#include <stdio.h>

/* 全局变量声明 */
int a = 20;
// define Book struct
struct Book {
    char *title;
    int book_id;
};

int main() {
    struct Book book1;
    book1.title = "aka";
    book1.book_id = 1;
    printf("title:%s,book_id:%d",book1.title,book1.book_id);
}
```

# union

共用体是一种特殊的数据类型，允许您在相同的内存位置存储不同的数据类型

```
#include <stdio.h>
#include <string.h>
 
union Data
{
   int i;
   float f;
   char  str[20];
};
 
int main( )
{
   union Data data;        
 
   data.i = 10;
   data.f = 220.5;
   strcpy( data.str, "C Programming");
 
   printf( "data.i : %d\n", data.i);
   printf( "data.f : %f\n", data.f);
   printf( "data.str : %s\n", data.str);
 
   return 0;
}
```