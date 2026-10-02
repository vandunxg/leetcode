---
comments: true
difficulty: Hard
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [668. Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table)

[中文文档](/solution/0600-0699/0668.Kth%20Smallest%20Number%20in%20Multiplication%20Table/README.md)

## Mô tả

<!-- description:start -->

<p>Hầu như ai cũng từng dùng <a href="https://en.wikipedia.org/wiki/Multiplication_table" target="_blank">bảng nhân</a>. Bảng nhân kích thước <code>m x n</code> là ma trận số nguyên <code>mat</code>, trong đó <code>mat[i][j] == i * j</code> (đánh chỉ số từ <strong>1</strong>).</p>

<p>Cho ba số nguyên <code>m</code>, <code>n</code> và <code>k</code>, hãy trả về <em>phần tử nhỏ thứ </em><code>k<sup>th</sup></code><em> trong bảng nhân kích thước </em><code>m x n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0668.Kth%20Smallest%20Number%20in%20Multiplication%20Table/images/multtable1-grid.jpg" style="width: 500px; height: 254px;" />
<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, k = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Số nhỏ thứ 5 là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0668.Kth%20Smallest%20Number%20in%20Multiplication%20Table/images/multtable2-grid.jpg" style="width: 493px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> m = 2, n = 3, k = 6
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Số nhỏ thứ 6 là 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= m * n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Không thể tạo toàn bộ bảng nhân kích thước $m\times n$ để tìm phần tử nhỏ thứ $k$ khi kích thước mỗi cạnh có thể lên đến $3\cdot 10^4$.
>
> Dùng binary search trên giá trị $x$. Hàng $i$ có $\min(\lfloor x/i\rfloor, n)$ phần tử $\le x$. Đáp án là giá trị $x$ nhỏ nhất sao cho số phần tử đếm được $\ge k$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKthNumber(self, m: int, n: int, k: int) -> int:
        left, right = 1, m * n
        while left < right:
            mid = (left + right) >> 1
            cnt = 0
            for i in range(1, m + 1):
                cnt += min(mid // i, n)
            if cnt >= k:
                right = mid
            else:
                left = mid + 1
        return left
```

#### Java

```java
class Solution {
    public int findKthNumber(int m, int n, int k) {
        int left = 1, right = m * n;
        while (left < right) {
            int mid = (left + right) >>> 1;
            int cnt = 0;
            for (int i = 1; i <= m; ++i) {
                cnt += Math.min(mid / i, n);
            }
            if (cnt >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findKthNumber(int m, int n, int k) {
        int left = 1, right = m * n;
        while (left < right) {
            int mid = (left + right) >> 1;
            int cnt = 0;
            for (int i = 1; i <= m; ++i) cnt += min(mid / i, n);
            if (cnt >= k)
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }
};
```

#### Go

```go
func findKthNumber(m int, n int, k int) int {
	left, right := 1, m*n
	for left < right {
		mid := (left + right) >> 1
		cnt := 0
		for i := 1; i <= m; i++ {
			cnt += min(mid/i, n)
		}
		if cnt >= k {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
