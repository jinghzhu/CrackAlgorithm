# <center>53 - Reverse Words (E)</center> 



<br></br>

* Difficulty: Easy
* Tag: String, Two Pointer
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Company: Apple, Snapchat, Microsoft
* Link: https://leetcode.com/problems/reverse-words-in-a-string/

<br></br>



## Description
----
Given an input string, reverse the string word by word.

<br></br>



## Example
----
1. Input:  "the sky is blue" Output:  "blue is sky the"
2. Input:  "hello world" Output:  "world hello"

<br></br>



## Solution
----
### Java
```java
public class ReverseWords {
	/*
     * @param s: A string
     * @return: A string
     */
    public String reverseWords(String s) {
    	if(s == null || s.length() == 0)
            return "";

        String[] array = s.split(" ");
        StringBuilder sb = new StringBuilder();
        
        for(int i = array.length - 1; i >= 0; i--){
            if(!array[i].equals("")) {
                if (sb.length() > 0)
                    sb.append(" ");
                sb.append(array[i]);
            }
        }
        
        return sb.toString();
    }
}
```