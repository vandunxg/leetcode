---
comments: true
difficulty: Medium
rating: 1968
source: Weekly Contest 477 Q3
tags:
    - Math
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [3756. Concatenate Non-Zero Digits and Multiply by Sum II](https://leetcode.com/problems/concatenate-non-zero-digits-and-multiply-by-sum-ii)

[中文文档](/solution/3700-3799/3756.Concatenate%20Non-Zero%20Digits%20and%20Multiply%20by%20Sum%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có độ dài <code>m</code>, chỉ gồm các chữ số. Bạn cũng được cho một mảng số nguyên 2D <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.</p>

<p>Với mỗi <code>queries[i]</code>, hãy trích xuất <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> <code>s[l<sub>i</sub>..r<sub>i</sub>]</code>. Sau đó, thực hiện các bước sau:</p>

<ul>
    <li>Tạo một số nguyên mới <code>x</code> bằng cách nối tất cả <strong>các chữ số khác 0</strong> trong chuỗi con theo đúng thứ tự ban đầu. Nếu không có chữ số khác 0 nào, <code>x = 0</code>.</li>
    <li>Gọi <code>sum</code> là <strong>tổng các chữ số</strong> trong <code>x</code>. Kết quả là <code>x * sum</code>.</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là kết quả của truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p>Vì các kết quả có thể rất lớn, hãy trả về chúng <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;10203004&quot;, queries = [[0,7],[1,3],[4,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[12340, 4, 9]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>s[0..7] = &quot;10203004&quot;</code>

    <ul>
        <li><code>x = 1234</code></li>
        <li><code>sum = 1 + 2 + 3 + 4 = 10</code></li>
        <li>Do đó, kết quả là <code>1234 * 10 = 12340</code>.</li>
    </ul>
    </li>
    <li><code>s[1..3] = &quot;020&quot;</code>
    <ul>
        <li><code>x = 2</code></li>
        <li><code>sum = 2</code></li>
        <li>Do đó, kết quả là <code>2 * 2 = 4</code>.</li>
    </ul>
    </li>
    <li><code>s[4..6] = &quot;300&quot;</code>
    <ul>
        <li><code>x = 3</code></li>
        <li><code>sum = 3</code></li>
        <li>Do đó, kết quả là <code>3 * 3 = 9</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1000&quot;, queries = [[0,3],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1, 0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>s[0..3] = &quot;1000&quot;</code>

    <ul>
        <li><code>x = 1</code></li>
        <li><code>sum = 1</code></li>
        <li>Do đó, kết quả là <code>1 * 1 = 1</code>.</li>
    </ul>
    </li>
    <li><code>s[1..1] = &quot;0&quot;</code>
    <ul>
        <li><code>x = 0</code></li>
        <li><code>sum = 0</code></li>
        <li>Do đó, kết quả là <code>0 * 0 = 0</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;9876543210&quot;, queries = [[0,9]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[444444137]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>s[0..9] = &quot;9876543210&quot;</code>

    <ul>
        <li><code>x = 987654321</code></li>
        <li><code>sum = 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 = 45</code></li>
        <li>Do đó, kết quả là <code>987654321 * 45 = 44444444445</code>.</li>
        <li>Ta trả về <code>44444444445 modulo (10<sup>9</sup> + 7) = 444444137</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ số.</li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code></li>
    <li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; m</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa $10^5$ truy vấn, nên ta không thể xây dựng lại từng chuỗi con. Ta tiền xử lý tổng chữ số, số lượng chữ số khác 0 và số nguyên được tạo bằng cách nối các chữ số khác 0; $x$ trên đoạn $[l,r]$ được suy ra từ hai giá trị prefix và một lũy thừa của mười, sau đó nhân với tổng chữ số trên đoạn.

<!-- thinking:end -->

Ta tiền xử lý ba mảng prefix:

- `sumD[i]` là tổng các chữ số trong $i$ ký tự đầu tiên của chuỗi;
- `cntN0[i]` là số lượng chữ số khác 0 trong $i$ ký tự đầu tiên;
- `p[i]` là số được tạo bằng cách nối tất cả chữ số khác 0 trong $i$ ký tự đầu tiên, lấy modulo $10^9 + 7$.

Với truy vấn $[l, r]$, số lượng chữ số khác 0 trong chuỗi con là $n_0 = cntN0[r + 1] - cntN0[l]$, và tổng các chữ số là $sd = sumD[r + 1] - sumD[l]$. Vì $p[r + 1] = p[l] \cdot 10^{n_0} + x$, ta có $x = p[r + 1] - p[l] \cdot 10^{n_0}$, và kết quả là $x \cdot sd$.

Ta tiền xử lý các lũy thừa của $10$ và trả lời mỗi truy vấn trong $O(1)$.

Độ phức tạp thời gian là $O(n + q)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi và $q$ là số lượng truy vấn.

<!-- tabs:start -->

#### Python3

```python
mx = 10**5 + 1
mod = 10**9 + 7
pow10 = [1] * mx
for i in range(1, mx):
    pow10[i] = pow10[i - 1] * 10 % mod


class Solution:
    def sumAndMultiply(self, s: str, queries: List[List[int]]) -> List[int]:
        n = len(s)
        sum_d = [0] * (n + 1)
        cnt_n0 = [0] * (n + 1)
        p = [0] * (n + 1)
        for i, d in enumerate(map(int, s), 1):
            sum_d[i] = sum_d[i - 1] + d
            cnt_n0[i] = cnt_n0[i - 1] + int(d > 0)
            p[i] = (p[i - 1] * 10 + d) % mod if d else p[i - 1]

        ans = []
        for l, r in queries:
            n0 = cnt_n0[r + 1] - cnt_n0[l]
            sd = sum_d[r + 1] - sum_d[l]
            x = p[r + 1] - p[l] * pow10[n0] % mod
            ans.append(x * sd % mod)
        return ans
```

#### Java

```java
class Solution {
    private static final int MX = 100001;
    private static final int MOD = 1_000_000_007;
    private static final long[] POW10 = new long[MX];

    static {
        POW10[0] = 1;
        for (int i = 1; i < MX; i++) {
            POW10[i] = POW10[i - 1] * 10 % MOD;
        }
    }

    public int[] sumAndMultiply(String s, int[][] queries) {
        int n = s.length();
        int[] sumD = new int[n + 1];
        int[] cntN0 = new int[n + 1];
        long[] p = new long[n + 1];

        for (int i = 1; i <= n; i++) {
            int d = s.charAt(i - 1) - '0';
            sumD[i] = sumD[i - 1] + d;
            cntN0[i] = cntN0[i - 1] + (d > 0 ? 1 : 0);
            p[i] = d > 0 ? (p[i - 1] * 10 + d) % MOD : p[i - 1];
        }

        int[] ans = new int[queries.length];
        for (int i = 0; i < queries.length; i++) {
            int l = queries[i][0], r = queries[i][1];
            int n0 = cntN0[r + 1] - cntN0[l];
            int sd = sumD[r + 1] - sumD[l];
            long x = (p[r + 1] - p[l] * POW10[n0] % MOD + MOD) % MOD;
            ans[i] = (int) (x * sd % MOD);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sumAndMultiply(string s, vector<vector<int>>& queries) {
        static const int MX = 100001;
        static const int MOD = 1000000007;
        static vector<long long> pow10 = [] {
            vector<long long> p(MX);
            p[0] = 1;
            for (int i = 1; i < MX; i++) {
                p[i] = p[i - 1] * 10 % MOD;
            }
            return p;
        }();

        int n = s.size();
        vector<int> sumD(n + 1), cntN0(n + 1);
        vector<long long> p(n + 1);

        for (int i = 1; i <= n; i++) {
            int d = s[i - 1] - '0';
            sumD[i] = sumD[i - 1] + d;
            cntN0[i] = cntN0[i - 1] + (d > 0);
            p[i] = d ? (p[i - 1] * 10 + d) % MOD : p[i - 1];
        }

        vector<int> ans;
        ans.reserve(queries.size());
        for (auto& q : queries) {
            int l = q[0], r = q[1];
            int n0 = cntN0[r + 1] - cntN0[l];
            int sd = sumD[r + 1] - sumD[l];
            long long x = (p[r + 1] - p[l] * pow10[n0] % MOD + MOD) % MOD;
            ans.push_back(x * sd % MOD);
        }
        return ans;
    }
};
```

#### Go

```go
const (
    mx        = 100001
    mod int64 = 1000000007
)

var pow10 = func() []int64 {
    p := make([]int64, mx)
    p[0] = 1
    for i := 1; i < mx; i++ {
        p[i] = p[i-1] * 10 % mod
    }
    return p
}()

func sumAndMultiply(s string, queries [][]int) []int {
    n := len(s)
    sumD := make([]int, n+1)
    cntN0 := make([]int, n+1)
    p := make([]int64, n+1)

    for i := 1; i <= n; i++ {
        d := int64(s[i-1] - '0')
        sumD[i] = sumD[i-1] + int(d)
        cntN0[i] = cntN0[i-1]
        if d > 0 {
            cntN0[i]++
            p[i] = (p[i-1]*10 + d) % mod
        } else {
            p[i] = p[i-1]
        }
    }

    ans := make([]int, len(queries))
    for i, q := range queries {
        l, r := q[0], q[1]
        n0 := cntN0[r+1] - cntN0[l]
        sd := int64(sumD[r+1] - sumD[l])
        x := (p[r+1] - p[l]*pow10[n0]%mod + mod) % mod
        ans[i] = int(x * sd % mod)
    }
    return ans
}
```

#### TypeScript

```ts
const MX = 100001;
const MOD = 1000000007n;

const pow10: bigint[] = Array(MX).fill(1n);
for (let i = 1; i < MX; i++) {
    pow10[i] = (pow10[i - 1] * 10n) % MOD;
}

function sumAndMultiply(s: string, queries: number[][]): number[] {
    const n = s.length;
    const sumD = Array<number>(n + 1).fill(0);
    const cntN0 = Array<number>(n + 1).fill(0);
    const p: bigint[] = Array(n + 1).fill(0n);

    for (let i = 1; i <= n; i++) {
        const d = s.charCodeAt(i - 1) - 48;
        sumD[i] = sumD[i - 1] + d;
        cntN0[i] = cntN0[i - 1] + (d > 0 ? 1 : 0);
        p[i] = d > 0 ? (p[i - 1] * 10n + BigInt(d)) % MOD : p[i - 1];
    }

    const ans: number[] = [];
    for (const [l, r] of queries) {
        const n0 = cntN0[r + 1] - cntN0[l];
        const sd = BigInt(sumD[r + 1] - sumD[l]);
        const x = (p[r + 1] - ((p[l] * pow10[n0]) % MOD) + MOD) % MOD;
        ans.push(Number((x * sd) % MOD));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
