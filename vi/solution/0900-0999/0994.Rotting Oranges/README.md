---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges)

[中文文档](/solution/0900-0999/0994.Rotting%20Oranges/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới <code>m x n</code> tên <code>grid</code>, mỗi ô có thể nhận một trong ba giá trị:</p>

<ul>
	<li><code>0</code> biểu thị ô trống,</li>
	<li><code>1</code> biểu thị cam tươi, hoặc</li>
	<li><code>2</code> biểu thị cam thối.</li>
</ul>

<p>Mỗi phút, cam tươi nào <strong>kề theo một trong bốn hướng</strong> với cam thối sẽ bị thối.</p>

<p>Trả về <em>số phút ít nhất cần trôi qua cho đến khi không còn ô nào chứa cam tươi</em>. Nếu <em>điều này không thể xảy ra, hãy trả về</em> <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0994.Rotting%20Oranges/images/oranges.png" style="width: 650px; height: 137px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,1,1],[1,1,0],[0,1,1]]
<strong>Đầu ra:</strong> 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[2,1,1],[0,1,1],[1,0,1]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Quả cam ở góc dưới bên trái (hàng 2, cột 0) không bao giờ bị thối, vì cam chỉ lây thối theo bốn hướng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,2]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì ngay tại phút 0 đã không còn cam tươi nào, đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10</code></li>
	<li><code>grid[i][j]</code> là <code>0</code>, <code>1</code> hoặc <code>2</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Cam thối lây sang các ô kề theo bốn hướng mỗi phút; ta cần tìm thời gian để mọi quả cam đều bị thối. Quá trình lây đồng thời từ nhiều nguồn tương đương bài toán tìm đường đi ngắn nhất không trọng số. Đưa vị trí của tất cả cam thối vào queue và đếm cam tươi, sau đó BFS theo từng lớp. Lớp làm số cam tươi về 0 chính là đáp án; nếu vẫn còn cam tươi thì trả về $-1$.

<!-- thinking:end -->

Đầu tiên, ta duyệt toàn bộ lưới một lần, đếm số cam tươi và ký hiệu là $\textit{cnt}$, đồng thời thêm tọa độ của tất cả cam thối vào queue $q$.

Tiếp theo, ta thực hiện BFS. Ở mỗi lượt, tất cả cam thối trong queue làm thối các quả cam tươi ở bốn hướng, cho đến khi queue rỗng hoặc số cam tươi bằng $0$.

Cuối cùng, nếu số cam tươi bằng $0$, ta trả về số lượt hiện tại; nếu không, trả về $-1$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        cnt = 0
        q = deque()
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x == 2:
                    q.append((i, j))
                elif x == 1:
                    cnt += 1
        ans = 0
        dirs = (-1, 0, 1, 0, -1)
        while q and cnt:
            ans += 1
            for _ in range(len(q)):
                i, j = q.popleft()
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if 0 <= x < m and 0 <= y < n and grid[x][y] == 1:
                        grid[x][y] = 2
                        q.append((x, y))
                        cnt -= 1
                        if cnt == 0:
                            return ans
        return -1 if cnt else 0
```

#### Java

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Deque<int[]> q = new ArrayDeque<>();
        int cnt = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    ++cnt;
                } else if (grid[i][j] == 2) {
                    q.offer(new int[] {i, j});
                }
            }
        }
        final int[] dirs = {-1, 0, 1, 0, -1};
        for (int ans = 1; !q.isEmpty() && cnt > 0; ++ans) {
            for (int k = q.size(); k > 0; --k) {
                var p = q.poll();
                for (int d = 0; d < 4; ++d) {
                    int x = p[0] + dirs[d], y = p[1] + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                        grid[x][y] = 2;
                        q.offer(new int[] {x, y});
                        if (--cnt == 0) {
                            return ans;
                        }
                    }
                }
            }
        }
        return cnt > 0 ? -1 : 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        queue<pair<int, int>> q;
        int cnt = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    ++cnt;
                } else if (grid[i][j] == 2) {
                    q.emplace(i, j);
                }
            }
        }
        const int dirs[5] = {-1, 0, 1, 0, -1};
        for (int ans = 1; q.size() && cnt; ++ans) {
            for (int k = q.size(); k; --k) {
                auto [i, j] = q.front();
                q.pop();
                for (int d = 0; d < 4; ++d) {
                    int x = i + dirs[d], y = j + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                        grid[x][y] = 2;
                        q.emplace(x, y);
                        if (--cnt == 0) {
                            return ans;
                        }
                    }
                }
            }
        }
        return cnt > 0 ? -1 : 0;
    }
};
```

#### Go

```go
func orangesRotting(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	q := [][2]int{}
	cnt := 0
	for i, row := range grid {
		for j, x := range row {
			if x == 1 {
				cnt++
			} else if x == 2 {
				q = append(q, [2]int{i, j})
			}
		}
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for ans := 1; len(q) > 0 && cnt > 0; ans++ {
		for k := len(q); k > 0; k-- {
			p := q[0]
			q = q[1:]
			for d := 0; d < 4; d++ {
				x, y := p[0]+dirs[d], p[1]+dirs[d+1]
				if x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1 {
					grid[x][y] = 2
					q = append(q, [2]int{x, y})
					if cnt--; cnt == 0 {
						return ans
					}
				}
			}
		}
	}
	if cnt > 0 {
		return -1
	}
	return 0
}
```

#### TypeScript

```ts
function orangesRotting(grid: number[][]): number {
    const m: number = grid.length;
    const n: number = grid[0].length;
    const q: number[][] = [];
    let cnt: number = 0;
    for (let i: number = 0; i < m; ++i) {
        for (let j: number = 0; j < n; ++j) {
            if (grid[i][j] === 1) {
                cnt++;
            } else if (grid[i][j] === 2) {
                q.push([i, j]);
            }
        }
    }
    const dirs: number[] = [-1, 0, 1, 0, -1];
    for (let ans = 1; q.length && cnt; ++ans) {
        const t: number[][] = [];
        for (const [i, j] of q) {
            for (let d = 0; d < 4; ++d) {
                const [x, y] = [i + dirs[d], j + dirs[d + 1]];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] === 1) {
                    grid[x][y] = 2;
                    t.push([x, y]);
                    if (--cnt === 0) {
                        return ans;
                    }
                }
            }
        }
        q.splice(0, q.length, ...t);
    }
    return cnt > 0 ? -1 : 0;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn oranges_rotting(mut grid: Vec<Vec<i32>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut q = VecDeque::new();
        let mut cnt = 0;
        for i in 0..m {
            for j in 0..n {
                if grid[i][j] == 1 {
                    cnt += 1;
                } else if grid[i][j] == 2 {
                    q.push_back((i, j));
                }
            }
        }

        let dirs = [-1, 0, 1, 0, -1];
        for ans in 1.. {
            if q.is_empty() || cnt == 0 {
                break;
            }
            let mut size = q.len();
            for _ in 0..size {
                let (x, y) = q.pop_front().unwrap();
                for d in 0..4 {
                    let nx = x as isize + dirs[d] as isize;
                    let ny = y as isize + dirs[d + 1] as isize;
                    if nx >= 0 && nx < m as isize && ny >= 0 && ny < n as isize {
                        let nx = nx as usize;
                        let ny = ny as usize;
                        if grid[nx][ny] == 1 {
                            grid[nx][ny] = 2;
                            q.push_back((nx, ny));
                            cnt -= 1;
                            if cnt == 0 {
                                return ans;
                            }
                        }
                    }
                }
            }
        }
        if cnt > 0 {
            -1
        } else {
            0
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var orangesRotting = function (grid) {
    const m = grid.length;
    const n = grid[0].length;
    let q = [];
    let cnt = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 1) {
                cnt++;
            } else if (grid[i][j] === 2) {
                q.push([i, j]);
            }
        }
    }

    const dirs = [-1, 0, 1, 0, -1];
    for (let ans = 1; q.length && cnt; ++ans) {
        let t = [];
        for (const [i, j] of q) {
            for (let d = 0; d < 4; ++d) {
                const x = i + dirs[d];
                const y = j + dirs[d + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] === 1) {
                    grid[x][y] = 2;
                    t.push([x, y]);
                    if (--cnt === 0) {
                        return ans;
                    }
                }
            }
        }
        q = [...t];
    }

    return cnt > 0 ? -1 : 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
