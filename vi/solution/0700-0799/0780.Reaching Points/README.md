---
comments: true
difficulty: Hard
tags:
    - Math
    - Greatest Common Divisor
    - Euclidean Algorithm
---

<!-- problem:start -->

# [780. Reaching Points](https://leetcode.com/problems/reaching-points)

[中文文档](/solution/0700-0799/0780.Reaching%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Cho bốn số nguyên <code>sx</code>, <code>sy</code>, <code>tx</code> và <code>ty</code>. Hãy trả về <code>true</code><em> nếu có thể biến đổi điểm </em><code>(sx, sy)</code><em> thành điểm </em><code>(tx, ty)</code> <em>bằng một số phép toán</em><em>; nếu không thì trả về </em><code>false</code>.</p>

<p>Với điểm <code>(x, y)</code>, phép toán được phép là biến đổi nó thành một trong hai điểm <code>(x, x + y)</code> hoặc <code>(x + y, y)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sx = 1, sy = 1, tx = 3, ty = 5
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Một chuỗi phép biến đổi đưa điểm ban đầu đến điểm đích là:
(1, 1) -&gt; (1, 2)
(1, 2) -&gt; (3, 2)
(3, 2) -&gt; (3, 5)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sx = 1, sy = 1, tx = 2, ty = 2
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sx = 1, sy = 1, tx = 1, ty = 1
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sx, sy, tx, ty &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Từ $(sx,sy)$, ta có thể cộng một tọa độ vào tọa độ còn lại. Tọa độ đích có thể rất lớn nên duyệt xuôi không hiệu quả. Phép biến đổi ngược là trừ tọa độ nhỏ hơn khỏi tọa độ lớn hơn, tương đương dùng phép modulo.
>
> Khi cả hai tọa độ đều lớn hơn tọa độ ban đầu tương ứng và chúng khác nhau, thay tọa độ lớn hơn bằng $a\bmod b$. Khi một tọa độ đã khớp, tọa độ còn lại phải có thể giảm về giá trị ban đầu bằng một bội của tọa độ đã khớp.
>
> Nếu cả hai tọa độ bằng điểm ban đầu thì thành công; nếu không thì thất bại. Phép modulo gộp nhiều lần phép trừ thành một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reachingPoints(self, sx: int, sy: int, tx: int, ty: int) -> bool:
        while tx > sx and ty > sy and tx != ty:
            if tx > ty:
                tx %= ty
            else:
                ty %= tx
        if tx == sx and ty == sy:
            return True
        if tx == sx:
            return ty > sy and (ty - sy) % tx == 0
        if ty == sy:
            return tx > sx and (tx - sx) % ty == 0
        return False
```

#### Java

```java
class Solution {
    public boolean reachingPoints(int sx, int sy, int tx, int ty) {
        while (tx > sx && ty > sy && tx != ty) {
            if (tx > ty) {
                tx %= ty;
            } else {
                ty %= tx;
            }
        }
        if (tx == sx && ty == sy) {
            return true;
        }
        if (tx == sx) {
            return ty > sy && (ty - sy) % tx == 0;
        }
        if (ty == sy) {
            return tx > sx && (tx - sx) % ty == 0;
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool reachingPoints(int sx, int sy, int tx, int ty) {
        while (tx > sx && ty > sy && tx != ty) {
            if (tx > ty)
                tx %= ty;
            else
                ty %= tx;
        }
        if (tx == sx && ty == sy) return true;
        if (tx == sx) return ty > sy && (ty - sy) % tx == 0;
        if (ty == sy) return tx > sx && (tx - sx) % ty == 0;
        return false;
    }
};
```

#### Go

```go
func reachingPoints(sx int, sy int, tx int, ty int) bool {
	for tx > sx && ty > sy && tx != ty {
		if tx > ty {
			tx %= ty
		} else {
			ty %= tx
		}
	}
	if tx == sx && ty == sy {
		return true
	}
	if tx == sx {
		return ty > sy && (ty-sy)%tx == 0
	}
	if ty == sy {
		return tx > sx && (tx-sx)%ty == 0
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
