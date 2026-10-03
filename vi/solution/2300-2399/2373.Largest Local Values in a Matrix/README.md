---
comments: true
difficulty: Easy
rating: 1331
source: Weekly Contest 306 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [2373. Largest Local Values in a Matrix](https://leetcode.com/problems/largest-local-values-in-a-matrix)

[中文文档](/solution/2300-2399/2373.Largest%20Local%20Values%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>grid</code> kích thước <code>n x n</code>.</p>

<p>Tạo một ma trận số nguyên <code>maxLocal</code> có kích thước <code>(n - 2) x (n - 2)</code> sao cho:</p>

<ul>
	<li><code>maxLocal[i][j]</code> bằng giá trị <strong>lớn nhất</strong> trong ma trận <code>3 x 3</code> của <code>grid</code> có tâm tại hàng <code>i + 1</code> và cột <code>j + 1</code>.</li>
</ul>

<p>Nói cách khác, cần tìm giá trị lớn nhất trong mọi ma trận <code>3 x 3</code> liên tiếp trong <code>grid</code>.</p>

<p>Trả về <em>ma trận đã tạo</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2373.Largest%20Local%20Values%20in%20a%20Matrix/images/ex1.png" style="width: 371px; height: 210px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[9,9,8,1],[5,6,2,6],[8,2,6,4],[6,2,2,2]]
<strong>Đầu ra:</strong> [[9,9],[8,6]]
<strong>Giải thích:</strong> Sơ đồ phía trên minh họa ma trận ban đầu và ma trận được tạo.
Lưu ý rằng mỗi giá trị trong ma trận được tạo tương ứng với giá trị lớn nhất của một ma trận 3 x 3 liên tiếp trong grid.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2373.Largest%20Local%20Values%20in%20a%20Matrix/images/ex2new2.png" style="width: 436px; height: 240px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,1,1],[1,1,1,1,1],[1,1,2,1,1],[1,1,1,1,1],[1,1,1,1,1]]
<strong>Đầu ra:</strong> [[2,2,2],[2,2,2],[2,2,2]]
<strong>Giải thích:</strong> Lưu ý rằng số 2 nằm trong mọi ma trận 3 x 3 liên tiếp trong grid.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>3 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ô đầu ra là giá trị lớn nhất của ma trận $3 \times 3$ có góc trên bên trái tại vị trí đó. Vì $n \le 100$, ta có thể liệt kê các cửa sổ.
>
> Với mỗi $(i,j)$, duyệt qua chín ô và ghi giá trị lớn nhất vào ma trận $(n-2)\times(n-2)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestLocal(self, grid: List[List[int]]) -> List[List[int]]:
        n = len(grid)
        ans = [[0] * (n - 2) for _ in range(n - 2)]
        for i in range(n - 2):
            for j in range(n - 2):
                ans[i][j] = max(
                    grid[x][y] for x in range(i, i + 3) for y in range(j, j + 3)
                )
        return ans
```

#### Java

```java
class Solution {
    public int[][] largestLocal(int[][] grid) {
        int n = grid.length;
        int[][] ans = new int[n - 2][n - 2];
        for (int i = 0; i < n - 2; ++i) {
            for (int j = 0; j < n - 2; ++j) {
                for (int x = i; x <= i + 2; ++x) {
                    for (int y = j; y <= j + 2; ++y) {
                        ans[i][j] = Math.max(ans[i][j], grid[x][y]);
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
    vector<vector<int>> largestLocal(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<int>> ans(n - 2, vector<int>(n - 2));
        for (int i = 0; i < n - 2; ++i) {
            for (int j = 0; j < n - 2; ++j) {
                for (int x = i; x <= i + 2; ++x) {
                    for (int y = j; y <= j + 2; ++y) {
                        ans[i][j] = max(ans[i][j], grid[x][y]);
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
func largestLocal(grid [][]int) [][]int {
	n := len(grid)
	ans := make([][]int, n-2)
	for i := range ans {
		ans[i] = make([]int, n-2)
		for j := 0; j < n-2; j++ {
			for x := i; x <= i+2; x++ {
				for y := j; y <= j+2; y++ {
					ans[i][j] = max(ans[i][j], grid[x][y])
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function largestLocal(grid: number[][]): number[][] {
    const n = grid.length;
    const res = Array.from({ length: n - 2 }, () => new Array(n - 2).fill(0));
    for (let i = 0; i < n - 2; i++) {
        for (let j = 0; j < n - 2; j++) {
            let max = 0;
            for (let k = i; k < i + 3; k++) {
                for (let z = j; z < j + 3; z++) {
                    max = Math.max(max, grid[k][z]);
                }
            }
            res[i][j] = max;
        }
    }
    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
