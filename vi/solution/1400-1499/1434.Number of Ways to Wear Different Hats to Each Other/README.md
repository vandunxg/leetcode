---
comments: true
difficulty: Hard
rating: 2273
source: Biweekly Contest 25 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
    - Bipartite Graph
    - Graph Matching
    - Perfect Matching
---

<!-- problem:start -->

# [1434. Number of Ways to Wear Different Hats to Each Other](https://leetcode.com/problems/number-of-ways-to-wear-different-hats-to-each-other)

[中文文档](/solution/1400-1499/1434.Number%20of%20Ways%20to%20Wear%20Different%20Hats%20to%20Each%20Other/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người và <code>40</code> loại mũ được đánh số từ <code>1</code> đến <code>40</code>.</p>

<p>Cho một mảng số nguyên 2 chiều <code>hats</code>, trong đó <code>hats[i]</code> là danh sách tất cả những chiếc mũ mà người thứ <code>i</code> ưa thích.</p>

<p>Hãy trả về số cách để <code>n</code> người đội những chiếc mũ <strong>khác nhau</strong>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về phần dư của đáp án khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> hats = [[3,4],[4,5],[5]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có một cách chọn mũ thỏa mãn các điều kiện.
Người thứ nhất chọn mũ 3, người thứ hai chọn mũ 4 và người cuối cùng chọn mũ 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> hats = [[3,5,1],[3,5]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 cách chọn mũ:
(3,5), (5,3), (1,3) và (1,5)
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> hats = [[1,2,3,4],[1,2,3,4],[1,2,3,4],[1,2,3,4]]
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Mỗi người có thể chọn mũ được đánh số từ 1 đến 4.
Số hoán vị của (1,2,3,4) = 24.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == hats.length</code></li>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>1 &lt;= hats[i].length &lt;= 40</code></li>
	<li><code>1 &lt;= hats[i][j] &lt;= 40</code></li>
	<li><code>hats[i]</code> chứa một danh sách các số nguyên <strong>không trùng nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Có $n\le 10$ người và nhiều nhất $40$ chiếc mũ. Việc gán mũ theo từng người khó nén trạng thái. Thay vào đó, ta duyệt qua các chiếc mũ và dùng bit mask để biểu diễn những người đã được đội mũ.
>
> $f[i][j]$ là số cách sử dụng $i$ chiếc mũ đầu tiên với tập người đã được gán là $j$. Chiếc mũ $i$ có thể không được sử dụng hoặc được gán cho người $k$ thích nó và vẫn chưa có mũ. Đáp án là $f[m][2^n-1]$.

<!-- thinking:end -->

Ta nhận thấy $n$ không lớn hơn $10$, vì vậy ta có thể dùng quy hoạch động với trạng thái nén để giải bài toán này.

Ta định nghĩa $f[i][j]$ là số cách gán $i$ chiếc mũ đầu tiên cho những người có trạng thái là $j$. Ở đây $j$ là một số nhị phân, biểu diễn một tập người. Ban đầu, ta có $f[0][0]=1$, và đáp án là $f[m][2^n - 1]$, trong đó $m$ là số mũ lớn nhất và $n$ là số người.

Xét $f[i][j]$. Nếu không gán chiếc mũ thứ $i$ cho ai, thì $f[i][j]=f[i-1][j]$; nếu gán chiếc mũ thứ $i$ cho người $k$ thích nó, thì $f[i][j]=f[i-1][j \oplus 2^k]$. Ở đây $\oplus$ biểu thị phép XOR. Do đó, ta có công thức chuyển trạng thái:

$$
f[i][j]=f[i-1][j]+ \sum_{k \in like[i]} f[i-1][j \oplus 2^k]
$$

trong đó $like[i]$ biểu thị tập những người thích chiếc mũ thứ $i$.

Đáp án cuối cùng là $f[m][2^n - 1]$, và đáp án có thể rất lớn, nên ta cần lấy phần dư khi chia cho $10^9 + 7$.

Độ phức tạp thời gian là $O(m \times 2^n \times n)$, độ phức tạp không gian là $O(m \times 2^n)$. Ở đây $m$ là số mũ lớn nhất, không quá $40$ trong bài toán này; còn $n$ là số người, không quá $10$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberWays(self, hats: List[List[int]]) -> int:
        g = defaultdict(list)
        for i, h in enumerate(hats):
            for v in h:
                g[v].append(i)
        mod = 10**9 + 7
        n = len(hats)
        m = max(max(h) for h in hats)
        f = [[0] * (1 << n) for _ in range(m + 1)]
        f[0][0] = 1
        for i in range(1, m + 1):
            for j in range(1 << n):
                f[i][j] = f[i - 1][j]
                for k in g[i]:
                    if j >> k & 1:
                        f[i][j] = (f[i][j] + f[i - 1][j ^ (1 << k)]) % mod
        return f[m][-1]
```

#### Java

```java
class Solution {
    public int numberWays(List<List<Integer>> hats) {
        int n = hats.size();
        int m = 0;
        for (var h : hats) {
            for (int v : h) {
                m = Math.max(m, v);
            }
        }
        List<Integer>[] g = new List[m + 1];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            for (int v : hats.get(i)) {
                g[v].add(i);
            }
        }
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[m + 1][1 << n];
        f[0][0] = 1;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j < 1 << n; ++j) {
                f[i][j] = f[i - 1][j];
                for (int k : g[i]) {
                    if ((j >> k & 1) == 1) {
                        f[i][j] = (f[i][j] + f[i - 1][j ^ (1 << k)]) % mod;
                    }
                }
            }
        }
        return f[m][(1 << n) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberWays(vector<vector<int>>& hats) {
        int n = hats.size();
        int m = 0;
        for (auto& h : hats) {
            m = max(m, *max_element(h.begin(), h.end()));
        }
        vector<vector<int>> g(m + 1);
        for (int i = 0; i < n; ++i) {
            for (int& v : hats[i]) {
                g[v].push_back(i);
            }
        }
        const int mod = 1e9 + 7;
        int f[m + 1][1 << n];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j < 1 << n; ++j) {
                f[i][j] = f[i - 1][j];
                for (int k : g[i]) {
                    if (j >> k & 1) {
                        f[i][j] = (f[i][j] + f[i - 1][j ^ (1 << k)]) % mod;
                    }
                }
            }
        }
        return f[m][(1 << n) - 1];
    }
};
```

#### Go

```go
func numberWays(hats [][]int) int {
	n := len(hats)
	m := 0
	for _, h := range hats {
		m = max(m, slices.Max(h))
	}
	g := make([][]int, m+1)
	for i, h := range hats {
		for _, v := range h {
			g[v] = append(g[v], i)
		}
	}
	const mod = 1e9 + 7
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, 1<<n)
	}
	f[0][0] = 1
	for i := 1; i <= m; i++ {
		for j := 0; j < 1<<n; j++ {
			f[i][j] = f[i-1][j]
			for _, k := range g[i] {
				if j>>k&1 == 1 {
					f[i][j] = (f[i][j] + f[i-1][j^(1<<k)]) % mod
				}
			}
		}
	}
	return f[m][(1<<n)-1]
}
```

#### TypeScript

```ts
function numberWays(hats: number[][]): number {
    const n = hats.length;
    const m = Math.max(...hats.flat());
    const g: number[][] = Array.from({ length: m + 1 }, () => []);
    for (let i = 0; i < n; ++i) {
        for (const v of hats[i]) {
            g[v].push(i);
        }
    }
    const f: number[][] = Array.from({ length: m + 1 }, () =>
        Array.from({ length: 1 << n }, () => 0),
    );
    f[0][0] = 1;
    const mod = 1e9 + 7;
    for (let i = 1; i <= m; ++i) {
        for (let j = 0; j < 1 << n; ++j) {
            f[i][j] = f[i - 1][j];
            for (const k of g[i]) {
                if (((j >> k) & 1) === 1) {
                    f[i][j] = (f[i][j] + f[i - 1][j ^ (1 << k)]) % mod;
                }
            }
        }
    }
    return f[m][(1 << n) - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
