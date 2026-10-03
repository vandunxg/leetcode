---
comments: true
difficulty: Medium
rating: 1782
source: Biweekly Contest 46 Q3
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [1765. Map of Highest Peak](https://leetcode.com/problems/map-of-highest-peak)

[中文文档](/solution/1700-1799/1765.Map%20of%20Highest%20Peak/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ma trận số nguyên <code>isWater</code> kích thước <code>m x n</code>, biểu thị bản đồ gồm các ô <strong>đất</strong> và <strong>nước</strong>.</p>

<ul>
	<li>Nếu <code>isWater[i][j] == 0</code>, ô <code>(i, j)</code> là ô <strong>đất</strong>.</li>
	<li>Nếu <code>isWater[i][j] == 1</code>, ô <code>(i, j)</code> là ô <strong>nước</strong>.</li>
</ul>

<p>Gán độ cao cho mỗi ô theo các quy tắc sau:</p>

<ul>
	<li>Độ cao của mỗi ô phải không âm.</li>
	<li>Nếu ô đó là ô <strong>nước</strong>, độ cao của nó phải bằng <code>0</code>.</li>
	<li>Mọi cặp ô kề nhau phải có chênh lệch độ cao tuyệt đối <strong>không quá</strong> <code>1</code>. Hai ô kề nhau nếu một ô nằm ngay phía bắc, đông, nam hoặc tây của ô kia (tức là hai cạnh của chúng tiếp xúc nhau).</li>
</ul>

<p>Tìm cách gán độ cao sao cho độ cao lớn nhất trong ma trận được <strong>tối đa hóa</strong>.</p>

<p>Trả về <em>ma trận số nguyên </em><code>height</code><em> kích thước </em><code>m x n</code><em>, trong đó </em><code>height[i][j]</code><em> là độ cao của ô </em><code>(i, j)</code><em>. Nếu có nhiều đáp án, trả về <strong>bất kỳ</strong> đáp án nào trong số đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1765.Map%20of%20Highest%20Peak/images/screenshot-2021-01-11-at-82045-am.png" style="width: 220px; height: 219px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> isWater = [[0,1],[0,0]]
<strong>Đầu ra:</strong> [[1,0],[2,1]]
<strong>Giải thích:</strong> Hình ảnh minh họa độ cao được gán cho mỗi ô.
Ô màu xanh dương là ô nước, còn các ô màu xanh lá là ô đất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1765.Map%20of%20Highest%20Peak/images/screenshot-2021-01-11-at-82050-am.png" style="width: 300px; height: 296px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> isWater = [[0,0,1],[1,0,0],[0,0,0]]
<strong>Đầu ra:</strong> [[1,1,0],[0,1,1],[1,2,2]]
<strong>Giải thích:</strong> Độ cao 2 là độ cao lớn nhất có thể của một cách gán.
Mọi cách gán có độ cao lớn nhất bằng 2 và vẫn thỏa mãn các quy tắc đều được chấp nhận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == isWater.length</code></li>
	<li><code>n == isWater[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
	<li><code>isWater[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li>Có ít nhất <strong>một</strong> ô nước.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống bài 542: <a href="https://leetcode.com/problems/01-matrix/description/" target="_blank">https://leetcode.com/problems/01-matrix/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ô nước phải có độ cao $0$, các ô kề nhau chênh lệch không quá $1$, còn ô đất cần cao nhất có thể. Độ cao chính là khoảng cách đến ô nước gần nhất.
>
> Dùng BFS đa nguồn: đưa mọi ô nước vào queue với độ cao $0$, rồi gán cho mỗi ô kề chưa thăm độ cao hiện tại cộng một. Ma trận khoảng cách chính là bản đồ độ cao.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def highestPeak(self, isWater: List[List[int]]) -> List[List[int]]:
        m, n = len(isWater), len(isWater[0])
        ans = [[-1] * n for _ in range(m)]
        q = deque()
        for i, row in enumerate(isWater):
            for j, v in enumerate(row):
                if v:
                    q.append((i, j))
                    ans[i][j] = 0
        while q:
            i, j = q.popleft()
            for a, b in pairwise((-1, 0, 1, 0, -1)):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and ans[x][y] == -1:
                    ans[x][y] = ans[i][j] + 1
                    q.append((x, y))
        return ans
```

#### Java

```java
class Solution {
    public int[][] highestPeak(int[][] isWater) {
        int m = isWater.length, n = isWater[0].length;
        int[][] ans = new int[m][n];
        Deque<int[]> q = new ArrayDeque<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[i][j] = isWater[i][j] - 1;
                if (ans[i][j] == 0) {
                    q.offer(new int[] {i, j});
                }
            }
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            var p = q.poll();
            int i = p[0], j = p[1];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1) {
                    ans[x][y] = ans[i][j] + 1;
                    q.offer(new int[] {x, y});
                }
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
    const int dirs[5] = {-1, 0, 1, 0, -1};

    vector<vector<int>> highestPeak(vector<vector<int>>& isWater) {
        int m = isWater.size(), n = isWater[0].size();
        vector<vector<int>> ans(m, vector<int>(n));
        queue<pair<int, int>> q;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[i][j] = isWater[i][j] - 1;
                if (ans[i][j] == 0) {
                    q.emplace(i, j);
                }
            }
        }
        while (!q.empty()) {
            auto [i, j] = q.front();
            q.pop();
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1) {
                    ans[x][y] = ans[i][j] + 1;
                    q.emplace(x, y);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func highestPeak(isWater [][]int) [][]int {
	m, n := len(isWater), len(isWater[0])
	ans := make([][]int, m)
	type pair struct{ i, j int }
	q := []pair{}
	for i, row := range isWater {
		ans[i] = make([]int, n)
		for j, v := range row {
			ans[i][j] = v - 1
			if v == 1 {
				q = append(q, pair{i, j})
			}
		}
	}
	dirs := []int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		i, j := p.i, p.j
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1 {
				ans[x][y] = ans[i][j] + 1
				q = append(q, pair{x, y})
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function highestPeak(isWater: number[][]): number[][] {
    const m = isWater.length;
    const n = isWater[0].length;
    let ans: number[][] = [];
    let q: number[][] = [];
    for (let i = 0; i < m; ++i) {
        ans.push(new Array(n).fill(-1));
        for (let j = 0; j < n; ++j) {
            if (isWater[i][j]) {
                q.push([i, j]);
                ans[i][j] = 0;
            }
        }
    }
    const dirs = [-1, 0, 1, 0, -1];
    while (q.length) {
        let tq: number[][] = [];
        for (const [i, j] of q) {
            for (let k = 0; k < 4; k++) {
                const [x, y] = [i + dirs[k], j + dirs[k + 1]];
                if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1) {
                    tq.push([x, y]);
                    ans[x][y] = ans[i][j] + 1;
                }
            }
        }
        q = tq;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    #[allow(dead_code)]
    pub fn highest_peak(is_water: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let n = is_water.len();
        let m = is_water[0].len();
        let mut ret_vec = vec![vec![-1; m]; n];
        let mut q: VecDeque<(usize, usize)> = VecDeque::new();
        let vis_pair: Vec<(i32, i32)> = vec![(-1, 0), (1, 0), (0, -1), (0, 1)];

        // Initialize the return vector
        for i in 0..n {
            for j in 0..m {
                if is_water[i][j] == 1 {
                    // This cell is water, the height of which must be 0
                    ret_vec[i][j] = 0;
                    q.push_back((i, j));
                }
            }
        }

        while !q.is_empty() {
            // Get the front X-Y Coordinates
            let (x, y) = q.front().unwrap().clone();
            q.pop_front();
            // Traverse through the vis pair
            for d in &vis_pair {
                let (dx, dy) = *d;
                if Self::check_bounds((x as i32) + dx, (y as i32) + dy, n as i32, m as i32) {
                    if ret_vec[((x as i32) + dx) as usize][((y as i32) + dy) as usize] == -1 {
                        // This cell hasn't been visited, update its height
                        ret_vec[((x as i32) + dx) as usize][((y as i32) + dy) as usize] =
                            ret_vec[x][y] + 1;
                        // Enqueue the current cell
                        q.push_back((((x as i32) + dx) as usize, ((y as i32) + dy) as usize));
                    }
                }
            }
        }

        ret_vec
    }

    #[allow(dead_code)]
    fn check_bounds(i: i32, j: i32, n: i32, m: i32) -> bool {
        i >= 0 && i < n && j >= 0 && j < m
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 vẫn là BFS đa nguồn, nhưng lấy rõ kích thước từng layer khi xử lý. Thuật toán và độ phức tạp không đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def highestPeak(self, isWater: List[List[int]]) -> List[List[int]]:
        m, n = len(isWater), len(isWater[0])
        ans = [[-1] * n for _ in range(m)]
        q = deque()
        for i, row in enumerate(isWater):
            for j, v in enumerate(row):
                if v:
                    q.append((i, j))
                    ans[i][j] = 0
        while q:
            for _ in range(len(q)):
                i, j = q.popleft()
                for a, b in pairwise((-1, 0, 1, 0, -1)):
                    x, y = i + a, j + b
                    if 0 <= x < m and 0 <= y < n and ans[x][y] == -1:
                        ans[x][y] = ans[i][j] + 1
                        q.append((x, y))
        return ans
```

#### Java

```java
class Solution {
    public int[][] highestPeak(int[][] isWater) {
        int m = isWater.length, n = isWater[0].length;
        int[][] ans = new int[m][n];
        Deque<int[]> q = new ArrayDeque<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[i][j] = isWater[i][j] - 1;
                if (ans[i][j] == 0) {
                    q.offer(new int[] {i, j});
                }
            }
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            for (int t = q.size(); t > 0; --t) {
                var p = q.poll();
                int i = p[0], j = p[1];
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1) {
                        ans[x][y] = ans[i][j] + 1;
                        q.offer(new int[] {x, y});
                    }
                }
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
    const int dirs[5] = {-1, 0, 1, 0, -1};

    vector<vector<int>> highestPeak(vector<vector<int>>& isWater) {
        int m = isWater.size(), n = isWater[0].size();
        vector<vector<int>> ans(m, vector<int>(n));
        queue<pair<int, int>> q;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[i][j] = isWater[i][j] - 1;
                if (ans[i][j] == 0) {
                    q.emplace(i, j);
                }
            }
        }
        while (!q.empty()) {
            for (int t = q.size(); t; --t) {
                auto [i, j] = q.front();
                q.pop();
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1) {
                        ans[x][y] = ans[i][j] + 1;
                        q.emplace(x, y);
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func highestPeak(isWater [][]int) [][]int {
	m, n := len(isWater), len(isWater[0])
	ans := make([][]int, m)
	type pair struct{ i, j int }
	q := []pair{}
	for i, row := range isWater {
		ans[i] = make([]int, n)
		for j, v := range row {
			ans[i][j] = v - 1
			if v == 1 {
				q = append(q, pair{i, j})
			}
		}
	}
	dirs := []int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		for t := len(q); t > 0; t-- {
			p := q[0]
			q = q[1:]
			i, j := p.i, p.j
			for k := 0; k < 4; k++ {
				x, y := i+dirs[k], j+dirs[k+1]
				if x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1 {
					ans[x][y] = ans[i][j] + 1
					q = append(q, pair{x, y})
				}
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
