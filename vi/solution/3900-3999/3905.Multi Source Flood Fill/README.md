---
comments: true
difficulty: Medium
rating: 1671
source: Weekly Contest 498 Q3
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [3905. Multi Source Flood Fill](https://leetcode.com/problems/multi-source-flood-fill)

[中文文档](/solution/3900-3999/3905.Multi%20Source%20Flood%20Fill/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>m</code> lần lượt biểu thị số hàng và số cột của một grid.</p>

<p>Ta cũng có một mảng số nguyên 2D <code>sources</code>, trong đó <code>sources[i] = [r<sub>i</sub>, c<sub>i</sub>, color<sub>​​​​​​​i</sub>]</code> cho biết ô <code>(r<sub>i</sub>, c<sub>i</sub>)</code> ban đầu được tô bằng <code>color<sub>i</sub></code>. Tất cả các ô khác ban đầu chưa được tô và được biểu diễn bằng 0.</p>

<p>Ở mỗi bước thời gian, mọi ô đã được tô hiện tại sẽ lan màu của nó sang tất cả các ô <strong>chưa được tô</strong> kề theo bốn hướng: lên, xuống, trái và phải. Tất cả các lần lan màu diễn ra đồng thời.</p>

<p>Nếu <strong>nhiều</strong> màu cùng đến một ô chưa được tô trong cùng một bước thời gian, ô đó nhận màu có giá trị <strong>lớn nhất</strong>.</p>

<p>Quá trình tiếp tục cho đến khi không còn ô nào có thể được tô.</p>

<p>Hãy trả về một mảng số nguyên 2D biểu diễn trạng thái cuối cùng của grid, trong đó mỗi ô chứa màu cuối cùng của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, m = 3, sources = [[0,0,1],[2,2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,1,2],[1,2,2],[2,2,2]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Grid ở mỗi bước thời gian như sau:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3905.Multi%20Source%20Flood%20Fill/images/g50new.png" style="width: 500px; height: 174px;" />​​​​​​​</p>

<p>Ở bước thời gian 2, các ô <code>(0, 2)</code>, <code>(1, 1)</code> và <code>(2, 0)</code> đều được cả hai màu tiếp cận, nên chúng được gán màu 2 vì đây là giá trị lớn nhất trong hai màu.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, m = 3, sources = [[0,1,3],[1,1,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[3,3,3],[5,5,5],[5,5,5]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Grid ở mỗi bước thời gian như sau:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3905.Multi%20Source%20Flood%20Fill/images/g51new.png" style="width: 500px; height: 177px;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, m = 2, sources = [[1,1,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[5,5],[5,5]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Grid ở mỗi bước thời gian như sau:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3905.Multi%20Source%20Flood%20Fill/images/g52new.png" style="width: 500px; height: 150px;" />​​​​​​​</p>

<p>Vì chỉ có một source nên tất cả các ô đều được gán cùng một màu.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= n * m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= sources.length &lt;= n * m</code></li>
	<li><code>sources[i] = [r<sub>i</sub>, c<sub>i</sub>, color<sub>i</sub>]</code></li>
	<li><code>0 &lt;= r<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>0 &lt;= c<sub>i</sub> &lt;= m - 1</code></li>
	<li><code>1 &lt;= color<sub>i</sub> &lt;= 10<sup>6</sup>​​​​​​​</code></li>
	<li>Tất cả <code>(r<sub>i</sub>, c<sub>i</sub>​​​​​​​)</code> trong <code>sources</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Multi-source BFS

<!-- thinking:start -->

> **Tư duy**
>
> Grid có kích thước $n\cdot m\le 10^5$, nên nếu thực hiện một BFS riêng từ mỗi source thì các ô giao nhau sẽ bị tô lại nhiều lần. Khi nhiều màu cùng đến trong một bước, ta phải giữ lại màu lớn hơn, vì vậy các source cần được mở rộng đồng thời theo từng bước.
>
> Multi-source BFS đưa tất cả source vào queue cùng lúc. Ở mỗi bước thời gian, ta dùng một map $\textit{vis}$ để thu thập các ô mới được tiếp cận, giữ lại màu lớn nhất cho mỗi ô, sau đó ghi các màu này và tiếp tục.
>
> Các ô đã được tô sẽ không được mở rộng lại, nên mỗi ô chỉ được chốt nhiều nhất một lần và tổng thời gian là tuyến tính theo kích thước grid.

<!-- thinking:end -->

Ta có thể dùng multi-source BFS để mô phỏng quá trình này.

Ta định nghĩa một queue $q$ để lưu các ô hiện đang lan màu của chúng. Ban đầu, thêm tất cả các source vào queue và gán màu của chúng trong mảng kết quả $\textit{ans}$.

Ở mỗi lần lặp, ta dùng một hash table $\textit{vis}$ để ghi nhận các ô được thăm trong bước thời gian hiện tại và giá trị màu lớn nhất của mỗi ô. Với mỗi ô trong queue, ta thử lan màu của nó theo bốn hướng kề nhau (lên, xuống, trái, phải). Nếu ô kề chưa được tô, ta thêm nó vào $\textit{vis}$ và cập nhật màu của nó thành giá trị lớn nhất giữa màu của ô hiện tại và màu hiện có trong $\textit{vis}$.

Sau khi xử lý tất cả các ô trong queue hiện tại, ta xóa queue và thêm các ô từ $\textit{vis}$ vào queue, đồng thời cập nhật màu tương ứng của chúng trong mảng kết quả $\textit{ans}$.

Ta lặp lại quá trình này cho đến khi queue rỗng, nghĩa là không còn ô nào có thể được tô.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n \times m)$, trong đó $n$ và $m$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def colorGrid(self, n: int, m: int, sources: list[list[int]]) -> list[list[int]]:
        ans = [[0] * m for _ in range(n)]
        q = sources
        dirs = (-1, 0, 1, 0, -1)
        for r, c, color in q:
            ans[r][c] = color
        while q:
            vis = defaultdict(int)
            for r, c, color in q:
                for a, b in pairwise(dirs):
                    x, y = r + a, c + b
                    if not 0 <= x < n or not 0 <= y < m or ans[x][y]:
                        continue
                    vis[(x, y)] = max(vis[(x, y)], color)
            q.clear()
            for (x, y), color in vis.items():
                q.append((x, y, color))
                ans[x][y] = color
        return ans
```

#### Java

```java
class Solution {
    public int[][] colorGrid(int n, int m, int[][] sources) {
        int[][] ans = new int[n][m];
        List<int[]> q = new ArrayList<>();
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int[] s : sources) {
            ans[s[0]][s[1]] = s[2];
            q.add(new int[] {s[0], s[1], s[2]});
        }
        while (!q.isEmpty()) {
            Map<Long, Integer> vis = new HashMap<>();
            for (int[] curr : q) {
                int r = curr[0], c = curr[1], color = curr[2];
                for (int i = 0; i < 4; i++) {
                    int x = r + dirs[i], y = c + dirs[i + 1];
                    if (x >= 0 && x < n && y >= 0 && y < m && ans[x][y] == 0) {
                        long key = (long) x * m + y;
                        vis.put(key, Math.max(vis.getOrDefault(key, 0), color));
                    }
                }
            }
            q.clear();
            for (Map.Entry<Long, Integer> entry : vis.entrySet()) {
                int x = (int) (entry.getKey() / m);
                int y = (int) (entry.getKey() % m);
                int color = entry.getValue();
                ans[x][y] = color;
                q.add(new int[] {x, y, color});
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> colorGrid(int n, int m, vector<vector<int>>& sources) {
        vector<vector<int>> ans(n, vector<int>(m, 0));
        vector<array<int, 3>> q;
        int dirs[] = {-1, 0, 1, 0, -1};
        for (auto& s : sources) {
            ans[s[0]][s[1]] = s[2];
            q.push_back({s[0], s[1], s[2]});
        }
        while (!q.empty()) {
            unordered_map<long long, int> vis;
            for (auto& curr : q) {
                int r = curr[0], c = curr[1], color = curr[2];
                for (int i = 0; i < 4; i++) {
                    int x = r + dirs[i], y = c + dirs[i + 1];
                    if (x >= 0 && x < n && y >= 0 && y < m && ans[x][y] == 0) {
                        long long key = (long long) x * m + y;
                        if (color > vis[key]) {
                            vis[key] = color;
                        }
                    }
                }
            }
            q.clear();
            for (auto const& [key, color] : vis) {
                int x = key / m;
                int y = key % m;
                ans[x][y] = color;
                q.push_back({x, y, color});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func colorGrid(n int, m int, sources [][]int) [][]int {
	ans := make([][]int, n)
	for i := range ans {
		ans[i] = make([]int, m)
	}
	q := make([][]int, len(sources))
	copy(q, sources)
	dirs := []int{-1, 0, 1, 0, -1}
	for _, s := range q {
		ans[s[0]][s[1]] = s[2]
	}
	for len(q) > 0 {
		vis := make(map[[2]int]int)
		for _, curr := range q {
			r, c, color := curr[0], curr[1], curr[2]
			for i := 0; i < 4; i++ {
				x, y := r+dirs[i], c+dirs[i+1]
				if x >= 0 && x < n && y >= 0 && y < m && ans[x][y] == 0 {
					if color > vis[[2]int{x, y}] {
						vis[[2]int{x, y}] = color
					}
				}
			}
		}
		q = nil
		for pos, color := range vis {
			ans[pos[0]][pos[1]] = color
			q = append(q, []int{pos[0], pos[1], color})
		}
	}
	return ans
}
```

#### TypeScript

```ts
function colorGrid(n: number, m: number, sources: number[][]): number[][] {
    const ans: number[][] = Array.from({ length: n }, () => Array(m).fill(0));
    let q: number[][] = [...sources.map(s => [...s])];
    const dirs = [-1, 0, 1, 0, -1];
    for (const [r, c, color] of q) {
        ans[r][c] = color;
    }
    while (q.length > 0) {
        const vis: Map<string, number> = new Map();
        for (const [r, c, color] of q) {
            for (let i = 0; i < 4; i++) {
                const x = r + dirs[i],
                    y = c + dirs[i + 1];
                if (x >= 0 && x < n && y >= 0 && y < m && ans[x][y] === 0) {
                    const key = `${x},${y}`;
                    vis.set(key, Math.max(vis.get(key) || 0, color));
                }
            }
        }
        q = [];
        for (const [key, color] of vis.entries()) {
            const [x, y] = key.split(',').map(Number);
            ans[x][y] = color;
            q.push([x, y, color]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
