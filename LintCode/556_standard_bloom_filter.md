# <center>556 - Standard Bloom Filter (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Tag: Hash
* Link: https://www.lintcode.com/problem/standard-bloom-filter/description

<br></br>



## Description
----
Implement a standard bloom filter. Support the following method:
1. `StandardBloomFilter(k)`: The constructor and you need to create k hash functions.
2. `add(string)`: Add a string into bloom filter.
3. `contains(string)`: Check a string whether exists in bloom filter.

<br></br>



## Solution
----
### Java
```java
public class BloomFilter2 {
    public BitSet bits;
    public int k; // Hash seed number.
    public List<HashFunction> hashFunc;

    public BloomFilter2(int k) {
        this.k = k;
        hashFunc = new ArrayList<HashFunction>();
        for (int i = 0; i < k; ++i)
            hashFunc.add(new HashFunction(100000 + i, 2 * i + 3));
        bits = new BitSet(100000 + k);
    }

    public void add(String word) {
        for (int i = 0; i < k; ++i) {
            int position = hashFunc.get(i).hash(word);
            bits.set(position);
        }
    }

    public boolean contains(String word) {
        for (int i = 0; i < k; ++i) {
            int position = hashFunc.get(i).hash(word);
            if (!bits.get(position))
                return false;
        }
        return true;
    }
    
    public static class HashFunction {
        public int cap, seed;

        public HashFunction(int cap, int seed) {
            this.cap = cap;
            this.seed = seed;
        }

        public int hash(String value) {
            int ret = 0;
            int n = value.length();
            for (int i = 0; i < n; ++i) {
                ret += seed * ret + value.charAt(i);
                ret %= cap;
            }
            
            return ret;
        }
    }
}
```

```java
public class BloomFilter {
	// BitSet is initialized to 2^24 bit.
	private static final int DEFAULT_SIZE = 1 << 25; 
	// All hash seeds should be primer.
	private static final int[] seeds = new int[] {5, 7, 11, 13, 31, 37, 61};
	private BitSet bits = new BitSet(DEFAULT_SIZE);
	// Hash featured objects. 
	private SimpleHash[] hashFunc = new SimpleHash[seeds.length];

	public BloomFilter1() {
		for (int i = 0; i < seeds.length; i++)
			hashFunc[i] = new SimpleHash(DEFAULT_SIZE, seeds[i]);
	}

	// Mark string into bits.
	public void add(String value) {
		for (SimpleHash f : hashFunc) 
			bits.set(f.hash(value), true);
	}

	public boolean isContain(String value) {
		if (value == null)
			return false;
		boolean ret = true;
		for (SimpleHash f : hashFunc) 
			ret = ret && bits.get(f.hash(value));
		
		return ret;
	}

	// Class of hash function.
	public static class SimpleHash {
		private int cap;
		private int seed;

		public SimpleHash(int cap, int seed) {
			this.cap = cap;
			this.seed = seed;
		}

		// to hash
		public int hash(String value) {
			int result = 0;
			int len = value.length();
			for (int i = 0; i < len; i++) 
				result = seed * result + value.charAt(i);
			
			return (cap - 1) & result;
		}
	}
}
```

<br>


### Python
```python
class BloomFilter:
    class HashFunction:
        def __init__(self, cap, seed):
            self.cap = cap
            self.seed = seed

        def hash(self, value):
            ret = 0
            for i in value:
                ret += self.seed * ret + ord(i)
                ret %= self.cap

            return ret

    # @param {int} k an integer
    def __init__(self, k):
        self.bitset = dict()
        self.hashFunc = []
        for i in range(k):
            self.hashFunc.append(self.HashFunction(random.randint(10000, 20000), i * 2 + 3))

    # @param {str} word a string
    def add(self, word):
        for f in self.hashFunc:
            position = f.hash(word)
            self.bitset[position] = 1

    # @param {str} word a string
    # @return {bool} True if word is exists in bllom filter or false
    def contains(self, word):
        for f in self.hashFunc:
            position = f.hash(word)
            if position not in self.bitset:
                return False

        return True
```

<br>


### Go
```go
import (
	"math/rand"
	"time"
)

type bloomHash struct {
	cap  int
	seed int
}

func newBloomHash(c, s int) *bloomHash {
	return &bloomHash{
		cap:  c,
		seed: s,
	}
}

func (b *bloomHash) hash(data string) int {
	result := 0
	for _, v := range data {
		result += b.seed*result + int(v)
		result %= b.cap
	}

	return result
}

// BloomFilter is a standard bloom filter. Support the following methods:
// 1. NewBloomFilter(k) - The constructor and you need to create k hash functions.
// 2. Add(string) - Add a string into bloom filter.
// 3. Contains(string) - Check a string whether exists in bloom filter.
type BloomFilter struct {
	BitSet   map[int]bool
	HashFunc []*bloomHash
}

// NewBloomFilter returns a new instance of Bloom Filter.
func NewBloomFilter(n int) *BloomFilter {
	b := &BloomFilter{
		BitSet:   make(map[int]bool),
		HashFunc: make([]*bloomHash, n, n),
	}
	for i := 0; i < n; i++ {
		rand.Seed(time.Now().UnixNano())
		b.HashFunc[i] = newBloomHash(rand.Intn(100000), i*2+3)
	}

	return b
}

// Add a string into bloom filter.
func (b *BloomFilter) Add(s string) {
	for _, h := range b.HashFunc {
		b.BitSet[h.hash(s)] = true
	}
}

// Contains checks a string whether exists in bloom filter.
func (b *BloomFilter) Contains(s string) bool {
	for _, h := range b.HashFunc {
		if _, ok := b.BitSet[h.hash(s)]; !ok {
			return false
		}
	}

	return true
}
```