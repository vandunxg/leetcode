---
comments: true
difficulty: Medium
tags:
    - Geometry
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [963. Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii)

[中文文档](/solution/0900-0999/0963.Minimum%20Area%20Rectangle%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các điểm <code>points</code> trên mặt phẳng <strong>X-Y</strong>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.</p>

<p>Trả về <em>diện tích nhỏ nhất của hình chữ nhật có thể tạo thành từ các điểm này, với các cạnh <strong>không nhất thiết song song</strong> với trục X và Y</em>. Nếu không thể tạo thành hình chữ nhật nào như vậy, trả về <code>0</code>.</p>

<p>Các đáp án sai lệch không quá <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0963.Minimum%20Area%20Rectangle%20II/images/1a.png" style="width: 398px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,2],[2,1],[1,0],[0,1]]
<strong>Đầu ra:</strong> 2.00000
<strong>Giải thích:</strong> Hình chữ nhật có diện tích nhỏ nhất được tạo từ các điểm [1,2],[2,1],[1,0],[0,1], với diện tích bằng 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0963.Minimum%20Area%20Rectangle%20II/images/2.png" style="width: 400px; height: 251px;" />
<pre>
<strong>Đầu vào:</strong> points = [[0,1],[2,1],[1,1],[1,0],[2,0]]
<strong>Đầu ra:</strong> 1.00000
<strong>Giải thích:</strong> Hình chữ nhật có diện tích nhỏ nhất được tạo từ các điểm [1,0],[1,1],[2,1],[2,0], với diện tích bằng 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0963.Minimum%20Area%20Rectangle%20II/images/3.png" style="width: 383px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> points = [[0,3],[1,2],[3,1],[1,3],[2,1]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể tạo thành hình chữ nhật nào từ các điểm này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 50</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 4 * 10<sup>4</sup></code></li>
	<li>Tất cả các điểm đã cho đều <strong>phân biệt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 50$, ta có thể liệt kê ba đỉnh để tìm hình chữ nhật (có thể bị xoay) có diện tích nhỏ nhất. Nếu góc vuông nằm tại $p_1$ và $\overrightarrow{p_1p_2}\perp\overrightarrow{p_1p_3}$, đỉnh thứ tư được xác định bằng tổng hai vector. Dùng hash set để kiểm tra sự tồn tại của điểm đó trong $O(1)$; diện tích bằng tích độ dài hai cạnh.

<!-- thinking:end -->

Ta dùng hash table để lưu tất cả các điểm, sau đó liệt kê ba điểm $p_1 = (x_1, y_1)$, $p_2 = (x_2, y_2)$, $p_3 = (x_3, y_3)$, trong đó $p_2$ và $p_3$ là hai đầu mút của một đường chéo hình chữ nhật. Nếu đường thẳng qua $p_1$ và $p_2$ vuông góc với đường thẳng qua $p_1$ và $p_3$, đồng thời điểm thứ tư $(x_4, y_4)=(x_2 - x_1 + x_3, y_2 - y_1 + y_3)$ có trong hash table, thì ta tìm được một hình chữ nhật. Khi đó, ta tính diện tích hình chữ nhật và cập nhật đáp án.

Cuối cùng, nếu tìm được các hình chữ nhật thỏa mãn điều kiện, trả về diện tích nhỏ nhất trong số đó. Nếu không, trả về $0$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số điểm trong mảng $\textit{points}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAreaFreeRect(self, points: List[List[int]]) -> float:
        s = {(x, y) for x, y in points}
        n = len(points)
        ans = inf
        for i in range(n):
            x1, y1 = points[i]
            for j in range(n):
                if j != i:
                    x2, y2 = points[j]
                    for k in range(j + 1, n):
                        if k != i:
                            x3, y3 = points[k]
                            x4 = x2 - x1 + x3
                            y4 = y2 - y1 + y3
                            if (x4, y4) in s:
                                v21 = (x2 - x1, y2 - y1)
                                v31 = (x3 - x1, y3 - y1)
                                if v21[0] * v31[0] + v21[1] * v31[1] == 0:
                                    w = sqrt(v21[0] ** 2 + v21[1] ** 2)
                                    h = sqrt(v31[0] ** 2 + v31[1] ** 2)
                                    ans = min(ans, w * h)
        return 0 if ans == inf else ans
```

#### Java

```java
class Solution {
    public double minAreaFreeRect(int[][] points) {
        int n = points.length;
        Set<Integer> s = new HashSet<>(n);
        for (int[] p : points) {
            s.add(f(p[0], p[1]));
        }
        double ans = Double.MAX_VALUE;
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = 0; j < n; ++j) {
                if (j != i) {
                    int x2 = points[j][0], y2 = points[j][1];
                    for (int k = j + 1; k < n; ++k) {
                        if (k != i) {
                            int x3 = points[k][0], y3 = points[k][1];
                            int x4 = x2 - x1 + x3, y4 = y2 - y1 + y3;
                            if (s.contains(f(x4, y4))) {
                                if ((x2 - x1) * (x3 - x1) + (y2 - y1) * (y3 - y1) == 0) {
                                    int ww = (x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1);
                                    int hh = (x3 - x1) * (x3 - x1) + (y3 - y1) * (y3 - y1);
                                    ans = Math.min(ans, Math.sqrt(1L * ww * hh));
                                }
                            }
                        }
                    }
                }
            }
        }
        return ans == Double.MAX_VALUE ? 0 : ans;
    }

    private int f(int x, int y) {
        return x * 40001 + y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double minAreaFreeRect(vector<vector<int>>& points) {
        auto f = [](int x, int y) {
            return x * 40001 + y;
        };
        int n = points.size();
        unordered_set<int> s;
        for (auto& p : points) {
            s.insert(f(p[0], p[1]));
        }
        double ans = 1e20;
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = 0; j < n; ++j) {
                if (j != i) {
                    int x2 = points[j][0], y2 = points[j][1];
                    for (int k = j + 1; k < n; ++k) {
                        if (k != i) {
                            int x3 = points[k][0], y3 = points[k][1];
                            int x4 = x2 - x1 + x3, y4 = y2 - y1 + y3;
                            if (x4 >= 0 && x4 < 40000 && y4 >= 0 && y4 <= 40000 && s.count(f(x4, y4))) {
                                if ((x2 - x1) * (x3 - x1) + (y2 - y1) * (y3 - y1) == 0) {
                                    int ww = (x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1);
                                    int hh = (x3 - x1) * (x3 - x1) + (y3 - y1) * (y3 - y1);
                                    ans = min(ans, sqrt(1LL * ww * hh));
                                }
                            }
                        }
                    }
                }
            }
        }
        return ans == 1e20 ? 0 : ans;
    }
};
```

#### Go

```go
func minAreaFreeRect(points [][]int) float64 {
	n := len(points)
	f := func(x, y int) int {
		return x*40001 + y
	}
	s := map[int]bool{}
	for _, p := range points {
		s[f(p[0], p[1])] = true
	}
	ans := 1e20
	for i := 0; i < n; i++ {
		x1, y1 := points[i][0], points[i][1]
		for j := 0; j < n; j++ {
			if j != i {
				x2, y2 := points[j][0], points[j][1]
				for k := j + 1; k < n; k++ {
					if k != i {
						x3, y3 := points[k][0], points[k][1]
						x4, y4 := x2-x1+x3, y2-y1+y3
						if s[f(x4, y4)] {
							if (x2-x1)*(x3-x1)+(y2-y1)*(y3-y1) == 0 {
								ww := (x2-x1)*(x2-x1) + (y2-y1)*(y2-y1)
								hh := (x3-x1)*(x3-x1) + (y3-y1)*(y3-y1)
								ans = math.Min(ans, math.Sqrt(float64(ww*hh)))
							}
						}
					}
				}
			}
		}
	}
	if ans == 1e20 {
		return 0
	}
	return ans
}
```

#### TypeScript

```ts
function minAreaFreeRect(points: number[][]): number {
    const n = points.length;
    const f = (x: number, y: number): number => x * 40001 + y;
    const s: Set<number> = new Set();
    for (const [x, y] of points) {
        s.add(f(x, y));
    }
    let ans = Number.MAX_VALUE;
    for (let i = 0; i < n; ++i) {
        const [x1, y1] = points[i];
        for (let j = 0; j < n; ++j) {
            if (j !== i) {
                const [x2, y2] = points[j];
                for (let k = j + 1; k < n; ++k) {
                    if (k !== i) {
                        const [x3, y3] = points[k];
                        const x4 = x2 - x1 + x3;
                        const y4 = y2 - y1 + y3;
                        if (s.has(f(x4, y4))) {
                            if ((x2 - x1) * (x3 - x1) + (y2 - y1) * (y3 - y1) === 0) {
                                const ww = (x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1);
                                const hh = (x3 - x1) * (x3 - x1) + (y3 - y1) * (y3 - y1);
                                ans = Math.min(ans, Math.sqrt(ww * hh));
                            }
                        }
                    }
                }
            }
        }
    }
    return ans === Number.MAX_VALUE ? 0 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
