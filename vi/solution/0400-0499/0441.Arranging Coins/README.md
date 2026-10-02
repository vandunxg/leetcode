---
comments: true
difficulty: Easy
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [441. Arranging Coins](https://leetcode.com/problems/arranging-coins)

[中文文档](/solution/0400-0499/0441.Arranging%20Coins/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> đồng xu và muốn dùng chúng để xếp thành cầu thang. Cầu thang gồm <code>k</code> hàng, trong đó hàng <code>i<sup>th</sup></code> có đúng <code>i</code> đồng xu. Hàng cuối cùng của cầu thang <strong>có thể</strong> chưa hoàn chỉnh.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số <strong>hàng hoàn chỉnh</strong> trong cầu thang bạn xếp được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0441.Arranging%20Coins/images/arrangecoins1-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Vì hàng thứ 3 chưa hoàn chỉnh nên ta trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0441.Arranging%20Coins/images/arrangecoins2-grid.jpg" style="width: 333px; height: 333px;" />
<pre>
<strong>Đầu vào:</strong> n = 8
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Vì hàng thứ 4 chưa hoàn chỉnh nên ta trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $x$ cần $x$ đồng xu, nên hàng hoàn chỉnh cuối cùng thỏa mãn $x(x+1)/2\le n$. Duyệt lần lượt $x$ sẽ tốn thời gian tuyến tính, trong khi $n$ có thể lên đến $2^{31}-1$.
>
> Giải bất đẳng thức ta được $x\le \sqrt{2}\,\sqrt{n+1/8}-1/2$. Viết biểu thức tách riêng như vậy giúp tránh tràn số khi tính $2n$ ở một số ngôn ngữ.
>
> Công thức đóng này được tính bằng số phép toán dấu phẩy động không đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrangeCoins(self, n: int) -> int:
        return int(math.sqrt(2) * math.sqrt(n + 0.125) - 0.5)
```

#### Java

```java
class Solution {
    public int arrangeCoins(int n) {
        return (int) (Math.sqrt(2) * Math.sqrt(n + 0.125) - 0.5);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int arrangeCoins(int n) {
        return (int) (sqrt(2) * sqrt(n + 0.125) - 0.5);
    }
};
```

#### Go

```go
func arrangeCoins(n int) int {
	return int(math.Sqrt(2)*math.Sqrt(float64(n)+0.125) - 0.5)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng căn bậc hai; với $n$ rất lớn, kết quả có thể mất độ chính xác nguyên. Dùng tìm kiếm nhị phân để tìm số hàng, kiểm tra bằng phép toán nguyên $mid(mid+1)/2\le n$ và làm tròn $mid$ lên khi điều kiện thỏa mãn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrangeCoins(self, n: int) -> int:
        left, right = 1, n
        while left < right:
            mid = (left + right + 1) >> 1
            if (1 + mid) * mid // 2 <= n:
                left = mid
            else:
                right = mid - 1
        return left
```

#### Java

```java
class Solution {
    public int arrangeCoins(int n) {
        long left = 1, right = n;
        while (left < right) {
            long mid = (left + right + 1) >>> 1;
            if ((1 + mid) * mid / 2 <= n) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return (int) left;
    }
}
```

#### C++

```cpp
using LL = long;

class Solution {
public:
    int arrangeCoins(int n) {
        LL left = 1, right = n;
        while (left < right) {
            LL mid = left + right + 1 >> 1;
            LL s = (1 + mid) * mid >> 1;
            if (n < s)
                right = mid - 1;
            else
                left = mid;
        }
        return left;
    }
};
```

#### Go

```go
func arrangeCoins(n int) int {
	left, right := 1, n
	for left < right {
		mid := (left + right + 1) >> 1
		if (1+mid)*mid/2 <= n {
			left = mid
		} else {
			right = mid - 1
		}
	}
	return left
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
