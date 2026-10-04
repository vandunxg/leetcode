---
comments: true
difficulty: Medium
rating: 1387
source: Biweekly Contest 136 Q2
tags:
    - Array
    - Two Pointers
    - Matrix
---

<!-- problem:start -->

# [3239. Minimum Number of Flips to Make Binary Grid Palindromic I](https://leetcode.com/problems/minimum-number-of-flips-to-make-binary-grid-palindromic-i)

[中文文档](/solution/3200-3299/3239.Minimum%20Number%20of%20Flips%20to%20Make%20Binary%20Grid%20Palindromic%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận nhị phân <code>m x n</code> là <code>grid</code>.</p>

<p>Một hàng hoặc cột được gọi là <strong>đối xứng</strong> nếu các giá trị của nó khi đọc từ đầu đến cuối và từ cuối về đầu giống nhau.</p>

<p>Ta có thể <strong>đảo</strong> giá trị của bất kỳ số ô nào trong <code>grid</code> từ <code>0</code> thành <code>1</code>, hoặc từ <code>1</code> thành <code>0</code>.</p>

<p>Hãy trả về số ô <strong>ít nhất</strong> cần đảo để <strong>hoặc</strong> tất cả các hàng <strong>hoặc</strong> tất cả các cột đều <strong>đối xứng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,0,0],[0,0,0],[0,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3239.Minimum%20Number%20of%20Flips%20to%20Make%20Binary%20Grid%20Palindromic%20I/images/screenshot-from-2024-07-08-00-20-10.png" style="width: 420px; height: 108px;" /></p>

<p>Đảo các ô được đánh dấu để tất cả các hàng trở nên đối xứng.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = </span>[[0,1],[0,1],[0,0]]</p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3239.Minimum%20Number%20of%20Flips%20to%20Make%20Binary%20Grid%20Palindromic%20I/images/screenshot-from-2024-07-08-00-31-23.png" style="width: 300px; height: 100px;" /></p>

<p>Đảo ô được đánh dấu để tất cả các cột trở nên đối xứng.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1],[0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các hàng đã đối xứng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m * n &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể làm cho mọi hàng hoặc mọi cột trở thành đối xứng, và cần chọn phương án có ít lần đảo hơn. Vì $mn\le 2\times 10^5$, ta đếm chi phí của cả hai phương án rồi lấy giá trị nhỏ hơn.
>
> Mỗi hàng cần một lần đảo cho mỗi cặp phần tử khác nhau; các cột cũng tương tự. Hai số đếm này độc lập với nhau. Không cần ghi lại ma trận.

<!-- thinking:end -->

Ta lần lượt đếm số lần đảo cần thiết cho các hàng và các cột, lần lượt ký hiệu là $\textit{cnt1}$ và $\textit{cnt2}$. Cuối cùng, ta lấy giá trị nhỏ hơn trong hai số này.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận $\textit{grid}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFlips(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        cnt1 = cnt2 = 0
        for row in grid:
            for j in range(n // 2):
                if row[j] != row[n - j - 1]:
                    cnt1 += 1
        for j in range(n):
            for i in range(m // 2):
                if grid[i][j] != grid[m - i - 1][j]:
                    cnt2 += 1
        return min(cnt1, cnt2)
```

#### Java

```java
class Solution {
    public int minFlips(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int cnt1 = 0, cnt2 = 0;
        for (var row : grid) {
            for (int j = 0; j < n / 2; ++j) {
                if (row[j] != row[n - j - 1]) {
                    ++cnt1;
                }
            }
        }
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < m / 2; ++i) {
                if (grid[i][j] != grid[m - i - 1][j]) {
                    ++cnt2;
                }
            }
        }
        return Math.min(cnt1, cnt2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFlips(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int cnt1 = 0, cnt2 = 0;
        for (const auto& row : grid) {
            for (int j = 0; j < n / 2; ++j) {
                if (row[j] != row[n - j - 1]) {
                    ++cnt1;
                }
            }
        }
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < m / 2; ++i) {
                if (grid[i][j] != grid[m - i - 1][j]) {
                    ++cnt2;
                }
            }
        }
        return min(cnt1, cnt2);
    }
};
```

#### Go

```go
func minFlips(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	cnt1, cnt2 := 0, 0
	for _, row := range grid {
		for j := 0; j < n/2; j++ {
			if row[j] != row[n-j-1] {
				cnt1++
			}
		}
	}
	for j := 0; j < n; j++ {
		for i := 0; i < m/2; i++ {
			if grid[i][j] != grid[m-i-1][j] {
				cnt2++
			}
		}
	}
	return min(cnt1, cnt2)
}
```

#### TypeScript

```ts
function minFlips(grid: number[][]): number {
    const [m, n] = [grid.length, grid[0].length];
    let [cnt1, cnt2] = [0, 0];
    for (const row of grid) {
        for (let j = 0; j < n / 2; ++j) {
            if (row[j] !== row[n - 1 - j]) {
                ++cnt1;
            }
        }
    }
    for (let j = 0; j < n; ++j) {
        for (let i = 0; i < m / 2; ++i) {
            if (grid[i][j] !== grid[m - 1 - i][j]) {
                ++cnt2;
            }
        }
    }
    return Math.min(cnt1, cnt2);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
