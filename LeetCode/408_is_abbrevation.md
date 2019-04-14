# <center>408 - Valid Word Abbreviation</center> 


* Tag: String
* Company: Google
* Author: Jinghua Zhu jhzhu@outlook.com

https://leetcode.com/problems/valid-word-abbreviation

<br></br>



## Description
----
Given a non-empty string word and an abbreviation abbr, return whether the string matches with the given abbreviation.

A string such as "word" contains only the following valid abbreviations:

["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]

<br></br>



## Example
----
1. Input : s = "internationalization", abbr = "i12iz4n" Output : true
2. Input : s = "apple", abbr = "a2e" Output : false

<br></br>



## Solution
----
### Go
```go
func ValidWordAbbreviation(word, abbr string) bool {
	if len(abbr) > len(word) || word == "" {
		return false
	}
	abbrLen, temp := 0, 0
	for _, v := range abbr {
		if v >= '0' && v <= '9' {
			data, _ := strconv.Atoi(string(v))
			temp = 10*temp + data
		} else {
			abbrLen += temp
			temp = 0
			if abbrLen > len(word)-1 { // important
				return false
			}
			if word[abbrLen] != byte(v) {
				return false
			}
			abbrLen++
		}
	}

	if temp != 0 { // important
		abbrLen += temp
	}

	return abbrLen == len(word)
}
```

<br>


### Java
```java
public class IsAbbreviation {
	/**
     * @param word: a non-empty string
     * @param abbr: an abbreviation
     * @return: true if string matches with the given abbr or false
     */
    public boolean validWordAbbreviation(String word, String abbr) {
        if (word == null || abbr == null || word.length() < abbr.length())
            return false;
        
        int abbrLen = 0, tmp = 0;
        char[] wordC = word.toCharArray();
        char[] abbrC = abbr.toCharArray();
        for (char ch : abbrC) {
            if (ch >= '0' && ch <= '9') {
                tmp = tmp * 10 + Integer.valueOf(String.valueOf(ch));
            } else {
                abbrLen += tmp;
                tmp = 0;
                if (abbrLen >= wordC.length) // important
                    return false;
                if (wordC[abbrLen] != ch)
                    return false;
                abbrLen++;
            }
        }
        if (tmp != 0) // important
            abbrLen += tmp;
        
        return abbrLen == wordC.length;
    }
}
```