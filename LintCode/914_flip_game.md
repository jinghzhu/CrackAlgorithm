# <center>914 - Flip Game</center> 


* Tag: String
* Company: Google
* Author: Jinghua Zhu jhzhu@outlook.com

https://www.lintcode.com/problem/flip-game/description

<br></br>



## Description
----
You are playing the following Flip Game with your friend: Given a string that contains only these two characters: + and -, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to compute all possible states of the string after one valid move.

<br></br>



## Example
----
Example 1:
```
Input:  s = "++++"
Output: 
[
  "--++",
  "+--+",
  "++--"
]
```

Example 2:
```
Input: s = "---+++-+++-+"
Output: 
[
	"---+++-+---+",
	"---+++---+-+",
	"---+---+++-+",
	"-----+-+++-+"
]
```

<br></br>



## Solution
----
### Go
```go
func generatePossibleNextMoves (s string) []string {
    results := make([]string, 0)
    for i := 0; i < len(s) - 1; i++ {
        if s[i] == '+' && s[i + 1] == '+' {
            results = append(results, helper(s, i))
        }
    }
    
    return results
}

func helper(s string, i int) string {
    s1 := s
    if i < len(s) - 2 {
        return s1[:i] + "--" + s1[i+2:]
    }
    
    return s1[:i] + "--"
}
```

<br>


### Java
```java
public class Solution {
    /**
     * @param s: the given string
     * @return: all the possible states of the string after one valid move
     */
    public List<String> generatePossibleNextMoves(String s) {
        ArrayList<String> results = new ArrayList<String>();
        if (s == null)
            return results;
        
        for (int i = 0; i < s.length() - 1; i++) 
            if (s.charAt(i) == '+' && s.charAt(i + 1) == '+')
                results.add(flipStr(s, i));
        
        return results;
    }
    
    private String flipStr(String s, int i) {
        char[] arr = s.toCharArray();
        arr[i] = '-';
        arr[i + 1] = '-';
        
        return String.valueOf(arr);
    }
}
```