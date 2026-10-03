---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Geometry
    - Array
    - Hash Table
    - Math
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [2152. Minimum Number of Lines to Cover Points 🔒](https://leetcode.com/problems/minimum-number-of-lines-to-cover-points)

[中文文档](/solution/2100-2199/2152.Minimum%20Number%20of%20Lines%20to%20Cover%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn một điểm trên mặt phẳng <strong>X-Y </strong>.</p>

<p>Ta sẽ thêm các <strong>đường thẳng</strong> vào mặt phẳng <strong>X-Y</strong> sao cho mọi điểm đều được phủ bởi <strong>ít nhất </strong>một đường thẳng.</p>

<p>Trả về <em>số lượng <strong>đường thẳng</strong> <strong>ít nhất </strong>cần thiết để phủ tất cả các điểm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2152.Minimum%20Number%20of%20Lines%20to%20Cover%20Points/images/image-20220123200023-1.png" style="width: 350px; height: 402px;" />
<pre>
<strong>Đầu vào:</strong> points = [[0,1],[2,3],[4,5],[4,3]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Số đường thẳng ít nhất cần thiết là hai. Một lời giải có thể là thêm:
- Một đường thẳng nối điểm tại (0, 1) với điểm tại (4, 5).
- Một đường thẳng khác nối điểm tại (2, 3) với điểm tại (4, 3).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2152.Minimum%20Number%20of%20Lines%20to%20Cover%20Points/images/image-20220123200057-3.png" style="width: 350px; height: 480px;" />
<pre>
<strong>Đầu vào:</strong> points = [[0,2],[-2,-2],[1,4]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Số đường thẳng ít nhất cần thiết là một. Lời giải duy nhất là thêm:
- Một đường thẳng nối điểm tại (-2, -2) với điểm tại (1, 4).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-100 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
	<li>Tất cả các <code>points</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phủ tối đa $10$ điểm bằng ít đường thẳng nhất có thể. Tìm kiếm theo phân hoạch có số trạng thái là hàm mũ, nhưng số điểm đủ nhỏ để dùng DP trên tập con.
>
> $\textit{state}$ là tập các điểm đã được phủ. Với một điểm chưa được phủ $i$, ta ghép nó với $j$, đưa vào mọi điểm khác cùng nằm trên một đường thẳng, rồi đệ quy. Nếu còn lại một điểm đơn lẻ, ta dùng riêng một đường thẳng cho nó.
>
> $dfs(\textit{state})$ có memo sẽ cộng thêm một đường thẳng cho mỗi lần chuyển trạng thái; việc kiểm tra các điểm thẳng hàng được thực hiện bằng tích có hướng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumLines(self, points: List[List[int]]) -> int:
        def check(i, j, k):
            x1, y1 = points[i]
            x2, y2 = points[j]
            x3, y3 = points[k]
            return (x2 - x1) * (y3 - y1) == (x3 - x1) * (y2 - y1)

        @cache
        def dfs(state):
            if state == (1 << n) - 1:
                return 0
            ans = inf
            for i in range(n):
                if not (state >> i & 1):
                    for j in range(i + 1, n):
                        nxt = state | 1 << i | 1 << j
                        for k in range(j + 1, n):
                            if not (nxt >> k & 1) and check(i, j, k):
                                nxt |= 1 << k
                        ans = min(ans, dfs(nxt) + 1)
                    if i == n - 1:
                        ans = min(ans, dfs(state | 1 << i) + 1)
            return ans

        n = len(points)
        return dfs(0)
```

#### Java

```java
class Solution {
    private int[] f;
    private int[][] points;
    private int n;

    public int minimumLines(int[][] points) {
        n = points.length;
        this.points = points;
        f = new int[1 << n];
        return dfs(0);
    }

    private int dfs(int state) {
        if (state == (1 << n) - 1) {
            return 0;
        }
        if (f[state] != 0) {
            return f[state];
        }
        int ans = 1 << 30;
        for (int i = 0; i < n; ++i) {
            if (((state >> i) & 1) == 0) {
                for (int j = i + 1; j < n; ++j) {
                    int nxt = state | 1 << i | 1 << j;
                    for (int k = j + 1; k < n; ++k) {
                        if (((state >> k) & 1) == 0 && check(i, j, k)) {
                            nxt |= 1 << k;
                        }
                    }
                    ans = Math.min(ans, dfs(nxt) + 1);
                }
                if (i == n - 1) {
                    ans = Math.min(ans, dfs(state | 1 << i) + 1);
                }
            }
        }
        return f[state] = ans;
    }

    private boolean check(int i, int j, int k) {
        int x1 = points[i][0], y1 = points[i][1];
        int x2 = points[j][0], y2 = points[j][1];
        int x3 = points[k][0], y3 = points[k][1];
        return (x2 - x1) * (y3 - y1) == (x3 - x1) * (y2 - y1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumLines(vector<vector<int>>& points) {
        auto check = [&](int i, int j, int k) {
            int x1 = points[i][0], y1 = points[i][1];
            int x2 = points[j][0], y2 = points[j][1];
            int x3 = points[k][0], y3 = points[k][1];
            return (x2 - x1) * (y3 - y1) == (x3 - x1) * (y2 - y1);
        };
        int n = points.size();
        int f[1 << n];
        memset(f, 0, sizeof f);
        function<int(int)> dfs = [&](int state) -> int {
            if (state == (1 << n) - 1) return 0;
            if (f[state]) return f[state];
            int ans = 1 << 30;
            for (int i = 0; i < n; ++i) {
                if (!(state >> i & 1)) {
                    for (int j = i + 1; j < n; ++j) {
                        int nxt = state | 1 << i | 1 << j;
                        for (int k = j + 1; k < n; ++k) {
                            if (!(nxt >> k & 1) && check(i, j, k)) {
                                nxt |= 1 << k;
                            }
                        }
                        ans = min(ans, dfs(nxt) + 1);
                    }
                    if (i == n - 1) {
                        ans = min(ans, dfs(state | 1 << i) + 1);
                    }
                }
            }
            return f[state] = ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func minimumLines(points [][]int) int {
	check := func(i, j, k int) bool {
		x1, y1 := points[i][0], points[i][1]
		x2, y2 := points[j][0], points[j][1]
		x3, y3 := points[k][0], points[k][1]
		return (x2-x1)*(y3-y1) == (x3-x1)*(y2-y1)
	}
	n := len(points)
	f := make([]int, 1<<n)
	var dfs func(int) int
	dfs = func(state int) int {
		if state == (1<<n)-1 {
			return 0
		}
		if f[state] > 0 {
			return f[state]
		}
		ans := 1 << 30
		for i := 0; i < n; i++ {
			if (state >> i & 1) == 0 {
				for j := i + 1; j < n; j++ {
					nxt := state | 1<<i | 1<<j
					for k := j + 1; k < n; k++ {
						if (nxt>>k&1) == 0 && check(i, j, k) {
							nxt |= 1 << k
						}
					}
					ans = min(ans, dfs(nxt)+1)
				}
				if i == n-1 {
					ans = min(ans, dfs(state|1<<i)+1)
				}
			}
		}
		f[state] = ans
		return ans
	}
	return dfs(0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
