---
comments: true
difficulty: Medium
rating: 1289
source: Weekly Contest 313 Q2
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [2428. Maximum Sum of an Hourglass](https://leetcode.com/problems/maximum-sum-of-an-hourglass)

[中文文档](/solution/2400-2499/2428.Maximum%20Sum%20of%20an%20Hourglass/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một ma trận số nguyên <code>grid</code> có kích thước <code>m x n</code>.</p>

<p>Ta định nghĩa một <strong>hình đồng hồ cát</strong> là một phần của ma trận có dạng như sau:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2428.Maximum%20Sum%20of%20an%20Hourglass/images/img.jpg" style="width: 243px; height: 243px;" />
<p>Hãy trả về <em>tổng <strong>lớn nhất</strong> của các phần tử trong một hình đồng hồ cát</em>.</p>

<p><strong>Lưu ý</strong> rằng không thể xoay hình đồng hồ cát và hình này phải nằm hoàn toàn trong ma trận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2428.Maximum%20Sum%20of%20an%20Hourglass/images/1.jpg" style="width: 323px; height: 323px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[6,2,1,3],[4,2,1,5],[9,2,8,7],[4,1,2,9]]
<strong>Đầu ra:</strong> 30
<strong>Giải thích:</strong> Các ô được hiển thị ở trên tạo thành hình đồng hồ cát có tổng lớn nhất: 6 + 2 + 1 + 2 + 9 + 2 + 8 = 30.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2428.Maximum%20Sum%20of%20an%20Hourglass/images/2.jpg" style="width: 243px; height: 243px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,3],[4,5,6],[7,8,9]]
<strong>Đầu ra:</strong> 35
<strong>Giải thích:</strong> Trong ma trận chỉ có một hình đồng hồ cát, với tổng: 1 + 2 + 3 + 5 + 7 + 8 + 9 = 35.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>3 &lt;= m, n &lt;= 150</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một hình đồng hồ cát là một khối $3\times 3$ bỏ đi hai ô ở hai bên của hàng giữa. Vì ma trận có kích thước tối đa $150\times 150$, ta có thể liệt kê mọi tâm $(i,j)$ rồi tính tổng bảy ô trong $O(mn)$.

<!-- thinking:end -->

Từ mô tả bài toán, ta nhận thấy mỗi hình đồng hồ cát là một ma trận $3 \times 3$ trong đó phần tử đầu tiên và cuối cùng của hàng giữa bị loại bỏ. Vì vậy, ta có thể bắt đầu từ góc trên bên trái, liệt kê tọa độ tâm $(i, j)$ của từng hình đồng hồ cát, sau đó tính tổng các phần tử trong hình và lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        ans = 0
        for i in range(1, m - 1):
            for j in range(1, n - 1):
                s = -grid[i][j - 1] - grid[i][j + 1]
                s += sum(
                    grid[x][y] for x in range(i - 1, i + 2) for y in range(j - 1, j + 2)
                )
                ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public int maxSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int ans = 0;
        for (int i = 1; i < m - 1; ++i) {
            for (int j = 1; j < n - 1; ++j) {
                int s = -grid[i][j - 1] - grid[i][j + 1];
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        s += grid[x][y];
                    }
                }
                ans = Math.max(ans, s);
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
    int maxSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        for (int i = 1; i < m - 1; ++i) {
            for (int j = 1; j < n - 1; ++j) {
                int s = -grid[i][j - 1] - grid[i][j + 1];
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        s += grid[x][y];
                    }
                }
                ans = max(ans, s);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSum(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	for i := 1; i < m-1; i++ {
		for j := 1; j < n-1; j++ {
			s := -grid[i][j-1] - grid[i][j+1]
			for x := i - 1; x <= i+1; x++ {
				for y := j - 1; y <= j+1; y++ {
					s += grid[x][y]
				}
			}
			ans = max(ans, s)
		}
	}
	return
}
```

#### TypeScript

```ts
function maxSum(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    let ans = 0;
    for (let i = 1; i < m - 1; ++i) {
        for (let j = 1; j < n - 1; ++j) {
            let s = -grid[i][j - 1] - grid[i][j + 1];
            for (let x = i - 1; x <= i + 1; ++x) {
                for (let y = j - 1; y <= j + 1; ++y) {
                    s += grid[x][y];
                }
            }
            ans = Math.max(ans, s);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
