---
comments: true
difficulty: Medium
rating: 1515
source: Weekly Contest 362 Q2
tags:
    - Math
---

<!-- problem:start -->

# [2849. Determine if a Cell Is Reachable at a Given Time](https://leetcode.com/problems/determine-if-a-cell-is-reachable-at-a-given-time)

[中文文档](/solution/2800-2899/2849.Determine%20if%20a%20Cell%20Is%20Reachable%20at%20a%20Given%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho bốn số nguyên <code>sx</code>, <code>sy</code>, <code>fx</code>, <code>fy</code> và một số nguyên <strong>không âm</strong> <code>t</code>.</p>

<p>Trên một lưới 2D vô hạn, bạn bắt đầu tại ô <code>(sx, sy)</code>. Mỗi giây, bạn <strong>phải</strong> di chuyển đến một trong các ô kề với ô hiện tại.</p>

<p>Trả về <code>true</code> <em>nếu bạn có thể đến ô </em><code>(fx, fy)</code> <em>sau<strong> đúng</strong></em> <code>t</code> <strong><em>giây</em></strong>, <em>hoặc</em> <code>false</code> <em>nếu không</em>.</p>

<p><strong>Các ô kề</strong> với một ô là 8 ô xung quanh nó có chung ít nhất một góc. Bạn có thể đi qua cùng một ô nhiều lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2849.Determine%20if%20a%20Cell%20Is%20Reachable%20at%20a%20Given%20Time/images/example2.svg" style="width: 443px; height: 243px;" />
<pre>
<strong>Đầu vào:</strong> sx = 2, sy = 4, fx = 7, fy = 7, t = 6
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bắt đầu tại ô (2, 4), ta có thể đến ô (7, 7) sau đúng 6 giây bằng cách đi qua các ô được minh họa trong hình trên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2849.Determine%20if%20a%20Cell%20Is%20Reachable%20at%20a%20Given%20Time/images/example1.svg" style="width: 383px; height: 202px;" />
<pre>
<strong>Đầu vào:</strong> sx = 3, sy = 1, fx = 7, fy = 3, t = 3
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Bắt đầu tại ô (3, 1), cần ít nhất 4 giây để đến ô (7, 3) bằng cách đi qua các ô được minh họa trong hình trên. Vì vậy, ta không thể đến ô (7, 3) ở giây thứ ba.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sx, sy, fx, fy &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= t &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Với một bước đi theo tám hướng, thời gian ngắn nhất là khoảng cách Chebyshev $\max(|dx|,|dy|)$. Nếu điểm bắt đầu trùng với đích, $t=1$ buộc ta phải rời khỏi ô và không thể quay lại trong một bước duy nhất, vì vậy chỉ $t\ne 1$ mới được; trong các trường hợp khác, mọi $t$ ít nhất bằng khoảng cách đều đủ, vì thời gian dư có thể dùng để di chuyển vòng quanh.

<!-- thinking:end -->

Nếu điểm bắt đầu và đích trùng nhau, ta chỉ có thể đến đích trong thời gian đã cho khi $t \neq 1$.

Nếu không, ta tính độ chênh lệch giữa tọa độ x và y của điểm bắt đầu và đích, sau đó lấy giá trị lớn hơn. Nếu giá trị lớn hơn này nhỏ hơn hoặc bằng thời gian đã cho, ta có thể đến đích trong thời gian đó.

Độ phức tạp thời gian là $O(1)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isReachableAtTime(self, sx: int, sy: int, fx: int, fy: int, t: int) -> bool:
        if sx == fx and sy == fy:
            return t != 1
        dx = abs(sx - fx)
        dy = abs(sy - fy)
        return max(dx, dy) <= t
```

#### Java

```java
class Solution {
    public boolean isReachableAtTime(int sx, int sy, int fx, int fy, int t) {
        if (sx == fx && sy == fy) {
            return t != 1;
        }
        int dx = Math.abs(sx - fx);
        int dy = Math.abs(sy - fy);
        return Math.max(dx, dy) <= t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isReachableAtTime(int sx, int sy, int fx, int fy, int t) {
        if (sx == fx && sy == fy) {
            return t != 1;
        }
        int dx = abs(fx - sx), dy = abs(fy - sy);
        return max(dx, dy) <= t;
    }
};
```

#### Go

```go
func isReachableAtTime(sx int, sy int, fx int, fy int, t int) bool {
	if sx == fx && sy == fy {
		return t != 1
	}
	dx := abs(sx - fx)
	dy := abs(sy - fy)
	return max(dx, dy) <= t
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function isReachableAtTime(sx: number, sy: number, fx: number, fy: number, t: number): boolean {
    if (sx === fx && sy === fy) {
        return t !== 1;
    }
    const dx = Math.abs(sx - fx);
    const dy = Math.abs(sy - fy);
    return Math.max(dx, dy) <= t;
}
```

#### C#

```cs
public class Solution {
    public bool IsReachableAtTime(int sx, int sy, int fx, int fy, int t) {
        if (sx == fx && sy == fy) {
            return t != 1;
        }
        return Math.Max(Math.Abs(sx - fx), Math.Abs(sy - fy)) <= t;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
