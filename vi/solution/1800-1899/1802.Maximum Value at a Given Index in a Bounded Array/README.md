---
comments: true
difficulty: Medium
rating: 1929
source: Weekly Contest 233 Q3
tags:
    - Greedy
    - Math
    - Binary Search
---

<!-- problem:start -->

# [1802. Maximum Value at a Given Index in a Bounded Array](https://leetcode.com/problems/maximum-value-at-a-given-index-in-a-bounded-array)

[中文文档](/solution/1800-1899/1802.Maximum%20Value%20at%20a%20Given%20Index%20in%20a%20Bounded%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên dương: <code>n</code>, <code>index</code> và <code>maxSum</code>. Hãy xây dựng một mảng <code>nums</code> (<strong>đánh chỉ số từ 0</strong>)<strong> </strong>thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>nums.length == n</code></li>
	<li><code>nums[i]</code> là số nguyên <strong>dương</strong> với <code>0 &lt;= i &lt; n</code>.</li>
	<li><code>abs(nums[i] - nums[i+1]) &lt;= 1</code> với <code>0 &lt;= i &lt; n-1</code>.</li>
	<li>Tổng tất cả phần tử của <code>nums</code> không vượt quá <code>maxSum</code>.</li>
	<li><code>nums[index]</code> được <strong>tối đa hóa</strong>.</li>
</ul>

<p>Hãy trả về <code>nums[index]</code><em> của mảng được xây dựng</em>.</p>

<p>Lưu ý rằng <code>abs(x)</code> bằng <code>x</code> nếu <code>x &gt;= 0</code>, và bằng <code>-x</code> trong trường hợp ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, index = 2,  maxSum = 6
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> nums = [1,2,<u><strong>2</strong></u>,1] là một mảng thỏa mãn mọi điều kiện.
Không có mảng nào thỏa mãn mọi điều kiện và có nums[2] == 3, nên 2 là giá trị nums[2] lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, index = 1,  maxSum = 10
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= maxSum &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= index &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tối đa hóa $nums[\textit{index}]$ với điều kiện hiệu giữa hai phần tử kề nhau không quá $1$ và tổng bị giới hạn bởi $\textit{maxSum}$. Thử từng giá trị ứng viên rồi xây dựng mảng tốn $O(n)$ cho mỗi lần kiểm tra, trong khi giá trị có thể lớn tới $\textit{maxSum}$, nên quá chậm.
>
> Khi cố định $nums[\textit{index}]=x$, mảng có tổng nhỏ nhất sẽ giảm dần ở hai phía theo $x-1,x-2,\ldots$ và giữ ở $1$ sau đó. Tổng nhỏ nhất này tăng theo $x$, nên ta tìm kiếm nhị phân $x$ và kiểm tra công thức tổng với $\textit{maxSum}$. Hàm hỗ trợ $\textit{sum}(x,\textit{cnt})$ phân biệt trường hợp $x$ đủ lớn để phủ hết $\textit{cnt}$ vị trí.

<!-- thinking:end -->

Theo mô tả bài toán, nếu xác định giá trị của $nums[index]$ là $x$, ta có thể tìm được tổng nhỏ nhất của mảng. Cụ thể, các phần tử bên trái của $index$ giảm từ $x-1$ xuống $1$; nếu vẫn còn phần tử thì các phần tử còn lại đều bằng $1$. Tương tự, phần tử tại $index$ và các phần tử bên phải giảm từ $x$ xuống $1$; nếu vẫn còn phần tử thì các phần tử còn lại đều bằng $1$.

Theo cách này, ta có thể tính tổng mảng. Nếu tổng nhỏ hơn hoặc bằng $maxSum$, thì $x$ hiện tại là hợp lệ. Khi $x$ tăng, tổng mảng cũng tăng, nên ta có thể dùng tìm kiếm nhị phân để tìm $x$ lớn nhất thỏa mãn điều kiện.

Để thuận tiện tính tổng các phần tử ở bên trái và bên phải mảng, ta định nghĩa hàm $sum(x, cnt)$, biểu diễn tổng của một mảng có $cnt$ phần tử và giá trị lớn nhất là $x$. Hàm $sum(x, cnt)$ có hai trường hợp:

- Nếu $x \geq cnt$, tổng mảng là $\frac{(x + x - cnt + 1) \times cnt}{2}$
- Nếu $x \lt cnt$, tổng mảng là $\frac{(x + 1) \times x}{2} + cnt - x$

Tiếp theo, đặt biên trái của tìm kiếm nhị phân là $left = 1$, biên phải là $right = maxSum$, rồi tìm kiếm giá trị $mid$ của $nums[index]$. Nếu $sum(mid - 1, index) + sum(mid, n - index) \leq maxSum$, thì $mid$ hiện tại hợp lệ và ta cập nhật $left$ thành $mid$; ngược lại, cập nhật $right$ thành $mid - 1$.

Cuối cùng, trả về $left$ làm đáp án.

Độ phức tạp thời gian là $O(\log M)$, trong đó $M=maxSum$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValue(self, n: int, index: int, maxSum: int) -> int:
        def sum(x, cnt):
            return (
                (x + x - cnt + 1) * cnt // 2 if x >= cnt else (x + 1) * x // 2 + cnt - x
            )

        left, right = 1, maxSum
        while left < right:
            mid = (left + right + 1) >> 1
            if sum(mid - 1, index) + sum(mid, n - index) <= maxSum:
                left = mid
            else:
                right = mid - 1
        return left
```

#### Java

```java
class Solution {
    public int maxValue(int n, int index, int maxSum) {
        int left = 1, right = maxSum;
        while (left < right) {
            int mid = (left + right + 1) >>> 1;
            if (sum(mid - 1, index) + sum(mid, n - index) <= maxSum) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

    private long sum(long x, int cnt) {
        return x >= cnt ? (x + x - cnt + 1) * cnt / 2 : (x + 1) * x / 2 + cnt - x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxValue(int n, int index, int maxSum) {
        auto sum = [](long x, int cnt) {
            return x >= cnt ? (x + x - cnt + 1) * cnt / 2 : (x + 1) * x / 2 + cnt - x;
        };
        int left = 1, right = maxSum;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (sum(mid - 1, index) + sum(mid, n - index) <= maxSum) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func maxValue(n int, index int, maxSum int) int {
	sum := func(x, cnt int) int {
		if x >= cnt {
			return (x + x - cnt + 1) * cnt / 2
		}
		return (x+1)*x/2 + cnt - x
	}
	return sort.Search(maxSum, func(x int) bool {
		x++
		return sum(x-1, index)+sum(x, n-index) > maxSum
	})
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
