#运算符、表达式和语句

## 左值、右值、运算符

| 术语 | 含义 |
| --- | --- |
| 数据对象 | 用于储存值的数据存储区域，也称对象 |
| 左值 | 用于标识特定数据对象的名称或表达式；可修改的左值用于标识可修改的对象，又称对象定位值。 |
| 右值 | 能赋值给可修改左值的量，本身不是左值 |
| 运算符 |  |

## 运算符优先级

| 优先级 | 运算符 | 结合方向 | 类型 |
| --- | --- | --- | --- |
| 1 | () ; [] ; -> ; . | -> | 括号与索引 |
| 2 | ! ; ~ ; ++ ; -- ; - ; (transform) ; * ; & ; sizeof | <- | 单目 |
| 3 | * ; / ; % | ->| 乘除类 |
| 4 | + ; - | -> | 加减 |
| 5 | << ; >> | -> | 左右移动 |
| 6 | （大小关系等）| -> | 关系 |
| 7 | （相等关系等运算符）| -> | 相等 |
随后优先级依次降低为：  

按位: 与 >异或 >或 > 逻辑: 与 > 或  

随后结合方向变为`从右至左`: `? :` > 赋值运算符  > 逗号  

## 函数

| 术语 | 含义 |
| --- | --- |
| 形式参数 | 定义和声明函数时声明的参数 |
| 实际参数 | 函数调用时传递的值 |
| 函数的类型 | 函数返回值的数据类型 |

## 分支与跳转

## String.h

| 函数 | 功能 |
| --- | --- |
| char *strcpy(char *destin, char *source);  | 将source字符串复制到destin，包括\0。 |
| char *strcat(char *destin, char *source); | 把source所代表的字符串连接在destin的结尾处，覆盖最后的\0。 |
| int strcmp(char *str1, char *str2); | 比较ascii码，两串相等返回0。大于返回值>0。  |
| size_t strlen(const char *s); | 统计字符串长度，直到\0后停止，不包括\0。 |

## 类函数宏

```
#include <stdio.h>
#define TestMacro(Y) Y * Y  //Y为宏参数，在调用时为传入的字符串
                            //str，此处则将宏调用替换为 str * str
#define TestMacro2(X) "This is a nice string X" , X
                            //双引号中的X不会被替换，后面的X会被替换

int main(void) {
    int x = 5;#
    printf("%d", TestMacro( x + 2 )); //替换为 x + 2 * x + 2 
                                      //结果为 5 + 2 * 2 + 2 = 17
}

```

## C语言隐式类型转换
### 规则
1. 类型转换出现在表达式时，`short` 和 `char` 会被转化为 `int`。若`short`和 `int` 一样长，则`unsigned short`比 `unsigned int` 长，则转化为 `unsigned int`。（K&R版本的C，float会被自动升级为double）
2. 涉及到两种类型的运算，两种值会自动升级为两种类型的更高级别。
3. 赋值表达式中，计算的结果会转化为左值规定的类型，可能导致降级。
4. 作为参数传递的时候，实际参数会被转化为形式参数的类型。


| 类型的级别 |
| --- |
| long double |
| double |
| float |
| unsigned long long |
| long long |
| unsigned long |
| long |
| unsigned int |
| int  |

### 不匹配的情况

| 情况 | 结果 |
| --- | --- |
| 无符号整数 = 整数 | 原始值截掉多余位 |
| 有符号 = 整数 | 不一定 |
| 整形 = 浮点数 | 未定义 |




## C语言效率问题

| 措施 | 效果 |
| --- | --- |
| 编译器 | 强类型、编译型语言，可以在编译阶段就对代码执行优化 |
| 指针 | 由于指针，C语言拥有直接操作内存的能力，程序员对于程序的控制达到字节级别，提升效率。传递参数也可以只传指针，减少调用时的开销。 |
| 位操作 | 位操作的效率较高，可以代替诸如乘法，除法，以及掩码等算法，提升效率。 |
| API | 可以直接调用系统提供的C API，无需中间层，提升了效率。 |
| 宏函数 | 使用宏函数，宏函数仅仅为代码替换。无需像函数调用那样需要传递参数，保存上下文与跳转回来的恢复现场，以及一些栈操作。同时宏参数不占用内存空间。 |
| 强制类型转换 |实现了在不创建新对象或变量的情况下，让一个数据可以在多个兼容的数据类型之间相互转换。|
| 执行 | 编译为机器码，直接运行，而不是诸如java的虚拟机。|

## Const在参数中

| 用法 | 效果 |
| --- | --- |
| 修饰局部变量 |`const int a` `int const a` 变量不能被修改。 |
| 常量指针 | `const int *p` `int const *p` 表示这个指针指向的是常量。**需注意：1. 不能通过这个指针改变对象，但也许可以通过其他指针改变 2. 这个指针的指向也可以改变。** |
| 指针常量 |`int *const n` 指针的指向不能改变，但指向的对象值可以改变。|
| 指向常量的常指针 |`const int* const p` 既不能改变指针的指向，也不能通过这个指针改变值。|
| 修饰函数的参数 |1. 防止修改参数传递进来的指针所指向的对象。`void StringCopy(const char *strSource);` 2. 防止修改指针的指向。 `void swap ( int * const p1 , int * const p2 )`|
| 修饰函数的返回值 | 定义 `const char * GetString(void);` 返回值只能赋值给常量指针，防止返回值被修改。|

## 常用算法
### 快排

```
int swapInt (int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}


int Partition(int *data, int low, int high) {
    int pivocKey = data[low];
    while (low < high) {
        while (low < high && data[high] >= pivocKey)
            high--;
        swapInt(&data[low], &data[high]);
        while (low < high && data[low] <= pivocKey)
            low++;
        swapInt(&data[low], &data[high]);
    }
    return low;
}

int quickSorting (int *data, int low, int high) {

    if (data == NULL || low > high)
        return -1;

    if (low == high)
        return 0;

    else {
        int pivotLoc = Partition(data, low, high);
        quickSorting(data, pivotLoc + 1, high);
        quickSorting(data, low, pivotLoc - 1);
        return 1;
    }
}
```

## 判断溢出
```
int add(long long x, long long y, int *overflow) {
    int z = x + y;
    if (x > 0 && y > 0 && z < 0 || x < 0 && y < 0 && z > 0)
        *overflow = 1;
    else
        *overflow = 0;

    return z;
}
```



## 堆、栈、全局/静态区、文字常量区、代码区

| 地址 | 区 | 描述 |
| --- | --- | --- |
| 高地址 |  | 命令行参数与环境变量 |
|  | 栈↓ |  |
|  |空内存  |  |
|  | 堆↑ | 程序员自行申请的内存区域在堆，索引这块内存的指针存放在栈区。 |
|  | 未初始化变量 | 被exec初始化为0。存放全局变量和静态变量|
|  | 初始化变量 | 存放全局变量和静态变量。 |
| 低地址 | 文字常量区 | 存放文字等不可修改的常量。但索引其的指针存于栈上。|

### 全局变量与静态变量
- 全局变量：位于所有函数外部定义的变量，在整个工程中可见，可修改。
- 静态变量：位于所有函数内部定义的由 static 修饰的变量，仅在定义的函数中可见，可修改。（这是它与全局变量的关键区别）










