---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [562. Longest Line of Consecutive One in Matrix 🔒](https://leetcode.com/problems/longest-line-of-consecutive-one-in-matrix)

[中文文档](/solution/0500-0599/0562.Longest%20Line%20of%20Consecutive%20One%20in%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>mat</code>, hãy trả về <em>độ dài của dãy số 1 liên tiếp dài nhất trong ma trận</em>.</p>

<p>Dãy có thể nằm ngang, dọc, theo đường chéo hoặc đường chéo phụ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0562.Longest%20Line%20of%20Consecutive%20One%20in%20Matrix/images/long1-grid.jpg" style="width: 333px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[0,1,1,0],[0,1,1,0],[0,0,0,1]]
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0562.Longest%20Line%20of%20Consecutive%20One%20in%20Matrix/images/long2-grid.jpg" style="width: 333px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1,1,1,1],[0,1,1,0],[0,0,0,1]]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>4</sup></code></li>
	<li><code>mat[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Dãy số 1 dài nhất có thể nằm ngang, dọc hoặc theo một trong hai đường chéo. Nếu mở rộng từ mọi điểm bắt đầu thì độ phức tạp là $O(mn \cdot (m+n))$.
>
> Dùng bốn bảng để lưu độ dài dãy kết thúc tại $(i,j)$ theo từng hướng, lấy giá trị ở ô phía trên, bên trái, trên-trái hoặc trên-phải rồi cộng thêm một. Các ô có giá trị 0 giữ nguyên bằng 0. Thêm viền rộng một ô để không cần kiểm tra biên. Cập nhật giá trị lớn nhất toàn cục.

<!-- thinking:end -->

Ta định nghĩa $f[i][j][k]$ là độ dài dãy số $1$ liên tiếp dài nhất kết thúc tại $(i, j)$ theo hướng $k$. $k$ nhận các giá trị $0, 1, 2, 3$, lần lượt ứng với hướng ngang, dọc, chéo và chéo phụ.

> Ta cũng có thể dùng bốn mảng 2D để lưu độ dài dãy số $1$ liên tiếp dài nhất theo bốn hướng.

Ta duyệt ma trận và khi gặp giá trị $1$, cập nhật $f[i][j][k]$. Với mỗi vị trí $(i, j)$, chỉ cần cập nhật giá trị theo bốn hướng rồi cập nhật đáp án.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestLine(self, mat: List[List[int]]) -> int:
        m, n = len(mat), len(mat[0])
        a = [[0] * (n + 2) for _ in range(m + 2)]
        b = [[0] * (n + 2) for _ in range(m + 2)]
        c = [[0] * (n + 2) for _ in range(m + 2)]
        d = [[0] * (n + 2) for _ in range(m + 2)]
        ans = 0
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if mat[i - 1][j - 1]:
                    a[i][j] = a[i - 1][j] + 1
                    b[i][j] = b[i][j - 1] + 1
                    c[i][j] = c[i - 1][j - 1] + 1
                    d[i][j] = d[i - 1][j + 1] + 1
                    ans = max(ans, a[i][j], b[i][j], c[i][j], d[i][j])
        return ans
```

#### Java

```java
class Solution {
    public int longestLine(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int[][] a = new int[m + 2][n + 2];
        int[][] b = new int[m + 2][n + 2];
        int[][] c = new int[m + 2][n + 2];
        int[][] d = new int[m + 2][n + 2];
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (mat[i - 1][j - 1] == 1) {
                    a[i][j] = a[i - 1][j] + 1;
                    b[i][j] = b[i][j - 1] + 1;
                    c[i][j] = c[i - 1][j - 1] + 1;
                    d[i][j] = d[i - 1][j + 1] + 1;
                    ans = max(ans, a[i][j], b[i][j], c[i][j], d[i][j]);
                }
            }
        }
        return ans;
    }

    private int max(int... arr) {
        int ans = 0;
        for (int v : arr) {
            ans = Math.max(ans, v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestLine(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        vector<vector<int>> a(m + 2, vector<int>(n + 2));
        vector<vector<int>> b(m + 2, vector<int>(n + 2));
        vector<vector<int>> c(m + 2, vector<int>(n + 2));
        vector<vector<int>> d(m + 2, vector<int>(n + 2));
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (mat[i - 1][j - 1]) {
                    a[i][j] = a[i - 1][j] + 1;
                    b[i][j] = b[i][j - 1] + 1;
                    c[i][j] = c[i - 1][j - 1] + 1;
                    d[i][j] = d[i - 1][j + 1] + 1;
                    ans = max(ans, max(a[i][j], max(b[i][j], max(c[i][j], d[i][j]))));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestLine(mat [][]int) (ans int) {
	m, n := len(mat), len(mat[0])
	f := make([][][4]int, m+2)
	for i := range f {
		f[i] = make([][4]int, n+2)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if mat[i-1][j-1] == 1 {
				f[i][j][0] = f[i-1][j][0] + 1
				f[i][j][1] = f[i][j-1][1] + 1
				f[i][j][2] = f[i-1][j-1][2] + 1
				f[i][j][3] = f[i-1][j+1][3] + 1
				for _, v := range f[i][j] {
					if ans < v {
						ans = v
					}
				}
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
