# <center>1186 - Encode and Decode TinyURL (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String, Hash, Math
* Comapny: Google, Uber, Facebook, Amazon
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/encode-and-decode-tinyurl/description

<br></br>



## Description
----
TinyURL is a URL shortening service where you enter a URL such as https://leetcode.com/problems/design-tinyurl and it returns a short URL such as http://tinyurl.com/4e9iAk.

Design the encode and decode methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

<br></br>



## Solution
----
算法：
1. 引入全局自增ID，长URL互转URL缓存。
2. Create操作时，先检查是否存在。如果不存在，获得ID，然后按62位编码。

<br>


## Go
```go
var (
	tiny_url_id int               = 0
	l2sM        map[string]string = make(map[string]string)
	s2lM        map[string]string = make(map[string]string)
)

const (
	tiny_url_base62 string = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
)

func tinyURLEncode(longUrl string) string {
	if v, ok := l2sM[longUrl]; ok {
		return v
	}
	id := tinyURLGetNextID()
	shortUrl := tinyRULEncodeBase62(id)
	l2sM[longUrl] = shortUrl
	s2lM[shortUrl] = longUrl

	return shortUrl
}

func tinyURLDecode(shortUrl string) string {
	return s2lM[shortUrl]
}

func tinyURLGetNextID() int {
	tiny_url_id++
	return tiny_url_id
}

func tinyRULEncodeBase62(id int) string {
	res := ""
	for ; id > 0; id /= 62 {
		i := id % 62
		res = string(tiny_url_base62[i]) + res
	}
	for len(res) < 6 {
		res = "0" + res
	}

	return res
}
```

<br>


### Python
```python
class TinyURL:
    def __init__(self):
        self.l2s_m = {}
        self.s2l_m = {}
        self.BASE64 = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
        self.global_id = -1

    def encode(self, long_url: str) -> str:
        if long_url in self.l2s_m:
            return self.l2s_m[long_url]
        global_id = self.get_next_id()
        short_url = self.encode_base62(global_id)
        self.l2s_m[long_url] = short_url
        self.s2l_m[short_url] = long_url

        return short_url

    def decode(self, short_url):
        return self.s2l_m[short_url]

    def get_next_id(self):
        self.global_id += 1
        return self.global_id

    def encode_base62(self, global_id: int) -> str:
        res = ""
        while global_id > 0:
            res = self.BASE64[global_id % 62] + res
            global_id = global_id // 62
        while len(res) < 6:
            res = "0" + res

        return res

    def decode(self, short_url: str) -> str:
        return self.s2l_m[short_url]
```