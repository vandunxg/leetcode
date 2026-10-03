---
comments: true
difficulty: Medium
rating: 1758
source: Weekly Contest 252 Q3
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [1954. Minimum Garden Perimeter to Collect Enough Apples](https://leetcode.com/problems/minimum-garden-perimeter-to-collect-enough-apples)

[中文文档](/solution/1900-1999/1954.Minimum%20Garden%20Perimeter%20to%20Collect%20Enough%20Apples/README.md)

## Mô tả

<!-- description:start -->

<p>Một khu vườn được biểu diễn bằng một lưới 2D vô hạn, trong đó tại <strong>mọi</strong> tọa độ nguyên đều có một cây táo. Cây táo tại tọa độ nguyên <code>(i, j)</code> có <code>|i| + |j|</code> quả táo.</p>

<p>Bạn sẽ mua một <strong>mảnh đất hình vuông</strong> có các cạnh song song với trục tọa độ và tâm tại <code>(0, 0)</code>.</p>

<p>Cho một số nguyên <code>neededApples</code>, hãy trả về <em><strong>chu vi nhỏ nhất</strong> của mảnh đất sao cho <strong>có ít nhất</strong></em><strong> </strong><code>neededApples</code> <em>quả táo nằm <strong>bên trong hoặc trên</strong> chu vi của mảnh đất đó</em>.</p>

<p>Giá trị của <code>|x|</code> được định nghĩa như sau:</p>

<ul>
	<li><code>x</code> nếu <code>x &gt;= 0</code></li>
	<li><code>-x</code> nếu <code>x &lt; 0</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1954.Minimum%20Garden%20Perimeter%20to%20Collect%20Enough%20Apples/images/1527_example_1_2.png" style="width: 442px; height: 449px;" />
<pre>
<strong>Đầu vào:</strong> neededApples = 1
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Một mảnh đất hình vuông có độ dài cạnh bằng 1 không chứa quả táo nào.
Tuy nhiên, một mảnh đất hình vuông có độ dài cạnh bằng 2 chứa 12 quả táo (như trong hình trên).
Chu vi là 2 * 4 = 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> neededApples = 13
<strong>Đầu ra:</strong> 16
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> neededApples = 1000000000
<strong>Đầu ra:</strong> 5040
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= neededApples &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một hình vuông có đỉnh $(x,x)$ chứa $2x(x+1)(2x+1)$ quả táo và có chu vi $8x$. $x$ có bậc độ lớn xấp xỉ căn bậc ba.
>
> Tăng dần $x$ từ $1$ cho đến khi công thức đạt đủ số lượng cần thiết, rồi trả về $8x$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPerimeter(self, neededApples: int) -> int:
        x = 1
        while 2 * x * (x + 1) * (2 * x + 1) < neededApples:
            x += 1
        return x * 8
```

#### Java

```java
class Solution {
    public long minimumPerimeter(long neededApples) {
        long x = 1;
        while (2 * x * (x + 1) * (2 * x + 1) < neededApples) {
            ++x;
        }
        return 8 * x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumPerimeter(long long neededApples) {
        long long x = 1;
        while (2 * x * (x + 1) * (2 * x + 1) < neededApples) {
            ++x;
        }
        return 8 * x;
    }
};
```

#### Go

```go
func minimumPerimeter(neededApples int64) int64 {
	var x int64 = 1
	for 2*x*(x+1)*(2*x+1) < neededApples {
		x++
	}
	return 8 * x
}
```

#### TypeScript

```ts
function minimumPerimeter(neededApples: number): number {
    let x = 1;
    while (2 * x * (x + 1) * (2 * x + 1) < neededApples) {
        ++x;
    }
    return 8 * x;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm tuyến tính vẫn phải thực hiện số bước cỡ căn bậc ba. Công thức là đơn điệu, vì vậy ta tìm kiếm nhị phân giá trị $x$ nhỏ nhất trong $[1,10^5]$ rồi nhân với $8$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPerimeter(self, neededApples: int) -> int:
        l, r = 1, 100000
        while l < r:
            mid = (l + r) >> 1
            if 2 * mid * (mid + 1) * (2 * mid + 1) >= neededApples:
                r = mid
            else:
                l = mid + 1
        return l * 8
```

#### Java

```java
class Solution {
    public long minimumPerimeter(long neededApples) {
        long l = 1, r = 100000;
        while (l < r) {
            long mid = (l + r) >> 1;
            if (2 * mid * (mid + 1) * (2 * mid + 1) >= neededApples) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l * 8;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumPerimeter(long long neededApples) {
        long long l = 1, r = 100000;
        while (l < r) {
            long mid = (l + r) >> 1;
            if (2 * mid * (mid + 1) * (2 * mid + 1) >= neededApples) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l * 8;
    }
};
```

#### Go

```go
func minimumPerimeter(neededApples int64) int64 {
	var l, r int64 = 1, 100000
	for l < r {
		mid := (l + r) >> 1
		if 2*mid*(mid+1)*(2*mid+1) >= neededApples {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return l * 8
}
```

#### TypeScript

```ts
function minimumPerimeter(neededApples: number): number {
    let l = 1;
    let r = 100000;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (2 * mid * (mid + 1) * (2 * mid + 1) >= neededApples) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return 8 * l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
