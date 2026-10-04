---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [3235. Check if the Rectangle Corner Is Reachable](https://leetcode.com/problems/check-if-the-rectangle-corner-is-reachable)

[中文文档](/solution/3200-3299/3235.Check%20if%20the%20Rectangle%20Corner%20Is%20Reachable/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>xCorner</code> và <code>yCorner</code>, cùng một mảng 2 chiều <code>circles</code>, trong đó <code>circles[i] = [x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub>]</code> biểu diễn một đường tròn có tâm tại <code>(x<sub>i</sub>, y<sub>i</sub>)</code> và bán kính <code>r<sub>i</sub></code>.</p>

<p>Trong mặt phẳng tọa độ có một hình chữ nhật với góc dưới bên trái tại gốc tọa độ và góc trên bên phải tại tọa độ <code>(xCorner, yCorner)</code>. Hãy kiểm tra xem có tồn tại một đường đi từ góc dưới bên trái đến góc trên bên phải sao cho <strong>toàn bộ đường đi</strong> nằm trong hình chữ nhật, <strong>không</strong> chạm hoặc nằm trong <strong>bất kỳ</strong> đường tròn nào, và chỉ chạm vào hình chữ nhật <strong>tại</strong> hai góc hay không.</p>

<p>Trả về <code>true</code> nếu tồn tại đường đi như vậy, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">xCorner = 3, yCorner = 4, circles = [[2,1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3235.Check%20if%20the%20Rectangle%20Corner%20Is%20Reachable/images/example2circle1.png" style="width: 346px; height: 264px;" /></p>

<p>Đường cong màu đen biểu diễn một đường đi khả thi giữa <code>(0, 0)</code> và <code>(3, 4)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">xCorner = 3, yCorner = 3, circles = [[1,1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3235.Check%20if%20the%20Rectangle%20Corner%20Is%20Reachable/images/example1circle.png" style="width: 346px; height: 264px;" /></p>

<p>Không tồn tại đường đi từ <code>(0, 0)</code> đến <code>(3, 3)</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">xCorner = 3, yCorner = 3, circles = [[2,1,1],[1,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3235.Check%20if%20the%20Rectangle%20Corner%20Is%20Reachable/images/example0circle.png" style="width: 346px; height: 264px;" /></p>

<p>Không tồn tại đường đi từ <code>(0, 0)</code> đến <code>(3, 3)</code>.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">xCorner = 4, yCorner = 4, circles = [[5,5,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3235.Check%20if%20the%20Rectangle%20Corner%20Is%20Reachable/images/rectangles.png" style="width: 346px; height: 264px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= xCorner, yCorner &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= circles.length &lt;= 1000</code></li>
	<li><code>circles[i].length == 3</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Ta đi từ $(0,0)$ đến góc đối diện mà không chạm vào đường tròn nào. Tọa độ có thể lên đến $10^9$ nên không thể dùng lưới; chỉ có nhiều nhất $10^3$ đường tròn, và chướng ngại vật được biểu diễn bởi đồ thị giao nhau của chúng trong hình chữ nhật.
>
> Nếu điểm đầu hoặc điểm cuối nằm trong một đường tròn thì ta thất bại. Các đường tròn giao nhau bên trong hình chữ nhật, đồng thời chạm cả “bên trái hoặc phía trên” và “bên phải hoặc phía dưới”, sẽ chặn đường đi. Ta DFS từ một đường tròn chạm bên trái/phía trên, đi theo các cạnh biểu diễn giao nhau bên trong hình chữ nhật; nếu đến được một đường tròn chạm bên phải/phía dưới thì thất bại. Hình học quyết định liệu điểm giao có nằm bên trong hay không.

<!-- thinking:end -->

Theo mô tả bài toán, ta xét các trường hợp sau:

Khi chỉ có một đường tròn trong `circles`:

1. Nếu điểm bắt đầu $(0, 0)$ nằm trong đường tròn (bao gồm cả biên), hoặc điểm kết thúc $(\textit{xCorner}, \textit{yCorner})$ nằm trong đường tròn, thì không thể thỏa mãn điều kiện "không chạm vào đường tròn".
2. Nếu đường tròn giao với cạnh trái hoặc cạnh trên của hình chữ nhật, đồng thời giao với cạnh phải hoặc cạnh dưới của hình chữ nhật, thì đường tròn sẽ chặn đường đi từ góc dưới bên trái đến góc trên bên phải của hình chữ nhật, khiến ta không thể thỏa mãn điều kiện "không chạm vào đường tròn".

Khi có nhiều đường tròn trong `circles`:

1. Tương tự trường hợp trên, nếu điểm bắt đầu hoặc điểm kết thúc nằm trong một đường tròn, thì không thể thỏa mãn điều kiện "không chạm vào đường tròn".
2. Nếu có nhiều đường tròn, chúng có thể giao nhau bên trong hình chữ nhật và tạo thành một vùng chướng ngại vật lớn hơn. Chỉ cần vùng chướng ngại vật này giao với cạnh trái hoặc cạnh trên của hình chữ nhật, đồng thời giao với cạnh phải hoặc cạnh dưới của hình chữ nhật, thì không thể thỏa mãn điều kiện "không chạm vào đường tròn". Nếu vùng giao nhau không nằm trong hình chữ nhật, ta không thể gộp chúng vì vùng giao nhau không thể chặn đường đi bên trong hình chữ nhật. Ngoài ra, nếu một phần vùng giao nhau nằm trong hình chữ nhật và một phần nằm ngoài, các đường tròn này có thể được dùng làm điểm bắt đầu hoặc điểm kết thúc, đồng thời có thể được gộp hoặc không. Ta chỉ cần chọn một trong các điểm giao nhau. Nếu điểm này nằm trong hình chữ nhật, ta có thể gộp các đường tròn.

Dựa trên phân tích trên, ta duyệt qua tất cả các đường tròn. Với đường tròn hiện tại, nếu điểm bắt đầu hoặc điểm kết thúc nằm trong đường tròn, ta trả về `false`. Ngược lại, nếu đường tròn này chưa được thăm và giao với cạnh trái hoặc cạnh trên của hình chữ nhật, ta bắt đầu tìm kiếm theo chiều sâu (DFS) từ đường tròn này. Trong quá trình tìm kiếm, nếu tìm thấy một đường tròn giao với cạnh phải hoặc cạnh dưới của hình chữ nhật, điều đó có nghĩa là vùng chướng ngại vật tạo bởi các đường tròn đã chặn đường đi từ góc dưới bên trái đến góc trên bên phải của hình chữ nhật, và ta trả về `false`.

Ta định nghĩa $\textit{dfs}(i)$ là bắt đầu một DFS từ đường tròn thứ $i$. Nếu tìm thấy một đường tròn giao với cạnh phải hoặc cạnh dưới của hình chữ nhật, ta trả về `true`; ngược lại trả về `false`.

Quá trình thực thi hàm $\textit{dfs}(i)$ như sau:

1. Nếu đường tròn hiện tại giao với cạnh phải hoặc cạnh dưới của hình chữ nhật, trả về `true`;
2. Ngược lại, đánh dấu đường tròn hiện tại đã được thăm;
3. Tiếp theo, duyệt qua tất cả các đường tròn khác. Nếu đường tròn $j$ chưa được thăm, đường tròn $i$ giao với đường tròn $j$, và một trong các điểm giao của hai đường tròn nằm trong hình chữ nhật, tiếp tục DFS từ đường tròn $j$. Nếu tìm thấy một đường tròn giao với cạnh phải hoặc cạnh dưới của hình chữ nhật, trả về `true`;
4. Nếu không tìm thấy đường tròn nào như vậy, trả về `false`.

Trong quá trình trên, ta cần xác định hai đường tròn $O_1 = (x_1, y_1, r_1)$ và $O_2 = (x_2, y_2, r_2)$ có giao nhau hay không. Nếu khoảng cách giữa tâm hai đường tròn không vượt quá tổng bán kính của chúng, tức là $(x_1 - x_2)^2 + (y_1 - y_2)^2 \le (r_1 + r_2)^2$, thì chúng giao nhau.

Ta cũng cần tìm một điểm giao của hai đường tròn. Chọn một điểm $A = (x, y)$ sao cho $\frac{O_1 A}{O_1 O_2} = \frac{r_1}{r_1 + r_2}$. Nếu hai đường tròn giao nhau, điểm $A$ chắc chắn nằm trong vùng giao nhau. Khi đó, $\frac{x - x_1}{x_2 - x_1} = \frac{r_1}{r_1 + r_2}$, suy ra $x = \frac{x_1 r_2 + x_2 r_1}{r_1 + r_2}$. Tương tự, $y = \frac{y_1 r_2 + y_2 r_1}{r_1 + r_2}$. Chỉ cần điểm này nằm trong hình chữ nhật, ta có thể tiếp tục DFS, thỏa mãn:

$$
\begin{cases}
x_1 r_2 + x_2 r_1 < (r_1 + r_2) \times \textit{xCorner} \\
y_1 r_2 + y_2 r_1 < (r_1 + r_2) \times \textit{yCorner}
\end{cases}
$$

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số đường tròn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canReachCorner(
        self, xCorner: int, yCorner: int, circles: List[List[int]]
    ) -> bool:
        def in_circle(x: int, y: int, cx: int, cy: int, r: int) -> int:
            return (x - cx) ** 2 + (y - cy) ** 2 <= r**2

        def cross_left_top(cx: int, cy: int, r: int) -> bool:
            a = abs(cx) <= r and 0 <= cy <= yCorner
            b = abs(cy - yCorner) <= r and 0 <= cx <= xCorner
            return a or b

        def cross_right_bottom(cx: int, cy: int, r: int) -> bool:
            a = abs(cx - xCorner) <= r and 0 <= cy <= yCorner
            b = abs(cy) <= r and 0 <= cx <= xCorner
            return a or b

        def dfs(i: int) -> bool:
            x1, y1, r1 = circles[i]
            if cross_right_bottom(x1, y1, r1):
                return True
            vis[i] = True
            for j, (x2, y2, r2) in enumerate(circles):
                if vis[j] or not ((x1 - x2) ** 2 + (y1 - y2) ** 2 <= (r1 + r2) ** 2):
                    continue
                if (
                    (x1 * r2 + x2 * r1 < (r1 + r2) * xCorner)
                    and (y1 * r2 + y2 * r1 < (r1 + r2) * yCorner)
                    and dfs(j)
                ):
                    return True
            return False

        vis = [False] * len(circles)
        for i, (x, y, r) in enumerate(circles):
            if in_circle(0, 0, x, y, r) or in_circle(xCorner, yCorner, x, y, r):
                return False
            if (not vis[i]) and cross_left_top(x, y, r) and dfs(i):
                return False
        return True
```

#### Java

```java
class Solution {
    private int[][] circles;
    private int xCorner, yCorner;
    private boolean[] vis;

    public boolean canReachCorner(int xCorner, int yCorner, int[][] circles) {
        int n = circles.length;
        this.circles = circles;
        this.xCorner = xCorner;
        this.yCorner = yCorner;
        vis = new boolean[n];
        for (int i = 0; i < n; ++i) {
            var c = circles[i];
            int x = c[0], y = c[1], r = c[2];
            if (inCircle(0, 0, x, y, r) || inCircle(xCorner, yCorner, x, y, r)) {
                return false;
            }
            if (!vis[i] && crossLeftTop(x, y, r) && dfs(i)) {
                return false;
            }
        }
        return true;
    }

    private boolean inCircle(long x, long y, long cx, long cy, long r) {
        return (x - cx) * (x - cx) + (y - cy) * (y - cy) <= r * r;
    }

    private boolean crossLeftTop(long cx, long cy, long r) {
        boolean a = Math.abs(cx) <= r && (cy >= 0 && cy <= yCorner);
        boolean b = Math.abs(cy - yCorner) <= r && (cx >= 0 && cx <= xCorner);
        return a || b;
    }

    private boolean crossRightBottom(long cx, long cy, long r) {
        boolean a = Math.abs(cx - xCorner) <= r && (cy >= 0 && cy <= yCorner);
        boolean b = Math.abs(cy) <= r && (cx >= 0 && cx <= xCorner);
        return a || b;
    }

    private boolean dfs(int i) {
        var c = circles[i];
        long x1 = c[0], y1 = c[1], r1 = c[2];
        if (crossRightBottom(x1, y1, r1)) {
            return true;
        }
        vis[i] = true;
        for (int j = 0; j < circles.length; ++j) {
            var c2 = circles[j];
            long x2 = c2[0], y2 = c2[1], r2 = c2[2];
            if (vis[j]) {
                continue;
            }
            if ((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2) > (r1 + r2) * (r1 + r2)) {
                continue;
            }
            if (x1 * r2 + x2 * r1 < (r1 + r2) * xCorner && y1 * r2 + y2 * r1 < (r1 + r2) * yCorner
                && dfs(j)) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canReachCorner(int xCorner, int yCorner, vector<vector<int>>& circles) {
        using ll = long long;
        auto inCircle = [&](ll x, ll y, ll cx, ll cy, ll r) {
            return (x - cx) * (x - cx) + (y - cy) * (y - cy) <= r * r;
        };
        auto crossLeftTop = [&](ll cx, ll cy, ll r) {
            bool a = abs(cx) <= r && (cy >= 0 && cy <= yCorner);
            bool b = abs(cy - yCorner) <= r && (cx >= 0 && cx <= xCorner);
            return a || b;
        };
        auto crossRightBottom = [&](ll cx, ll cy, ll r) {
            bool a = abs(cx - xCorner) <= r && (cy >= 0 && cy <= yCorner);
            bool b = abs(cy) <= r && (cx >= 0 && cx <= xCorner);
            return a || b;
        };

        int n = circles.size();
        vector<bool> vis(n);
        auto dfs = [&](this auto&& dfs, int i) -> bool {
            auto c = circles[i];
            ll x1 = c[0], y1 = c[1], r1 = c[2];
            if (crossRightBottom(x1, y1, r1)) {
                return true;
            }
            vis[i] = true;
            for (int j = 0; j < n; ++j) {
                if (vis[j]) {
                    continue;
                }
                auto c2 = circles[j];
                ll x2 = c2[0], y2 = c2[1], r2 = c2[2];
                if ((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2) > (r1 + r2) * (r1 + r2)) {
                    continue;
                }
                if (x1 * r2 + x2 * r1 < (r1 + r2) * xCorner && y1 * r2 + y2 * r1 < (r1 + r2) * yCorner
                    && dfs(j)) {
                    return true;
                }
            }
            return false;
        };

        for (int i = 0; i < n; ++i) {
            auto c = circles[i];
            ll x = c[0], y = c[1], r = c[2];
            if (inCircle(0, 0, x, y, r) || inCircle(xCorner, yCorner, x, y, r)) {
                return false;
            }
            if (!vis[i] && crossLeftTop(x, y, r) && dfs(i)) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canReachCorner(xCorner int, yCorner int, circles [][]int) bool {
	inCircle := func(x, y, cx, cy, r int) bool {
		dx, dy := x-cx, y-cy
		return dx*dx+dy*dy <= r*r
	}

	crossLeftTop := func(cx, cy, r int) bool {
		a := abs(cx) <= r && cy >= 0 && cy <= yCorner
		b := abs(cy-yCorner) <= r && cx >= 0 && cx <= xCorner
		return a || b
	}

	crossRightBottom := func(cx, cy, r int) bool {
		a := abs(cx-xCorner) <= r && cy >= 0 && cy <= yCorner
		b := abs(cy) <= r && cx >= 0 && cx <= xCorner
		return a || b
	}

	vis := make([]bool, len(circles))

	var dfs func(int) bool
	dfs = func(i int) bool {
		c := circles[i]
		x1, y1, r1 := c[0], c[1], c[2]
		if crossRightBottom(x1, y1, r1) {
			return true
		}
		vis[i] = true
		for j, c2 := range circles {
			if vis[j] {
				continue
			}
			x2, y2, r2 := c2[0], c2[1], c2[2]
			if (x1-x2)*(x1-x2)+(y1-y2)*(y1-y2) > (r1+r2)*(r1+r2) {
				continue
			}
			if x1*r2+x2*r1 < (r1+r2)*xCorner && y1*r2+y2*r1 < (r1+r2)*yCorner && dfs(j) {
				return true
			}
		}
		return false
	}

	for i, c := range circles {
		x, y, r := c[0], c[1], c[2]
		if inCircle(0, 0, x, y, r) || inCircle(xCorner, yCorner, x, y, r) {
			return false
		}
		if !vis[i] && crossLeftTop(x, y, r) && dfs(i) {
			return false
		}
	}
	return true
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
function canReachCorner(xCorner: number, yCorner: number, circles: number[][]): boolean {
    const inCircle = (x: bigint, y: bigint, cx: bigint, cy: bigint, r: bigint): boolean => {
        const dx = x - cx;
        const dy = y - cy;
        return dx * dx + dy * dy <= r * r;
    };

    const crossLeftTop = (cx: bigint, cy: bigint, r: bigint): boolean => {
        const a = BigInt(Math.abs(Number(cx))) <= r && cy >= 0n && cy <= BigInt(yCorner);
        const b =
            BigInt(Math.abs(Number(cy - BigInt(yCorner)))) <= r &&
            cx >= 0n &&
            cx <= BigInt(xCorner);
        return a || b;
    };

    const crossRightBottom = (cx: bigint, cy: bigint, r: bigint): boolean => {
        const a =
            BigInt(Math.abs(Number(cx - BigInt(xCorner)))) <= r &&
            cy >= 0n &&
            cy <= BigInt(yCorner);
        const b = BigInt(Math.abs(Number(cy))) <= r && cx >= 0n && cx <= BigInt(xCorner);
        return a || b;
    };

    const n = circles.length;
    const vis: boolean[] = new Array(n).fill(false);

    const dfs = (i: number): boolean => {
        const [x1, y1, r1] = circles[i].map(BigInt);
        if (crossRightBottom(x1, y1, r1)) {
            return true;
        }
        vis[i] = true;
        for (let j = 0; j < n; j++) {
            if (vis[j]) continue;
            const [x2, y2, r2] = circles[j].map(BigInt);
            if ((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2) > (r1 + r2) * (r1 + r2)) {
                continue;
            }
            if (
                x1 * r2 + x2 * r1 < (r1 + r2) * BigInt(xCorner) &&
                y1 * r2 + y2 * r1 < (r1 + r2) * BigInt(yCorner) &&
                dfs(j)
            ) {
                return true;
            }
        }
        return false;
    };

    for (let i = 0; i < n; i++) {
        const [x, y, r] = circles[i].map(BigInt);
        if (inCircle(0n, 0n, x, y, r) || inCircle(BigInt(xCorner), BigInt(yCorner), x, y, r)) {
            return false;
        }
        if (!vis[i] && crossLeftTop(x, y, r) && dfs(i)) {
            return false;
        }
    }

    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_reach_corner(x_corner: i32, y_corner: i32, circles: Vec<Vec<i32>>) -> bool {
        let n = circles.len();
        let mut vis = vec![false; n];

        let in_circle = |x: i64, y: i64, cx: i64, cy: i64, r: i64| -> bool {
            (x - cx) * (x - cx) + (y - cy) * (y - cy) <= r * r
        };

        let cross_left_top = |cx: i64, cy: i64, r: i64| -> bool {
            let a = cx.abs() <= r && (cy >= 0 && cy <= y_corner as i64);
            let b = (cy - y_corner as i64).abs() <= r && (cx >= 0 && cx <= x_corner as i64);
            a || b
        };

        let cross_right_bottom = |cx: i64, cy: i64, r: i64| -> bool {
            let a = (cx - x_corner as i64).abs() <= r && (cy >= 0 && cy <= y_corner as i64);
            let b = cy.abs() <= r && (cx >= 0 && cx <= x_corner as i64);
            a || b
        };
        fn dfs(
            circles: &Vec<Vec<i32>>,
            vis: &mut Vec<bool>,
            i: usize,
            x_corner: i32,
            y_corner: i32,
            cross_right_bottom: &dyn Fn(i64, i64, i64) -> bool,
        ) -> bool {
            let c = &circles[i];
            let (x1, y1, r1) = (c[0] as i64, c[1] as i64, c[2] as i64);

            if cross_right_bottom(x1, y1, r1) {
                return true;
            }

            vis[i] = true;

            for j in 0..circles.len() {
                if vis[j] {
                    continue;
                }

                let c2 = &circles[j];
                let (x2, y2, r2) = (c2[0] as i64, c2[1] as i64, c2[2] as i64);

                if (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2) > (r1 + r2) * (r1 + r2) {
                    continue;
                }

                if x1 * r2 + x2 * r1 < (r1 + r2) * x_corner as i64
                    && y1 * r2 + y2 * r1 < (r1 + r2) * y_corner as i64
                    && dfs(circles, vis, j, x_corner, y_corner, cross_right_bottom)
                {
                    return true;
                }
            }
            false
        }

        for i in 0..n {
            let c = &circles[i];
            let (x, y, r) = (c[0] as i64, c[1] as i64, c[2] as i64);

            if in_circle(0, 0, x, y, r) || in_circle(x_corner as i64, y_corner as i64, x, y, r) {
                return false;
            }

            if !vis[i]
                && cross_left_top(x, y, r)
                && dfs(
                    &circles,
                    &mut vis,
                    i,
                    x_corner,
                    y_corner,
                    &cross_right_bottom,
                )
            {
                return false;
            }
        }

        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
