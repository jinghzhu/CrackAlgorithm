# <center>Climb Stairs II (H)</center> 



<br></br>

* Difficulty: Hard
* Tag: DP
* Link: https://www.nowcoder.com/questionTerminal/22243d016f6b47f2a6928b4313c85387

<br></br>



## Description
一次可跳上1级台阶，也可2级，甚至n级。求跳上n级台阶有多少种跳法。

<br></br>



## Solution
简单而言，因为n级台阶，第一步有n种跳法：跳1级、跳2级、直到跳n级。跳1级，剩下n-1级，剩下跳法是f(n-1)；跳2级，剩下n-2级，剩下跳法是f(n-2)。所以`f(n)=f(n-1)+f(n-2)+...+f(1)`。因为`f(n-1)=f(n-2)+f(n-3)+...+f(1)`，两式相减得`f(n)=2*f(n-1)`。
	
详细分析如下:
1. `f(1) = 1`
2. `f(2) = f(2-1) + f(2-2)` 其中，`f(2-2)`表示2阶一次跳2阶的次数。
3. `f(3) = f(3-1) + f(3-2) + f(3-3) `
4. （以此类推）
5. `f(n) = f(n-1) + f(n-2) + f(n-3) + ... + f(n-(n-1)) + f(n-n)`

说明： 
1. `f(n)`代表`n`个台阶有1，2，...n阶的跳法数。
2. `n = 1`时，只有1种跳法，`f(1) = 1`。
3. `n = 2`时，有两个跳方式，一次1阶或2阶，回归到问题1，即`f(2) = f(2-1) + f(2-2)`。 
4. `n = 3`时，有三种方式，1阶、2阶和3阶。第一次跳出1阶后面剩下`f(3-1)`；第一次跳出2阶，剩下`f(3-2)`；第一次3阶，剩下`f(3-3)`。结论是`f(3) = f(3-1) + f(3-2) + f(3-3)`。
5. `n = n`时，有`n`种跳的方式，1阶、2阶...n阶，得出结论：`f(n) = f(n-1)+f(n-2)+...+f(n-(n-1)) + f(n-n) => f(0) + f(1) + f(2) + f(3) + ... + f(n-1)`。继续简化：`f(n-1) = f(0) + f(1)+f(2)+f(3) + ... + f((n-1)-1) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2)`，然后`f(n) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2) + f(n-1) = f(n-1) + f(n-1)`。上述两式相减得出`f(n) = 2*f(n-1)`。
6. 最终结论：
```
        | -1         if n < 1
f(n) =  | 1          if n = 1
        | 2 * f(n-1) if n >= 2
```

<br>


### Java
```java
public class ClimbStairs2 {
	public int solution1(int target) {
        if (target <= 0)
            return -1;
        
        if (target == 1)
            return 1;
        
        return 2 * solution1(target - 1);
    }
}
```

<br>


### Go
```go
func ClimbStairs2(n int) int {
	if n < 1 {
		return -1
	}
	if n == 1 {
		return 1
	}
	return 2 * ClimbStairs2(n-1)
}
```

<br>


### Python
```python
class ClimbStairs2:
    def solution(self, number):
        if number < 1:
            return -1
        if number == 1:
            return 1

        return 2 * self.solution(number - 1)
```