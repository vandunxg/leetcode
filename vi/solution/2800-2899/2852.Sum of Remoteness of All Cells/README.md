---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [2852. Sum of Remoteness of All Cells 🔒](https://leetcode.com/problems/sum-of-remoteness-of-all-cells)

[中文文档](/solution/2800-2899/2852.Sum%20of%20Remoteness%20of%20All%20Cells/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <strong>0-indexed</strong> <code>grid</code> có kích thước <code>n * n</code>. Mỗi ô trong ma trận này có giá trị <code>grid[i][j]</code> là một số nguyên <strong>dương</strong> hoặc <code>-1</code>, biểu diễn một ô bị chặn.</p>

<p>Bạn có thể di chuyển từ một ô không bị chặn đến bất kỳ ô không bị chặn nào chung cạnh với nó.</p>

<p>Với mỗi ô <code>(i, j)</code>, ta định nghĩa <strong>độ xa cách</strong> của nó là <code>R[i][j]</code>, được xác định như sau:</p>

<ul>
	<li>Nếu ô <code>(i, j)</code> là ô <strong>không bị chặn</strong>, <code>R[i][j]</code> là tổng các giá trị <code>grid[x][y]</code> sao cho <strong>không có đường đi</strong> từ ô <strong>không bị chặn</strong> <code>(x, y)</code> đến ô <code>(i, j)</code>.</li>
	<li>Với các ô bị chặn, <code>R[i][j] == 0</code>.</li>
</ul>

<p>Trả về <em>tổng của </em><code>R[i][j]</code><em> trên tất cả các ô.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2852.Sum%20of%20Remoteness%20of%20All%20Cells/images/1-new.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 400px; height: 304px;" /></p>

<pre>
<strong>Đầu vào:</strong> grid = [[-1,1,-1],[5,-1,4],[-1,3,-1]]
<strong>Đầu ra:</strong> 39
<strong>Giải thích:</strong> Trong hình trên có bốn lưới. Lưới phía trên bên trái chứa các giá trị ban đầu trong lưới. Các ô bị chặn được tô màu đen, còn các ô khác giữ nguyên giá trị như trong đầu vào. Trong lưới phía trên bên phải, ta có thể thấy giá trị của R[i][j] cho tất cả các ô. Vì vậy, đáp án là tổng của chúng, cụ thể là: 0 + 12 + 0 + 8 + 0 + 9 + 0 + 10 + 0 = 39.
Hãy chuyển sang lưới phía dưới bên trái trong hình trên và tính R[0][1] (ô đích được tô màu xanh lá). Ta cần cộng giá trị của các ô mà ô (0, 1) không thể đi tới. Các ô này được tô màu vàng trong lưới. Do đó, R[0][1] = 5 + 4 + 3 = 12.
Tiếp theo, hãy chuyển sang lưới phía dưới bên phải trong hình trên và tính R[1][2] (ô đích được tô màu xanh lá). Ta cần cộng giá trị của các ô mà ô (1, 2) không thể đi tới. Các ô này được tô màu vàng trong lưới. Do đó, R[1][2] = 1 + 5 + 3 = 9.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2852.Sum%20of%20Remoteness%20of%20All%20Cells/images/2.png" style="width: 400px; height: 302px; background: #fff; border-radius: .5rem;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[-1,3,4],[-1,-1,-1],[3,-1,-1]]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Trong hình trên có bốn lưới. Lưới phía trên bên trái chứa các giá trị ban đầu trong lưới. Các ô bị chặn được tô màu đen, còn các ô khác giữ nguyên giá trị như trong đầu vào. Trong lưới phía trên bên phải, ta có thể thấy giá trị của R[i][j] cho tất cả các ô. Vì vậy, đáp án là tổng của chúng, cụ thể là: 3 + 3 + 0 + 0 + 0 + 0 + 7 + 0 + 0 = 13.
Hãy chuyển sang lưới phía dưới bên trái trong hình trên và tính R[0][2] (ô đích được tô màu xanh lá). Ta cần cộng giá trị của ô mà ô (0, 2) không thể đi tới. Ô này được tô màu vàng trong lưới. Do đó, R[0][2] = 3.
Tiếp theo, hãy chuyển sang lưới phía dưới bên phải trong hình trên và tính R[2][0] (ô đích được tô màu xanh lá). Ta cần cộng giá trị của các ô mà ô (2, 0) không thể đi tới. Các ô này được tô màu vàng trong lưới. Do đó, R[2][0] = 3 + 4 = 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì không có ô nào khác ngoài (0, 0), R[0][0] bằng 0. Do đó, tổng của R[i][j] trên tất cả các ô cũng bằng 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 300</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>6</sup></code> hoặc <code>grid[i][j] == -1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Độ xa cách của một ô là tổng giá trị của tất cả các ô không bị chặn nằm ngoài thành phần liên thông chứa nó. Gọi số ô đó là $cnt$, sau đó dùng DFS trên từng thành phần để tính tổng $s$ và kích thước $t$; thành phần đó đóng góp $(cnt-t)\times s$.

<!-- thinking:end -->

Trước hết, ta đếm số ô không bị chặn trong ma trận, ký hiệu là $cnt$. Sau đó, bắt đầu từ mỗi ô không bị chặn, ta dùng DFS để tính tổng $s$ của các ô trong mỗi thành phần liên thông và số lượng ô $t$. Khi đó, tất cả $(cnt - t)$ ô thuộc các thành phần liên thông khác có thể được cộng với $s$. Ta cộng kết quả của tất cả các thành phần liên thông.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài cạnh của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumRemoteness(self, grid: List[List[int]]) -> int:
        def dfs(i: int, j: int) -> (int, int):
            s, t = grid[i][j], 1
            grid[i][j] = 0
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < n and 0 <= y < n and grid[x][y] > 0:
                    s1, t1 = dfs(x, y)
                    s, t = s + s1, t + t1
            return s, t

        n = len(grid)
        dirs = (-1, 0, 1, 0, -1)
        cnt = sum(x > 0 for row in grid for x in row)
        ans = 0
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x > 0:
                    s, t = dfs(i, j)
                    ans += (cnt - t) * s
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int[][] grid;
    private final int[] dirs = {-1, 0, 1, 0, -1};

    public long sumRemoteness(int[][] grid) {
        n = grid.length;
        this.grid = grid;
        int cnt = 0;
        for (int[] row : grid) {
            for (int x : row) {
                if (x > 0) {
                    ++cnt;
                }
            }
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] > 0) {
                    long[] res = dfs(i, j);
                    ans += (cnt - res[1]) * res[0];
                }
            }
        }
        return ans;
    }

    private long[] dfs(int i, int j) {
        long[] res = new long[2];
        res[0] = grid[i][j];
        res[1] = 1;
        grid[i][j] = 0;
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] > 0) {
                long[] tmp = dfs(x, y);
                res[0] += tmp[0];
                res[1] += tmp[1];
            }
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long sumRemoteness(vector<vector<int>>& grid) {
        using pli = pair<long long, int>;
        int n = grid.size();
        int cnt = 0;
        for (auto& row : grid) {
            for (int x : row) {
                cnt += x > 0;
            }
        }
        int dirs[5] = {-1, 0, 1, 0, -1};
        function<pli(int, int)> dfs = [&](int i, int j) {
            long long s = grid[i][j];
            int t = 1;
            grid[i][j] = 0;
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] > 0) {
                    auto [ss, tt] = dfs(x, y);
                    s += ss;
                    t += tt;
                }
            }
            return pli(s, t);
        };
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] > 0) {
                    auto [s, t] = dfs(i, j);
                    ans += (cnt - t) * s;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumRemoteness(grid [][]int) (ans int64) {
	n := len(grid)
	cnt := 0
	for _, row := range grid {
		for _, x := range row {
			if x > 0 {
				cnt++
			}
		}
	}
	var dfs func(i, j int) (int, int)
	dfs = func(i, j int) (int, int) {
		s, t := grid[i][j], 1
		grid[i][j] = 0
		dirs := [5]int{-1, 0, 1, 0, -1}
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < n && y >= 0 && y < n && grid[x][y] > 0 {
				ss, tt := dfs(x, y)
				s += ss
				t += tt
			}
		}
		return s, t
	}
	for i := range grid {
		for j := range grid[i] {
			if grid[i][j] > 0 {
				s, t := dfs(i, j)
				ans += int64(cnt-t) * int64(s)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function sumRemoteness(grid: number[][]): number {
    const n = grid.length;
    let cnt = 0;
    for (const row of grid) {
        for (const x of row) {
            if (x > 0) {
                cnt++;
            }
        }
    }
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number): [number, number] => {
        let s = grid[i][j];
        let t = 1;
        grid[i][j] = 0;
        for (let k = 0; k < 4; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] > 0) {
                const [ss, tt] = dfs(x, y);
                s += ss;
                t += tt;
            }
        }
        return [s, t];
    };
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] > 0) {
                const [s, t] = dfs(i, j);
                ans += (cnt - t) * s;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
