---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Array
    - Math
---

<!-- problem:start -->

# [3535. Unit Conversion II 🔒](https://leetcode.com/problems/unit-conversion-ii)

[中文文档](/solution/3500-3599/3535.Unit%20Conversion%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> loại đơn vị được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn được cho một mảng số nguyên 2 chiều <code>conversions</code> có độ dài <code>n - 1</code>, trong đó <code>conversions[i] = [sourceUnit<sub>i</sub>, targetUnit<sub>i</sub>, conversionFactor<sub>i</sub>]</code>. Điều này cho biết một đơn vị loại <code>sourceUnit<sub>i</sub></code> tương đương với <code>conversionFactor<sub>i</sub></code> đơn vị loại <code>targetUnit<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code> có độ dài <code>q</code>, trong đó <code>queries[i] = [unitA<sub>i</sub>, unitB<sub>i</sub>]</code>.</p>

<p>Hãy trả về một mảng <code face="monospace">answer</code> có độ dài <code>q</code>, trong đó <code>answer[i]</code> là số đơn vị loại <code>unitB<sub>i</sub></code> tương đương với 1 đơn vị loại <code>unitA<sub>i</sub></code>, và có thể biểu diễn dưới dạng <code>p/q</code> với <code>p</code> và <code>q</code> nguyên tố cùng nhau. Trả về mỗi <code>answer[i]</code> dưới dạng <code>pq<sup>-1</sup></code> <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>, trong đó <code>q<sup>-1</sup></code> là nghịch đảo nhân của <code>q</code> theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">conversions = [[0,1,2],[0,2,6]], queries = [[1,2],[1,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,500000004]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Trong truy vấn đầu tiên, ta có thể chuyển đổi một đơn vị loại 1 thành 3 đơn vị loại 2 bằng nghịch đảo của <code>conversions[0]</code>, sau đó dùng <code>conversions[1]</code>.</li>
    <li>Trong truy vấn thứ hai, ta có thể chuyển đổi một đơn vị loại 1 thành 1/2 đơn vị loại 0 bằng nghịch đảo của <code>conversions[0]</code>. Ta trả về 500000004 vì đây là nghịch đảo nhân của 2.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3535.Unit%20Conversion%20II/images/example1.png" style="width: 500px; height: 500px;" /></div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">conversions = [[0,1,2],[0,2,6],[0,3,8],[2,4,2],[2,5,4],[3,6,3]], queries = [[1,2],[0,4],[6,5],[4,6],[6,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,12,1,2,83333334]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Trong truy vấn đầu tiên, ta có thể chuyển đổi một đơn vị loại 1 thành 3 đơn vị loại 2 bằng nghịch đảo của <code>conversions[0]</code>, sau đó dùng <code>conversions[1]</code>.</li>
    <li>Trong truy vấn thứ hai, ta có thể chuyển đổi một đơn vị loại 0 thành 12 đơn vị loại 4 bằng <code>conversions[1]</code>, sau đó dùng <code>conversions[3]</code>.</li>
    <li>Trong truy vấn thứ ba, ta có thể chuyển đổi một đơn vị loại 6 thành 1 đơn vị loại 5 bằng nghịch đảo của <code>conversions[5]</code>, nghịch đảo của <code>conversions[2]</code>, <code>conversions[1]</code>, sau đó dùng <code>conversions[4]</code>.</li>
    <li>Trong truy vấn thứ tư, ta có thể chuyển đổi một đơn vị loại 4 thành 2 đơn vị loại 6 bằng nghịch đảo của <code>conversions[3]</code>, nghịch đảo của <code>conversions[1]</code>, <code>conversions[2]</code>, sau đó dùng <code>conversions[5]</code>.</li>
    <li>Trong truy vấn thứ năm, ta có thể chuyển đổi một đơn vị loại 6 thành 1/12 đơn vị loại 1 bằng nghịch đảo của <code>conversions[5]</code>, nghịch đảo của <code>conversions[2]</code>, sau đó dùng <code>conversions[0]</code>. Ta trả về 83333334 vì đây là nghịch đảo nhân của 12.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3535.Unit%20Conversion%20II/images/example2.png" style="width: 504px; height: 493px;" /></div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>conversions.length == n - 1</code></li>
    <li><code>0 &lt;= sourceUnit<sub>i</sub>, targetUnit<sub>i</sub> &lt; n</code></li>
    <li><code>1 &lt;= conversionFactor<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= q &lt;= 10<sup>5</sup></code></li>
    <li><code>queries.length == q</code></li>
    <li><code>0 &lt;= unitA<sub>i</sub>, unitB<sub>i</sub> &lt; n</code></li>
    <li>Đảm bảo rằng đơn vị 0 có thể được chuyển đổi <strong>duy nhất</strong> thành mọi đơn vị khác thông qua một tổ hợp các phép chuyển đổi xuôi hoặc ngược.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Nghịch đảo modulo

<!-- thinking:start -->

> **Tư duy**
>
> Cây chuyển đổi vẫn giống như trước, nên $\textit{res}[i]$ — số đơn vị loại $i$ tương đương với một đơn vị loại $0$ — đã được biết. Các truy vấn yêu cầu tỉ lệ giữa hai loại đơn vị.
>
> Hệ số từ $\textit{unitA}$ đến $\textit{unitB}$ là $\textit{res}[B] \cdot \textit{res}[A]^{-1}$. Modulo là số nguyên tố, nên nghịch đảo là $a^{MOD-2}$.

<!-- thinking:end -->

Các quan hệ chuyển đổi tạo thành một cây có hướng với gốc là $0$. Bắt đầu DFS từ node $0$, ta duy trì `res[i]` là số đơn vị loại $i$ tương đương với $1$ đơn vị loại $0$.

Với truy vấn $(unitA, unitB)$, đáp án là $\frac{res[unitB]}{res[unitA]}$, theo modulo $10^9 + 7$ tương đương với `res[unitB] * res[unitA]^(MOD - 2) % MOD`, trong đó `MOD - 2` được dùng để tính nghịch đảo modulo bằng định lý nhỏ Fermat.

Độ phức tạp thời gian là $O(n + q \log MOD)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số loại đơn vị và $q$ là số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def queryConversions(
        self, conversions: List[List[int]], queries: List[List[int]]
    ) -> List[int]:
        def dfs(s: int, mul: int) -> None:
            res[s] = mul
            for t, w in g[s]:
                dfs(t, mul * w % mod)

        mod = 10**9 + 7
        n = len(conversions) + 1
        g = [[] for _ in range(n)]
        for s, t, w in conversions:
            g[s].append((t, w))
        res = [0] * n
        dfs(0, 1)
        ans = []
        for x, y in queries:
            ans.append(res[y] * pow(res[x], mod - 2, mod) % mod)
        return ans
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;
    private List<int[]>[] g;
    private int[] res;

    public int[] queryConversions(int[][] conversions, int[][] queries) {
        int n = conversions.length + 1;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : conversions) {
            g[e[0]].add(new int[] {e[1], e[2]});
        }

        res = new int[n];
        dfs(0, 1);

        int[] ans = new int[queries.length];
        for (int i = 0; i < queries.length; i++) {
            int x = queries[i][0], y = queries[i][1];
            ans[i] = (int) ((long) res[y] * qpow(res[x], mod - 2) % mod);
        }
        return ans;
    }

    private void dfs(int s, long mul) {
        res[s] = (int) mul;
        for (var e : g[s]) {
            dfs(e[0], mul * e[1] % mod);
        }
    }

    private long qpow(long x, int n) {
        long res = 1;
        while (n > 0) {
            if ((n & 1) == 1) {
                res = res * x % mod;
            }
            x = x * x % mod;
            n >>= 1;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> queryConversions(vector<vector<int>>& conversions, vector<vector<int>>& queries) {
        const int mod = 1e9 + 7;
        int n = conversions.size() + 1;
        vector<vector<pair<int, int>>> g(n);
        for (auto& e : conversions) {
            g[e[0]].emplace_back(e[1], e[2]);
        }

        vector<int> res(n);

        auto dfs = [&](this auto&& dfs, int s, long long mul) -> void {
            res[s] = mul;
            for (auto [t, w] : g[s]) {
                dfs(t, mul * w % mod);
            }
        };
        dfs(0, 1);

        auto qpow = [&](long long x, int n) {
            long long res = 1;
            while (n) {
                if (n & 1) {
                    res = res * x % mod;
                }
                x = x * x % mod;
                n >>= 1;
            }
            return res;
        };

        vector<int> ans;
        for (auto& q : queries) {
            ans.push_back(res[q[1]] * qpow(res[q[0]], mod - 2) % mod);
        }
        return ans;
    }
};
```

#### Go

```go
func queryConversions(conversions [][]int, queries [][]int) []int {
    const mod = int(1e9 + 7)
    n := len(conversions) + 1

    g := make([][]struct{ t, w int }, n)
    for _, e := range conversions {
        s, t, w := e[0], e[1], e[2]
        g[s] = append(g[s], struct{ t, w int }{t, w})
    }

    res := make([]int, n)

    var dfs func(int, int)
    dfs = func(s, mul int) {
        res[s] = mul
        for _, e := range g[s] {
            dfs(e.t, mul*e.w%mod)
        }
    }
    dfs(0, 1)

    qpow := func(x, n int) int {
        res := 1
        for n > 0 {
            if n&1 > 0 {
                res = res * x % mod
            }
            x = x * x % mod
            n >>= 1
        }
        return res
    }

    ans := make([]int, len(queries))
    for i, q := range queries {
        ans[i] = res[q[1]] * qpow(res[q[0]], mod-2) % mod
    }
    return ans
}
```

#### TypeScript

```ts
function queryConversions(conversions: number[][], queries: number[][]): number[] {
    const mod = BigInt(1e9 + 7);
    const n = conversions.length + 1;

    const g: { t: number; w: number }[][] = Array.from({ length: n }, () => []);
    for (const [s, t, w] of conversions) {
        g[s].push({ t, w });
    }

    const res: number[] = Array(n).fill(0);

    const dfs = (s: number, mul: number): void => {
        res[s] = mul;
        for (const { t, w } of g[s]) {
            dfs(t, Number((BigInt(mul) * BigInt(w)) % mod));
        }
    };
    dfs(0, 1);

    const qpow = (x: number, n: number): number => {
        let res = 1n;
        let a = BigInt(x);
        while (n > 0) {
            if (n & 1) {
                res = (res * a) % mod;
            }
            a = (a * a) % mod;
            n >>= 1;
        }
        return Number(res);
    };

    const ans: number[] = [];
    for (const [x, y] of queries) {
        ans.push(Number((BigInt(res[y]) * BigInt(qpow(res[x], 1e9 + 5))) % mod));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn query_conversions(conversions: Vec<Vec<i32>>, queries: Vec<Vec<i32>>) -> Vec<i32> {
        const MOD: i64 = 1_000_000_007;
        let n = conversions.len() + 1;

        let mut g = vec![Vec::<(usize, i64)>::new(); n];
        for e in conversions {
            g[e[0] as usize].push((e[1] as usize, e[2] as i64));
        }

        let mut res = vec![0_i64; n];

        fn dfs(s: usize, mul: i64, g: &Vec<Vec<(usize, i64)>>, res: &mut Vec<i64>) {
            res[s] = mul;
            for &(t, w) in &g[s] {
                dfs(t, mul * w % MOD, g, res);
            }
        }

        dfs(0, 1, &g, &mut res);

        fn qpow(mut x: i64, mut n: i32) -> i64 {
            let mut res = 1_i64;
            while n > 0 {
                if n & 1 == 1 {
                    res = res * x % MOD;
                }
                x = x * x % MOD;
                n >>= 1;
            }
            res
        }

        let mut ans = Vec::with_capacity(queries.len());
        for q in queries {
            let x = q[0] as usize;
            let y = q[1] as usize;
            ans.push((res[y] * qpow(res[x], 1_000_000_005) % MOD) as i32);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
