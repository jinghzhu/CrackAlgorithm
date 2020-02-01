# <center>430 - Scramble String (H)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Divide and Conquer, String
* Difficulty: Hard
* Company: Google
* Link: https://www.lintcode.com/problem/scramble-string

<br></br>



## Description
----
Given a string s1, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of s1 = "great":

```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```

To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node "gr" and swap its two children, it produces a scrambled string "rgeat".

```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```

We say that "rgeat" is a scrambled string of "great".

<br></br>



## Solution
----
算法：
1. 记忆化搜索
2. 如果s1和s2满足条件，必定存在一个索引i，满足0 <= i <= l，把s1和s2分别分割成s11，s12和s21，s22。
3. 那么，转化为子问题(isScramble(s11, s21) && isScramble(s12, s22)) || (isScramble(s11, s22) && isScramble(s12, s21))
4. 使用一个哈希表记录已经检查过的配对。
5. 递归返回条件为检查哈希或判断长度。长度小于等于3是必可以scramble的。

<br>


### Go
```go
func IsScramble(s1, s2 string) bool {
	m := make(map[string]bool)

	return isHelper(s1, s2, m)
}

func isHelper(s1, s2 string, m map[string]bool) bool {
	if len(s1) != len(s2) {
		return false
	}
	if _, ok := m[s1+"#"+s2]; ok {
		return m[s1+"#"+s2]
	}

	l := len(s1)
	if l <= 3 {
		return true
	}

	for i := 1; i < l; i++ {
		r1, r2 := isHelper(s1[:i], s2[:i], m), isHelper(s1[i:], s2[i:], m)
		r3, r4 := isHelper(s1[:i], s2[l-i:], m), isHelper(s1[i:], s2[:l-i], m)
		if (r1 && r2) || (r3 && r4) {
			m[s1+"#"+s2] = true
			return true
		}
	}

	m[s1+"#"+s2] = false
	return false
}
```

<br>
