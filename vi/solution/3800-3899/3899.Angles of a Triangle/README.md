---
comments: true
difficulty: Medium
rating: 1407
source: Weekly Contest 497 Q2
tags:
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [3899. Angles of a Triangle](https://leetcode.com/problems/angles-of-a-triangle)

[中文文档](/solution/3800-3899/3899.Angles%20of%20a%20Triangle/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>sides</code> có độ dài 3.</p>

<p>Hãy xác định xem có tồn tại một tam giác có <strong>diện tích dương</strong> với độ dài ba cạnh được cho bởi các phần tử của <code>sides</code> hay không.</p>

<p>Nếu tồn tại tam giác như vậy, hãy trả về một mảng gồm ba số thực biểu diễn các góc trong của nó (theo <strong>độ</strong>), được <strong>sắp xếp</strong> theo thứ tự <strong>không giảm</strong>. Nếu không, hãy trả về một mảng rỗng.</p>

<p>Các đáp án có sai số không quá <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sides = [3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[36.86990,53.13010,90.00000]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể tạo thành một tam giác vuông với độ dài các cạnh là 3, 4 và 5. Các góc trong của tam giác này lần lượt xấp xỉ bằng 36.869897646, 53.130102354 và 90 độ.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sides = [2,4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể tạo thành một tam giác có diện tích dương với độ dài các cạnh là 2, 4 và 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>sides.length == 3</code></li>
	<li><code>1 &lt;= sides[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem ba cạnh có tạo thành một tam giác có diện tích dương hay không; nếu có, trả về các góc trong theo độ và theo thứ tự không giảm.
>
> Sau khi sắp xếp, điều kiện $a+b \le c$ cho biết ba cạnh không tạo thành tam giác hợp lệ. Nếu không, định lý cosin cho phép tính hai góc, còn góc thứ ba bằng $180^\circ$ trừ đi hai góc đó.
>
> Việc sắp xếp các cạnh cũng sắp xếp các góc đối diện, nên bộ ba góc đã ở đúng thứ tự không giảm.
>
> Đổi arccos sang độ vẫn đảm bảo sai số nằm trong giới hạn cho phép.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng $\textit{sides}$ theo thứ tự không giảm và ký hiệu ba độ dài cạnh là $a$, $b$ và $c$, trong đó $a \le b \le c$.

Theo bất đẳng thức tam giác, nếu $a + b \le c$ thì ba cạnh này không thể tạo thành một tam giác có diện tích dương, do đó ta trả về ngay một mảng rỗng.

Ngược lại, ba cạnh có thể tạo thành một tam giác hợp lệ. Theo định lý cosin, ta có:

$$
\cos A = \frac{b^2 + c^2 - a^2}{2bc}
$$

$$
\cos B = \frac{a^2 + c^2 - b^2}{2ac}
$$

Do đó, ta có thể lần lượt tính các góc $A$ và $B$. Cuối cùng, dựa vào tổng các góc trong của một tam giác bằng $180^\circ$, ta có:

$$
C = 180^\circ - A - B
$$

Cuối cùng, ta trả về ba góc trong.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def internalAngles(self, sides: list[int]) -> list[float]:
        sides.sort()
        a, b, c = sides
        if a + b <= c:
            return []
        A = degrees(acos((b**2 + c**2 - a**2) / (2 * b * c)))
        B = degrees(acos((a**2 + c**2 - b**2) / (2 * a * c)))
        C = 180 - A - B
        return [A, B, C]
```

#### Java

```java
class Solution {
    public double[] internalAngles(int[] sides) {
        Arrays.sort(sides);
        int a = sides[0], b = sides[1], c = sides[2];
        if (a + b <= c) {
            return new double[0];
        }
        double A = Math.toDegrees(Math.acos((b * b + c * c - a * a) / (2.0 * b * c)));
        double B = Math.toDegrees(Math.acos((a * a + c * c - b * b) / (2.0 * a * c)));
        double C = 180.0 - A - B;
        return new double[] {A, B, C};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<double> internalAngles(vector<int>& sides) {
        sort(sides.begin(), sides.end());
        int a = sides[0], b = sides[1], c = sides[2];
        if (a + b <= c) {
            return {};
        }
        double A = acos((1.0 * b * b + 1.0 * c * c - 1.0 * a * a) / (2.0 * b * c)) * 180.0 / acos(-1.0);
        double B = acos((1.0 * a * a + 1.0 * c * c - 1.0 * b * b) / (2.0 * a * c)) * 180.0 / acos(-1.0);
        double C = 180.0 - A - B;
        return {A, B, C};
    }
};
```

#### Go

```go
func internalAngles(sides []int) []float64 {
	sort.Ints(sides)
	a, b, c := sides[0], sides[1], sides[2]
	if a+b <= c {
		return []float64{}
	}
	A := math.Acos(float64(b*b+c*c-a*a)/float64(2*b*c)) * 180 / math.Pi
	B := math.Acos(float64(a*a+c*c-b*b)/float64(2*a*c)) * 180 / math.Pi
	C := 180 - A - B
	return []float64{A, B, C}
}
```

#### TypeScript

```ts
function internalAngles(sides: number[]): number[] {
    sides.sort((a, b) => a - b);
    const [a, b, c] = sides;
    if (a + b <= c) {
        return [];
    }
    const A = (Math.acos((b * b + c * c - a * a) / (2 * b * c)) * 180) / Math.PI;
    const B = (Math.acos((a * a + c * c - b * b) / (2 * a * c)) * 180) / Math.PI;
    const C = 180 - A - B;
    return [A, B, C];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
