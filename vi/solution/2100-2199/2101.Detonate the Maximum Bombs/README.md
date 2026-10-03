---
comments: true
difficulty: Medium
rating: 1880
source: Biweekly Contest 67 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [2101. Detonate the Maximum Bombs](https://leetcode.com/problems/detonate-the-maximum-bombs)

[中文文档](/solution/2100-2199/2101.Detonate%20the%20Maximum%20Bombs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một danh sách các quả bom. <strong>Phạm vi</strong> của một quả bom được định nghĩa là khu vực có thể cảm nhận được tác động của nó. Khu vực này có dạng <strong>hình tròn</strong> với tâm là vị trí của quả bom.</p>

<p>Các quả bom được biểu diễn bằng một mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>bombs</code>, trong đó <code>bombs[i] = [x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub>]</code>. <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> lần lượt là tọa độ X và Y của vị trí quả bom thứ <code>i<sup>th</sup></code>, còn <code>r<sub>i</sub></code> là <strong>bán kính</strong> phạm vi của nó.</p>

<p>Bạn có thể chọn kích nổ <strong>một</strong> quả bom. Khi một quả bom phát nổ, nó sẽ kích nổ <strong>tất cả các quả bom</strong> nằm trong phạm vi của nó. Các quả bom này tiếp tục kích nổ những quả bom nằm trong phạm vi của chúng.</p>

<p>Với danh sách <code>bombs</code>, hãy trả về <em><strong>số lượng bom lớn nhất</strong> có thể được kích nổ nếu bạn chỉ được phép kích nổ <strong>một</strong> quả bom</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2101.Detonate%20the%20Maximum%20Bombs/images/desmos-eg-3.png" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> bombs = [[2,1,3],[6,1,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Hình trên biểu diễn vị trí và phạm vi của 2 quả bom.
Nếu kích nổ quả bom bên trái, quả bom bên phải sẽ không bị ảnh hưởng.
Nhưng nếu kích nổ quả bom bên phải, cả hai quả bom đều sẽ phát nổ.
Vì vậy, số lượng bom lớn nhất có thể được kích nổ là max(1, 2) = 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2101.Detonate%20the%20Maximum%20Bombs/images/desmos-eg-2.png" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> bombs = [[1,1,5],[10,10,5]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:
</strong>Kích nổ quả bom nào cũng không làm quả bom còn lại phát nổ, nên số lượng bom lớn nhất có thể được kích nổ là 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2101.Detonate%20the%20Maximum%20Bombs/images/desmos-eg1.png" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> bombs = [[1,2,3],[2,3,1],[3,4,2],[4,5,3],[5,6,4]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Quả bom tốt nhất để kích nổ là quả bom 0 vì:
- Quả bom 0 kích nổ các quả bom 1 và 2. Vòng tròn màu đỏ biểu diễn phạm vi của quả bom 0.
- Quả bom 2 kích nổ quả bom 3. Vòng tròn màu xanh dương biểu diễn phạm vi của quả bom 2.
- Quả bom 3 kích nổ quả bom 4. Vòng tròn màu xanh lá biểu diễn phạm vi của quả bom 3.
Do đó, cả 5 quả bom đều được kích nổ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= bombs.length&nbsp;&lt;= 100</code></li>
	<li><code>bombs[i].length == 3</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Vụ nổ lan truyền theo quan hệ “nằm trong phạm vi”, vì vậy ta cần tìm tập hợp các quả bom có thể tiếp cận lớn nhất khi bắt đầu từ một quả bom. Nếu coi quan hệ này là vô hướng, ta sẽ bỏ sót khả năng đi một chiều khi bán kính khác nhau; việc quét lại tất cả các quả bom sau mỗi lần kích nổ cũng lặp lại cùng một phép tính khoảng cách.
>
> Với $n\le 100$, việc xây dựng đồ thị có hướng $g$ từ mọi cặp quả bom có độ phức tạp $O(n^2)$. Sau đó, chuỗi kích nổ trở thành một bài toán tìm kiếm các đỉnh có thể tiếp cận, và BFS giải quyết bài toán này trong $O(n^2)$ cho mỗi đỉnh bắt đầu.
>
> Vì vậy, ta thêm các cạnh bằng cách kiểm tra từng cặp khoảng cách, sau đó chạy BFS từ mỗi quả bom và trả về $n$ ngay khi có một lần tìm kiếm đi qua tất cả các đỉnh.

<!-- thinking:end -->

Ta định nghĩa một mảng $g$ có độ dài $n$, trong đó $g[i]$ biểu diễn chỉ số của tất cả các quả bom có thể được kích nổ bởi quả bom $i$ trong phạm vi phát nổ của nó.

Tiếp theo, ta duyệt qua tất cả các quả bom. Với hai quả bom $(x_1, y_1, r_1)$ và $(x_2, y_2, r_2)$, ta tính khoảng cách giữa chúng là $\textit{dist} = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}$. Nếu $\textit{dist} \leq r_1$, quả bom $i$ có thể kích nổ quả bom $j$ trong phạm vi phát nổ của nó, nên ta thêm $j$ vào $g[i]$. Nếu $\textit{dist} \leq r_2$, quả bom $j$ có thể kích nổ quả bom $i$ trong phạm vi phát nổ của nó, nên ta thêm $i$ vào $g[j]$.

Sau đó, ta duyệt qua tất cả các quả bom. Với mỗi quả bom $k$, ta dùng tìm kiếm theo chiều rộng để tính các chỉ số của tất cả các quả bom có thể được kích nổ bởi quả bom $k$ trong phạm vi phát nổ của nó và ghi nhận chúng. Nếu số lượng quả bom này bằng $n$, ta có thể kích nổ tất cả các quả bom và trả về trực tiếp $n$. Nếu không, ta ghi nhận số lượng quả bom này và trả về giá trị lớn nhất.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số lượng quả bom.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumDetonation(self, bombs: List[List[int]]) -> int:
        n = len(bombs)
        g = [[] for _ in range(n)]
        for i in range(n - 1):
            x1, y1, r1 = bombs[i]
            for j in range(i + 1, n):
                x2, y2, r2 = bombs[j]
                dist = hypot(x1 - x2, y1 - y2)
                if dist <= r1:
                    g[i].append(j)
                if dist <= r2:
                    g[j].append(i)
        ans = 0
        for k in range(n):
            vis = {k}
            q = [k]
            for i in q:
                for j in g[i]:
                    if j not in vis:
                        vis.add(j)
                        q.append(j)
            if len(vis) == n:
                return n
            ans = max(ans, len(vis))
        return ans
```

#### Java

```java
class Solution {
    public int maximumDetonation(int[][] bombs) {
        int n = bombs.length;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < n - 1; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int[] p1 = bombs[i], p2 = bombs[j];
                double dist = Math.hypot(p1[0] - p2[0], p1[1] - p2[1]);
                if (dist <= p1[2]) {
                    g[i].add(j);
                }
                if (dist <= p2[2]) {
                    g[j].add(i);
                }
            }
        }
        int ans = 0;
        boolean[] vis = new boolean[n];
        for (int k = 0; k < n; ++k) {
            Arrays.fill(vis, false);
            vis[k] = true;
            int cnt = 0;
            Deque<Integer> q = new ArrayDeque<>();
            q.offer(k);
            while (!q.isEmpty()) {
                int i = q.poll();
                ++cnt;
                for (int j : g[i]) {
                    if (!vis[j]) {
                        vis[j] = true;
                        q.offer(j);
                    }
                }
            }
            if (cnt == n) {
                return n;
            }
            ans = Math.max(ans, cnt);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumDetonation(vector<vector<int>>& bombs) {
        int n = bombs.size();
        vector<int> g[n];
        for (int i = 0; i < n - 1; ++i) {
            for (int j = i + 1; j < n; ++j) {
                auto& p1 = bombs[i];
                auto& p2 = bombs[j];
                auto dist = hypot(p1[0] - p2[0], p1[1] - p2[1]);
                if (dist <= p1[2]) {
                    g[i].push_back(j);
                }
                if (dist <= p2[2]) {
                    g[j].push_back(i);
                }
            }
        }
        int ans = 0;
        bool vis[n];
        for (int k = 0; k < n; ++k) {
            memset(vis, false, sizeof(vis));
            queue<int> q;
            q.push(k);
            vis[k] = true;
            int cnt = 0;
            while (!q.empty()) {
                int i = q.front();
                q.pop();
                ++cnt;
                for (int j : g[i]) {
                    if (!vis[j]) {
                        vis[j] = true;
                        q.push(j);
                    }
                }
            }
            if (cnt == n) {
                return n;
            }
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumDetonation(bombs [][]int) (ans int) {
	n := len(bombs)
	g := make([][]int, n)
	for i, p1 := range bombs[:n-1] {
		for j := i + 1; j < n; j++ {
			p2 := bombs[j]
			dist := math.Hypot(float64(p1[0]-p2[0]), float64(p1[1]-p2[1]))
			if dist <= float64(p1[2]) {
				g[i] = append(g[i], j)
			}
			if dist <= float64(p2[2]) {
				g[j] = append(g[j], i)
			}
		}
	}
	for k := 0; k < n; k++ {
		q := []int{k}
		vis := make([]bool, n)
		vis[k] = true
		cnt := 0
		for len(q) > 0 {
			i := q[0]
			q = q[1:]
			cnt++
			for _, j := range g[i] {
				if !vis[j] {
					vis[j] = true
					q = append(q, j)
				}
			}
		}
		if cnt == n {
			return n
		}
		ans = max(ans, cnt)
	}
	return
}
```

#### TypeScript

```ts
function maximumDetonation(bombs: number[][]): number {
    const n = bombs.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (let i = 0; i < n - 1; ++i) {
        const [x1, y1, r1] = bombs[i];
        for (let j = i + 1; j < n; ++j) {
            const [x2, y2, r2] = bombs[j];
            const d = Math.hypot(x1 - x2, y1 - y2);
            if (d <= r1) {
                g[i].push(j);
            }
            if (d <= r2) {
                g[j].push(i);
            }
        }
    }
    let ans = 0;
    for (let k = 0; k < n; ++k) {
        const vis: Set<number> = new Set([k]);
        const q: number[] = [k];
        for (const i of q) {
            for (const j of g[i]) {
                if (!vis.has(j)) {
                    vis.add(j);
                    q.push(j);
                }
            }
        }
        if (vis.size === n) {
            return n;
        }
        ans = Math.max(ans, vis.size);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
