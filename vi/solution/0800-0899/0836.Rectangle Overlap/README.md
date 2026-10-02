---
comments: true
difficulty: Easy
tags:
    - Geometry
    - Math
---

<!-- problem:start -->

# [836. Rectangle Overlap](https://leetcode.com/problems/rectangle-overlap)

[中文文档](/solution/0800-0899/0836.Rectangle%20Overlap/README.md)

## Mô tả

<!-- description:start -->

<p>Hình chữ nhật có các cạnh song song với trục tọa độ được biểu diễn bằng danh sách <code>[x1, y1, x2, y2]</code>, trong đó <code>(x1, y1)</code> là tọa độ góc dưới bên trái và <code>(x2, y2)</code> là tọa độ góc trên bên phải. Cạnh trên và cạnh dưới song song với trục X; cạnh trái và cạnh phải song song với trục Y.</p>

<p>Hai hình chữ nhật giao nhau nếu diện tích phần giao của chúng <strong>lớn hơn 0</strong>. Nói rõ hơn, hai hình chữ nhật chỉ chạm nhau tại góc hoặc cạnh thì không được xem là giao nhau.</p>

<p>Cho hai hình chữ nhật có các cạnh song song với trục tọa độ <code>rec1</code> và <code>rec2</code>. Hãy trả về <code>true</code><em> nếu chúng giao nhau; nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> rec1 = [0,0,2,2], rec2 = [1,1,3,3]
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> rec1 = [0,0,1,1], rec2 = [1,0,2,1]
<strong>Đầu ra:</strong> false
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> rec1 = [0,0,1,1], rec2 = [2,2,3,3]
<strong>Đầu ra:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>rec1.length == 4</code></li>
	<li><code>rec2.length == 4</code></li>
	<li><code>-10<sup>9</sup> &lt;= rec1[i], rec2[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>rec1</code> và <code>rec2</code> đều biểu diễn hình chữ nhật hợp lệ có diện tích khác 0.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xác định các trường hợp không giao nhau

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xác định hai hình chữ nhật có các cạnh song song với trục tọa độ có phần giao diện tích dương hay không. Mô tả trực tiếp phần giao khá rắc rối; xét trường hợp ngược lại đơn giản hơn: chúng không giao nhau nếu bị tách rời theo chiều dọc hoặc chiều ngang.
>
> Nếu một trong bốn điều kiện tách rời đúng thì hai hình không giao nhau; phủ định phép kiểm tra đó sẽ cho điều kiện giao nhau. Diện tích mỗi hình đều khác 0 nên không có cạnh suy biến.

<!-- thinking:end -->

Gọi tọa độ của hình chữ nhật $\text{rec1}$ là $(x_1, y_1, x_2, y_2)$ và của hình chữ nhật $\text{rec2}$ là $(x_3, y_3, x_4, y_4)$.

Hai hình chữ nhật $\text{rec1}$ và $\text{rec2}$ không giao nhau nếu thỏa mãn bất kỳ điều kiện nào sau đây:

- $y_3 \geq y_2$: $\text{rec2}$ nằm phía trên $\text{rec1}$;
- $y_4 \leq y_1$: $\text{rec2}$ nằm phía dưới $\text{rec1}$;
- $x_3 \geq x_2$: $\text{rec2}$ nằm bên phải $\text{rec1}$;
- $x_4 \leq x_1$: $\text{rec2}$ nằm bên trái $\text{rec1}$.

Nếu không điều kiện nào ở trên đúng thì hai hình chữ nhật $\text{rec1}$ và $\text{rec2}$ giao nhau.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isRectangleOverlap(self, rec1: List[int], rec2: List[int]) -> bool:
        x1, y1, x2, y2 = rec1
        x3, y3, x4, y4 = rec2
        return not (y3 >= y2 or y4 <= y1 or x3 >= x2 or x4 <= x1)
```

#### Java

```java
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        int x1 = rec1[0], y1 = rec1[1], x2 = rec1[2], y2 = rec1[3];
        int x3 = rec2[0], y3 = rec2[1], x4 = rec2[2], y4 = rec2[3];
        return !(y3 >= y2 || y4 <= y1 || x3 >= x2 || x4 <= x1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        int x1 = rec1[0], y1 = rec1[1], x2 = rec1[2], y2 = rec1[3];
        int x3 = rec2[0], y3 = rec2[1], x4 = rec2[2], y4 = rec2[3];
        return !(y3 >= y2 || y4 <= y1 || x3 >= x2 || x4 <= x1);
    }
};
```

#### Go

```go
func isRectangleOverlap(rec1 []int, rec2 []int) bool {
	x1, y1, x2, y2 := rec1[0], rec1[1], rec1[2], rec1[3]
	x3, y3, x4, y4 := rec2[0], rec2[1], rec2[2], rec2[3]
	return !(y3 >= y2 || y4 <= y1 || x3 >= x2 || x4 <= x1)
}
```

#### TypeScript

```ts
function isRectangleOverlap(rec1: number[], rec2: number[]): boolean {
    const [x1, y1, x2, y2] = rec1;
    const [x3, y3, x4, y4] = rec2;
    return !(y3 >= y2 || y4 <= y1 || x3 >= x2 || x4 <= x1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
