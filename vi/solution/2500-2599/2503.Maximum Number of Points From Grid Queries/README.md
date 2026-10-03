---
comments: true
difficulty: Hard
rating: 2195
source: Weekly Contest 323 Q4
tags:
    - Breadth-First Search
    - Union Find
    - Array
    - Two Pointers
    - Matrix
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2503. Maximum Number of Points From Grid Queries](https://leetcode.com/problems/maximum-number-of-points-from-grid-queries)

[中文文档](/solution/2500-2599/2503.Maximum%20Number%20of%20Points%20From%20Grid%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên <code>m x n</code> <code>grid</code> và một mảng <code>queries</code> có kích thước <code>k</code>.</p>

<p>Hãy tìm một mảng <code>answer</code> có kích thước <code>k</code> sao cho với mỗi số nguyên <code>queries[i]</code>, bạn bắt đầu tại ô <strong>trên cùng bên trái</strong> của ma trận và lặp lại quy trình sau:</p>

<ul>
	<li>Nếu <code>queries[i]</code> <strong>lớn hơn nghiêm ngặt</strong> giá trị của ô hiện tại, bạn nhận được một điểm nếu đây là lần đầu tiên bạn thăm ô này, đồng thời có thể di chuyển đến bất kỳ ô <strong>kề</strong> nào theo cả <code>4</code> hướng: lên, xuống, trái và phải.</li>
	<li>Ngược lại, bạn không nhận được điểm nào và kết thúc quy trình.</li>
</ul>

<p>Sau quy trình, <code>answer[i]</code> là số điểm <strong>lớn nhất</strong> bạn có thể nhận được. <strong>Lưu ý</strong> rằng với mỗi truy vấn, bạn được phép đi qua cùng một ô <strong>nhiều lần</strong>.</p>

<p>Trả về <em>mảng kết quả</em> <code>answer</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2503.Maximum%20Number%20of%20Points%20From%20Grid%20Queries/images/image1.png" style="width: 571px; height: 152px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,3],[2,5,7],[3,5,1]], queries = [5,6,2]
<strong>Đầu ra:</strong> [5,8,1]
<strong>Giải thích:</strong> Các sơ đồ phía trên cho biết những ô được chúng ta đi qua để nhận điểm.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2503.Maximum%20Number%20of%20Points%20From%20Grid%20Queries/images/yetgriddrawio-2.png" />
<pre>
<strong>Đầu vào:</strong> grid = [[5,2,1],[1,1,2]], queries = [3]
<strong>Đầu ra:</strong> [0]
<strong>Giải thích:</strong> Chúng ta không thể nhận được điểm nào vì giá trị của ô trên cùng bên trái đã lớn hơn hoặc bằng 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 1000</code></li>
	<li><code>4 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>k == queries.length</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= grid[i][j], queries[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy vấn offline + BFS + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn bắt đầu tại ô trên cùng bên trái và chỉ có thể đi vào các ô có giá trị nhỏ hơn nghiêm ngặt giá trị truy vấn. Nếu tìm kiếm lại từ đầu cho mỗi truy vấn, ta sẽ phải duyệt lại phần lớn ma trận khi $k$ lớn.
>
> Các truy vấn độc lập và đơn điệu theo ngưỡng: giá trị lớn hơn chỉ có thể mở rộng tập ô có thể đi đến. Ta sắp xếp các truy vấn offline và mở rộng frontier bằng min-heap; khi ngưỡng tăng, lấy ra mọi ô nhỏ hơn ngưỡng rồi mở rộng bốn ô lân cận. Mỗi ô chỉ được đưa vào heap một lần, còn các đáp án được ghi lại theo thứ tự ban đầu.

<!-- thinking:end -->

Theo mô tả đề bài, mỗi truy vấn là độc lập, thứ tự của các truy vấn không ảnh hưởng đến kết quả, và mỗi lần ta phải bắt đầu từ góc trên bên trái, đếm số ô có thể đi đến với giá trị nhỏ hơn giá trị của truy vấn hiện tại.

Do đó, trước hết ta có thể sắp xếp mảng `queries`, sau đó xử lý từng truy vấn theo thứ tự tăng dần.

Ta sử dụng một priority queue (min heap) để duy trì ô có giá trị nhỏ nhất trong số các ô hiện đã đi đến, đồng thời dùng một mảng hoặc hash table `vis` để ghi lại ô hiện tại đã được thăm hay chưa. Ban đầu, ta thêm dữ liệu $(grid[0][0], 0, 0)$ của ô trên cùng bên trái dưới dạng một tuple vào priority queue, đồng thời đặt `vis[0][0]` thành `True`.

Với mỗi truy vấn `queries[i]`, ta kiểm tra xem giá trị nhỏ nhất trong priority queue hiện tại có nhỏ hơn `queries[i]` hay không. Nếu có, ta lấy phần tử nhỏ nhất hiện tại ra, tăng biến đếm `cnt`, rồi thêm bốn ô phía trên, phía dưới, bên trái và bên phải của ô hiện tại vào priority queue, đồng thời kiểm tra xem chúng đã được thăm hay chưa. Lặp lại thao tác trên cho đến khi giá trị nhỏ nhất trong priority queue lớn hơn hoặc bằng `queries[i]`; khi đó, `cnt` là đáp án cho truy vấn hiện tại.

Độ phức tạp thời gian là $O(k \times \log k + m \times n \log(m \times n))$, còn độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận, còn $k$ là số truy vấn. Ta cần sắp xếp mảng `queries`, có độ phức tạp thời gian là $O(k \times \log k)$. Mỗi ô trong ma trận được thăm nhiều nhất một lần, còn độ phức tạp thời gian của mỗi thao tác thêm và lấy phần tử là $O(\log(m \times n))$. Vì vậy, tổng độ phức tạp thời gian là $O(k \times \log k + m \times n \log(m \times n))$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPoints(self, grid: List[List[int]], queries: List[int]) -> List[int]:
        m, n = len(grid), len(grid[0])
        qs = sorted((v, i) for i, v in enumerate(queries))
        ans = [0] * len(qs)
        q = [(grid[0][0], 0, 0)]
        cnt = 0
        vis = [[False] * n for _ in range(m)]
        vis[0][0] = True
        for v, k in qs:
            while q and q[0][0] < v:
                _, i, j = heappop(q)
                cnt += 1
                for a, b in pairwise((-1, 0, 1, 0, -1)):
                    x, y = i + a, j + b
                    if 0 <= x < m and 0 <= y < n and not vis[x][y]:
                        heappush(q, (grid[x][y], x, y))
                        vis[x][y] = True
            ans[k] = cnt
        return ans
```

#### Java

```java
class Solution {
    public int[] maxPoints(int[][] grid, int[] queries) {
        int k = queries.length;
        int[][] qs = new int[k][2];
        for (int i = 0; i < k; ++i) {
            qs[i] = new int[] {queries[i], i};
        }
        Arrays.sort(qs, (a, b) -> a[0] - b[0]);
        int[] ans = new int[k];
        int m = grid.length, n = grid[0].length;
        boolean[][] vis = new boolean[m][n];
        vis[0][0] = true;
        PriorityQueue<int[]> q = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        q.offer(new int[] {grid[0][0], 0, 0});
        int[] dirs = new int[] {-1, 0, 1, 0, -1};
        int cnt = 0;
        for (var e : qs) {
            int v = e[0];
            k = e[1];
            while (!q.isEmpty() && q.peek()[0] < v) {
                var p = q.poll();
                ++cnt;
                for (int h = 0; h < 4; ++h) {
                    int x = p[1] + dirs[h], y = p[2] + dirs[h + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y]) {
                        vis[x][y] = true;
                        q.offer(new int[] {grid[x][y], x, y});
                    }
                }
            }
            ans[k] = cnt;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int dirs[5] = {-1, 0, 1, 0, -1};

    vector<int> maxPoints(vector<vector<int>>& grid, vector<int>& queries) {
        int k = queries.size();
        vector<pair<int, int>> qs(k);
        for (int i = 0; i < k; ++i) qs[i] = {queries[i], i};
        sort(qs.begin(), qs.end());
        vector<int> ans(k);
        int m = grid.size(), n = grid[0].size();
        bool vis[m][n];
        memset(vis, 0, sizeof vis);
        vis[0][0] = true;
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> q;
        q.push({grid[0][0], 0, 0});
        int cnt = 0;
        for (auto& e : qs) {
            int v = e.first;
            k = e.second;
            while (!q.empty() && get<0>(q.top()) < v) {
                auto [_, i, j] = q.top();
                q.pop();
                ++cnt;
                for (int h = 0; h < 4; ++h) {
                    int x = i + dirs[h], y = j + dirs[h + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y]) {
                        vis[x][y] = true;
                        q.push({grid[x][y], x, y});
                    }
                }
            }
            ans[k] = cnt;
        }
        return ans;
    }
};
```

#### Go

```go
func maxPoints(grid [][]int, queries []int) []int {
	k := len(queries)
	qs := make([]pair, k)
	for i, v := range queries {
		qs[i] = pair{v, i}
	}
	sort.Slice(qs, func(i, j int) bool { return qs[i].v < qs[j].v })
	ans := make([]int, k)
	m, n := len(grid), len(grid[0])
	q := hp{}
	heap.Push(&q, tuple{grid[0][0], 0, 0})
	dirs := []int{-1, 0, 1, 0, -1}
	vis := map[int]bool{0: true}
	cnt := 0
	for _, e := range qs {
		v := e.v
		k = e.i
		for len(q) > 0 && q[0].v < v {
			p := heap.Pop(&q).(tuple)
			i, j := p.i, p.j
			cnt++
			for h := 0; h < 4; h++ {
				x, y := i+dirs[h], j+dirs[h+1]
				if x >= 0 && x < m && y >= 0 && y < n && !vis[x*n+y] {
					vis[x*n+y] = true
					heap.Push(&q, tuple{grid[x][y], x, y})
				}
			}
		}
		ans[k] = cnt
	}
	return ans
}

type pair struct{ v, i int }

type tuple struct{ v, i, j int }
type hp []tuple

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].v < h[j].v }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
