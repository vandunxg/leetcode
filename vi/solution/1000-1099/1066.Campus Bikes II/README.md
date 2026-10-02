---
comments: true
difficulty: Medium
rating: 1885
source: Biweekly Contest 1 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
    - Hungarian Algorithm
    - Bipartite Graph
    - Graph Matching
    - Min-Cost Flow
    - SSP
    - Network Flow
---

<!-- problem:start -->

# [1066. Campus Bikes II 🔒](https://leetcode.com/problems/campus-bikes-ii)

[中文文档](/solution/1000-1099/1066.Campus%20Bikes%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Trong khuôn viên trường được biểu diễn bằng lưới 2D, có <code>n</code> nhân viên và <code>m</code> chiếc xe đạp, với <code>n &lt;= m</code>. Mỗi nhân viên và mỗi xe đạp tương ứng với một tọa độ 2D trên lưới.</p>

<p>Ta phân cho mỗi nhân viên một chiếc xe đạp riêng sao cho tổng <strong>khoảng cách Manhattan</strong> giữa mỗi nhân viên và xe được phân công là nhỏ nhất.</p>

<p>Trả về <code>tổng khoảng cách Manhattan nhỏ nhất có thể giữa mỗi nhân viên và chiếc xe được phân cho họ</code>.</p>

<p><strong>Khoảng cách Manhattan</strong> giữa hai điểm <code>p1</code> và <code>p2</code> được tính bằng <code>Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1066.Campus%20Bikes%20II/images/1261_example_1_v2.png" style="width: 376px; height: 366px;" />
<pre>
<strong>Đầu vào:</strong> workers = [[0,0],[2,1]], bikes = [[1,2],[3,3]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 
Ta phân xe đạp 0 cho nhân viên 0 và xe đạp 1 cho nhân viên 1. Khoảng cách Manhattan của mỗi cặp được phân là 3, nên tổng là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1066.Campus%20Bikes%20II/images/1261_example_2_v2.png" style="width: 376px; height: 366px;" />
<pre>
<strong>Đầu vào:</strong> workers = [[0,0],[1,1],[2,0]], bikes = [[1,0],[2,2],[2,1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>
Trước tiên, ta phân xe đạp 0 cho nhân viên 0, sau đó phân xe đạp 1 cho nhân viên 1 hoặc 2, và xe đạp 2 cho nhân viên còn lại. Cả hai cách phân đều cho tổng khoảng cách Manhattan bằng 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> workers = [[0,0],[1,0],[2,0],[3,0],[4,0]], bikes = [[0,999],[1,999],[2,999],[3,999],[4,999]]
<strong>Đầu ra:</strong> 4995
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == workers.length</code></li>
	<li><code>m == bikes.length</code></li>
	<li><code>1 &lt;= n &lt;= m &lt;= 10</code></li>
	<li><code>workers[i].length == 2</code></li>
	<li><code>bikes[i].length == 2</code></li>
	<li><code>0 &lt;= workers[i][0], workers[i][1], bikes[i][0], bikes[i][1] &lt; 1000</code></li>
	<li>Tọa độ của tất cả nhân viên và xe đạp đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa 10 nhân viên và xe đạp; liệt kê mọi cách phân công cần xét $P(m,n)$ trường hợp. Tập xe đã dùng có thể biểu diễn bằng mask $m$ bit, nên có $n\cdot 2^m$ trạng thái.
>
> $f[i][j]$ là tổng khoảng cách nhỏ nhất sau khi phân xe cho $i$ nhân viên, với mask $j$ biểu diễn các xe đã dùng. Với mỗi bit $k$ được bật trong $j$, ta chuyển từ $f[i-1][j\oplus 2^k]$ rồi cộng khoảng cách Manhattan của cặp tương ứng.
>
> Đáp án là giá trị nhỏ nhất ở hàng $n$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> int:
        n, m = len(workers), len(bikes)
        f = [[inf] * (1 << m) for _ in range(n + 1)]
        f[0][0] = 0
        for i, (x1, y1) in enumerate(workers, 1):
            for j in range(1 << m):
                for k, (x2, y2) in enumerate(bikes):
                    if j >> k & 1:
                        f[i][j] = min(
                            f[i][j],
                            f[i - 1][j ^ (1 << k)] + abs(x1 - x2) + abs(y1 - y2),
                        )
        return min(f[n])
```

#### Java

```java
class Solution {
    public int assignBikes(int[][] workers, int[][] bikes) {
        int n = workers.length;
        int m = bikes.length;
        int[][] f = new int[n + 1][1 << m];
        for (var g : f) {
            Arrays.fill(g, 1 << 30);
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 1 << m; ++j) {
                for (int k = 0; k < m; ++k) {
                    if ((j >> k & 1) == 1) {
                        int d = Math.abs(workers[i - 1][0] - bikes[k][0])
                            + Math.abs(workers[i - 1][1] - bikes[k][1]);
                        f[i][j] = Math.min(f[i][j], f[i - 1][j ^ (1 << k)] + d);
                    }
                }
            }
        }
        return Arrays.stream(f[n]).min().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int assignBikes(vector<vector<int>>& workers, vector<vector<int>>& bikes) {
        int n = workers.size(), m = bikes.size();
        int f[n + 1][1 << m];
        memset(f, 0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 1 << m; ++j) {
                for (int k = 0; k < m; ++k) {
                    if (j >> k & 1) {
                        int d = abs(workers[i - 1][0] - bikes[k][0]) + abs(workers[i - 1][1] - bikes[k][1]);
                        f[i][j] = min(f[i][j], f[i - 1][j ^ (1 << k)] + d);
                    }
                }
            }
        }
        return *min_element(f[n], f[n] + (1 << m));
    }
};
```

#### Go

```go
func assignBikes(workers [][]int, bikes [][]int) int {
	n, m := len(workers), len(bikes)
	f := make([][]int, n+1)
	const inf = 1 << 30
	for i := range f {
		f[i] = make([]int, 1<<m)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= n; i++ {
		for j := 0; j < 1<<m; j++ {
			for k := 0; k < m; k++ {
				if j>>k&1 == 1 {
					d := abs(workers[i-1][0]-bikes[k][0]) + abs(workers[i-1][1]-bikes[k][1])
					f[i][j] = min(f[i][j], f[i-1][j^(1<<k)]+d)
				}
			}
		}
	}
	return slices.Min(f[n])
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function assignBikes(workers: number[][], bikes: number[][]): number {
    const n = workers.length;
    const m = bikes.length;
    const inf = 1 << 30;
    const f: number[][] = new Array(n + 1).fill(0).map(() => new Array(1 << m).fill(inf));
    f[0][0] = 0;
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j < 1 << m; ++j) {
            for (let k = 0; k < m; ++k) {
                if (((j >> k) & 1) === 1) {
                    const d =
                        Math.abs(workers[i - 1][0] - bikes[k][0]) +
                        Math.abs(workers[i - 1][1] - bikes[k][1]);
                    f[i][j] = Math.min(f[i][j], f[i - 1][j ^ (1 << k)] + d);
                }
            }
        }
    }
    return Math.min(...f[n]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
