---
comments: true
difficulty: Hard
rating: 2077
source: Biweekly Contest 119 Q4
tags:
    - Bit Manipulation
    - Graph
    - Enumeration
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2959. Number of Possible Sets of Closing Branches](https://leetcode.com/problems/number-of-possible-sets-of-closing-branches)

[中文文档](/solution/2900-2999/2959.Number%20of%20Possible%20Sets%20of%20Closing%20Branches/README.md)

## Mô tả

<!-- description:start -->

<p>Có một công ty có <code>n</code> chi nhánh trên toàn quốc, một số chi nhánh được nối với nhau bằng các con đường. Ban đầu, mọi chi nhánh đều có thể đi đến nhau bằng cách đi qua một số con đường.</p>

<p>Công ty nhận thấy họ đang mất quá nhiều thời gian để di chuyển giữa các chi nhánh. Vì vậy, họ quyết định đóng cửa một số chi nhánh (<strong>có thể không đóng chi nhánh nào</strong>). Tuy nhiên, họ muốn đảm bảo rằng khoảng cách giữa mọi cặp chi nhánh còn lại không vượt quá <code>maxDistance</code>.</p>

<p><strong>Khoảng cách</strong> giữa hai chi nhánh là <strong>tổng độ dài nhỏ nhất</strong> cần đi qua để đến một chi nhánh từ chi nhánh kia.</p>

<p>Cho các số nguyên <code>n</code>, <code>maxDistance</code> và một mảng 2 chiều <strong>đánh chỉ số từ 0</strong> <code>roads</code>, trong đó <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu diễn con đường <strong>vô hướng</strong> nối hai chi nhánh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>, có độ dài <code>w<sub>i</sub></code>.</p>

<p>Trả về <em>số tập hợp các chi nhánh có thể đóng sao cho khoảng cách từ mỗi chi nhánh đến mọi chi nhánh khác không vượt quá </em><code>maxDistance</code><em>.</em></p>

<p><strong>Lưu ý</strong> rằng sau khi đóng một chi nhánh, công ty sẽ không còn sử dụng được bất kỳ con đường nào nối với chi nhánh đó.</p>

<p><strong>Lưu ý</strong> rằng có thể có nhiều con đường nối cùng một cặp chi nhánh.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2959.Number%20of%20Possible%20Sets%20of%20Closing%20Branches/images/example11.png" style="width: 221px; height: 191px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, maxDistance = 5, roads = [[0,1,2],[1,2,10],[0,2,10]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các tập hợp chi nhánh có thể đóng là:
- Tập [2], sau khi đóng cửa, các chi nhánh đang hoạt động là [0,1] và chúng có thể đi đến nhau với khoảng cách 2.
- Tập [0,1], sau khi đóng cửa, chi nhánh đang hoạt động là [2].
- Tập [1,2], sau khi đóng cửa, chi nhánh đang hoạt động là [0].
- Tập [0,2], sau khi đóng cửa, chi nhánh đang hoạt động là [1].
- Tập [0,1,2], sau khi đóng cửa, không còn chi nhánh nào đang hoạt động.
Có thể chứng minh rằng chỉ có 5 tập hợp có thể đóng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2959.Number%20of%20Possible%20Sets%20of%20Closing%20Branches/images/example22.png" style="width: 221px; height: 241px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, maxDistance = 5, roads = [[0,1,20],[0,1,10],[1,2,2],[0,2,2]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các tập hợp chi nhánh có thể đóng là:
- Tập [], sau khi đóng cửa, các chi nhánh đang hoạt động là [0,1,2] và chúng có thể đi đến nhau với khoảng cách 4.
- Tập [0], sau khi đóng cửa, các chi nhánh đang hoạt động là [1,2] và chúng có thể đi đến nhau với khoảng cách 2.
- Tập [1], sau khi đóng cửa, các chi nhánh đang hoạt động là [0,2] và chúng có thể đi đến nhau với khoảng cách 2.
- Tập [0,1], sau khi đóng cửa, chi nhánh đang hoạt động là [2].
- Tập [1,2], sau khi đóng cửa, chi nhánh đang hoạt động là [0].
- Tập [0,2], sau khi đóng cửa, chi nhánh đang hoạt động là [1].
- Tập [0,1,2], sau khi đóng cửa, không còn chi nhánh nào đang hoạt động.
Có thể chứng minh rằng chỉ có 7 tập hợp có thể đóng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, maxDistance = 10, roads = []
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các tập hợp chi nhánh có thể đóng là:
- Tập [], sau khi đóng cửa, chi nhánh đang hoạt động là [0].
- Tập [0], sau khi đóng cửa, không còn chi nhánh nào đang hoạt động.
Có thể chứng minh rằng chỉ có 2 tập hợp có thể đóng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>1 &lt;= maxDistance &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= roads.length &lt;= 1000</code></li>
	<li><code>roads[i].length == 3</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 1000</code></li>
	<li>Mọi chi nhánh đều có thể đi đến nhau bằng cách đi qua một số con đường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân + thuật toán Floyd

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi đóng một tập con các chi nhánh, đường đi ngắn nhất giữa mọi cặp chi nhánh còn lại phải không vượt quá $maxDistance$. Vì $n \le 10$, ta có thể tạo $2^n$ mask. Với mỗi mask, chạy Floyd trên các cạnh còn lại rồi kiểm tra khoảng cách giữa các chi nhánh trong tập con.
>
> Với các cạnh song song, giữ lại cạnh nhẹ hơn. Tập rỗng và các tập chỉ có một phần tử đều hợp lệ (các phần tử trên đường chéo được gán bằng $0$).

<!-- thinking:end -->

Ta nhận thấy rằng $n \leq 10$, vì vậy ta có thể dùng phương pháp liệt kê nhị phân để liệt kê tất cả các tập con của các chi nhánh.

Với mỗi tập con các chi nhánh, ta có thể dùng thuật toán Floyd để tính khoảng cách ngắn nhất giữa các chi nhánh còn lại, sau đó kiểm tra xem có thỏa mãn yêu cầu của đề bài hay không. Cụ thể, trước hết ta liệt kê điểm trung gian $k$, rồi lần lượt liệt kê điểm bắt đầu $i$ và điểm kết thúc $j$. Nếu $g[i][k] + g[k][j] < g[i][j]$, ta cập nhật $g[i][j]$ bằng khoảng cách ngắn hơn $g[i][k] + g[k][j]$.

Độ phức tạp thời gian là $O(2^n \times (n^3 + m))$, và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là số chi nhánh và $m$ là số con đường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSets(self, n: int, maxDistance: int, roads: List[List[int]]) -> int:
        ans = 0
        for mask in range(1 << n):
            g = [[inf] * n for _ in range(n)]
            for u, v, w in roads:
                if mask >> u & 1 and mask >> v & 1:
                    g[u][v] = min(g[u][v], w)
                    g[v][u] = min(g[v][u], w)
            for k in range(n):
                if mask >> k & 1:
                    g[k][k] = 0
                    for i in range(n):
                        for j in range(n):
                            # g[i][j] = min(g[i][j], g[i][k] + g[k][j])
                            if g[i][k] + g[k][j] < g[i][j]:
                                g[i][j] = g[i][k] + g[k][j]
            if all(
                g[i][j] <= maxDistance
                for i in range(n)
                for j in range(n)
                if mask >> i & 1 and mask >> j & 1
            ):
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSets(int n, int maxDistance, int[][] roads) {
        int ans = 0;
        for (int mask = 0; mask < 1 << n; ++mask) {
            int[][] g = new int[n][n];
            for (var e : g) {
                Arrays.fill(e, 1 << 29);
            }
            for (var e : roads) {
                int u = e[0], v = e[1], w = e[2];
                if ((mask >> u & 1) == 1 && (mask >> v & 1) == 1) {
                    g[u][v] = Math.min(g[u][v], w);
                    g[v][u] = Math.min(g[v][u], w);
                }
            }
            for (int k = 0; k < n; ++k) {
                if ((mask >> k & 1) == 1) {
                    g[k][k] = 0;
                    for (int i = 0; i < n; ++i) {
                        for (int j = 0; j < n; ++j) {
                            g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
                        }
                    }
                }
            }
            int ok = 1;
            for (int i = 0; i < n && ok == 1; ++i) {
                for (int j = 0; j < n && ok == 1; ++j) {
                    if ((mask >> i & 1) == 1 && (mask >> j & 1) == 1) {
                        if (g[i][j] > maxDistance) {
                            ok = 0;
                        }
                    }
                }
            }
            ans += ok;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSets(int n, int maxDistance, vector<vector<int>>& roads) {
        int ans = 0;
        for (int mask = 0; mask < 1 << n; ++mask) {
            int g[n][n];
            memset(g, 0x3f, sizeof(g));
            for (auto& e : roads) {
                int u = e[0], v = e[1], w = e[2];
                if ((mask >> u & 1) & (mask >> v & 1)) {
                    g[u][v] = min(g[u][v], w);
                    g[v][u] = min(g[v][u], w);
                }
            }
            for (int k = 0; k < n; ++k) {
                if (mask >> k & 1) {
                    g[k][k] = 0;
                    for (int i = 0; i < n; ++i) {
                        for (int j = 0; j < n; ++j) {
                            g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                        }
                    }
                }
            }
            int ok = 1;
            for (int i = 0; i < n && ok == 1; ++i) {
                for (int j = 0; j < n && ok == 1; ++j) {
                    if ((mask >> i & 1) & (mask >> j & 1) && g[i][j] > maxDistance) {
                        ok = 0;
                    }
                }
            }
            ans += ok;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSets(n int, maxDistance int, roads [][]int) (ans int) {
	for mask := 0; mask < 1<<n; mask++ {
		g := make([][]int, n)
		for i := range g {
			g[i] = make([]int, n)
			for j := range g[i] {
				g[i][j] = 1 << 29
			}
		}
		for _, e := range roads {
			u, v, w := e[0], e[1], e[2]
			if mask>>u&1 == 1 && mask>>v&1 == 1 {
				g[u][v] = min(g[u][v], w)
				g[v][u] = min(g[v][u], w)
			}
		}
		for k := 0; k < n; k++ {
			if mask>>k&1 == 1 {
				g[k][k] = 0
				for i := 0; i < n; i++ {
					for j := 0; j < n; j++ {
						g[i][j] = min(g[i][j], g[i][k]+g[k][j])
					}
				}
			}
		}
		ok := 1
		for i := 0; i < n && ok == 1; i++ {
			for j := 0; j < n && ok == 1; j++ {
				if mask>>i&1 == 1 && mask>>j&1 == 1 && g[i][j] > maxDistance {
					ok = 0
				}
			}
		}
		ans += ok
	}
	return
}
```

#### TypeScript

```ts
function numberOfSets(n: number, maxDistance: number, roads: number[][]): number {
    let ans = 0;
    for (let mask = 0; mask < 1 << n; ++mask) {
        const g: number[][] = Array.from({ length: n }, () => Array(n).fill(Infinity));
        for (const [u, v, w] of roads) {
            if ((mask >> u) & 1 && (mask >> v) & 1) {
                g[u][v] = Math.min(g[u][v], w);
                g[v][u] = Math.min(g[v][u], w);
            }
        }
        for (let k = 0; k < n; ++k) {
            if ((mask >> k) & 1) {
                g[k][k] = 0;
                for (let i = 0; i < n; ++i) {
                    for (let j = 0; j < n; ++j) {
                        g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
                    }
                }
            }
        }
        let ok = 1;
        for (let i = 0; i < n && ok; ++i) {
            for (let j = 0; j < n && ok; ++j) {
                if ((mask >> i) & 1 && (mask >> j) & 1 && g[i][j] > maxDistance) {
                    ok = 0;
                }
            }
        }
        ans += ok;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
