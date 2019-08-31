# <center>9 - Fizz Buzz (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/fizz-buzz/description

<br></br>



## Description
----
Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

<br></br>



## Example
----
n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param n: An integer
     * @return: A list of strings.
     */
    public List<String> fizzBuzz(int n) {
        List<String> ans = new ArrayList<String>();

        for (int num = 1; num <= n; num++) {

            boolean divisibleBy3 = (num % 3 == 0);
            boolean divisibleBy5 = (num % 5 == 0);

            if (divisibleBy3 && divisibleBy5)
                ans.add("fizz buzz");
            else if (divisibleBy3)
                ans.add("fizz");
            else if (divisibleBy5)
                ans.add("buzz");
            else
                ans.add(Integer.toString(num));
            }

        return ans;
    }
}
```

<br>


### Python
```python
class Solution:
    def fizzBuzz(self, n: int) -> List[str]:
        """
        :type n: int
        :rtype: List[str]
        """
        # ans list
        ans = []

        for num in range(1,n+1):

            divisible_by_3 = (num % 3 == 0)
            divisible_by_5 = (num % 5 == 0)

            if divisible_by_3 and divisible_by_5:
                ans.append("FizzBuzz")
            elif divisible_by_3:
                ans.append("Fizz")
            elif divisible_by_5:
                ans.append("Buzz")
            else:
                ans.append(str(num))

        return ans
```

<br>
