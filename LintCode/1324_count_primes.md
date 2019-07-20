# <center>1324 - Count Primes (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/count-primes/description

<br></br>



## Description
----
Count the number of prime numbers less than a non-negative number.

<br></br>



## Example
----
Input: n = 4
Output: 2
Explanation：2, 3 are prime number

<br></br>



## Solution
----
### Java
```java
public class CountPrimes {
	public int countPrimes(int n) {
		int count = 0;
		for (int i = 2; i < n; i++)
			if (isPrime(i))
				count++;
		
		return count;
    }
    
    private boolean isPrime(int num) {
    	for (int i = 2; i * i <= num; i++)
    		if (num % i == 0)
    			return false;
    	
    	return true;
    }
}
```

<br>


### Python
```python
class CountPrimes:
    """
    @param n: a integer
    @return: return a integer
    """
    def count_primes(self, n):
        count = 0
        for i in range(2, n):
            if self.is_prime(i):
                count += 1

        return count

    def is_prime(self, n):
        i = 2
        while i * i <= n:
            if n % i == 0:
                return False
            i += 1

        return True
```

<br>


### Go
```go
func CountPrimes(n int) int {
	count := 0
	for i := 2; i < n; i++ {
		if isPrime(i) {
			count++
		}
	}

	return count
}

func isPrime(n int) bool {
	for i := 2; i*i <= n; i++ {
		if n%i == 0 {
			return false
		}
	}

	return true
}
```
