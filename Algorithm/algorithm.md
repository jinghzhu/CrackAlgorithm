<h1 style="text-align: center;"><strong>Algorithm</strong></h1>

- [基本数据类型](#基本数据类型)
- [尾递归](#尾递归)
- [References](#references)

<br></br>



# 基本数据类型
基本数据类型以二进制形式存储。一个二进制位为$1$ bit比特。$1$字节byte由$8$比特组成。

> 1 byte = 8 bits, 1024 bytes = 1k

以Java为例：
- `byte`占用$1$字节=$8$比特，可表示$2^{8}$个数字。
- `int`占用$4$字节=$32$比特，可表示$2^{32}$个数字。

| 符号   | 占用空间 | 最小值                   | 最大值                  | 默认值         |
| ------ | -------- | ----------------------- | --------------------- | -------------- |
| byte   | 1字节   | $-2^7$ ($-128$)          | $2^7 - 1$ ($127$)       | $0$            |
| short  | 2字节   | $-2^{15}$                | $2^{15} - 1$            | $0$            |
| int    | 4字节   | $-2^{31}$                | $2^{31} - 1$            | $0$            |
| long   | 8字节   | $-2^{63}$                | $2^{63} - 1$            | $0$            |
| float  | 4字节   | $1.175 \times 10^{-38}$  | $3.403 \times 10^{38}$  | $0.0\text{f}$  |
| double | 8字节   | $2.225 \times 10^{-308}$ | $1.798 \times 10^{308}$ | $0.0$          |
| char   | 2字节   | $0$                      | $2^{16} - 1$            | $0$            |
| bool   | 1字节   | $\text{false}$           | $\text{true}$           | $\text{false}$ |

对于Go：

```go
fmt.Printf("int8 range: [%d, %d]\n", math.MinInt8, math.MaxInt8)
fmt.Printf("int16 range: [%d, %d]\n", math.MinInt16, math.MaxInt16)
fmt.Printf("int32 range: [%d, %d]\n", math.MinInt32, math.MaxInt32)
fmt.Printf("int64 range: [%d, %d]\n", math.MinInt64, math.MaxInt64)
fmt.Printf("int range: [%d, %d]\n", int(^uint(0)>>1)*-1-1, int(^uint(0)>>1)) // int has a machine-dependent size. Minimum and maximum int
var a = int(5)
fmt.Println("signed integer a's length is", unsafe.Sizeof(a)) // 8

fmt.Printf("uint8 range: [%d, %d]\n", 0, math.MaxUint8)
fmt.Printf("uint16 range: [%d, %d]\n", 0, math.MaxUint16)
fmt.Printf("uint32 range: [%d, %d]\n", 0, math.MaxUint32)
fmt.Printf("uint64 range: [%d, %d]\n", 0, uint64(math.MaxUint64))
fmt.Printf("uint range: [%d, %d]\n", 0, ^uint(0)) // uint has a machine-dependent size
var b = uint(5)
fmt.Println("unsigned integer b's length is", unsafe.Sizeof(b)) // 8

fmt.Printf("float32 range: [%g, %g]\n", -math.MaxFloat32, math.MaxFloat32)
fmt.Printf("float64 range: [%g, %g]\n", -math.MaxFloat64, math.MaxFloat64)

var p uintptr = 0x12345678
fmt.Printf("pointer p's length is %d\n", unsafe.Sizeof(p)) // uintptr has a machine-dependent size. 8
```

|   Type  |                       Size         |     Min                  |                  Max                  |
|---------|------------------------------------|--------------------------|---------------------------------------|
| int8    | 1 byte                             | $-2^7$ ($-128$)          | $2^7 - 1$ ($127$)                     |
| uint8   | 1 byte                             | $0$                      | $2^8 - 1$ ($255$)                     |
| int16   | 2 bytes                            | $-2^{15}$                | $2^{15} - 1$ ($32767$)                |
| uint16  | 2 bytes                            | $0$                      | $2^{16} - 1$ ($65535$)                |
| int32   | 4 bytes                            | $-2^{31}$                | $2^{31} - 1$ ($2147483647$)           |
| uint32  | 4 bytes                            | $0$                      | $2^{32} - 1$ ($4294967295$)           |
| int64   | 8 bytes                            | $-2^{63}$                | $2^{63} - 1$ ($9223372036854775807$)  |
| uint64  | 8 bytes                            | $0$                      | $2^{64} - 1$ ($18446744073709551615$) |
| float32 | 4 bytes                            | $1.175 \times 10^{-38}$  | $3.402 \times 10^{38}$                |
| float64 | 8 bytes                            | $2.225 \times 10^{-308}$ | $1.798 \times 10^{308}$               |
| bool    | 1 byte                             | false                    | true                                  |
| byte    | 1 byte                             | $0$                      | $255$                                 |
| rune    | 4 bytes                            | $0$                      | $2^{32} - 1$ ($4294967295$)           |
| int     | 4 bytes (32bit) or 8 bytes (64bit) | $-2^{31}$ or $-2^{63}$   | $2^{31}-1$ or $2^{63}-1$              |
| uint    | 4 bytes (32bit) or 8 bytes (64bit) | $0$                      | $2^{32}-1$ or $2^{64}-1$              |
| uintptr | 4 or 8 bytes (depends on arch)     | $0$                      | 大足以存储任意指针的值                  |

Note:
* `unitptr`, `int` and `uint` can be either 4 bytes (32-bit) or 8 bytes (64-bit).
* `rune` is alias for `int32`, representing Unicode code points.
* `byte` is alias for `uint8`.

<br></br>



# 尾递归
如果函数在返回前最后一步才递归调用，则该函数可被编译器或解释器优化，使其在空间效率上与迭代相当。这种情况被称为尾递归（tail recursion）。

- 普通递归：当函数返回到上一层函数后，需继续执行代码，因此系统需保存上一层调用上下文。
- 尾递归：递归调用是函数返回前最后一个操作，意味函数返回到上一层后，无须继续执行其他操作，因此无须保存上一层函数上下文。

以计算$1+2+...+n$为例，可将结果`res`设为函数参数，从而实现尾递归：

```go
// 普通递归
func recur(n int) int {
    if n == 1 { // 终止条件
        return 1
    }

    res := recur(n - 1) // 递：递归调用
    return n + res // 归：返回结果
}

// 尾递归
func tailRecur(n int, res int) int {
    if n == 0 { // 终止条件
        return res
    }

    return tailRecur(n-1, res+n) // 尾递归调用
}
```

对比普通递归和尾递归，两者求和操作执行点不同：
* 普通递归：求和操作在“归”过程中执行，每层返回后都要再执行一次求和操作。
* 尾递归：求和操作在“递”过程中执行，“归”的过程只需层层返回。

![](./Images/tail_recursion_sum.png)

理论上，尾递归空间复杂度可优化至$O(1)$。不过多数编程语言（Java、Python、C++、Go、C#等）不支持自动优化尾递归，因此常认为空间复杂度是$O(n)$。

<br></br>



# References
* [Hello Algorithm](hello-algo.com)