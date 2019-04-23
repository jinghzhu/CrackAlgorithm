# <center>500 - Keyboard Row</center> 


* Tag: String
* Author: Jinghua Zhu jhzhu@outlook.com

https://leetcode.com/problems/keyboard-row/

<br></br>



## Description
----
Given a List of words, return the words that can be typed using letters of alphabet on only one row's of American keyboard like the image below.

![](./Images/keyboard.png)

<br></br>



## Example
----
Input: ["Hello", "Alaska", "Dad", "Peace"]
Output: ["Alaska", "Dad"]

<br></br>



## Solution
----
### Go
```go
import "strings"

const (
    row1 string = "qwertyuiop"
    row2 string = "asdfghjkl"
    row3 string = "zxcvbnm"
)

func findWords(words []string) []string {
    result := make([]string, 0)
    for _, v := range words {
        if valid(strings.ToLower(v)) {
            result = append(result, v)
        }
    }
    
    return result
}

func valid(word string) bool {
    if len(word) < 2 {
        return true
    }
    
    id := getID(word[0])
    if id == -1 {
        return false
    }
    for i := 1; i < len(word); i++ {
        if getID(word[i]) != id {
            return false
        }
    }
    
    return true
}

func getID(char byte) int {
    for i := 0; i < len(row1); i++ {
        if row1[i] == char {
            return 1
        }
    }
    for i := 0; i < len(row2); i++ {
        if row2[i] == char {
            return 2
        }
    }
    for i := 0; i < len(row3); i++ {
        if row3[i] == char {
            return 3
        }
    }
    
    return -1
}
```

<br>


### Java
```java
public class Solution {
    /**
     * @param words: a list of strings
     * @return: return a list of strings
     */
    public String[] findWords(String[] words) {
        if (words == null || words.length < 1)
            return words;
        
        ArrayList<String> result = new ArrayList<String>();
        for (String s : words)
            if (isValid(s.toLowerCase()))
                result.add(s);
        
        return result.toArray(new String[result.size()]);
    }
    
    private boolean isValid(String str) {
        if (str == null || str.length() < 2)
            return true;
        
        char[] arr = str.toCharArray();
        int rowID = getID(arr[0]);
        if (rowID == -1)
            return false;
        for (int i = 1; i < arr.length; i++)
            if (getID(arr[i]) != rowID)
                return false;
        
        return true;
    }
    
    private int getID(char ch) {
        if (str1.indexOf(ch) >= 0)
            return 1;
        
        if (str2.indexOf(ch) >= 0)
            return 2;
            
        if (str3.indexOf(ch) >= 0)
            return 3;
        
        return -1;
    }
    
    private String str1 = "qwertyuiop";
    private String str2 = "asdfghjkl";
    private String str3 = "zxcvbnm";
}
```

<br>


### Python
```python
class Solution:
    def findWords(self, words: List[str]) -> List[str]:
        result = []
        for word in words:
            if self.is_valid(word.lower()):
                result.append(word)
        
        return result
    
    def is_valid(self, word: str) -> bool:
        if len(word) < 2:
            return True
        
        row_id = self.get_id(word[0])
        if row_id == -1:
            return False
        n = 1
        while n < len(word):
            if self.get_id(word[n]) != row_id:
                return False
            n += 1
        
        return True
    
    def get_id(self, s: str) -> int:
        row1 = "qwertyuiop"
        row2 = "asdfghjkl"
        row3 = "zxcvbnm"
        if row1.find(s) != -1:
            return 1
        if row2.find(s) != -1:
            return 2
        if row3.find(s) != -1:
            return 3
        
        return -1
```