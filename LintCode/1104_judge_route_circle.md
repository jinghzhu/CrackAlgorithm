# <center>1104 - Judge Route Circle</center> 


* Tag: String
* Company: Google
* Author: Jinghua Zhu jhzhu@outlook.com

https://www.lintcode.com/problem/flip-game/description

<br></br>



## Description
----
Initially, there is a Robot at position (0, 0). Given a sequence of its moves, judge if this robot makes a circle, which means it moves back to the original place finally.

The move sequence is represented by a string. And each move is represent by a character. The valid robot moves are R (Right), L (Left), U (Up) and D (down). The output should be true or false representing whether the robot makes a circle.

<br></br>



## Example
----
1. Input: "UD" Output: true
2. Input: "LL" Output: false

<br></br>



## Solution
----
### Go
```go
func judgeCircle (moves string) bool {
    if len(moves) % 2 != 0 {
        return false
    }
    lr, ud := 0, 0
    for i := 0; i < len(moves); i++ {
        ch := moves[i]
        if ch == 'L' {
            lr++
        } else if ch == 'R' {
            lr--
        } else if ch == 'U' {
            ud++
        } else {
            ud--
        }
    }
    
    return ud == 0 && lr == 0
}
```

<br>


### Java
```java
public class Solution {
    /**
     * @param moves: a sequence of its moves
     * @return: if this robot makes a circle
     */
    public boolean judgeCircle(String moves) {
        if (moves == null || moves.length() % 2 != 0)
            return false;
        int lr = 0, ud = 0;
        for (int i = 0; i < moves.length(); i++) {
            char ch = moves.charAt(i);
            if (ch == 'U')
                ud++;
            else if (ch == 'D')
                ud--;
            else if (ch == 'L')
                lr++;
            else
                lr--;
        }
        
        return lr == 0 && ud == 0;
    }
}
```