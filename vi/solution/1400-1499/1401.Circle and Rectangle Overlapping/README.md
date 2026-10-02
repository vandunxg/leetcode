---
comments: true
difficulty: Medium
rating: 1708
source: Biweekly Contest 23 Q3
tags:
    - Geometry
    - Math
---

<!-- problem:start -->

# [1401. Circle and Rectangle Overlapping](https://leetcode.com/problems/circle-and-rectangle-overlapping)

[中文文档](/solution/1400-1499/1401.Circle%20and%20Rectangle%20Overlapping/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một hình tròn được biểu diễn bởi <code>(radius, xCenter, yCenter)</code> và một hình chữ nhật có các cạnh song song với trục tọa độ, được biểu diễn bởi <code>(x1, y1, x2, y2)</code>, trong đó <code>(x1, y1)</code> là tọa độ góc dưới bên trái và <code>(x2, y2)</code> là tọa độ góc trên bên phải của hình chữ nhật.</p>

<p>Trả về <code>true</code><em> nếu hình tròn và hình chữ nhật giao nhau, nếu không thì trả về </em><code>false</code>. Nói cách khác, hãy kiểm tra xem có <strong>bất kỳ</strong> điểm nào <code>(x<sub>i</sub>, y<sub>i</sub>)</code> đồng thời thuộc hình tròn và hình chữ nhật hay không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1401.Circle%20and%20Rectangle%20Overlapping/images/sample_4_1728.png" style="width: 258px; height: 167px;" />
<pre>
<strong>Đầu vào:</strong> radius = 1, xCenter = 0, yCenter = 0, x1 = 1, y1 = -1, x2 = 3, y2 = 1
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hình tròn và hình chữ nhật cùng có điểm (1,0).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> radius = 1, xCenter = 1, yCenter = 1, x1 = 1, y1 = -3, x2 = 2, y2 = -1
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1401.Circle%20and%20Rectangle%20Overlapping/images/sample_2_1728.png" style="width: 150px; height: 135px;" />
<pre>
<strong>Đầu vào:</strong> radius = 1, xCenter = 0, yCenter = 0, x1 = -1, y1 = 0, x2 = 0, y2 = 1
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= radius &lt;= 2000</code></li>
	<li><code>-10<sup>4</sup> &lt;= xCenter, yCenter &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= x1 &lt; x2 &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= y1 &lt; y2 &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Việc lấy mẫu các điểm trên lưới hoặc chỉ kiểm tra bốn góc có thể bỏ sót giao điểm trên cạnh hoặc bên trong. Tọa độ có thể lên tới $10^4$, vì vậy ta cần một phép kiểm tra hình học chính xác.
>
> Hình tròn giao với hình chữ nhật khi và chỉ khi điểm gần tâm hình tròn nhất trong hình chữ nhật nằm bên trong hình tròn. Tọa độ $x$ và $y$ của điểm đó có thể được giới hạn độc lập lần lượt trong $[x_1,x_2]$ và $[y_1,y_2]$.
>
> Nếu một tọa độ đã nằm trong khoảng tương ứng thì nó đóng góp khoảng cách bằng $0$; nếu không, ta chọn đầu mút gần hơn. So sánh tổng bình phương khoảng cách với $radius^2$.

<!-- thinking:end -->

Với một điểm $(x, y)$, khoảng cách ngắn nhất từ điểm đó đến tâm hình tròn $(xCenter, yCenter)$ là $\sqrt{(x - xCenter)^2 + (y - yCenter)^2}$. Nếu khoảng cách này nhỏ hơn hoặc bằng bán kính $radius$, thì điểm đó nằm trong hình tròn (bao gồm cả đường biên).

Với các điểm nằm trong hình chữ nhật (bao gồm cả đường biên), tọa độ $x$ của chúng thỏa mãn $x_1 \leq x \leq x_2$, còn tọa độ $y$ thỏa mãn $y_1 \leq y \leq y_2$. Để xác định hình tròn và hình chữ nhật có giao nhau hay không, ta cần tìm một điểm $(x, y)$ nằm trong hình chữ nhật sao cho $a = |x - xCenter|$ và $b = |y - yCenter|$ đạt giá trị nhỏ nhất. Nếu $a^2 + b^2 \leq radius^2$, thì hình tròn và hình chữ nhật giao nhau.

Do đó, bài toán được chuyển thành việc tìm giá trị nhỏ nhất của $a = |x - xCenter|$ khi $x \in [x_1, x_2]$, và giá trị nhỏ nhất của $b = |y - yCenter|$ khi $y \in [y_1, y_2]$.

Với $x \in [x_1, x_2]$:

- Nếu $x_1 \leq xCenter \leq x_2$, giá trị nhỏ nhất của $|x - xCenter|$ là $0$;
- Nếu $xCenter < x_1$, giá trị nhỏ nhất của $|x - xCenter|$ là $x_1 - xCenter$;
- Nếu $xCenter > x_2$, giá trị nhỏ nhất của $|x - xCenter|$ là $xCenter - x_2$.

Tương tự, ta có thể tìm giá trị nhỏ nhất của $|y - yCenter|$ khi $y \in [y_1, y_2]$. Ta có thể dùng một hàm $f(i, j, k)$ để xử lý các trường hợp trên.

Cụ thể, $a = f(x_1, x_2, xCenter)$, $b = f(y_1, y_2, yCenter)$. Nếu $a^2 + b^2 \leq radius^2$, thì hình tròn và hình chữ nhật giao nhau.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkOverlap(
        self,
        radius: int,
        xCenter: int,
        yCenter: int,
        x1: int,
        y1: int,
        x2: int,
        y2: int,
    ) -> bool:
        def f(i: int, j: int, k: int) -> int:
            if i <= k <= j:
                return 0
            return i - k if k < i else k - j

        a = f(x1, x2, xCenter)
        b = f(y1, y2, yCenter)
        return a * a + b * b <= radius * radius
```

#### Java

```java
class Solution {
    public boolean checkOverlap(
        int radius, int xCenter, int yCenter, int x1, int y1, int x2, int y2) {
        int a = f(x1, x2, xCenter);
        int b = f(y1, y2, yCenter);
        return a * a + b * b <= radius * radius;
    }

    private int f(int i, int j, int k) {
        if (i <= k && k <= j) {
            return 0;
        }
        return k < i ? i - k : k - j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkOverlap(int radius, int xCenter, int yCenter, int x1, int y1, int x2, int y2) {
        auto f = [](int i, int j, int k) -> int {
            if (i <= k && k <= j) {
                return 0;
            }
            return k < i ? i - k : k - j;
        };
        int a = f(x1, x2, xCenter);
        int b = f(y1, y2, yCenter);
        return a * a + b * b <= radius * radius;
    }
};
```

#### Go

```go
func checkOverlap(radius int, xCenter int, yCenter int, x1 int, y1 int, x2 int, y2 int) bool {
	f := func(i, j, k int) int {
		if i <= k && k <= j {
			return 0
		}
		if k < i {
			return i - k
		}
		return k - j
	}
	a := f(x1, x2, xCenter)
	b := f(y1, y2, yCenter)
	return a*a+b*b <= radius*radius
}
```

#### TypeScript

```ts
function checkOverlap(
    radius: number,
    xCenter: number,
    yCenter: number,
    x1: number,
    y1: number,
    x2: number,
    y2: number,
): boolean {
    const f = (i: number, j: number, k: number) => {
        if (i <= k && k <= j) {
            return 0;
        }
        return k < i ? i - k : k - j;
    };
    const a = f(x1, x2, xCenter);
    const b = f(y1, y2, yCenter);
    return a * a + b * b <= radius * radius;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
