# <center>137 - Single Number I (M)</center> 



<br></br>

* Tag: Bit
* Difficulty: Medium
* Link: https://leetcode.com/problems/single-number-ii/

<br></br>



## Description
----
Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

<br></br>



## Example
----
Input: [2,2,3,2]
Output: 3

<br></br>



## Solution
----
有个数只出现一次，即二进制每一位上都有一个数只出现一次。

假设有三个数1、2和1，其二进制分别为：
* 1 -> 0,0,0,1
* 2 -> 0,0,1,0
* 1 -> 0,0,0,1

其中：
1. 第一位：1,0,1
2. 第二位：0,1,0
3. 第三位：0,0,0
4. 第四位：0,0,0

使用三个变量`one`、`two`和`three`存储每一位上出现的1、2和3次情况，即模拟3进制。

先看第一位，以前述第一位序列1,0,1为例子：
1. 先是1，则`one`变成1，`two`和`three`不变；
2. 然后是0，则都不变；
3. 最后是1，`one`需变成0，`two`变成1，`three`不变。

如果再来一个1，则`one`和`two`是1，`three`变成0。

因此，算法是：
```java
two = two  | (one & num[i])
one = one ^ num[i]
three = one & two
one = ~three
two = ~three
```

<br>


### Java
```java
public class SingleNum2 {
	// 创建一个长度为sizeof(int)数组count[sizeof(int)]，count[i]表示在在i位出现的1的次数。
	// 如果count[i]是3的整数倍，则忽略；否则把该位取出来组成答案。
    public int solution1(int[] nums) {
        int[] count = new int[Integer.SIZE];  // count[i]表示在在i位出现的1的次数。
        for (int i : nums)
            for (int j = 0; j < Integer.SIZE; j++) {
                count[j] += (nums[i] >> j) & 1;
                count[j] %= 3;
            }
        int result = 0;
        for (int i = 0; i < Integer.SIZE; i++)
            result += (count[i] << i);

        return result;
    }
    
    // one记录到当前为止，二进制1出现“1次”（mod 3后的1）的有哪些二进制位；two记录当前为止，二进制1出现
    // “2次”（mod 3后的2）的有哪些二进制位。当one和two中某一位同时为1时，表示该二进制位上1出现3次，需
    // 清零。即用二进制模拟三进制运算，one是最终结果。
    public int solution2(int[] nums) {
    	int one = 0, two = 0, three = 0;
        for (int i : nums) {
            two |= (one & i);
            one ^= i;
            three = ~(one & two);
            one &= three;
            two &= three;
        }

        return one;
    }
}

```

<br>


### Python
```python
class SingleNum2:
    def solution(self, nums) -> int:
        one = 0
        two = 0
        three = 0
        for i in range(0,len(nums)):
            # 当新来的为0时，two = two | 0，two不变。
            # 当新来的为1时：
            #   当one此时为0，则two = two|0，two不变；
            #   当one此时为1时，则two = two | 1，two变为1。
            two |= one & nums[i]
            # 新来的为0，one不变，新来为1时，one是0、1交替改变。
            one ^= nums[i]
            # 当one和two为1是，three为1（此时代表要把one和two清零了）。
            three = one & two
            # 把one清0。
            one &= ~three
            # 把two清0
            two &= ~three

        return one
```

<br>


### Go
one记录当前为止，二进制1出现“1次”（mod 3后的1）的有哪些二进制位；two记录当前为止，二进制1出现“2次”（mod 3后的2）的有哪些二进制位。当one和two中某一位同时为1时，表示该二进制位上1出现3次，需清零。即用二进制模拟三进制运算，one是最终结果。
```go
func SingleNum2_1(nums []int) int {
	one, two, three := 0, 0, 0
	for _, v := range nums {
		two = two | (one & v) //计算每个比特位中的1是否出现了两次
		one = one ^ v         //计算每个比特位是否出现了一次或三次
		three = one & two     //当one和two的相应位为1时，表示相应位出现了3次
		one = one & (^three)  //当相应位出现了3次时，把相应的位出现次数置为0
		two = two & (^three)
	}

	return one
}
```

创建一个长度为`sizeof(int`)数组，`count[sizeof(int)]`。`count[i]`表示在在`i`位出现的1的次数。如果`count[i]`是3的整数倍，忽略；否则把该位取出来组成答案。
```go
func SingleNum2_2(nums []int) int {
	count := make([]int, 32, 32)
	for _, v0 := range nums {
		for k1, v1 := range count {
			v1 += (v0 >> uint(k1)) & 1
			v1 %= 3
		}
	}
	var result int
	for k, v := range count {
		result += (v << uint(k))
	}

	return result
}
```