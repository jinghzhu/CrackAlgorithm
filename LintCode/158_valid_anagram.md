# <center>158 - Valid Anagram (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String, Sort
* Difficulty: Easy
* Company: Uber, Amazon
* Link: https://www.lintcode.com/problem/valid-anagram/description

<br></br>



## Description
----
Given two strings s and t , write a function to determine if t is an anagram of s.

<br></br>



## Example
----
Input: s = "anagram", t = "nagaram"
Output: true

Input: s = "rat", t = "car"
Output: false

<br></br>



## Solution
----
There are 3 solutions:
1. Sort them and compare it. Time is O(nlogn) and space is O(logn).
2. Using a hash map to record all characters of first string. Then check with the other string. Time is O(n) and space is O(n).
3. Create two arrays whose lengths is 256. For each string, record the number of its characters by AscII. But it doesn't work for string like Chinese. Time is O(n) and space is O(1).

<br>


### Java
```java
public class ValidAnagram {
    /**
     * @param s: The first string
     * @param b: The second string
     * @return true or false
     */
	public boolean isAnagramByHash(String s, String t) {
        if (s == null && t == null)
        	return true;
        if (s == null || t == null || s.length() != t.length())
        	return false;
        
        HashMap<Character, Integer> count = new HashMap<Character, Integer>();
        char[] charS = s.toCharArray(), charT = t.toCharArray();
        for (char ch : charS) {
        	if (!count.containsKey(ch)) {
        		count.put(ch, 1);
        	} else {
        		int num = count.get(ch);
        		count.put(ch, ++num);
        	}
        }
        for (char ch : charT) {
        	if (!count.containsKey(ch))
        		return false;
        	int num = count.get(ch);
        	if (num == 1)
        		count.remove(ch);
        	else
        		count.put(ch, --num);
        }
        
        return true;
    }

    /**
     * @param s: The first string
     * @param b: The second string
     * @return true or false
     */
    public boolean isAnagramByAscII(String s, String t) {
        int[] cntS = new int[256];
        int[] cntT = new int[256];
        
        for (char c : s.toCharArray())
            cntS[c]++;
        for (char c : t.toCharArray())
            cntT[c]++;
        for (int i = 0; i < 256; i++)
            if (cntS[i] != cntT[i])
                return false;
        
        return true;
    }
	
    /**
     * @param s: The first string
     * @param b: The second string
     * @return true or false
     */
	public boolean isAnagramBySort(String s, String t) {
		if (s == null && t == null)
        	return true;
        if (s == null || t == null || s.length() != t.length())
        	return false;
        
        char[] charS = s.toCharArray(), charT = t.toCharArray();
        sort(charS);
        sort(charT);
        for (int i = 0; i < charS.length; i++)
        	if (charS[i] != charT[i])
        		return false;
        
        return true;
    }
	
	private void sort(char[] A) {
		if (A != null && A.length > 1)
			sortPartition(A, 0, A.length - 1);
	}
	
	private void sortPartition(char[] A, int start, int end) {
		if (A != null && start < end && start > -1 && end < A.length) {
			int partition = getPartition(A, start, end);
			sortPartition(A, start, partition - 1);
			sortPartition(A, partition + 1, end);
		}
	}
	
	private int getPartition(char[] A, int start, int end) {
		int i = start - 1, pivot = A[end];
		for (int j = start; j < end; j++) {
			if (A[j] <= pivot) {
				i++;
				if (i != j)
					swap(A, i, j);
			}	
		}
		swap(A, i + 1, end);
		
		return i + 1; // Pivot is at i + 1.
	}
	
	private void swap(char[] data, int left, int right) {
		char temp = data[left];
		data[left] = data[right];
		data[right] = temp;
	}
}
```

<br>


### Go
```go
func IsAnagramByHash(s, t string) bool {
	if len(s) != len(t) {
		return false
	}
	count := make(map[byte]int)
	for i := 0; i < len(s); i++ {
		num, ok := count[s[i]]
		if !ok {
			count[s[i]] = 1
		} else {
			count[s[i]] = num + 1
		}
	}
	for i := 0; i < len(t); i++ {
		num, ok := count[t[i]]
		if !ok {
			return false
		}
		if num == 1 {
			delete(count, t[i])
		} else {
			count[t[i]] = num - 1
		}
	}

	return true
}
```

```go
func IsAnagramByAscII(s, t string) bool {
	if len(s) != len(t) {
		return false
	}
	chS, chT := make([]int32, 256, 256), make([]int32, 256, 256)
	for k := range s {
		chS[s[k]]++
		chT[t[k]]++
	}
	for k := range chS {
		if chS[k] != chT[k] {
			return false
		}
	}

	return true
}
```

```go
func IsAnagramBySort(s, t string) bool {
	if len(s) != len(t) {
		return false
	}
	bs, bt := []byte(s), []byte(t)
	sort242(bs)
	sort242(bt)
	for k, v := range bs {
		if v != bt[k] {
			return false
		}
	}

	return true
}

func sort242(data []byte) {
	if len(data) > 1 {
		sortHelper(data, 0, len(data)-1)
	}
}

func sortHelper(data []byte, start, end int) {
	if start < 0 || end < start || end >= len(data) {
		return
	}

	left, right, pivot := start, end, data[start+(end-start)/2]

	for left <= right {
		for (left <= right) && (data[left] < pivot) {
			left++
		}
		for (left <= right) && (data[right] > pivot) {
			right--
		}
		if left <= right {
			temp := data[left]
			data[left] = data[right]
			data[right] = temp
			left++
			right--
		}
	}

	sortHelper(data, start, right)
	sortHelper(data, left, end)
}
```

<br>


### Python
----
```python
class IsAnagram:
    def solution_sort(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)

    def solution_ascii(self, s, t):
        ch1, ch2 = [], []
        for i in range(256):
            ch1.append(0)
            ch2.append(0)
        for i in range(len(s)):
            ch1[ord(s[i])] += 1
        for i in range(len(t)):
            ch2[ord(t[i])] += 1
        for i in range(256):
            if ch1[i] != ch2[i]:
                return False
        return True
```