---
comments: true
difficulty: Medium
tags:
    - Geometry
    - Math
---

<!-- problem:start -->

# [593. Valid Square](https://leetcode.com/problems/valid-square)

[中文文档](/solution/0500-0599/0593.Valid%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Cho tọa độ của bốn điểm trong không gian 2D là <code>p1</code>, <code>p2</code>, <code>p3</code> và <code>p4</code>, hãy trả về <code>true</code> <em>nếu bốn điểm tạo thành một hình vuông</em>.</p>

<p>Tọa độ của điểm <code>p<sub>i</sub></code> được biểu diễn bằng <code>[x<sub>i</sub>, y<sub>i</sub>]</code>. Các điểm đầu vào <strong>không</strong> được sắp xếp theo thứ tự nào.</p>

<p><strong>Hình vuông hợp lệ</strong> có bốn cạnh dài bằng nhau và độ dài dương, cùng bốn góc bằng nhau (góc 90 độ).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,12]
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> p1 = [1,0], p2 = [-1,0], p3 = [0,1], p4 = [0,-1]
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>p1.length == p2.length == p3.length == p4.length == 2</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bốn điểm tạo thành hình vuông khi các cạnh bằng nhau và hai cạnh kề vuông góc — tương đương với việc mọi bộ ba điểm tạo thành tam giác vuông cân. Kiểm tra mọi cách sắp xếp các đỉnh của tứ giác khá rắc rối.
>
> Với mỗi bộ ba điểm, kiểm tra xem có hai bình phương độ dài cạnh bằng nhau và thỏa định lý Pythagore hay không; đồng thời loại trường hợp độ dài bằng $0$. Nếu cả bốn bộ ba đều đạt, các điểm tạo thành hình vuông.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validSquare(
        self, p1: List[int], p2: List[int], p3: List[int], p4: List[int]
    ) -> bool:
        def check(a, b, c):
            (x1, y1), (x2, y2), (x3, y3) = a, b, c
            d1 = (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2)
            d2 = (x1 - x3) * (x1 - x3) + (y1 - y3) * (y1 - y3)
            d3 = (x2 - x3) * (x2 - x3) + (y2 - y3) * (y2 - y3)
            return any(
                [
                    d1 == d2 and d1 + d2 == d3 and d1,
                    d2 == d3 and d2 + d3 == d1 and d2,
                    d1 == d3 and d1 + d3 == d2 and d1,
                ]
            )

        return (
            check(p1, p2, p3)
            and check(p2, p3, p4)
            and check(p1, p3, p4)
            and check(p1, p2, p4)
        )
```

#### Java

```java
class Solution {
    public boolean validSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        return check(p1, p2, p3) && check(p1, p3, p4) && check(p1, p2, p4) && check(p2, p3, p4);
    }

    private boolean check(int[] a, int[] b, int[] c) {
        int x1 = a[0], y1 = a[1];
        int x2 = b[0], y2 = b[1];
        int x3 = c[0], y3 = c[1];
        int d1 = (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2);
        int d2 = (x1 - x3) * (x1 - x3) + (y1 - y3) * (y1 - y3);
        int d3 = (x2 - x3) * (x2 - x3) + (y2 - y3) * (y2 - y3);
        if (d1 == d2 && d1 + d2 == d3 && d1 > 0) {
            return true;
        }
        if (d1 == d3 && d1 + d3 == d2 && d1 > 0) {
            return true;
        }
        if (d2 == d3 && d2 + d3 == d1 && d2 > 0) {
            return true;
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validSquare(vector<int>& p1, vector<int>& p2, vector<int>& p3, vector<int>& p4) {
        return check(p1, p2, p3) && check(p1, p3, p4) && check(p1, p2, p4) && check(p2, p3, p4);
    }

    bool check(vector<int>& a, vector<int>& b, vector<int>& c) {
        int x1 = a[0], y1 = a[1];
        int x2 = b[0], y2 = b[1];
        int x3 = c[0], y3 = c[1];
        int d1 = (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2);
        int d2 = (x1 - x3) * (x1 - x3) + (y1 - y3) * (y1 - y3);
        int d3 = (x2 - x3) * (x2 - x3) + (y2 - y3) * (y2 - y3);
        if (d1 == d2 && d1 + d2 == d3 && d1 > 0) return true;
        if (d1 == d3 && d1 + d3 == d2 && d1 > 0) return true;
        if (d2 == d3 && d2 + d3 == d1 && d2 > 0) return true;
        return false;
    }
};
```

#### Go

```go
func validSquare(p1 []int, p2 []int, p3 []int, p4 []int) bool {
	check := func(a, b, c []int) bool {
		x1, y1 := a[0], a[1]
		x2, y2 := b[0], b[1]
		x3, y3 := c[0], c[1]
		d1 := (x1-x2)*(x1-x2) + (y1-y2)*(y1-y2)
		d2 := (x1-x3)*(x1-x3) + (y1-y3)*(y1-y3)
		d3 := (x2-x3)*(x2-x3) + (y2-y3)*(y2-y3)
		if d1 == d2 && d1+d2 == d3 && d1 > 0 {
			return true
		}
		if d1 == d3 && d1+d3 == d2 && d1 > 0 {
			return true
		}
		if d2 == d3 && d2+d3 == d1 && d2 > 0 {
			return true
		}
		return false
	}
	return check(p1, p2, p3) && check(p1, p3, p4) && check(p1, p2, p4) && check(p2, p3, p4)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
