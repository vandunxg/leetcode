---
comments: true
difficulty: Medium
rating: 1748
source: Weekly Contest 168 Q3
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1297. Maximum Number of Occurrences of a Substring](https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring)

[中文文档](/solution/1200-1299/1297.Maximum%20Number%20of%20Occurrences%20of%20a%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về số lần xuất hiện lớn nhất của <strong>bất kỳ</strong> chuỗi con nào thỏa các quy tắc sau:</p>

<ul>
	<li>Số ký tự khác nhau trong chuỗi con phải nhỏ hơn hoặc bằng <code>maxLetters</code>.</li>
	<li>Độ dài chuỗi con nằm trong đoạn bao gồm cả hai đầu mút <code>minSize</code> và <code>maxSize</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aababcaab&quot;, maxLetters = 2, minSize = 3, maxSize = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chuỗi con &quot;aab&quot; xuất hiện 2 lần trong chuỗi ban đầu.
Chuỗi này thỏa điều kiện: có 2 ký tự khác nhau và độ dài 3 (nằm trong khoảng từ minSize đến maxSize).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaa&quot;, maxLetters = 1, minSize = 3, maxSize = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chuỗi con &quot;aaa&quot; xuất hiện 2 lần trong chuỗi. Các lần xuất hiện có thể chồng lấn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxLetters &lt;= 26</code></li>
	<li><code>1 &lt;= minSize &lt;= maxSize &lt;= min(26, s.length)</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Trong các chuỗi con thỏa giới hạn về số ký tự và độ dài, ta cần tìm chuỗi xuất hiện nhiều nhất. Nếu một chuỗi hợp lệ dài hơn xuất hiện $t$ lần, mỗi tiền tố dài $minSize$ của nó cũng xuất hiện ít nhất $t$ lần. Vì vậy, chỉ cần đếm các cửa sổ độ dài $minSize$ có không quá $maxLetters$ ký tự khác nhau.
>
> Với $n \le 10^5$ và $minSize \le 26$, ta liệt kê các cửa sổ này, dùng set kiểm tra tập ký tự rồi lấy tần suất lớn nhất trong hash map.

<!-- thinking:end -->

Theo đề bài, nếu một chuỗi dài thỏa điều kiện thì chuỗi con có độ dài $\textit{minSize}$ của nó cũng thỏa điều kiện. Vì vậy, ta chỉ cần liệt kê mọi chuỗi con độ dài $\textit{minSize}$ trong $s$, dùng hash table ghi lại tần suất xuất hiện của chúng rồi tìm tần suất lớn nhất làm đáp án.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n \times m)$, trong đó $n$ là độ dài chuỗi $s$ và $m$ là giá trị $\textit{minSize}$. Trong bài này, $m$ không vượt quá $26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFreq(self, s: str, maxLetters: int, minSize: int, maxSize: int) -> int:
        ans = 0
        cnt = Counter()
        for i in range(len(s) - minSize + 1):
            t = s[i : i + minSize]
            ss = set(t)
            if len(ss) <= maxLetters:
                cnt[t] += 1
                ans = max(ans, cnt[t])
        return ans
```

#### Java

```java
class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        int ans = 0;
        Map<String, Integer> cnt = new HashMap<>();
        for (int i = 0; i < s.length() - minSize + 1; ++i) {
            String t = s.substring(i, i + minSize);
            Set<Character> ss = new HashSet<>();
            for (int j = 0; j < minSize; ++j) {
                ss.add(t.charAt(j));
            }
            if (ss.size() <= maxLetters) {
                cnt.merge(t, 1, Integer::sum);
                ans = Math.max(ans, cnt.get(t));
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) {
        int ans = 0;
        unordered_map<string, int> cnt;
        for (int i = 0; i < s.size() - minSize + 1; ++i) {
            string t = s.substr(i, minSize);
            unordered_set<char> ss(t.begin(), t.end());
            if (ss.size() <= maxLetters) {
                ans = max(ans, ++cnt[t]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxFreq(s string, maxLetters int, minSize int, maxSize int) (ans int) {
	cnt := map[string]int{}
	for i := 0; i < len(s)-minSize+1; i++ {
		t := s[i : i+minSize]
		ss := map[rune]bool{}
		for _, c := range t {
			ss[c] = true
		}
		if len(ss) <= maxLetters {
			cnt[t]++
			if ans < cnt[t] {
				ans = cnt[t]
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function maxFreq(s: string, maxLetters: number, minSize: number, maxSize: number): number {
    const cnt = new Map<string, number>();
    let ans = 0;
    for (let i = 0; i < s.length - minSize + 1; ++i) {
        const t = s.slice(i, i + minSize);
        const ss = new Set(t.split(''));
        if (ss.size <= maxLetters) {
            cnt.set(t, (cnt.get(t) || 0) + 1);
            ans = Math.max(ans, cnt.get(t)!);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::{HashMap, HashSet};

impl Solution {
    pub fn max_freq(s: String, max_letters: i32, min_size: i32, _max_size: i32) -> i32 {
        let n = s.len();
        let m = min_size as usize;
        let max_letters = max_letters as usize;
        let bytes = s.as_bytes();
        let mut cnt: HashMap<&[u8], i32> = HashMap::new();
        let mut ans = 0;

        for i in 0..=n - m {
            let t = &bytes[i..i + m];

            let mut set = HashSet::new();
            for &c in t {
                set.insert(c);
                if set.len() > max_letters {
                    break;
                }
            }
            if set.len() <= max_letters {
                let v = cnt.entry(t).or_insert(0);
                *v += 1;
                ans = ans.max(*v);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding window + string hashing

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng trực tiếp chuỗi làm key và tạo mỗi cửa sổ trong $O(minSize)$. Sliding window duy trì số ký tự khác nhau; string hash chuyển cửa sổ thành key số nguyên trong $O(1)$, giúp đạt thời gian tuyến tính.

<!-- thinking:end -->

Ta có thể dùng sliding window để duy trì số ký tự khác nhau trong chuỗi con hiện tại, đồng thời dùng string hashing để tính nhanh hash value của chuỗi con. Nhờ đó, không cần dùng chuỗi làm key cho hash table và hiệu năng được cải thiện.

Cụ thể, ta định nghĩa class $\textit{Hashing}$ để tiền xử lý các giá trị prefix hash và lũy thừa của chuỗi $s$, nhờ đó có thể tính hash value của bất kỳ chuỗi con nào trong $O(1)$.

Sau đó, dùng sliding window duyệt chuỗi $s$ và duy trì số ký tự khác nhau trong cửa sổ hiện tại. Khi cửa sổ đạt kích thước $\textit{minSize}$, ta tính hash value của chuỗi con hiện tại và cập nhật số lần xuất hiện. Tiếp theo, trượt cửa sổ sang phải một vị trí rồi cập nhật tần suất ký tự và số ký tự khác nhau.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Hashing:
    __slots__ = ["mod", "h", "p"]

    def __init__(
        self, s: Union[str, List[str]], base: int = 13331, mod: int = 998244353
    ):
        self.mod = mod
        self.h = [0] * (len(s) + 1)
        self.p = [1] * (len(s) + 1)
        for i in range(1, len(s) + 1):
            self.h[i] = (self.h[i - 1] * base + ord(s[i - 1])) % mod
            self.p[i] = (self.p[i - 1] * base) % mod

    def query(self, l: int, r: int) -> int:
        return (self.h[r] - self.h[l - 1] * self.p[r - l + 1]) % self.mod


class Solution:
    def maxFreq(self, s: str, maxLetters: int, minSize: int, maxSize: int) -> int:
        freq = Counter()
        hashing = Hashing(s)
        cnt = Counter()
        ans = k = 0
        for i, c in enumerate(s, 1):
            freq[c] += 1
            if freq[c] == 1:
                k += 1
            if i >= minSize:
                if k <= maxLetters:
                    x = hashing.query(i - minSize + 1, i)
                    cnt[x] += 1
                    ans = max(ans, cnt[x])
                j = i - minSize
                freq[s[j]] -= 1
                if freq[s[j]] == 0:
                    k -= 1
        return ans
```

#### Java

```java
class Hashing {
    private final long[] p;
    private final long[] h;
    private final long mod;

    public Hashing(String word) {
        this(word, 13331, 998244353);
    }

    public Hashing(String word, long base, int mod) {
        int n = word.length();
        p = new long[n + 1];
        h = new long[n + 1];
        p[0] = 1;
        this.mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = p[i - 1] * base % mod;
            h[i] = (h[i - 1] * base + word.charAt(i - 1)) % mod;
        }
    }

    public long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
}

class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        int n = s.length();
        Hashing hashing = new Hashing(s);
        int[] freq = new int[256];
        int k = 0;
        int ans = 0;
        Map<Long, Integer> cnt = new HashMap<>();

        for (int i = 1; i <= n; i++) {
            if (++freq[s.charAt(i - 1)] == 1) {
                k++;
            }

            if (i >= minSize) {
                if (k <= maxLetters) {
                    long x = hashing.query(i - minSize + 1, i);
                    ans = Math.max(ans, cnt.merge(x, 1, Integer::sum));
                }
                int j = i - minSize;
                if (--freq[s.charAt(j)] == 0) {
                    k--;
                }
            }
        }

        return ans;
    }
}
```

#### C++

```cpp
class Hashing {
public:
    vector<long long> p, h;
    long long mod;

    Hashing(const string& word)
        : Hashing(word, 13331, 998244353) {}

    Hashing(const string& word, long long base, long long mod)
        : mod(mod) {
        int n = word.size();
        p.assign(n + 1, 1);
        h.assign(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            p[i] = p[i - 1] * base % mod;
            h[i] = (h[i - 1] * base + word[i - 1]) % mod;
        }
    }

    long long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
};

class Solution {
public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) {
        int n = s.size();
        Hashing hashing(s);
        vector<int> freq(256, 0);
        int k = 0;
        int ans = 0;
        unordered_map<long long, int> cnt;

        for (int i = 1; i <= n; i++) {
            if (++freq[s[i - 1]] == 1) {
                k++;
            }

            if (i >= minSize) {
                if (k <= maxLetters) {
                    long long x = hashing.query(i - minSize + 1, i);
                    ans = max(ans, ++cnt[x]);
                }
                int j = i - minSize;
                if (--freq[s[j]] == 0) {
                    k--;
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxFreq(s string, maxLetters int, minSize int, maxSize int) int {
	n := len(s)
	hashing := NewHashing(s)
	freq := make([]int, 256)
	k := 0
	ans := 0
	cnt := make(map[uint64]int)

	for i := 1; i <= n; i++ {
		c := s[i-1]
		freq[c]++
		if freq[c] == 1 {
			k++
		}

		if i >= minSize {
			if k <= maxLetters {
				x := hashing.Query(i-minSize+1, i)
				cnt[x]++
				if cnt[x] > ans {
					ans = cnt[x]
				}
			}
			j := i - minSize
			c2 := s[j]
			freq[c2]--
			if freq[c2] == 0 {
				k--
			}
		}
	}

	return ans
}

type Hashing struct {
	p, h []uint64
	mod  uint64
	base uint64
}

func NewHashing(word string) *Hashing {
	return NewHashingWithBase(word, 13331, 998244353)
}

func NewHashingWithBase(word string, base uint64, mod uint64) *Hashing {
	n := len(word)
	p := make([]uint64, n+1)
	h := make([]uint64, n+1)
	p[0] = 1
	for i := 1; i <= n; i++ {
		p[i] = p[i-1] * base % mod
		h[i] = (h[i-1]*base + uint64(word[i-1])) % mod
	}
	return &Hashing{p, h, mod, base}
}

func (hs *Hashing) Query(l, r int) uint64 {
	return (hs.h[r] + hs.mod - hs.h[l-1]*hs.p[r-l+1]%hs.mod) % hs.mod
}
```

#### TypeScript

```ts
class Hashing {
    private p: bigint[];
    private h: bigint[];
    private mod: bigint;

    constructor(s: string, base: bigint = 13331n, mod: bigint = 998244353n) {
        const n = s.length;
        this.mod = mod;
        this.p = new Array<bigint>(n + 1).fill(1n);
        this.h = new Array<bigint>(n + 1).fill(0n);
        for (let i = 1; i <= n; i++) {
            this.p[i] = (this.p[i - 1] * base) % mod;
            this.h[i] = (this.h[i - 1] * base + BigInt(s.charCodeAt(i - 1))) % mod;
        }
    }

    query(l: number, r: number): bigint {
        return (this.h[r] - ((this.h[l - 1] * this.p[r - l + 1]) % this.mod) + this.mod) % this.mod;
    }
}

function maxFreq(s: string, maxLetters: number, minSize: number, maxSize: number): number {
    const n = s.length;
    const hashing = new Hashing(s);
    const freq = new Array<number>(256).fill(0);
    let k = 0;
    let ans = 0;
    const cnt = new Map<bigint, number>();

    for (let i = 1; i <= n; i++) {
        const c = s.charCodeAt(i - 1);
        if (++freq[c] === 1) {
            k++;
        }

        if (i >= minSize) {
            if (k <= maxLetters) {
                const x = hashing.query(i - minSize + 1, i);
                const v = (cnt.get(x) || 0) + 1;
                cnt.set(x, v);
                ans = Math.max(ans, v);
            }
            const j = i - minSize;
            const c2 = s.charCodeAt(j);
            if (--freq[c2] === 0) {
                k--;
            }
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_freq(s: String, max_letters: i32, min_size: i32, _max_size: i32) -> i32 {
        let n = s.len();
        let bytes = s.as_bytes();
        let hashing = Hashing::new(bytes.to_vec());
        let mut freq = [0i32; 256];
        let mut k = 0;
        let mut ans = 0;
        let mut cnt: std::collections::HashMap<u64, i32> = std::collections::HashMap::new();

        for i in 1..=n {
            let c = bytes[i - 1] as usize;
            freq[c] += 1;
            if freq[c] == 1 {
                k += 1;
            }

            if i as i32 >= min_size {
                if k <= max_letters {
                    let x = hashing.query(i - min_size as usize, i - 1);
                    let v = cnt.entry(x).and_modify(|v| *v += 1).or_insert(1);
                    ans = ans.max(*v);
                }
                let j = i - min_size as usize;
                let c2 = bytes[j] as usize;
                freq[c2] -= 1;
                if freq[c2] == 0 {
                    k -= 1;
                }
            }
        }

        ans
    }
}

struct Hashing {
    p: Vec<u64>,
    h: Vec<u64>,
    base: u64,
    modv: u64,
}

impl Hashing {
    fn new(s: Vec<u8>) -> Self {
        Self::with_params(s, 13331, 998244353)
    }

    fn with_params(s: Vec<u8>, base: u64, modv: u64) -> Self {
        let n = s.len();
        let mut p = vec![0u64; n + 1];
        let mut h = vec![0u64; n + 1];
        p[0] = 1;
        for i in 1..=n {
            p[i] = p[i - 1].wrapping_mul(base) % modv;
            h[i] = (h[i - 1].wrapping_mul(base) + s[i - 1] as u64) % modv;
        }
        Self { p, h, base, modv }
    }

    fn query(&self, l: usize, r: usize) -> u64 {
        let mut res = self.h[r + 1] + self.modv - (self.h[l] * self.p[r - l + 1] % self.modv);
        res %= self.modv;
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
