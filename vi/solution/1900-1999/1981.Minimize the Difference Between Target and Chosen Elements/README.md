---
comments: true
difficulty: Medium
rating: 2009
source: Weekly Contest 255 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1981. Minimize the Difference Between Target and Chosen Elements](https://leetcode.com/problems/minimize-the-difference-between-target-and-chosen-elements)

[中文文档](/solution/1900-1999/1981.Minimize%20the%20Difference%20Between%20Target%20and%20Chosen%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>m x n</code> <code>mat</code> và một số nguyên <code>target</code>.</p>

<p>Chọn một số nguyên từ <strong>mỗi hàng</strong> của ma trận sao cho <strong>độ lệch tuyệt đối</strong> giữa <code>target</code> và <strong>tổng</strong> của các phần tử đã chọn là <strong>nhỏ nhất</strong>.</p>

<p>Trả về <em><strong>độ lệch tuyệt đối nhỏ nhất</strong></em>.</p>

<p><strong>Độ lệch tuyệt đối</strong> giữa hai số <code>a</code> và <code>b</code> là giá trị tuyệt đối của <code>a - b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1981.Minimize%20the%20Difference%20Between%20Target%20and%20Chosen%20Elements/images/matrix1.png" style="width: 181px; height: 181px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1,2,3],[4,5,6],[7,8,9]], target = 13
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Một cách chọn có thể là:
- Chọn 1 từ hàng đầu tiên.
- Chọn 5 từ hàng thứ hai.
- Chọn 7 từ hàng thứ ba.
Tổng các phần tử đã chọn là 13, bằng với target, nên độ lệch tuyệt đối là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1981.Minimize%20the%20Difference%20Between%20Target%20and%20Chosen%20Elements/images/matrix1-1.png" style="width: 61px; height: 181px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1],[2],[3]], target = 100
<strong>Đầu ra:</strong> 94
<strong>Giải thích:</strong> Lựa chọn tốt nhất có thể là:
- Chọn 1 từ hàng đầu tiên.
- Chọn 2 từ hàng thứ hai.
- Chọn 3 từ hàng thứ ba.
Tổng là 6 và độ lệch tuyệt đối là 94.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1981.Minimize%20the%20Difference%20Between%20Target%20and%20Chosen%20Elements/images/matrix1-3.png" style="width: 301px; height: 61px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1,2,9,8,7]], target = 6
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Lựa chọn tốt nhất là chọn 7 từ hàng đầu tiên.
Độ lệch tuyệt đối là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 70</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 70</code></li>
	<li><code>1 &lt;= target &lt;= 800</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Ba lô theo nhóm)

<!-- thinking:start -->

> **Tư duy**
>
> Việc chọn một giá trị từ mỗi hàng để tối thiểu hóa $|sum-\textit{target}|$ có độ phức tạp theo hàm mũ. Số hàng và các giá trị đều không vượt quá $70$, nên các tổng có thể đạt được có thể được lưu trong một mảng trạng thái cuốn chiếu.
>
> Với mỗi hàng mới, ta thay $f$ bằng tập $\{a+b\mid a\in f,\,b\in\textit{row}\}$. Giá trị gần $\textit{target}$ nhất là đáp án.
>
> Việc gộp các trạng thái trùng nhau giúp tập trạng thái nhỏ hơn nhiều so với tích số tổ hợp ban đầu.

<!-- thinking:end -->

Gọi $f[i][j]$ là trạng thái cho biết có thể chọn các phần tử từ $i$ hàng đầu tiên để có tổng bằng $j$ hay không. Khi đó, ta có công thức chuyển trạng thái:

$$
f[i][j] = \begin{cases} 1 & \textit{if there exists } x \in row[i] \textit{ such that } f[i - 1][j - x] = 1 \\ 0 & \textit{otherwise} \end{cases}
$$

Trong đó, $row[i]$ biểu diễn tập các phần tử ở hàng thứ $i$.

Vì $f[i][j]$ chỉ liên quan đến $f[i - 1][j]$, ta có thể sử dụng mảng cuốn chiếu để tối ưu độ phức tạp không gian.

Cuối cùng, duyệt qua mảng $f$ để tìm độ lệch tuyệt đối nhỏ nhất.

Độ phức tạp thời gian là $O(m^2 \times n \times C)$ và độ phức tạp không gian là $O(m \times C)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận, còn $C$ là giá trị lớn nhất của các phần tử trong ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeTheDifference(self, mat: List[List[int]], target: int) -> int:
        f = [True]
        for row in mat:
            mx = max(row)
            g = [False] * (len(f) + mx)
            for x in row:
                for j in range(x, len(f) + x):
                    g[j] |= f[j - x]
            f = g
        return min(abs(j - target) for j, ok in enumerate(f) if ok)
```

#### Java

```java
class Solution {
    public int minimizeTheDifference(int[][] mat, int target) {
        boolean[] f = {true};
        for (var row : mat) {
            int mx = 0;
            for (int x : row) {
                mx = Math.max(mx, x);
            }
            boolean[] g = new boolean[f.length + mx];
            for (int x : row) {
                for (int j = x; j < f.length + x; ++j) {
                    g[j] |= f[j - x];
                }
            }
            f = g;
        }
        int ans = 1 << 30;
        for (int j = 0; j < f.length; ++j) {
            if (f[j]) {
                ans = Math.min(ans, Math.abs(j - target));
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
    int minimizeTheDifference(vector<vector<int>>& mat, int target) {
        vector<int> f = {1};
        for (auto& row : mat) {
            int mx = *max_element(row.begin(), row.end());
            vector<int> g(f.size() + mx);
            for (int x : row) {
                for (int j = x; j < f.size() + x; ++j) {
                    g[j] |= f[j - x];
                }
            }
            f = move(g);
        }
        int ans = 1 << 30;
        for (int j = 0; j < f.size(); ++j) {
            if (f[j]) {
                ans = min(ans, abs(j - target));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimizeTheDifference(mat [][]int, target int) int {
	f := []int{1}
	for _, row := range mat {
		mx := slices.Max(row)
		g := make([]int, len(f)+mx)
		for _, x := range row {
			for j := x; j < len(f)+x; j++ {
				g[j] |= f[j-x]
			}
		}
		f = g
	}
	ans := 1 << 30
	for j, v := range f {
		if v == 1 {
			ans = min(ans, abs(j-target))
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
