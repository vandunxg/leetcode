---
comments: true
difficulty: Hard
rating: 2661
source: Weekly Contest 415 Q4
tags:
    - Greedy
    - Segment Tree
    - Array
    - String
    - Binary Search
    - Dynamic Programming
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3292. Minimum Number of Valid Strings to Form Target II](https://leetcode.com/problems/minimum-number-of-valid-strings-to-form-target-ii)

[中文文档](/solution/3200-3299/3292.Minimum%20Number%20of%20Valid%20Strings%20to%20Form%20Target%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> và một chuỗi <code>target</code>.</p>

<p>Một chuỗi <code>x</code> được gọi là <strong>hợp lệ</strong> nếu <code>x</code> là <span data-keyword="string-prefix">tiền tố</span> của <strong>bất kỳ</strong> chuỗi nào trong <code>words</code>.</p>

<p>Trả về số lượng <strong>nhỏ nhất</strong> các chuỗi <strong>hợp lệ</strong> có thể được <em>nối</em> lại để tạo thành <code>target</code>. Nếu <strong>không</strong> thể tạo thành <code>target</code>, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abc&quot;,&quot;aaaaa&quot;,&quot;bcdef&quot;], target = &quot;aabcdabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi đích có thể được tạo thành bằng cách nối:</p>

<ul>
	<li>Tiền tố có độ dài 2 của <code>words[1]</code>, tức là <code>&quot;aa&quot;</code>.</li>
	<li>Tiền tố có độ dài 3 của <code>words[2]</code>, tức là <code>&quot;bcd&quot;</code>.</li>
	<li>Tiền tố có độ dài 3 của <code>words[0]</code>, tức là <code>&quot;abc&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abababab&quot;,&quot;ab&quot;], target = &quot;ababaababa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi đích có thể được tạo thành bằng cách nối:</p>

<ul>
	<li>Tiền tố có độ dài 5 của <code>words[0]</code>, tức là <code>&quot;ababa&quot;</code>.</li>
	<li>Tiền tố có độ dài 5 của <code>words[0]</code>, tức là <code>&quot;ababa&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abcdef&quot;], target = &quot;xyz&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 5 * 10<sup>4</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>sum(words[i].length) &lt;= 10<sup>5</sup></code>.</li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= target.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash chuỗi + Tìm kiếm nhị phân + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Phát biểu giống bài I, nhưng độ dài có thể lên đến $5\times 10^4$, nên trie kết hợp memoization với độ phức tạp $O(n^2)$ là quá chậm. Độ dài tiền tố khớp dài nhất bắt đầu từ $i$ có tính đơn điệu, vì vậy ta có thể dùng hashing kết hợp tìm kiếm nhị phân để tìm nó trong thời gian logarithm.
>
> Gom các hash của từ theo độ dài tiền tố. $f(i)$ tìm kiếm nhị phân độ dài lớn nhất $L$ sao cho hash của $\textit{target}[i..i+L)$ nằm trong nhóm tương ứng. Các điểm xa nhất này tạo thành một bài toán nhảy: duy trì điểm xa nhất hiện tại và thực hiện một lần nhảy khi con trỏ chạm tới điểm đó.

<!-- thinking:end -->

Do quy mô dữ liệu lớn của bài toán này, phương pháp "Trie + Memoization" sẽ bị quá thời gian. Ta cần tìm một lời giải hiệu quả hơn.

Xét việc bắt đầu từ ký tự thứ $i$ của chuỗi $\textit{target}$ và tìm độ dài lớn nhất của đoạn con khớp, ký hiệu là $\textit{dist}$. Với mọi $j \in [i, i + \textit{dist} - 1]$, ta có thể tìm một chuỗi trong $\textit{words}$ sao cho $\textit{target}[i..j]$ là tiền tố của chuỗi đó. Điều này có tính đơn điệu, nên ta có thể dùng tìm kiếm nhị phân để xác định $\textit{dist}$.

Cụ thể, trước tiên ta tiền xử lý các giá trị hash của mọi tiền tố của các chuỗi trong $\textit{words}$ và lưu chúng trong mảng $\textit{s}$, được nhóm theo độ dài tiền tố. Đồng thời, ta tiền xử lý các giá trị hash của $\textit{target}$ và lưu trong $\textit{hashing}$ để thuận tiện truy vấn giá trị hash của bất kỳ đoạn $\textit{target}[l..r]$ nào.

Tiếp theo, ta thiết kế hàm $\textit{f}(i)$ biểu diễn độ dài lớn nhất của đoạn con khớp bắt đầu từ ký tự thứ $i$ của chuỗi $\textit{target}$. Ta có thể xác định $\textit{f}(i)$ bằng tìm kiếm nhị phân.

Đặt biên trái của tìm kiếm nhị phân là $l = 0$ và biên phải là $r = \min(n - i, m)$, trong đó $n$ là độ dài của chuỗi $\textit{target}$ và $m$ là độ dài lớn nhất của các chuỗi trong $\textit{words}$. Trong quá trình tìm kiếm nhị phân, ta cần kiểm tra xem $\textit{target}[i..i+\textit{mid}-1]$ có phải là một trong các giá trị hash trong $\textit{s}[\textit{mid}]$ hay không. Nếu có, cập nhật biên trái $l$ thành $\textit{mid}$; nếu không, cập nhật biên phải $r$ thành $\textit{mid} - 1$. Sau khi tìm kiếm nhị phân kết thúc, trả về $l$.

Sau khi tính được $\textit{f}(i)$, bài toán trở thành một bài toán tham lam kinh điển. Bắt đầu từ $i = 0$, với mỗi vị trí $i$, vị trí xa nhất ta có thể đi tới là $i + \textit{f}(i)$. Ta cần tìm số bước nhỏ nhất để đi tới cuối chuỗi.

Ta dùng $\textit{last}$ biểu diễn vị trí của lần di chuyển gần nhất và $\textit{mx}$ biểu diễn vị trí xa nhất có thể đi tới từ vị trí hiện tại. Ban đầu, $\textit{last} = \textit{mx} = 0$. Ta duyệt từ $i = 0$. Nếu $i$ bằng $\textit{last}$, nghĩa là ta cần di chuyển thêm một lần. Nếu $\textit{last} = \textit{mx}$, nghĩa là không thể đi xa hơn, nên trả về $-1$. Ngược lại, cập nhật $\textit{last}$ thành $\textit{mx}$ và tăng đáp án lên một.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n + L)$, còn độ phức tạp không gian là $O(n + L)$. Ở đây, $n$ là độ dài của chuỗi $\textit{target}$ và $L$ là tổng độ dài của tất cả các chuỗi hợp lệ.

<!-- tabs:start -->

#### Python3

```python
class Hashing:
    __slots__ = ["mod", "h", "p"]

    def __init__(self, s: List[str], base: int, mod: int):
        self.mod = mod
        self.h = [0] * (len(s) + 1)
        self.p = [1] * (len(s) + 1)
        for i in range(1, len(s) + 1):
            self.h[i] = (self.h[i - 1] * base + ord(s[i - 1])) % mod
            self.p[i] = (self.p[i - 1] * base) % mod

    def query(self, l: int, r: int) -> int:
        return (self.h[r] - self.h[l - 1] * self.p[r - l + 1]) % self.mod


class Solution:
    def minValidStrings(self, words: List[str], target: str) -> int:
        def f(i: int) -> int:
            l, r = 0, min(n - i, m)
            while l < r:
                mid = (l + r + 1) >> 1
                sub = hashing.query(i + 1, i + mid)
                if sub in s[mid]:
                    l = mid
                else:
                    r = mid - 1
            return l

        base, mod = 13331, 998244353
        hashing = Hashing(target, base, mod)
        m = max(len(w) for w in words)
        s = [set() for _ in range(m + 1)]
        for w in words:
            h = 0
            for j, c in enumerate(w, 1):
                h = (h * base + ord(c)) % mod
                s[j].add(h)
        ans = last = mx = 0
        n = len(target)
        for i in range(n):
            dist = f(i)
            mx = max(mx, i + dist)
            if i == last:
                if i == mx:
                    return -1
                last = mx
                ans += 1
        return ans
```

#### Java

```java
class Hashing {
    private final long[] p;
    private final long[] h;
    private final long mod;

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
    private Hashing hashing;
    private Set<Long>[] s;

    public int minValidStrings(String[] words, String target) {
        int base = 13331, mod = 998244353;
        hashing = new Hashing(target, base, mod);
        int m = Arrays.stream(words).mapToInt(String::length).max().orElse(0);
        s = new Set[m + 1];
        Arrays.setAll(s, k -> new HashSet<>());
        for (String w : words) {
            long h = 0;
            for (int j = 0; j < w.length(); j++) {
                h = (h * base + w.charAt(j)) % mod;
                s[j + 1].add(h);
            }
        }

        int ans = 0;
        int last = 0;
        int mx = 0;
        int n = target.length();
        for (int i = 0; i < n; i++) {
            int dist = f(i, n, m);
            mx = Math.max(mx, i + dist);
            if (i == last) {
                if (i == mx) {
                    return -1;
                }
                last = mx;
                ans++;
            }
        }
        return ans;
    }

    private int f(int i, int n, int m) {
        int l = 0, r = Math.min(n - i, m);
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            long sub = hashing.query(i + 1, i + mid);
            if (s[mid].contains(sub)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Hashing {
private:
    vector<long long> p;
    vector<long long> h;
    long long mod;

public:
    Hashing(const string& word, long long base, int mod) {
        int n = word.size();
        p.resize(n + 1);
        h.resize(n + 1);
        p[0] = 1;
        this->mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = (p[i - 1] * base) % mod;
            h[i] = (h[i - 1] * base + word[i - 1]) % mod;
        }
    }

    long long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
};

class Solution {
public:
    int minValidStrings(vector<string>& words, string target) {
        int base = 13331, mod = 998244353;
        Hashing hashing(target, base, mod);
        int m = 0, n = target.size();
        for (const string& word : words) {
            m = max(m, (int) word.size());
        }

        vector<unordered_set<long long>> s(m + 1);
        for (const string& w : words) {
            long long h = 0;
            for (int j = 0; j < w.size(); j++) {
                h = (h * base + w[j]) % mod;
                s[j + 1].insert(h);
            }
        }

        auto f = [&](int i) -> int {
            int l = 0, r = min(n - i, m);
            while (l < r) {
                int mid = (l + r + 1) >> 1;
                long long sub = hashing.query(i + 1, i + mid);
                if (s[mid].count(sub)) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            return l;
        };

        int ans = 0, last = 0, mx = 0;
        for (int i = 0; i < n; i++) {
            int dist = f(i);
            mx = max(mx, i + dist);
            if (i == last) {
                if (i == mx) {
                    return -1;
                }
                last = mx;
                ans++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
type Hashing struct {
	p   []int64
	h   []int64
	mod int64
}

func NewHashing(word string, base int64, mod int64) *Hashing {
	n := len(word)
	p := make([]int64, n+1)
	h := make([]int64, n+1)
	p[0] = 1
	for i := 1; i <= n; i++ {
		p[i] = (p[i-1] * base) % mod
		h[i] = (h[i-1]*base + int64(word[i-1])) % mod
	}
	return &Hashing{p, h, mod}
}

func (hashing *Hashing) Query(l, r int) int64 {
	return (hashing.h[r] - hashing.h[l-1]*hashing.p[r-l+1]%hashing.mod + hashing.mod) % hashing.mod
}

func minValidStrings(words []string, target string) (ans int) {
	base, mod := int64(13331), int64(998244353)
	hashing := NewHashing(target, base, mod)

	m, n := 0, len(target)
	for _, w := range words {
		m = max(m, len(w))
	}

	s := make([]map[int64]bool, m+1)

	f := func(i int) int {
		l, r := 0, int(math.Min(float64(n-i), float64(m)))
		for l < r {
			mid := (l + r + 1) >> 1
			sub := hashing.Query(i+1, i+mid)
			if s[mid][sub] {
				l = mid
			} else {
				r = mid - 1
			}
		}
		return l
	}

	for _, w := range words {
		h := int64(0)
		for j := 0; j < len(w); j++ {
			h = (h*base + int64(w[j])) % mod
			if s[j+1] == nil {
				s[j+1] = make(map[int64]bool)
			}
			s[j+1][h] = true
		}
	}

	var last, mx int

	for i := 0; i < n; i++ {
		dist := f(i)
		mx = max(mx, i+dist)
		if i == last {
			if i == mx {
				return -1
			}
			last = mx
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minValidStrings(words: string[], target: string): number {
    class Hashing {
        private p: bigint[];
        private h: bigint[];
        private mod: bigint;

        constructor(word: string, base: bigint, mod: bigint) {
            const n = word.length;
            this.p = new Array<bigint>(n + 1).fill(0n);
            this.h = new Array<bigint>(n + 1).fill(0n);
            this.mod = mod;
            this.p[0] = 1n;
            for (let i = 1; i <= n; ++i) {
                this.p[i] = (this.p[i - 1] * base) % mod;
                this.h[i] = (this.h[i - 1] * base + BigInt(word.charCodeAt(i - 1))) % mod;
            }
        }

        query(l: number, r: number): bigint {
            const res =
                (this.h[r] - ((this.h[l - 1] * this.p[r - l + 1]) % this.mod) + this.mod) %
                this.mod;
            return res;
        }
    }

    const base = 13331n;
    const mod = 998244353n;
    const hashing = new Hashing(target, base, mod);

    const m = Math.max(0, ...words.map(w => w.length));
    const s: Set<bigint>[] = Array.from({ length: m + 1 }, () => new Set<bigint>());

    for (const w of words) {
        let h = 0n;
        for (let j = 0; j < w.length; ++j) {
            h = (h * base + BigInt(w.charCodeAt(j))) % mod;
            s[j + 1].add(h);
        }
    }

    const n = target.length;
    let ans = 0;
    let last = 0;
    let mx = 0;

    const f = (i: number): number => {
        let l = 0;
        let r = Math.min(n - i, m);
        while (l < r) {
            const mid = (l + r + 1) >> 1;
            const sub = hashing.query(i + 1, i + mid);
            if (s[mid].has(sub)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    };

    for (let i = 0; i < n; ++i) {
        const dist = f(i);
        mx = Math.max(mx, i + dist);
        if (i === last) {
            if (i === mx) {
                return -1;
            }
            last = mx;
            ans++;
        }
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;
use std::cmp::max;

struct Hashing {
    p: Vec<i64>,
    h: Vec<i64>,
    base: i64,
    modv: i64,
}

impl Hashing {
    fn new(word: &str, base: i64, modv: i64) -> Self {
        let n = word.len();
        let mut p = vec![0; n + 1];
        let mut h = vec![0; n + 1];
        let bytes = word.as_bytes();
        p[0] = 1;
        for i in 1..=n {
            p[i] = p[i - 1] * base % modv;
            h[i] = (h[i - 1] * base + bytes[i - 1] as i64) % modv;
        }
        Self { p, h, base, modv }
    }

    fn query(&self, l: usize, r: usize) -> i64 {
        let mut res = self.h[r] - self.h[l - 1] * self.p[r - l + 1] % self.modv;
        if res < 0 {
            res += self.modv;
        }
        res % self.modv
    }
}

impl Solution {
    pub fn min_valid_strings(words: Vec<String>, target: String) -> i32 {
        let base = 13331;
        let modv = 998_244_353;
        let hashing = Hashing::new(&target, base, modv);
        let m = words.iter().map(|w| w.len()).max().unwrap_or(0);
        let mut s: Vec<HashSet<i64>> = vec![HashSet::new(); m + 1];

        for w in &words {
            let mut h = 0i64;
            for (j, &b) in w.as_bytes().iter().enumerate() {
                h = (h * base + b as i64) % modv;
                s[j + 1].insert(h);
            }
        }

        let n = target.len();
        let bytes = target.as_bytes();
        let mut ans = 0;
        let mut last = 0;
        let mut mx = 0;

        let f = |i: usize, n: usize, m: usize, s: &Vec<HashSet<i64>>, hashing: &Hashing| -> usize {
            let mut l = 0;
            let mut r = std::cmp::min(n - i, m);
            while l < r {
                let mid = (l + r + 1) >> 1;
                let sub = hashing.query(i + 1, i + mid);
                if s[mid].contains(&sub) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            l
        };

        for i in 0..n {
            let dist = f(i, n, m, &s, &hashing);
            mx = max(mx, i + dist);
            if i == last {
                if i == mx {
                    return -1;
                }
                last = mx;
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
