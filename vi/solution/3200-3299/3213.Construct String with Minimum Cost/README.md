---
comments: true
difficulty: Hard
rating: 2170
source: Weekly Contest 405 Q4
tags:
    - Array
    - String
    - Dynamic Programming
    - Suffix Array
---

<!-- problem:start -->

# [3213. Construct String with Minimum Cost](https://leetcode.com/problems/construct-string-with-minimum-cost)

[中文文档](/solution/3200-3299/3213.Construct%20String%20with%20Minimum%20Cost/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>target</code>, một mảng các chuỗi <code>words</code> và một mảng số nguyên <code>costs</code>, hai mảng có cùng độ dài.</p>

<p>Hãy hình dung một chuỗi rỗng <code>s</code>.</p>

<p>Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý (kể cả <strong>không lần nào</strong>):</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong khoảng <code>[0, words.length - 1]</code>.</li>
	<li>Nối <code>words[i]</code> vào <code>s</code>.</li>
	<li>Chi phí của thao tác là <code>costs[i]</code>.</li>
</ul>

<p>Trả về chi phí <strong>nhỏ nhất</strong> để biến <code>s</code> thành <code>target</code>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = &quot;abcdef&quot;, words = [&quot;abdef&quot;,&quot;abc&quot;,&quot;d&quot;,&quot;def&quot;,&quot;ef&quot;], costs = [100,1,1,10,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể đạt được chi phí nhỏ nhất bằng cách thực hiện các thao tác sau:</p>

<ul>
	<li>Chọn chỉ số 1 và nối <code>&quot;abc&quot;</code> vào <code>s</code> với chi phí 1, khi đó <code>s = &quot;abc&quot;</code>.</li>
	<li>Chọn chỉ số 2 và nối <code>&quot;d&quot;</code> vào <code>s</code> với chi phí 1, khi đó <code>s = &quot;abcd&quot;</code>.</li>
	<li>Chọn chỉ số 4 và nối <code>&quot;ef&quot;</code> vào <code>s</code> với chi phí 5, khi đó <code>s = &quot;abcdef&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = &quot;aaaa&quot;, words = [&quot;z&quot;,&quot;zz&quot;,&quot;zzz&quot;], costs = [1,10,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể biến <code>s</code> thành <code>target</code>, nên ta trả về -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words.length == costs.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= target.length</code></li>
	<li>Tổng <code>words[i].length</code> không vượt quá <code>5 * 10<sup>4</sup></code>.</li>
	<li><code>target</code> và <code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= costs[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash chuỗi + Quy hoạch động + Liệt kê độ dài

<!-- thinking:start -->

> **Tư duy**
>
> Ta nối các phần tử trong $\textit{words}$ để tạo thành $\textit{target}$ với chi phí nhỏ nhất; cả $n$ và tổng độ dài các từ đều bằng $5\times 10^4$. Một quy hoạch động thử mọi từ tại mọi vị trí sẽ quá tốn kém.
>
> Chỉ có $O(\sqrt{L})$ độ dài từ khác nhau. Ta băm mỗi từ theo chi phí nhỏ nhất của nó, đặt $f[i]$ là cách rẻ nhất để tạo ra $i$ ký tự đầu tiên, rồi chỉ thử các độ dài $j$ xuất hiện và kiểm tra $\textit{target}[i-j+1..i]$ trong $O(1)$ bằng hash tiền tố. Khi đó, các phép chuyển có độ phức tạp $O(n\sqrt{L})$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là chi phí nhỏ nhất để tạo ra $i$ ký tự đầu tiên của $\textit{target}$, với điều kiện ban đầu $f[0] = 0$ và tất cả các giá trị còn lại được đặt là vô hạn. Đáp án là $f[n]$, trong đó $n$ là độ dài của $\textit{target}$.

Với $f[i]$ hiện tại, ta xét lần lượt độ dài $j$ của từ. Nếu $j \leq i$, ta có thể tính giá trị hash của đoạn từ $i - j + 1$ đến $i$. Nếu giá trị hash này tương ứng với một từ tồn tại, ta có thể chuyển từ $f[i - j]$ sang $f[i]$. Công thức chuyển trạng thái như sau:

$$
f[i] = \min(f[i], f[i - j] + \textit{cost}[k])
$$

trong đó $\textit{cost}[k]$ là chi phí nhỏ nhất của một từ có độ dài $j$ và giá trị hash trùng với $\textit{target}[i - j + 1, i]$.

Độ phức tạp thời gian là $O(n \times \sqrt{L})$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của $\textit{target}$, còn $L$ là tổng độ dài của tất cả các từ trong mảng $\textit{words}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, target: str, words: List[str], costs: List[int]) -> int:
        base, mod = 13331, 998244353
        n = len(target)
        h = [0] * (n + 1)
        p = [1] * (n + 1)
        for i, c in enumerate(target, 1):
            h[i] = (h[i - 1] * base + ord(c)) % mod
            p[i] = (p[i - 1] * base) % mod
        f = [0] + [inf] * n
        ss = sorted(set(map(len, words)))
        d = defaultdict(lambda: inf)
        min = lambda a, b: a if a < b else b
        for w, c in zip(words, costs):
            x = 0
            for ch in w:
                x = (x * base + ord(ch)) % mod
            d[x] = min(d[x], c)
        for i in range(1, n + 1):
            for j in ss:
                if j > i:
                    break
                x = (h[i] - h[i - j] * p[j]) % mod
                f[i] = min(f[i], f[i - j] + d[x])
        return f[n] if f[n] < inf else -1
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
    public int minimumCost(String target, String[] words, int[] costs) {
        final int base = 13331;
        final int mod = 998244353;
        final int inf = Integer.MAX_VALUE / 2;

        int n = target.length();
        Hashing hashing = new Hashing(target, base, mod);

        int[] f = new int[n + 1];
        Arrays.fill(f, inf);
        f[0] = 0;

        TreeSet<Integer> ss = new TreeSet<>();
        for (String w : words) {
            ss.add(w.length());
        }

        Map<Long, Integer> d = new HashMap<>();
        for (int i = 0; i < words.length; i++) {
            long x = 0;
            for (char c : words[i].toCharArray()) {
                x = (x * base + c) % mod;
            }
            d.merge(x, costs[i], Integer::min);
        }

        for (int i = 1; i <= n; i++) {
            for (int j : ss) {
                if (j > i) {
                    break;
                }
                long x = hashing.query(i - j + 1, i);
                f[i] = Math.min(f[i], f[i - j] + d.getOrDefault(x, inf));
            }
        }

        return f[n] >= inf ? -1 : f[n];
    }
}
```

#### C++

```cpp
class Hashing {
private:
    vector<long> p, h;
    long mod;

public:
    Hashing(const string& word, long base, long mod)
        : p(word.size() + 1, 1)
        , h(word.size() + 1, 0)
        , mod(mod) {
        for (int i = 1; i <= word.size(); ++i) {
            p[i] = p[i - 1] * base % mod;
            h[i] = (h[i - 1] * base + word[i - 1]) % mod;
        }
    }

    long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
};

class Solution {
public:
    int minimumCost(string target, vector<string>& words, vector<int>& costs) {
        const int base = 13331;
        const int mod = 998244353;
        const int inf = INT_MAX / 2;

        int n = target.size();
        Hashing hashing(target, base, mod);

        vector<int> f(n + 1, inf);
        f[0] = 0;

        set<int> ss;
        for (const string& w : words) {
            ss.insert(w.size());
        }

        unordered_map<long, int> d;
        for (int i = 0; i < words.size(); ++i) {
            long x = 0;
            for (char c : words[i]) {
                x = (x * base + c) % mod;
            }
            d[x] = d.find(x) == d.end() ? costs[i] : min(d[x], costs[i]);
        }

        for (int i = 1; i <= n; ++i) {
            for (int j : ss) {
                if (j > i) {
                    break;
                }
                long x = hashing.query(i - j + 1, i);
                if (d.contains(x)) {
                    f[i] = min(f[i], f[i - j] + d[x]);
                }
            }
        }

        return f[n] >= inf ? -1 : f[n];
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

func NewHashing(word string, base, mod int64) *Hashing {
    n := len(word)
    p := make([]int64, n+1)
    h := make([]int64, n+1)
    p[0] = 1
    for i := 1; i <= n; i++ {
        p[i] = p[i-1] * base % mod
        h[i] = (h[i-1]*base + int64(word[i-1])) % mod
    }
    return &Hashing{p, h, mod}
}

func (hs *Hashing) query(l, r int) int64 {
    return (hs.h[r] - hs.h[l-1]*hs.p[r-l+1]%hs.mod + hs.mod) % hs.mod
}

func minimumCost(target string, words []string, costs []int) int {
    const base = 13331
    const mod = 998244353
    const inf = math.MaxInt32 / 2

    n := len(target)
    hashing := NewHashing(target, base, mod)

    f := make([]int, n+1)
    for i := range f {
        f[i] = inf
    }
    f[0] = 0

    ss := make(map[int]struct{})
    for _, w := range words {
        ss[len(w)] = struct{}{}
    }
    lengths := make([]int, 0, len(ss))
    for length := range ss {
        lengths = append(lengths, length)
    }
    sort.Ints(lengths)

    d := make(map[int64]int)
    for i, w := range words {
        var x int64
        for _, c := range w {
            x = (x*base + int64(c)) % mod
        }
        if existingCost, exists := d[x]; exists {
            if costs[i] < existingCost {
                d[x] = costs[i]
            }
        } else {
            d[x] = costs[i]
        }
    }

    for i := 1; i <= n; i++ {
        for _, j := range lengths {
            if j > i {
                break
            }
            x := hashing.query(i-j+1, i)
            if cost, ok := d[x]; ok {
                f[i] = min(f[i], f[i-j]+cost)
            }
        }
    }

    if f[n] >= inf {
        return -1
    }
    return f[n]
}
```

#### TypeScript

```ts
class Hashing {
    private p: bigint[];
    private h: bigint[];
    private mod: bigint;

    constructor(word: string, base: number, mod: number) {
        const n = word.length;
        this.mod = BigInt(mod);
        const b = BigInt(base);
        this.p = new Array(n + 1);
        this.h = new Array(n + 1);
        this.p[0] = 1n;
        this.h[0] = 0n;
        for (let i = 1; i <= n; i++) {
            this.p[i] = (this.p[i - 1] * b) % this.mod;
            this.h[i] = (this.h[i - 1] * b + BigInt(word.charCodeAt(i - 1))) % this.mod;
        }
    }

    public query(l: number, r: number): number {
        const res =
            (this.h[r] - ((this.h[l - 1] * this.p[r - l + 1]) % this.mod) + this.mod) % this.mod;
        return Number(res);
    }
}

function minimumCost(target: string, words: string[], costs: number[]): number {
    const base = 13331;
    const mod = 998244353;
    const inf = 1e9;
    const n = target.length;
    const hashing = new Hashing(target, base, mod);
    const f = new Array(n + 1).fill(inf);
    f[0] = 0;

    const ss = Array.from(new Set(words.map(w => w.length))).sort((a, b) => a - b);
    const d = new Map<number, number>();

    for (let i = 0; i < words.length; i++) {
        let x = 0n;
        const b = BigInt(base);
        const m = BigInt(mod);
        const word = words[i];
        for (let j = 0; j < word.length; j++) {
            x = (x * b + BigInt(word.charCodeAt(j))) % m;
        }
        const hashVal = Number(x);
        d.set(hashVal, Math.min(d.get(hashVal) ?? inf, costs[i]));
    }

    for (let i = 1; i <= n; i++) {
        for (const j of ss) {
            if (j > i) break;
            const x = hashing.query(i - j + 1, i);
            if (d.has(x)) {
                f[i] = Math.min(f[i], f[i - j] + d.get(x)!);
            }
        }
    }

    return f[n] >= inf ? -1 : f[n];
}
```

#### Rust

```rust
use std::collections::{HashMap, BTreeSet};
use std::cmp::min;

struct Hashing {
    p: Vec<i64>,
    h: Vec<i64>,
    mod_val: i64,
}

impl Hashing {
    fn new(word: &str, base: i64, mod_val: i64) -> Self {
        let n = word.len();
        let mut p = vec![0; n + 1];
        let mut h = vec![0; n + 1];
        p[0] = 1;
        let chars: Vec<u8> = word.bytes().collect();
        for i in 1..=n {
            p[i] = p[i - 1] * base % mod_val;
            h[i] = (h[i - 1] * base + chars[i - 1] as i64) % mod_val;
        }
        Hashing { p, h, mod_val }
    }

    fn query(&self, l: usize, r: usize) -> i64 {
        (self.h[r] - self.h[l - 1] * self.p[r - l + 1] % self.mod_val + self.mod_val) % self.mod_val
    }
}

impl Solution {
    pub fn minimum_cost(target: String, words: Vec<String>, costs: Vec<i32>) -> i32 {
        let base = 13331i64;
        let mod_val = 998244353i64;
        let inf = i32::MAX / 2;
        let n = target.len();
        let hashing = Hashing::new(&target, base, mod_val);

        let mut f = vec![inf; n + 1];
        f[0] = 0;

        let mut ss = BTreeSet::new();
        for w in &words {
            ss.insert(w.len());
        }

        let mut d = HashMap::new();
        for i in 0..words.len() {
            let mut x = 0i64;
            for c in words[i].bytes() {
                x = (x * base + c as i64) % mod_val;
            }
            let entry = d.entry(x).or_insert(inf);
            *entry = min(*entry, costs[i]);
        }

        for i in 1..=n {
            for &j in &ss {
                if j > i {
                    break;
                }
                let x = hashing.query(i - j + 1, i);
                if let Some(&cost) = d.get(&x) {
                    f[i] = min(f[i], f[i - j] + cost);
                }
            }
        }

        if f[n] >= inf { -1 } else { f[n] }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
