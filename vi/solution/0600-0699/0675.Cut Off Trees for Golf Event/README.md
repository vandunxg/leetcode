---
comments: true
difficulty: Hard
tags:
    - Breadth-First Search
    - Array
    - Matrix
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [675. Cut Off Trees for Golf Event](https://leetcode.com/problems/cut-off-trees-for-golf-event)

[中文文档](/solution/0600-0699/0675.Cut%20Off%20Trees%20for%20Golf%20Event/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được yêu cầu đốn tất cả cây trong một khu rừng để tổ chức sự kiện golf. Khu rừng được biểu diễn bằng ma trận <code>m x n</code>. Trong ma trận này:</p>

<ul>
	<li><code>0</code> nghĩa là không thể đi qua ô đó.</li>
	<li><code>1</code> biểu thị ô trống có thể đi qua.</li>
	<li>Số lớn hơn <code>1</code> biểu thị một cây ở ô có thể đi qua; số đó chính là chiều cao của cây.</li>
</ul>

<p>Mỗi bước, bạn có thể đi theo một trong bốn hướng: bắc, đông, nam hoặc tây. Nếu đang đứng ở ô có cây, bạn có thể chọn đốn cây đó.</p>

<p>Bạn phải đốn cây theo thứ tự từ thấp nhất đến cao nhất. Khi đốn một cây, giá trị tại ô đó trở thành <code>1</code> (ô trống).</p>

<p>Bắt đầu từ điểm <code>(0, 0)</code>, hãy trả về <em>số bước ít nhất cần đi để đốn hết tất cả cây</em>. Nếu không thể đốn hết cây, trả về <code>-1</code>.</p>

<p><strong>Lưu ý:</strong> Input được tạo sao cho không có hai cây nào cùng chiều cao và có ít nhất một cây cần được đốn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0675.Cut%20Off%20Trees%20for%20Golf%20Event/images/trees1.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> forest = [[1,2,3],[0,0,4],[7,6,5]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Đi theo đường đi ở trên giúp bạn đốn các cây từ thấp nhất đến cao nhất trong 6 bước.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0675.Cut%20Off%20Trees%20for%20Golf%20Event/images/trees2.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> forest = [[1,2,3],[0,0,0],[7,6,5]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể đến được các cây ở hàng dưới cùng vì hàng giữa bị chặn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> forest = [[2,3,4],[0,0,5],[8,7,6]]
<strong>Đầu ra:</strong> 6
<b>Giải thích:</b> Bạn có thể đi theo đường đi như ở Ví dụ 1 để đốn tất cả cây.
Lưu ý, bạn có thể đốn cây đầu tiên tại (0, 0) trước khi di chuyển bước nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == forest.length</code></li>
	<li><code>n == forest[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>0 &lt;= forest[i][j] &lt;= 10<sup>9</sup></code></li>
	<li>Chiều cao của tất cả cây đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cây phải được đốn theo thứ tự chiều cao tăng dần và không trùng nhau. Tổng quãng đường là tổng độ dài đường đi ngắn nhất giữa các cây liên tiếp cần đến.
>
> Sắp xếp cây theo chiều cao, rồi dùng A* cho từng cặp liên tiếp: key của heap là khoảng cách cộng với ước lượng Manhattan. Lần đầu lấy được đích khỏi heap là đường đi tối ưu; nếu không thể đến đích thì trả về $-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cutOffTree(self, forest: List[List[int]]) -> int:
        def f(i, j, x, y):
            return abs(i - x) + abs(j - y)

        def bfs(i, j, x, y):
            q = [(f(i, j, x, y), i, j)]
            dist = {i * n + j: 0}
            while q:
                _, i, j = heappop(q)
                step = dist[i * n + j]
                if (i, j) == (x, y):
                    return step
                for a, b in [[0, -1], [0, 1], [-1, 0], [1, 0]]:
                    c, d = i + a, j + b
                    if 0 <= c < m and 0 <= d < n and forest[c][d] > 0:
                        if c * n + d not in dist or dist[c * n + d] > step + 1:
                            dist[c * n + d] = step + 1
                            heappush(q, (dist[c * n + d] + f(c, d, x, y), c, d))
            return -1

        m, n = len(forest), len(forest[0])
        trees = [
            (forest[i][j], i, j) for i in range(m) for j in range(n) if forest[i][j] > 1
        ]
        trees.sort()
        i = j = 0
        ans = 0
        for _, x, y in trees:
            t = bfs(i, j, x, y)
            if t == -1:
                return -1
            ans += t
            i, j = x, y
        return ans
```

#### Java

```java
class Solution {
    private int[] dist = new int[3600];
    private List<List<Integer>> forest;
    private int m;
    private int n;

    public int cutOffTree(List<List<Integer>> forest) {
        this.forest = forest;
        m = forest.size();
        n = forest.get(0).size();
        List<int[]> trees = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (forest.get(i).get(j) > 1) {
                    trees.add(new int[] {forest.get(i).get(j), i * n + j});
                }
            }
        }
        trees.sort(Comparator.comparingInt(a -> a[0]));
        int ans = 0;
        int start = 0;
        for (int[] tree : trees) {
            int end = tree[1];
            int t = bfs(start, end);
            if (t == -1) {
                return -1;
            }
            ans += t;
            start = end;
        }
        return ans;
    }

    private int bfs(int start, int end) {
        PriorityQueue<int[]> q = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        q.offer(new int[] {f(start, end), start});
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[start] = 0;
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            int state = q.poll()[1];
            if (state == end) {
                return dist[state];
            }
            for (int k = 0; k < 4; ++k) {
                int x = state / n + dirs[k];
                int y = state % n + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && forest.get(x).get(y) > 0) {
                    if (dist[x * n + y] > dist[state] + 1) {
                        dist[x * n + y] = dist[state] + 1;
                        q.offer(new int[] {dist[x * n + y] + f(x * n + y, end), x * n + y});
                    }
                }
            }
        }
        return -1;
    }

    private int f(int start, int end) {
        int a = start / n;
        int b = start % n;
        int c = end / n;
        int d = end % n;
        return Math.abs(a - c) + Math.abs(b - d);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int m;
    int n;
    vector<int> dist;

    int cutOffTree(vector<vector<int>>& forest) {
        m = forest.size();
        n = forest[0].size();
        dist.resize(3600);
        vector<pair<int, int>> trees;
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (forest[i][j] > 1)
                    trees.push_back({forest[i][j], i * n + j});
        sort(trees.begin(), trees.end());
        int ans = 0;
        int start = 0;
        for (auto& tree : trees) {
            int end = tree.second;
            int t = bfs(start, end, forest);
            if (t == -1) return -1;
            ans += t;
            start = end;
        }
        return ans;
    }

    int bfs(int start, int end, vector<vector<int>>& forest) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;
        q.push({f(start, end), start});
        fill(dist.begin(), dist.end(), INT_MAX);
        dist[start] = 0;
        vector<int> dirs = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            int state = q.top().second;
            q.pop();
            if (state == end) return dist[state];
            for (int k = 0; k < 4; ++k) {
                int x = state / n + dirs[k], y = state % n + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && forest[x][y] && dist[x * n + y] > dist[state] + 1) {
                    dist[x * n + y] = dist[state] + 1;
                    q.push({dist[x * n + y] + f(x * n + y, end), x * n + y});
                }
            }
        }
        return -1;
    }

    int f(int start, int end) {
        int a = start / n, b = start % n;
        int c = end / n, d = end % n;
        return abs(a - c) + abs(b - d);
    }
};
```

#### Go

```go
var dirs = [][]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

type tree struct {
	height int
	pos    int
}

func cutOffTree(forest [][]int) int {
	row, col := len(forest), len(forest[0])
	f := func(a, b int) int {
		return abs(a/col-b/col) + abs(a%col-b%col)
	}
	bfs := func(start, end int) int {
		q := hp{{f(start, end), start}}
		heap.Init(&q)
		dist := map[int]int{start: 0}
		for q.Len() > 0 {
			state := heap.Pop(&q).(pair).pos
			if state == end {
				return dist[state]
			}
			step := dist[state]
			for k := 0; k < 4; k++ {
				x, y := state/col+dirs[k][0], state%col+dirs[k][1]
				if x >= 0 && x < row && y >= 0 && y < col && forest[x][y] != 0 {
					nxt := x*col + y
					if d, ok := dist[nxt]; !ok || d > step+1 {
						dist[nxt] = step + 1
						heap.Push(&q, pair{dist[nxt] + f(nxt, end), nxt})
					}
				}
			}
		}
		return -1
	}

	var trees []tree
	for i := 0; i < row; i++ {
		for j := 0; j < col; j++ {
			if forest[i][j] > 1 {
				trees = append(trees, tree{forest[i][j], i*col + j})
			}
		}
	}
	sort.Slice(trees, func(i, j int) bool {
		return trees[i].height < trees[j].height
	})

	ans, start := 0, 0
	for _, t := range trees {
		step := bfs(start, t.pos)
		if step == -1 {
			return -1
		}
		ans += step
		start = t.pos
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

type pair struct{ prio, pos int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].prio < h[j].prio }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### Rust

```rust
use std::collections::HashSet;
use std::collections::VecDeque;

const DIRS: [[i32; 2]; 4] = [[-1, 0], [1, 0], [0, -1], [0, 1]];

impl Solution {
    pub fn cut_off_tree(forest: Vec<Vec<i32>>) -> i32 {
        let (row, col) = (forest.len() as i32, forest[0].len() as i32);

        let bfs = |start: i32, end: i32| -> i32 {
            let mut queue = VecDeque::new();
            let mut vis = HashSet::new();
            queue.push_back(start);
            vis.insert(start);
            let mut step = 0;
            while !queue.is_empty() {
                let n = queue.len();
                for _ in 0..n {
                    let state = queue.pop_front().unwrap();
                    if state == end {
                        return step;
                    }
                    for k in 0..4 {
                        let x = state / col + DIRS[k][0];
                        let y = (state % col) + DIRS[k][1];
                        let nxt = x * col + y;
                        if x >= 0
                            && x < row
                            && y >= 0
                            && y < col
                            && forest[x as usize][y as usize] != 0
                            && !vis.contains(&nxt)
                        {
                            queue.push_back(nxt);
                            vis.insert(nxt);
                        }
                    }
                }
                step += 1;
            }
            -1
        };

        let mut trees = Vec::new();
        for i in 0..row {
            for j in 0..col {
                let height = forest[i as usize][j as usize];
                if height > 1 {
                    trees.push((height, i * col + j));
                }
            }
        }
        trees.sort();

        let (mut ans, mut start) = (0, 0);
        for t in &trees {
            let end = t.1;
            let step = bfs(start, end);
            if step == -1 {
                return -1;
            }
            ans += step;
            start = end;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
