---
comments: true
difficulty: Hard
rating: 2104
source: Biweekly Contest 66 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2088. Count Fertile Pyramids in a Land](https://leetcode.com/problems/count-fertile-pyramids-in-a-land)

[中文文档](/solution/2000-2099/2088.Count%20Fertile%20Pyramids%20in%20a%20Land/README.md)

## Mô tả

<!-- description:start -->

<p>Một người nông dân có một <strong>mảnh đất hình chữ nhật</strong> gồm <code>m</code> hàng và <code>n</code> cột, có thể được chia thành các ô đơn vị. Mỗi ô hoặc là <strong>màu mỡ</strong> (được biểu diễn bằng <code>1</code>), hoặc là <strong>cằn cỗi</strong> (được biểu diễn bằng <code>0</code>). Mọi ô nằm ngoài lưới đều được xem là cằn cỗi.</p>

<p>Một <strong>vùng đất hình kim tự tháp</strong> được định nghĩa là một tập hợp các ô thỏa mãn những tiêu chí sau:</p>

<ol>
	<li>Số ô trong tập hợp phải <strong>lớn hơn </strong><code>1</code> và tất cả các ô đều phải <strong>màu mỡ</strong>.</li>
	<li><strong>Đỉnh</strong> của một kim tự tháp là ô <strong>trên cùng</strong> của kim tự tháp. <strong>Chiều cao</strong> của kim tự tháp là số hàng mà nó bao phủ. Gọi <code>(r, c)</code> là đỉnh của kim tự tháp và chiều cao của nó là <code>h</code>. Khi đó, vùng đất bao gồm các ô <code>(i, j)</code> sao cho <code>r &lt;= i &lt;= r + h - 1</code> <strong>và</strong> <code>c - (i - r) &lt;= j &lt;= c + (i - r)</code>.</li>
</ol>

<p>Một <strong>vùng đất hình kim tự tháp ngược</strong> được định nghĩa là một tập hợp các ô với những tiêu chí tương tự:</p>

<ol>
	<li>Số ô trong tập hợp phải <strong>lớn hơn </strong><code>1</code> và tất cả các ô đều phải <strong>màu mỡ</strong>.</li>
	<li><strong>Đỉnh</strong> của một kim tự tháp ngược là ô <strong>dưới cùng</strong> của kim tự tháp ngược. <strong>Chiều cao</strong> của kim tự tháp ngược là số hàng mà nó bao phủ. Gọi <code>(r, c)</code> là đỉnh của kim tự tháp và chiều cao của nó là <code>h</code>. Khi đó, vùng đất bao gồm các ô <code>(i, j)</code> sao cho <code>r - h + 1 &lt;= i &lt;= r</code> <strong>và</strong> <code>c - (r - i) &lt;= j &lt;= c + (r - i)</code>.</li>
</ol>

<p>Một số ví dụ về các vùng đất hình kim tự tháp và kim tự tháp ngược hợp lệ và không hợp lệ được minh họa bên dưới. Các ô màu đen biểu thị những ô màu mỡ.</p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2088.Count%20Fertile%20Pyramids%20in%20a%20Land/images/image.png" style="width: 700px; height: 156px;" />
<p>Cho ma trận nhị phân <code>m x n</code> <code>grid</code> <strong>được đánh chỉ số từ 0</strong> biểu diễn mảnh đất, hãy trả về <em><strong>tổng số</strong> các vùng đất hình kim tự tháp và kim tự tháp ngược có thể tìm thấy trong</em> <code>grid</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2088.Count%20Fertile%20Pyramids%20in%20a%20Land/images/1.jpg" style="width: 575px; height: 109px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,1,0],[1,1,1,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 2 vùng đất hình kim tự tháp có thể tạo thành được minh họa lần lượt bằng màu xanh dương và đỏ.
Không có vùng đất hình kim tự tháp ngược nào trong lưới này.
Do đó, tổng số vùng đất hình kim tự tháp và kim tự tháp ngược là 2 + 0 = 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2088.Count%20Fertile%20Pyramids%20in%20a%20Land/images/2.jpg" style="width: 502px; height: 120px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,1,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Vùng đất hình kim tự tháp được minh họa bằng màu xanh dương, còn vùng đất hình kim tự tháp ngược được minh họa bằng màu đỏ.
Do đó, tổng số vùng đất là 1 + 1 = 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2088.Count%20Fertile%20Pyramids%20in%20a%20Land/images/3.jpg" style="width: 676px; height: 148px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,1,0],[1,1,1,1,1],[1,1,1,1,1],[0,1,0,0,1]]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Có 7 vùng đất hình kim tự tháp, trong đó 3 vùng được minh họa ở hình thứ 2 và thứ 3.
Có 6 vùng đất hình kim tự tháp ngược, trong đó 2 vùng được minh họa ở hình cuối cùng.
Tổng số vùng đất là 7 + 6 = 13.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>grid[i][j]</code> hoặc là <code>0</code>, hoặc là <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một kim tự tháp dựa trên ba ô ở hàng bên dưới, vì vậy có thể dùng DP theo chiều cao. Điều kiện $mn \le 10^5$ không cho phép kiểm tra mọi đỉnh. $f[i][j]$ là chiều cao lớn nhất của kim tự tháp có đỉnh tại đó (bản thân ô có chiều cao $0$ và không được tính).
>
> Duyệt từ dưới lên: một ô bên trong có màu mỡ nhận giá trị $1+$ giá trị nhỏ nhất trong ba ô bên dưới, rồi cộng chiều cao đó vào kết quả (chiều cao $h$ tạo ra $h$ kim tự tháp). Với kim tự tháp ngược, ta dùng cùng công thức khi duyệt từ trên xuống và tái sử dụng $f$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPyramids(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        f = [[0] * n for _ in range(m)]
        ans = 0
        for i in range(m - 1, -1, -1):
            for j in range(n):
                if grid[i][j] == 0:
                    f[i][j] = -1
                elif not (i == m - 1 or j == 0 or j == n - 1):
                    f[i][j] = min(f[i + 1][j - 1], f[i + 1][j], f[i + 1][j + 1]) + 1
                    ans += f[i][j]
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    f[i][j] = -1
                elif i == 0 or j == 0 or j == n - 1:
                    f[i][j] = 0
                else:
                    f[i][j] = min(f[i - 1][j - 1], f[i - 1][j], f[i - 1][j + 1]) + 1
                    ans += f[i][j]
        return ans
```

#### Java

```java
class Solution {
    public int countPyramids(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] f = new int[m][n];
        int ans = 0;
        for (int i = m - 1; i >= 0; --i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    f[i][j] = -1;
                } else if (i == m - 1 || j == 0 || j == n - 1) {
                    f[i][j] = 0;
                } else {
                    f[i][j] = Math.min(f[i + 1][j - 1], Math.min(f[i + 1][j], f[i + 1][j + 1])) + 1;
                    ans += f[i][j];
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    f[i][j] = -1;
                } else if (i == 0 || j == 0 || j == n - 1) {
                    f[i][j] = 0;
                } else {
                    f[i][j] = Math.min(f[i - 1][j - 1], Math.min(f[i - 1][j], f[i - 1][j + 1])) + 1;
                    ans += f[i][j];
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
    int countPyramids(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int f[m][n];
        int ans = 0;
        for (int i = m - 1; ~i; --i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    f[i][j] = -1;
                } else if (i == m - 1 || j == 0 || j == n - 1) {
                    f[i][j] = 0;
                } else {
                    f[i][j] = min({f[i + 1][j - 1], f[i + 1][j], f[i + 1][j + 1]}) + 1;
                    ans += f[i][j];
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    f[i][j] = -1;
                } else if (i == 0 || j == 0 || j == n - 1) {
                    f[i][j] = 0;
                } else {
                    f[i][j] = min({f[i - 1][j - 1], f[i - 1][j], f[i - 1][j + 1]}) + 1;
                    ans += f[i][j];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPyramids(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
	}
	for i := m - 1; i >= 0; i-- {
		for j := 0; j < n; j++ {
			if grid[i][j] == 0 {
				f[i][j] = -1
			} else if i == m-1 || j == 0 || j == n-1 {
				f[i][j] = 0
			} else {
				f[i][j] = min(f[i+1][j-1], min(f[i+1][j], f[i+1][j+1])) + 1
				ans += f[i][j]
			}
		}
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 0 {
				f[i][j] = -1
			} else if i == 0 || j == 0 || j == n-1 {
				f[i][j] = 0
			} else {
				f[i][j] = min(f[i-1][j-1], min(f[i-1][j], f[i-1][j+1])) + 1
				ans += f[i][j]
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
