---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [764. Largest Plus Sign](https://leetcode.com/problems/largest-plus-sign)

[中文文档](/solution/0700-0799/0764.Largest%20Plus%20Sign/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>. Bạn có một lưới nhị phân <code>n x n</code> tên là <code>grid</code>, ban đầu tất cả giá trị đều bằng <code>1</code>, ngoại trừ một số ô có chỉ số được cho trong mảng <code>mines</code>. Phần tử thứ <code>i<sup>th</sup></code> của mảng <code>mines</code> được định nghĩa là <code>mines[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>, trong đó <code>grid[x<sub>i</sub>][y<sub>i</sub>] == 0</code>.</p>

<p>Hãy trả về <em>cấp độ của dấu cộng lớn nhất </em>1<em> theo trục nằm trong </em><code>grid</code>. Nếu không có dấu cộng nào, trả về <code>0</code>.</p>

<p>Dấu cộng <strong>nằm theo trục</strong> gồm các số <code>1</code>, có cấp độ <code>k</code>, sẽ có tâm tại một ô <code>grid[r][c] == 1</code> cùng bốn nhánh dài <code>k - 1</code> hướng lên, xuống, trái và phải, tất cả đều gồm các số <code>1</code>. Lưu ý rằng bên ngoài các nhánh của dấu cộng có thể có số <code>0</code> hoặc <code>1</code>; chỉ cần kiểm tra các ô thuộc phạm vi dấu cộng có giá trị <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0764.Largest%20Plus%20Sign/images/plus1-grid.jpg" style="width: 404px; height: 405px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, mines = [[4,2]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong lưới phía trên, dấu cộng lớn nhất chỉ có thể đạt cấp độ 2. Một trong các dấu cộng đó được minh họa trong hình.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0764.Largest%20Plus%20Sign/images/plus2-grid.jpg" style="width: 84px; height: 85px;" />
<pre>
<strong>Đầu vào:</strong> n = 1, mines = [[0,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có dấu cộng nào, nên trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>1 &lt;= mines.length &lt;= 5000</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt; n</code></li>
	<li>Tất cả các cặp <code>(x<sub>i</sub>, y<sub>i</sub>)</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm dấu cộng lớn nhất cấp độ $k$ trong lưới $n\le 500$. Mở rộng từ từng tâm sẽ tốn $O(n^3)$. Cấp độ của dấu cộng bằng độ dài nhỏ nhất của nhánh gồm các số $1$ trong bốn hướng.
>
> Bốn đoạn liên tiếp này có thể được tính bằng các lượt duyệt tuyến tính. Đánh dấu các ô mìn là $0$, khởi tạo những ô còn lại bằng $n$, rồi lấy min với bốn bộ đếm.
>
> Một lượt duyệt với hai chỉ số cập nhật đồng thời các hướng trái, phải, lên và xuống. Đáp án là giá trị lớn nhất trong các ô. Độ phức tạp là $O(n^2)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def orderOfLargestPlusSign(self, n: int, mines: List[List[int]]) -> int:
        dp = [[n] * n for _ in range(n)]
        for x, y in mines:
            dp[x][y] = 0
        for i in range(n):
            left = right = up = down = 0
            for j, k in zip(range(n), reversed(range(n))):
                left = left + 1 if dp[i][j] else 0
                right = right + 1 if dp[i][k] else 0
                up = up + 1 if dp[j][i] else 0
                down = down + 1 if dp[k][i] else 0
                dp[i][j] = min(dp[i][j], left)
                dp[i][k] = min(dp[i][k], right)
                dp[j][i] = min(dp[j][i], up)
                dp[k][i] = min(dp[k][i], down)
        return max(max(v) for v in dp)
```

#### Java

```java
class Solution {
    public int orderOfLargestPlusSign(int n, int[][] mines) {
        int[][] dp = new int[n][n];
        for (var e : dp) {
            Arrays.fill(e, n);
        }
        for (var e : mines) {
            dp[e[0]][e[1]] = 0;
        }
        for (int i = 0; i < n; ++i) {
            int left = 0, right = 0, up = 0, down = 0;
            for (int j = 0, k = n - 1; j < n; ++j, --k) {
                left = dp[i][j] > 0 ? left + 1 : 0;
                right = dp[i][k] > 0 ? right + 1 : 0;
                up = dp[j][i] > 0 ? up + 1 : 0;
                down = dp[k][i] > 0 ? down + 1 : 0;
                dp[i][j] = Math.min(dp[i][j], left);
                dp[i][k] = Math.min(dp[i][k], right);
                dp[j][i] = Math.min(dp[j][i], up);
                dp[k][i] = Math.min(dp[k][i], down);
            }
        }
        return Arrays.stream(dp).flatMapToInt(Arrays::stream).max().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int orderOfLargestPlusSign(int n, vector<vector<int>>& mines) {
        vector<vector<int>> dp(n, vector<int>(n, n));
        for (auto& e : mines) dp[e[0]][e[1]] = 0;
        for (int i = 0; i < n; ++i) {
            int left = 0, right = 0, up = 0, down = 0;
            for (int j = 0, k = n - 1; j < n; ++j, --k) {
                left = dp[i][j] ? left + 1 : 0;
                right = dp[i][k] ? right + 1 : 0;
                up = dp[j][i] ? up + 1 : 0;
                down = dp[k][i] ? down + 1 : 0;
                dp[i][j] = min(dp[i][j], left);
                dp[i][k] = min(dp[i][k], right);
                dp[j][i] = min(dp[j][i], up);
                dp[k][i] = min(dp[k][i], down);
            }
        }
        int ans = 0;
        for (auto& e : dp) ans = max(ans, *max_element(e.begin(), e.end()));
        return ans;
    }
};
```

#### Go

```go
func orderOfLargestPlusSign(n int, mines [][]int) (ans int) {
	dp := make([][]int, n)
	for i := range dp {
		dp[i] = make([]int, n)
		for j := range dp[i] {
			dp[i][j] = n
		}
	}
	for _, e := range mines {
		dp[e[0]][e[1]] = 0
	}
	for i := 0; i < n; i++ {
		var left, right, up, down int
		for j, k := 0, n-1; j < n; j, k = j+1, k-1 {
			left, right, up, down = left+1, right+1, up+1, down+1
			if dp[i][j] == 0 {
				left = 0
			}
			if dp[i][k] == 0 {
				right = 0
			}
			if dp[j][i] == 0 {
				up = 0
			}
			if dp[k][i] == 0 {
				down = 0
			}
			dp[i][j] = min(dp[i][j], left)
			dp[i][k] = min(dp[i][k], right)
			dp[j][i] = min(dp[j][i], up)
			dp[k][i] = min(dp[k][i], down)
		}
	}
	for _, e := range dp {
		ans = max(ans, slices.Max(e))
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
