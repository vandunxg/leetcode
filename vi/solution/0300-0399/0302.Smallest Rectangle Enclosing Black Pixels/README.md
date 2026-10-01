---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Binary Search
    - Matrix
---

<!-- problem:start -->

# [302. Smallest Rectangle Enclosing Black Pixels 🔒](https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels)

[中文文档](/solution/0300-0399/0302.Smallest%20Rectangle%20Enclosing%20Black%20Pixels/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>image</code>, trong đó <code>0</code> biểu thị pixel trắng và <code>1</code> biểu thị pixel đen.</p>

<p>Các pixel đen liên thông với nhau (tức là chỉ có một vùng đen). Hai pixel được xem là liên thông nếu nằm cạnh nhau theo chiều ngang hoặc chiều dọc.</p>

<p>Cho hai số nguyên <code>x</code> và <code>y</code> biểu thị tọa độ của một pixel đen. Hãy trả về <em>diện tích hình chữ nhật nhỏ nhất (có các cạnh song song với trục tọa độ) bao quanh tất cả pixel đen</em>.</p>

<p>Bạn phải viết thuật toán có độ phức tạp thời gian nhỏ hơn <code>O(mn)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0302.Smallest%20Rectangle%20Enclosing%20Black%20Pixels/images/pixel-grid.jpg" style="width: 333px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> image = [[&quot;0&quot;,&quot;0&quot;,&quot;1&quot;,&quot;0&quot;],[&quot;0&quot;,&quot;1&quot;,&quot;1&quot;,&quot;0&quot;],[&quot;0&quot;,&quot;1&quot;,&quot;0&quot;,&quot;0&quot;]], x = 0, y = 2
<strong>Đầu ra:</strong> 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> image = [[&quot;1&quot;]], x = 0, y = 0
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == image.length</code></li>
	<li><code>n == image[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>image[i][j]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>0 &lt;= x &lt; m</code></li>
	<li><code>0 &lt;= y &lt; n</code></li>
	<li><code>image[x][y] == &#39;1&#39;.</code></li>
	<li>Các pixel đen trong <code>image</code> chỉ tạo thành <strong>một thành phần liên thông</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các pixel đen nằm trong một hình chữ nhật song song với các trục tọa độ; ta cần tính diện tích hình đó. Có thể flood-fill từ ô đen đã cho hoặc quét toàn bộ ma trận, nhưng duyệt mọi ô sẽ tốn kém khi $m$ và $n$ lớn.
>
> Việc một hàng (cột) có chứa pixel đen hay không biến thiên đơn điệu theo chỉ số hàng (cột), và ô đã biết $(x,y)$ nằm bên trong hình chữ nhật. Dùng tìm kiếm nhị phân để xác định bốn cạnh: kiểm tra xem hàng/cột ở giữa có chứa `'1'` hay không. Các cận hội tụ trong $O((m+n)\log(mn))$ thời gian; diện tích bằng chiều cao nhân chiều rộng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minArea(self, image: List[List[str]], x: int, y: int) -> int:
        m, n = len(image), len(image[0])
        left, right = 0, x
        while left < right:
            mid = (left + right) >> 1
            c = 0
            while c < n and image[mid][c] == '0':
                c += 1
            if c < n:
                right = mid
            else:
                left = mid + 1
        u = left
        left, right = x, m - 1
        while left < right:
            mid = (left + right + 1) >> 1
            c = 0
            while c < n and image[mid][c] == '0':
                c += 1
            if c < n:
                left = mid
            else:
                right = mid - 1
        d = left
        left, right = 0, y
        while left < right:
            mid = (left + right) >> 1
            r = 0
            while r < m and image[r][mid] == '0':
                r += 1
            if r < m:
                right = mid
            else:
                left = mid + 1
        l = left
        left, right = y, n - 1
        while left < right:
            mid = (left + right + 1) >> 1
            r = 0
            while r < m and image[r][mid] == '0':
                r += 1
            if r < m:
                left = mid
            else:
                right = mid - 1
        r = left
        return (d - u + 1) * (r - l + 1)
```

#### Java

```java
class Solution {

    public int minArea(char[][] image, int x, int y) {
        int m = image.length, n = image[0].length;
        int left = 0, right = x;
        while (left < right) {
            int mid = (left + right) >> 1;
            int c = 0;
            while (c < n && image[mid][c] == '0') {
                ++c;
            }
            if (c < n) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        int u = left;
        left = x;
        right = m - 1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            int c = 0;
            while (c < n && image[mid][c] == '0') {
                ++c;
            }
            if (c < n) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        int d = left;
        left = 0;
        right = y;
        while (left < right) {
            int mid = (left + right) >> 1;
            int r = 0;
            while (r < m && image[r][mid] == '0') {
                ++r;
            }
            if (r < m) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        int l = left;
        left = y;
        right = n - 1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            int r = 0;
            while (r < m && image[r][mid] == '0') {
                ++r;
            }
            if (r < m) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        int r = left;
        return (d - u + 1) * (r - l + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minArea(vector<vector<char>>& image, int x, int y) {
        int m = image.size(), n = image[0].size();
        int left = 0, right = x;
        while (left < right) {
            int mid = (left + right) >> 1;
            int c = 0;
            while (c < n && image[mid][c] == '0') ++c;
            if (c < n)
                right = mid;
            else
                left = mid + 1;
        }
        int u = left;
        left = x;
        right = m - 1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            int c = 0;
            while (c < n && image[mid][c] == '0') ++c;
            if (c < n)
                left = mid;
            else
                right = mid - 1;
        }
        int d = left;
        left = 0;
        right = y;
        while (left < right) {
            int mid = (left + right) >> 1;
            int r = 0;
            while (r < m && image[r][mid] == '0') ++r;
            if (r < m)
                right = mid;
            else
                left = mid + 1;
        }
        int l = left;
        left = y;
        right = n - 1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            int r = 0;
            while (r < m && image[r][mid] == '0') ++r;
            if (r < m)
                left = mid;
            else
                right = mid - 1;
        }
        int r = left;
        return (d - u + 1) * (r - l + 1);
    }
};
```

#### Go

```go
func minArea(image [][]byte, x int, y int) int {
	m, n := len(image), len(image[0])
	left, right := 0, x
	for left < right {
		mid := (left + right) >> 1
		c := 0
		for c < n && image[mid][c] == '0' {
			c++
		}
		if c < n {
			right = mid
		} else {
			left = mid + 1
		}
	}
	u := left
	left, right = x, m-1
	for left < right {
		mid := (left + right + 1) >> 1
		c := 0
		for c < n && image[mid][c] == '0' {
			c++
		}
		if c < n {
			left = mid
		} else {
			right = mid - 1
		}
	}
	d := left
	left, right = 0, y
	for left < right {
		mid := (left + right) >> 1
		r := 0
		for r < m && image[r][mid] == '0' {
			r++
		}
		if r < m {
			right = mid
		} else {
			left = mid + 1
		}
	}
	l := left
	left, right = y, n-1
	for left < right {
		mid := (left + right + 1) >> 1
		r := 0
		for r < m && image[r][mid] == '0' {
			r++
		}
		if r < m {
			left = mid
		} else {
			right = mid - 1
		}
	}
	r := left
	return (d - u + 1) * (r - l + 1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
