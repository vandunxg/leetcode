---
comments: true
difficulty: Hard
rating: 2530
source: Weekly Contest 437 Q4
tags:
    - Memoization
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3459. Length of Longest V-Shaped Diagonal Segment](https://leetcode.com/problems/length-of-longest-v-shaped-diagonal-segment)

[中文文档](/solution/3400-3499/3459.Length%20of%20Longest%20V-Shaped%20Diagonal%20Segment/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên 2D <code>grid</code> có kích thước <code>n x m</code>, trong đó mỗi phần tử là <code>0</code>, <code>1</code> hoặc <code>2</code>.</p>

<p>Một <strong>đoạn đường chéo hình chữ V</strong> được định nghĩa như sau:</p>

<ul>
	<li>Đoạn bắt đầu bằng <code>1</code>.</li>
	<li>Các phần tử tiếp theo tuân theo dãy vô hạn: <code>2, 0, 2, 0, ...</code>.</li>
	<li>Đoạn này:
	<ul>
		<li>Bắt đầu <strong>theo</strong> một hướng đường chéo (từ trên-trái đến dưới-phải, từ dưới-phải đến trên-trái, từ trên-phải đến dưới-trái hoặc từ dưới-trái đến trên-phải).</li>
		<li>Tiếp tục <strong>dãy</strong> theo cùng hướng đường chéo.</li>
		<li>Rẽ<strong> nhiều nhất một lần 90 độ theo chiều kim đồng hồ</strong><strong> sang một hướng đường chéo khác</strong> trong khi <strong>duy trì</strong> dãy trên.</li>
	</ul>
	</li>
</ul>

<p>Trả về <strong>độ dài</strong> của <strong>đoạn đường chéo hình chữ V</strong> <strong>dài nhất</strong>. Nếu không tồn tại <em>đoạn</em> hợp lệ nào, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[2,2,1,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3459.Length%20of%20Longest%20V-Shaped%20Diagonal%20Segment/images/matrix_1-2.jpg" style="width: 201px; height: 192px;" /></p>

<p>Đoạn đường chéo hình chữ V dài nhất có độ dài 5 và đi qua các tọa độ sau: <code>(0,2) &rarr; (1,3) &rarr; (2,4)</code>, rẽ <strong>90 độ theo chiều kim đồng hồ</strong> tại <code>(2,4)</code>, rồi tiếp tục qua <code>(3,3) &rarr; (4,2)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[2,2,2,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3459.Length%20of%20Longest%20V-Shaped%20Diagonal%20Segment/images/matrix_2.jpg" style="width: 201px; height: 201px;" /></strong></p>

<p>Đoạn đường chéo hình chữ V dài nhất có độ dài 4 và đi qua các tọa độ sau: <code>(2,3) &rarr; (3,2)</code>, rẽ <strong>90 độ theo chiều kim đồng hồ</strong> tại <code>(3,2)</code>, rồi tiếp tục qua <code>(2,1) &rarr; (1,0)</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2,2,2,2],[2,2,2,2,0],[2,0,0,0,0],[0,0,2,2,2],[2,0,0,2,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3459.Length%20of%20Longest%20V-Shaped%20Diagonal%20Segment/images/matrix_3.jpg" style="width: 201px; height: 201px;" /></strong></p>

<p>Đoạn đường chéo hình chữ V dài nhất có độ dài 5 và đi qua các tọa độ sau: <code>(0,0) &rarr; (1,1) &rarr; (2,2) &rarr; (3,3) &rarr; (4,4)</code>.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đoạn đường chéo hình chữ V dài nhất có độ dài 1 và đi qua tọa độ <code>(0,0)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>m == grid[i].length</code></li>
	<li><code>1 &lt;= n, m &lt;= 500</code></li>
	<li><code>grid[i][j]</code> là một trong các giá trị <code>0</code>, <code>1</code> hoặc <code>2</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một đoạn đường chéo hình chữ V bắt đầu bằng $1$, tiếp tục theo $2,0,2,0,\ldots$, và có thể rẽ theo chiều kim đồng hồ nhiều nhất một lần. Với ma trận $500\times 500$, nếu tìm kiếm từng đường gấp khúc từ đầu thì cùng một trạng thái sẽ bị duyệt lại.
>
> Một trạng thái gồm ô trước đó, hướng di chuyển và việc còn được phép rẽ hay không. Memoization giúp số lượng trạng thái chỉ còn tuyến tính.
>
> $\textit{dfs}(i,j,k,\textit{cnt})$ tiếp tục theo hướng $k$, hoặc rẽ sang hướng $(k+1)\bmod 4$ khi $\textit{cnt}>0$. Ta bắt đầu từ mọi ô có giá trị $1$ theo cả bốn hướng rồi cộng thêm một cho ô bắt đầu.

<!-- thinking:end -->

Ta xây dựng hàm $\text{dfs}(i, j, k, \textit{cnt})$ trả về độ dài của đoạn đường chéo hình chữ V dài nhất, trong đó $(i, j)$ là vị trí trước đó, $k$ là hướng hiện tại và $\textit{cnt}$ là số lần rẽ còn được phép.

Logic của hàm $\text{dfs}$ như sau:

Trước tiên, dựa trên vị trí trước đó và hướng hiện tại, ta tính vị trí hiện tại $(x, y)$ và xác định giá trị cần tìm $\textit{target}$. Nếu $x$ hoặc $y$ nằm ngoài phạm vi, hoặc nếu $\textit{grid}[x][y] \neq \textit{target}$, ta trả về $0$.

Nếu không, ta có hai lựa chọn:

Tiếp tục di chuyển theo hướng hiện tại.

Rẽ 90 độ theo chiều kim đồng hồ tại vị trí hiện tại, sau đó tiếp tục di chuyển.

Để thực hiện phép rẽ, nếu hướng hiện tại là $k$, hướng mới sau khi rẽ 90 độ theo chiều kim đồng hồ là $(k + 1) \bmod 4$. Ta lấy kết quả lớn hơn trong hai lựa chọn này làm kết quả cho trạng thái hiện tại.

Trong hàm chính, ta duyệt toàn bộ ma trận. Với mỗi vị trí có giá trị 1, ta thử tìm kiếm theo cả bốn hướng và cập nhật đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Để tránh tính toán lặp lại, ta dùng memoization để lưu các kết quả trung gian.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lenOfVDiagonal(self, grid: List[List[int]]) -> int:
        @cache
        def dfs(i: int, j: int, k: int, cnt: int) -> int:
            x, y = i + dirs[k], j + dirs[k + 1]
            target = 2 if grid[i][j] == 1 else (2 - grid[i][j])
            if not 0 <= x < m or not 0 <= y < n or grid[x][y] != target:
                return 0
            res = dfs(x, y, k, cnt)
            if cnt > 0:
                res = max(res, dfs(x, y, (k + 1) % 4, 0))
            return 1 + res

        m, n = len(grid), len(grid[0])
        dirs = (1, 1, -1, -1, 1)
        ans = 0
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x == 1:
                    for k in range(4):
                        ans = max(ans, dfs(i, j, k, 1) + 1)
        return ans
```

#### Java

```java
class Solution {
    private int m, n;
    private final int[] dirs = {1, 1, -1, -1, 1};
    private Integer[][][][] f;

    public int lenOfVDiagonal(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        f = new Integer[m][n][4][2];
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    for (int k = 0; k < 4; k++) {
                        ans = Math.max(ans, dfs(grid, i, j, k, 1) + 1);
                    }
                }
            }
        }
        return ans;
    }

    private int dfs(int[][] grid, int i, int j, int k, int cnt) {
        if (f[i][j][k][cnt] != null) {
            return f[i][j][k][cnt];
        }
        int x = i + dirs[k];
        int y = j + dirs[k + 1];
        int target = grid[i][j] == 1 ? 2 : (2 - grid[i][j]);
        if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] != target) {
            f[i][j][k][cnt] = 0;
            return 0;
        }
        int res = dfs(grid, x, y, k, cnt);
        if (cnt > 0) {
            res = Math.max(res, dfs(grid, x, y, (k + 1) % 4, 0));
        }
        f[i][j][k][cnt] = 1 + res;
        return 1 + res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    static constexpr int MAXN = 501;
    int f[MAXN][MAXN][4][2];

    int lenOfVDiagonal(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int dirs[5] = {1, 1, -1, -1, 1};
        memset(f, -1, sizeof(f));

        auto dfs = [&](this auto&& dfs, int i, int j, int k, int cnt) -> int {
            if (f[i][j][k][cnt] != -1) {
                return f[i][j][k][cnt];
            }
            int x = i + dirs[k];
            int y = j + dirs[k + 1];
            int target = grid[i][j] == 1 ? 2 : (2 - grid[i][j]);
            if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] != target) {
                f[i][j][k][cnt] = 0;
                return 0;
            }
            int res = dfs(x, y, k, cnt);
            if (cnt > 0) {
                res = max(res, dfs(x, y, (k + 1) % 4, 0));
            }
            f[i][j][k][cnt] = 1 + res;
            return 1 + res;
        };

        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    for (int k = 0; k < 4; ++k) {
                        ans = max(ans, dfs(i, j, k, 1) + 1);
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
func lenOfVDiagonal(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	dirs := []int{1, 1, -1, -1, 1}
	f := make([][][4][2]int, m)
	for i := range f {
		f[i] = make([][4][2]int, n)
	}

	var dfs func(i, j, k, cnt int) int
	dfs = func(i, j, k, cnt int) int {
		if f[i][j][k][cnt] != 0 {
			return f[i][j][k][cnt]
		}

		x := i + dirs[k]
		y := j + dirs[k+1]

		var target int
		if grid[i][j] == 1 {
			target = 2
		} else {
			target = 2 - grid[i][j]
		}

		if x < 0 || x >= m || y < 0 || y >= n || grid[x][y] != target {
			f[i][j][k][cnt] = 0
			return 0
		}

		res := dfs(x, y, k, cnt)
		if cnt > 0 {
			res = max(res, dfs(x, y, (k+1)%4, 0))
		}
		f[i][j][k][cnt] = res + 1
		return res + 1
	}

	ans := 0
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 1 {
				for k := 0; k < 4; k++ {
					ans = max(ans, dfs(i, j, k, 1)+1)
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
