# <center>771 - Jewels and Stones (E)</center> 



<br></br>

* Tag: Hash
* Company: Amazon
* Difficulty: Easy
* Link: https://leetcode.com/problems/jewels-and-stones/

<br></br>



## Description
----
You're given strings J representing the types of stones that are jewels, and S representing the stones you have. Each character in S is a type of stone you have. You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

<br></br>



## Example
----
1. Input: J = "aA", S = "aAAbbbb" Output: 3
2. Input: J = "z", S = "ZZ" Output: 0

<br></br>



## Solution
----
### Java
```java
public class JewelsStones {
    public int numJewelsInStones(String J, String S) {
        HashMap<Character, Integer> jewels = new HashMap<Character, Integer>();
        for(int i = 0; i < J.length(); i++)
            jewels.put(J.charAt(i), 1);
        int result = 0;
        for(int i = 0; i < S.length(); i++)
            if(jewels.containsKey(S.charAt(i)))
                result++;
        
        return result;
    }
}
```

<br>


### Go
```go
func NumJewelsInStones(J, S string) int {
	jewels := make(map[uint8]int)
	for i := 0; i < len(J); i++ {
		jewels[J[i]] = 1
	}
	result := 0
	for i := 0; i < len(S); i++ {
		if _, ok := jewels[S[i]]; ok {
			result++
		}
	}

	return result
}
```

<br>


### Python
```python
class Solution:
    def numJewelsInStones(self, J, S):
        """
        :type J: str
        :type S: str
        :rtype: int
        """
        return sum([ch in set(J) for ch in S])
```