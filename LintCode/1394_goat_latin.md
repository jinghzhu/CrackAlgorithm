# <center>1394 - Goat Latin</center> 


* Tag: String
* Company: Facebook

https://www.lintcode.com/problem/goat-latin/description

<br></br>



## Description
----
A sentence `S` is given, composed of words separated by spaces. Each word consists of lowercase and uppercase letters only. We would like to convert the sentence to "Goat Latin" (a made-up language similar to Pig Latin.)

The rules of Goat Latin are as follows:
* If a word begins with a vowel (a, e, i, o, or u), append "ma" to the end of the word. For example, the word 'apple' becomes 'applema'.
* If a word begins with a consonant (i.e. not a vowel), remove the first letter and append it to the end, then add "ma". For example, the word "goat" becomes "oatgma".
* Add one letter 'a' to the end of each word per its word index in the sentence, starting with 1.For example, the first word gets "a" added to the end, the second word gets "aa" added to the end and so on.

<br></br>



## Example
----
Example 1:
```
Input: "I speak Goat Latin"
Output: "Imaa peaksmaaa oatGmaaaa atinLmaaaaa"
```

Example 2:
```
Input: "The quick brown fox jumped over the lazy dog"
Output: "heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"
```

<br></br>



## Solution
----
### Java
```java
class Solution {
    public String toGoatLatin(String S) {
        String[] words = S.split(" ");
        
        for(int i = 0; i < words.length; i++) {
            // Apply rule 1 and 2
            String wordLower = words[i].toLowerCase();
            char first = wordLower.charAt(0);
            if("aeiou".indexOf(first) < 0) // not a vowel
                words[i] = words[i].substring(1) + words[i].charAt(0) + "ma";
            else // is vowel
                words[i] = words[i] + "ma";
            
            // Apply rule 3
            int index = i + 1;
            String as = "";
            for(int j = 0; j < index; j++)
                as = as + "a";
                
            words[i] = words[i] + as;
        }
        
        return String.join(" ", words);
    }
}
```