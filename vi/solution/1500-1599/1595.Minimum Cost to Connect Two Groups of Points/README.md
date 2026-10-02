---
comments: true
difficulty: Hard
rating: 2537
source: Weekly Contest 207 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
    - Matrix
    - Bipartite Graph
    - Graph Matching
    - Min-Cost Flow
    - SSP
    - Network Flow
---

<!-- problem:start -->

# [1595. Minimum Cost to Connect Two Groups of Points](https://leetcode.com/problems/minimum-cost-to-connect-two-groups-of-points)

[中文文档](/solution/1500-1599/1595.Minimum%20Cost%20to%20Connect%20Two%20Groups%20of%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai nhóm điểm, nhóm thứ nhất có <code>size<sub>1</sub></code> điểm, nhóm thứ hai có <code>size<sub>2</sub></code> điểm và <code>size<sub>1</sub> &gt;= size<sub>2</sub></code>.</p>

<p>The <code>cost</code> of the connection between any two points are given in an <code>size<sub>1</sub> x size<sub>2</sub></code> matrix where <code>cost[i][j]</code> is the cost of connecting point <code>i</code> of the first group and point <code>j</code> of the second group. The groups are connected if <strong>each point in both groups is connected to one or more points in the opposite group</strong>. In other words, each point in the first group must be connected to at least one point in the second group, and each point in the second group must be connected to at least one point in the first group.</p>

<p>Trả về <em>chi phí nhỏ nhất để nối hai nhóm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1595.Minimum%20Cost%20to%20Connect%20Two%20Groups%20of%20Points/images/ex1.jpg" style="width: 322px; height: 243px;" />
<pre>
<strong>Input:</strong> cost = [[15, 96], [36, 2]]
<strong>Output:</strong> 17
<strong>Explanation</strong>: The optimal way of connecting the groups is:
1--A
2--B
This results in a total cost of 17.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1595.Minimum%20Cost%20to%20Connect%20Two%20Groups%20of%20Points/images/ex2.jpg" style="width: 322px; height: 403px;" />
<pre>
<strong>Input:</strong> cost = [[1, 3, 5], [4, 1, 1], [1, 5, 3]]
<strong>Output:</strong> 4
<strong>Explanation</strong>: The optimal way of connecting the groups is:
1--A
2--B
2--C
3--A
This results in a total cost of 4.
Note that there are multiple points connected to point 2 in the first group and point A in the second group. This does not matter as there is no limit to the number of points that can be connected. We only care about the minimum total cost.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> cost = [[2, 5, 1], [3, 4, 7], [8, 1, 2], [6, 2, 4], [3, 8, 8]]
<strong>Output:</strong> 10
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>size<sub>1</sub> == cost.length</code></li>
	<li><code>size<sub>2</sub> == cost[i].length</code></li>
	<li><code>1 &lt;= size<sub>1</sub>, size<sub>2</sub> &lt;= 12</code></li>
	<li><code>size<sub>1</sub> &gt;= size<sub>2</sub></code></li>
	<li><code>0 &lt;= cost[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bitmask Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Nối hai nhóm sao cho mỗi điểm có ít nhất một cạnh và tổng chi phí nhỏ nhất. Mỗi bên có nhiều nhất $12$ điểm; không thể liệt kê các tập cạnh, nhưng nhóm thứ hai vừa với một mask $n$ bit biểu diễn các điểm đã được nối.
>
> Gọi $f[i][j]$ là chi phí nhỏ nhất sau khi xử lý $i$ điểm đầu của nhóm một, với mask của nhóm hai bằng $j$. Với mỗi bit $k$ bật trong $j$, chuyển từ trường hợp “vừa nối $k$ ở lượt này”, “đã có $j$ từ lượt trước”, hoặc “thiếu $k$ ở lượt trước”. Đáp án là $f[m][2^n-1]$.

<!-- thinking:end -->

Gọi $m$ và $n$ lần lượt là số điểm trong nhóm thứ nhất và nhóm thứ hai.

Since $1 \leq n \leq m \leq 12$, we can use an integer to represent the state of points in the second group — specifically, a binary integer of length $n$, where bit $k$ being $1$ means the $k$-th point in the second group is connected to some point in the first group, and $0$ means it is not.

Next, we define $f[i][j]$ as the minimum cost when the first $i$ points in the first group are all connected, and the state of points in the second group is $j$. Initially, $f[0][0] = 0$ and all other values are positive infinity. The answer is $f[m][2^n - 1]$.

Consider $f[i][j]$ where $i \geq 1$. We enumerate each point $k$ in the second group. If point $k$ is connected to the $i$-th point in the first group, we discuss the following two cases:

- If point $k$ is only connected to the $i$-th point in the first group, then $f[i][j]$ can be transitioned from $f[i][j \oplus 2^k]$ or $f[i - 1][j \oplus 2^k]$, where $\oplus$ denotes the XOR operation;
- If point $k$ is connected to the $i$-th point in the first group as well as other points, then $f[i][j]$ can be transitioned from $f[i - 1][j]$.

In both cases, we take the minimum transition value:

$$
f[i][j] = \min_{k \in \{0, 1, \cdots, n - 1\}} \{f[i][j \oplus 2^k], f[i - 1][j \oplus 2^k], f[i - 1][j]\} + cost[i - 1][k]
$$

Cuối cùng, ta trả về $f[m][2^n - 1]$.

The time complexity is $O(m \times n \times 2^n)$ and the space complexity is $O(m \times 2^n)$, where $m$ and $n$ are the number of points in the first group and the second group, respectively.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def connectTwoGroups(self, cost: List[List[int]]) -> int:
        m, n = len(cost), len(cost[0])
        f = [[inf] * (1 << n) for _ in range(m + 1)]
        f[0][0] = 0
        for i in range(1, m + 1):
            for j in range(1 << n):
                for k in range(n):
                    if (j >> k & 1) == 0:
                        continue
                    c = cost[i - 1][k]
                    x = min(f[i][j ^ (1 << k)], f[i - 1][j], f[i - 1][j ^ (1 << k)]) + c
                    f[i][j] = min(f[i][j], x)
        return f[m][-1]
```

#### Java

```java
class Solution {
    public int connectTwoGroups(List<List<Integer>> cost) {
        int m = cost.size(), n = cost.get(0).size();
        final int inf = 1 << 30;
        int[][] f = new int[m + 1][1 << n];
        for (int[] g : f) {
            Arrays.fill(g, inf);
        }
        f[0][0] = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j < 1 << n; ++j) {
                for (int k = 0; k < n; ++k) {
                    if ((j >> k & 1) == 1) {
                        int c = cost.get(i - 1).get(k);
                        f[i][j] = Math.min(f[i][j], f[i][j ^ (1 << k)] + c);
                        f[i][j] = Math.min(f[i][j], f[i - 1][j] + c);
                        f[i][j] = Math.min(f[i][j], f[i - 1][j ^ (1 << k)] + c);
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
    int connectTwoGroups(vector<vector<int>>& cost) {
        int m = cost.size(), n = cost[0].size();
        int f[m + 1][1 << n];
        memset(f, 0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j < 1 << n; ++j) {
                for (int k = 0; k < n; ++k) {
                    if (j >> k & 1) {
                        int c = cost[i - 1][k];
                        int x = min({f[i][j ^ (1 << k)], f[i - 1][j], f[i - 1][j ^ (1 << k)]}) + c;
                        f[i][j] = min(f[i][j], x);
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
func connectTwoGroups(cost [][]int) int {
	m, n := len(cost), len(cost[0])
	const inf = 1 << 30
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, 1<<n)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= m; i++ {
		for j := 0; j < 1<<n; j++ {
			for k := 0; k < n; k++ {
				c := cost[i-1][k]
				if j>>k&1 == 1 {
					f[i][j] = min(f[i][j], f[i][j^(1<<k)]+c)
					f[i][j] = min(f[i][j], f[i-1][j]+c)
					f[i][j] = min(f[i][j], f[i-1][j^(1<<k)]+c)
				}
			}
		}
	}
	return f[m][(1<<n)-1]
}
```

#### TypeScript

```ts
function connectTwoGroups(cost: number[][]): number {
    const m = cost.length;
    const n = cost[0].length;
    const inf = 1 << 30;
    const f = Array.from({ length: m + 1 }, () => Array(1 << n).fill(inf));
    f[0][0] = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 0; j < 1 << n; ++j) {
            for (let k = 0; k < n; ++k) {
                if (((j >> k) & 1) === 1) {
                    const c = cost[i - 1][k];
                    f[i][j] = Math.min(f[i][j], f[i][j ^ (1 << k)] + c);
                    f[i][j] = Math.min(f[i][j], f[i - 1][j] + c);
                    f[i][j] = Math.min(f[i][j], f[i - 1][j ^ (1 << k)] + c);
                }
            }
        }
    }
    return f[m][(1 << n) - 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn connect_two_groups(cost: Vec<Vec<i32>>) -> i32 {
        let m = cost.len();
        let n = cost[0].len();
        let inf = 1 << 30;

        let mut f = vec![vec![inf; 1 << n]; m + 1];
        f[0][0] = 0;

        for i in 1..=m {
            for j in 0..(1 << n) {
                for k in 0..n {
                    if (j >> k) & 1 == 1 {
                        let c = cost[i - 1][k];
                        f[i][j] = f[i][j].min(f[i][j ^ (1 << k)] + c);
                        f[i][j] = f[i][j].min(f[i - 1][j] + c);
                        f[i][j] = f[i][j].min(f[i - 1][j ^ (1 << k)] + c);
                    }
                }
            }
        }

        f[m][(1 << n) - 1]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Bitmask Dynamic Programming (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lớp $i$ chỉ đọc lớp $i-1$ và các giá trị đã điền trong cùng lớp, nên dùng hai mảng cuộn giảm bộ nhớ từ $O(m\cdot 2^n)$ xuống $O(2^n)$ mà không thay đổi công thức truy hồi.

<!-- thinking:end -->

Ta nhận thấy phép chuyển của $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][\cdot]$ và $f[i][\cdot]$, nên có thể dùng mảng cuộn để giảm độ phức tạp không gian xuống $O(2^n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def connectTwoGroups(self, cost: List[List[int]]) -> int:
        m, n = len(cost), len(cost[0])
        f = [inf] * (1 << n)
        f[0] = 0
        g = f[:]
        for i in range(1, m + 1):
            for j in range(1 << n):
                g[j] = inf
                for k in range(n):
                    if (j >> k & 1) == 0:
                        continue
                    c = cost[i - 1][k]
                    x = min(g[j ^ (1 << k)], f[j], f[j ^ (1 << k)]) + c
                    g[j] = min(g[j], x)
            f = g[:]
        return f[-1]
```

#### Java

```java
class Solution {
    public int connectTwoGroups(List<List<Integer>> cost) {
        int m = cost.size(), n = cost.get(0).size();
        final int inf = 1 << 30;
        int[] f = new int[1 << n];
        Arrays.fill(f, inf);
        f[0] = 0;
        int[] g = f.clone();
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j < 1 << n; ++j) {
                g[j] = inf;
                for (int k = 0; k < n; ++k) {
                    if ((j >> k & 1) == 1) {
                        int c = cost.get(i - 1).get(k);
                        g[j] = Math.min(g[j], g[j ^ (1 << k)] + c);
                        g[j] = Math.min(g[j], f[j] + c);
                        g[j] = Math.min(g[j], f[j ^ (1 << k)] + c);
                    }
                }
            }
            System.arraycopy(g, 0, f, 0, 1 << n);
        }
        return f[(1 << n) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int connectTwoGroups(vector<vector<int>>& cost) {
        int m = cost.size(), n = cost[0].size();
        const int inf = 1 << 30;
        vector<int> f(1 << n, inf);
        f[0] = 0;
        vector<int> g = f;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j < 1 << n; ++j) {
                g[j] = inf;
                for (int k = 0; k < n; ++k) {
                    if (j >> k & 1) {
                        int c = cost[i - 1][k];
                        int x = min({g[j ^ (1 << k)], f[j], f[j ^ (1 << k)]}) + c;
                        g[j] = min(g[j], x);
                    }
                }
            }
            f.swap(g);
        }
        return f[(1 << n) - 1];
    }
};
```

#### Go

```go
func connectTwoGroups(cost [][]int) int {
	m, n := len(cost), len(cost[0])
	const inf = 1 << 30
	f := make([]int, 1<<n)
	for i := range f {
		f[i] = inf
	}
	f[0] = 0
	g := make([]int, 1<<n)
	for i := 1; i <= m; i++ {
		for j := 0; j < 1<<n; j++ {
			g[j] = inf
			for k := 0; k < n; k++ {
				c := cost[i-1][k]
				if j>>k&1 == 1 {
					g[j] = min(g[j], g[j^1<<k]+c)
					g[j] = min(g[j], f[j]+c)
					g[j] = min(g[j], f[j^1<<k]+c)
				}
			}
		}
		copy(f, g)
	}
	return f[1<<n-1]
}
```

#### TypeScript

```ts
function connectTwoGroups(cost: number[][]): number {
    const m = cost.length;
    const n = cost[0].length;
    const inf = 1 << 30;
    const f: number[] = new Array(1 << n).fill(inf);
    f[0] = 0;
    const g = new Array(1 << n).fill(0);
    for (let i = 1; i <= m; ++i) {
        for (let j = 0; j < 1 << n; ++j) {
            g[j] = inf;
            for (let k = 0; k < n; ++k) {
                if (((j >> k) & 1) === 1) {
                    const c = cost[i - 1][k];
                    g[j] = Math.min(g[j], g[j ^ (1 << k)] + c);
                    g[j] = Math.min(g[j], f[j] + c);
                    g[j] = Math.min(g[j], f[j ^ (1 << k)] + c);
                }
            }
        }
        f.splice(0, f.length, ...g);
    }
    return f[(1 << n) - 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn connect_two_groups(cost: Vec<Vec<i32>>) -> i32 {
        let m = cost.len();
        let n = cost[0].len();
        let inf = 1 << 30;

        let mut f = vec![inf; 1 << n];
        f[0] = 0;

        let mut g = f.clone();

        for i in 1..=m {
            for j in 0..(1 << n) {
                g[j] = inf;
                for k in 0..n {
                    if (j >> k) & 1 == 1 {
                        let c = cost[i - 1][k];
                        g[j] = g[j].min(g[j ^ (1 << k)] + c);
                        g[j] = g[j].min(f[j] + c);
                        g[j] = g[j].min(f[j ^ (1 << k)] + c);
                    }
                }
            }
            f.clone_from_slice(&g);
        }

        f[(1 << n) - 1]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
