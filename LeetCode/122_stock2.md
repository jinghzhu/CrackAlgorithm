# <center>122 - Best Time to Buy and Sell Stock II (E)</center> 



<br></br>

* Tag: Array, Greddy
* Difficulty: Easy
* Link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
* Company: Bloomberg

<br></br>



## Description
----
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

<br></br>



## Example
----
Input: [7,1,5,3,6,4] Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4. Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

Input: [1,2,3,4,5] Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4. Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.

Input: [7,6,4,3,1] Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.

<br></br>



## Solution
----
### Java
```java
public class Stock2 {
	/**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        int profit = 0;
        for (int i = 0; i < prices.length - 1; i++) {
            int diff = prices[i+1] - prices[i];
            if (diff > 0)
                profit += diff;
        }
        
        return profit;
    }
}
```

<br>


## Go
```go
func BuySellStock2(prices []int) int {
	profit := 0
	for i := 0; i < len(prices)-1; i++ {
		tmp := prices[i+1] - prices[i]
		if tmp > 0 {
			profit += tmp
		}
	}

	return profit
}
```

<br>


## Python
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit, l = 0, len(prices)
        for i in range(l):
            if i < l - 1 and prices[i + 1] - prices[i] > 0:
                profit += prices[i + 1] - prices[i]
        
        return profit
```
