# <center>527 - Word Abbreviation (H)</center> 



<br></br>

* Tag: Hash, String
* Company: Google, Snapchat
* Difficulty: Hard
* Link: https://leetcode.com/problems/word-abbreviation

<br></br>



## Description
----
Given an array of n distinct non-empty strings, you need to generate minimal possible abbreviations for every word following rules below.

1. Begin with the first character and then the number of characters abbreviated, which followed by the last character.
2. If there are any conflict, that is more than one words share the same abbreviation, a longer prefix is used instead of only the first character until making the map from word to abbreviation become unique. In other words, a final abbreviation cannot map to more than one original words.
3. If the abbreviation doesn't make the word shorter, then keep it as original.

<br></br>



## Example
----
- Input: ["where","there","is","beautiful","way"]
- Output: ["w3e","t3e","is","b7l","way"]

<br></br>



## Solution
----
算法：
1. 引入变量round，标记字符串开始位置多少字符无需转换，初始化为1。
2. 遍历输入，按round = 1生成abbreviation，并在哈希表记录每个个数。
3. 遍历临时结果，如果哪个abbreviation在哈希表出现次数大于1，递增round重复前述，直到全部唯一。

<br>


### Java
```java
public class GetAbbreviation {
	public String[] solution(String[] dict) {
        if (dict == null)
            return null;

        String[] ans = new String[dict.length];
        HashMap<String, Integer> count = new HashMap<>();
        int round = 1;

        // Step 1
        for (int i = 0; i < dict.length; i++) {
            ans[i] = getAbbr(dict[i], round);
            count.put(ans[i], count.getOrDefault(ans[i], 0) + 1); 
        }

        // Step 2
        boolean unique = true;
        while (unique) {
        	unique = false;
            round++;
            for (int i = 0; i < dict.length; i++) {
                if (count.get(ans[i]) > 1) {
                    ans[i] = getAbbr(dict[i], round);
                    count.put(ans[i], count.getOrDefault(ans[i], 0) + 1);
                    unique = true;
                }
            }
        }
        
        return ans;
    }

    String getAbbr(String s, int p) { 
        if (p + 2 >= s.length())
            return s;

        return s.substring(0, p) + (s.length() - 1 - p) + s.charAt(s.length() - 1);
    }
}
```

<br>


### Go
```go
func GetAbbreviation(dict []string) []string {
	l, round := len(dict), 1
	res := make([]string, l, l)
	m := make(map[string]int)

	// Step 1
	for k, v := range dict {
		a := getAbbr(v, round)
		res[k] = a
		count, _ := m[a]
		m[a] = count + 1
	}

	// Step 2
	flag := true
	for flag {
		flag = false
		round++
		for k, v := range res {
			if m[v] > 1 {
				a := getAbbr(dict[k], round)
				res[k] = a
				count, _ := m[a]
				m[a] = count + 1
				flag = true
			}
		}
	}

	return res
}

func getAbbr(s string, r int) string {
	l := len(s)
	if 2+r >= l {
		return s
	}
	return s[:r] + fmt.Sprintf("%d", l-1-r) + s[l-1:]
}
```

<br>
