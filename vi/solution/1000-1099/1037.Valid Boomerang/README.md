---
comments: true
difficulty: Easy
rating: 1255
source: Weekly Contest 135 Q1
tags:
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [1037. Valid Boomerang](https://leetcode.com/problems/valid-boomerang)

[中文文档](/solution/1000-1099/1037.Valid%20Boomerang/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn một điểm trên mặt phẳng <strong>X-Y</strong>. Trả về <code>true</code> <em>nếu các điểm này tạo thành một <strong>boomerang</strong></em>.</p>

<p><strong>Boomerang</strong> là tập hợp gồm ba điểm <strong>đôi một khác nhau</strong> và <strong>không nằm trên cùng một đường thẳng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> points = [[1,1],[2,3],[3,2]]
<strong>Output:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> points = [[1,1],[2,2],[3,3]]
<strong>Output:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>points.length == 3</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So sánh hệ số góc

<!-- thinking:start -->

> **Tư duy**
>
> Ba điểm tạo thành boomerang khi và chỉ khi chúng đôi một khác nhau và không thẳng hàng. Lấy hiệu giữa các hệ số góc sẽ gặp vấn đề với đường thẳng đứng và sai số dấu phẩy động.
>
> Điều kiện hai hệ số góc không bằng nhau có thể viết thành tích chéo $(y_2-y_1)(x_3-x_2)\neq(y_3-y_2)(x_2-x_1)$, cách này cũng xử lý được các cạnh thẳng đứng và những điểm trùng nhau.
>
> Chỉ cần kiểm tra một phép nhân dựa trên tọa độ của ba điểm.

<!-- thinking:end -->

Gọi ba điểm lần lượt là $(x_1, y_1)$, $(x_2, y_2)$ và $(x_3, y_3)$. Công thức tính hệ số góc giữa hai điểm là $\frac{y_2 - y_1}{x_2 - x_1}$.

Để đảm bảo ba điểm không thẳng hàng, cần thỏa điều kiện $\frac{y_2 - y_1}{x_2 - x_1} \neq \frac{y_3 - y_2}{x_3 - x_2}$. Biến đổi biểu thức này, ta được $(y_2 - y_1) \cdot (x_3 - x_2) \neq (y_3 - y_2) \cdot (x_2 - x_1)$.

Lưu ý:

1. Khi hệ số góc giữa hai điểm không xác định, tức là $x_1 = x_2$, phương trình sau biến đổi vẫn đúng.
2. Nếu phép chia khi so sánh hệ số góc gây sai số, có thể chuyển phép so sánh thành phép nhân.

Độ phức tạp thời gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isBoomerang(self, points: List[List[int]]) -> bool:
        (x1, y1), (x2, y2), (x3, y3) = points
        return (y2 - y1) * (x3 - x2) != (y3 - y2) * (x2 - x1)
```

#### Java

```java
class Solution {
    public boolean isBoomerang(int[][] points) {
        int x1 = points[0][0], y1 = points[0][1];
        int x2 = points[1][0], y2 = points[1][1];
        int x3 = points[2][0], y3 = points[2][1];
        return (y2 - y1) * (x3 - x2) != (y3 - y2) * (x2 - x1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isBoomerang(vector<vector<int>>& points) {
        int x1 = points[0][0], y1 = points[0][1];
        int x2 = points[1][0], y2 = points[1][1];
        int x3 = points[2][0], y3 = points[2][1];
        return (y2 - y1) * (x3 - x2) != (y3 - y2) * (x2 - x1);
    }
};
```

#### Go

```go
func isBoomerang(points [][]int) bool {
	x1, y1 := points[0][0], points[0][1]
	x2, y2 := points[1][0], points[1][1]
	x3, y3 := points[2][0], points[2][1]
	return (y2-y1)*(x3-x2) != (y3-y2)*(x2-x1)
}
```

#### TypeScript

```ts
function isBoomerang(points: number[][]): boolean {
    const [x1, y1] = points[0];
    const [x2, y2] = points[1];
    const [x3, y3] = points[2];
    return (x1 - x2) * (y2 - y3) !== (x2 - x3) * (y1 - y2);
}
```

#### Rust

```rust
impl Solution {
    pub fn is_boomerang(points: Vec<Vec<i32>>) -> bool {
        let (x1, y1) = (points[0][0], points[0][1]);
        let (x2, y2) = (points[1][0], points[1][1]);
        let (x3, y3) = (points[2][0], points[2][1]);
        (x1 - x2) * (y2 - y3) != (x2 - x3) * (y1 - y2)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
