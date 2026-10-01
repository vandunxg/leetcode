---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.13. Bisect Squares](https://leetcode.cn/problems/bisect-squares-lcci)

[中文文档](/lcci/16.13.Bisect%20Squares/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai hình vuông trên mặt phẳng hai chiều, hãy tìm một đường thẳng chia đôi cả hai hình vuông. Giả sử cạnh trên và cạnh dưới của hình vuông song song với trục x.</p>
<p>Mỗi hình vuông được biểu diễn bởi ba giá trị: tọa độ của góc dưới bên trái&nbsp;<code>[X,Y] = [square[0],square[1]]</code> và độ dài cạnh của hình vuông <code>square[2]</code>. Đường thẳng sẽ cắt hai hình vuông tại bốn điểm. Hãy trả về tọa độ của hai điểm giao nhau <code>[X<sub>1</sub>,Y<sub>1</sub>]</code>&nbsp;và&nbsp;<code>[X<sub>2</sub>,Y<sub>2</sub>]</code> sao cho đoạn thẳng nối hai điểm này chứa hai điểm giao còn lại, theo định dạng <code>{X<sub>1</sub>,Y<sub>1</sub>,X<sub>2</sub>,Y<sub>2</sub>}</code>. Nếu <code>X<sub>1</sub> != X<sub>2</sub></code>, cần có&nbsp;<code>X<sub>1</sub> &lt; X<sub>2</sub></code>; nếu không, cần có&nbsp;<code>Y<sub>1</sub> &lt;= Y<sub>2</sub></code>.</p>
<p>Nếu có nhiều hơn một đường thẳng có thể chia đôi cả hai hình vuông, hãy trả về đường thẳng có hệ số góc lớn nhất (hệ số góc của đường thẳng song song với trục y được xem là vô cùng).</p>
<p><strong>Ví dụ: </strong></p>
<pre>

<strong>Đầu vào: </strong>

square1 = {-1, -1, 2}

square2 = {0, -1, 2}

<strong>Đầu ra:</strong> {-1,0,2,0}

<strong>Giải thích:</strong> y = 0 là đường thẳng có thể chia đôi hai hình vuông.

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>square.length == 3</code></li>
	<li><code>square[2] &gt; 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán hình học

<!-- thinking:start -->

> **Tư duy**
>
> Đường thẳng chia đôi cả hai hình vuông phải đi qua tâm của cả hai hình vuông. Ta giao đường thẳng đó với bounding box rồi sắp xếp hai đầu mút theo yêu cầu.
>
> Trường hợp đường thẳng đứng được xử lý riêng để hệ số góc luôn hữu hạn; các trường hợp còn lại dùng $k$ và $b$.
>
> Nếu $|k|>1$, đường thẳng cắt cạnh trên và cạnh dưới; nếu không thì cắt cạnh trái và cạnh phải. Đổi chỗ hai đầu mút khi cần để điểm đầu tiên nằm xa bên trái hơn (hoặc thấp hơn). Toàn bộ thao tác có độ phức tạp $O(1)$.

<!-- thinking:end -->

Ta biết rằng nếu một đường thẳng có thể chia đôi hai hình vuông thì đường thẳng đó phải đi qua tâm của cả hai hình vuông. Vì vậy, trước tiên ta có thể tính tâm của hai hình vuông, lần lượt ký hiệu là $(x_1, y_1)$ và $(x_2, y_2)$.

Nếu $x_1 = x_2$, đường thẳng vuông góc với trục $x$, và ta chỉ cần tìm giao điểm của cạnh trên và cạnh dưới của hai hình vuông.

Ngược lại, ta có thể tính hệ số góc $k$ và tung độ gốc $b$ của đường thẳng đi qua hai tâm, sau đó chia thành hai trường hợp dựa trên giá trị tuyệt đối của hệ số góc:

- Khi $|k| \gt 1$, đường thẳng đi qua hai tâm giao với cạnh trên và cạnh dưới của hai hình vuông. Ta tính giá trị lớn nhất và nhỏ nhất của tọa độ theo phương đứng của các cạnh trên và cạnh dưới, sau đó tính tọa độ theo phương ngang tương ứng bằng phương trình đường thẳng; đó là các giao điểm với hai hình vuông.
- Khi $|k| \le 1$, đường thẳng đi qua hai tâm giao với cạnh trái và cạnh phải của hai hình vuông. Ta tính giá trị lớn nhất và nhỏ nhất của tọa độ theo phương ngang của các cạnh trái và cạnh phải, sau đó tính tọa độ theo phương đứng tương ứng bằng phương trình đường thẳng; đó là các giao điểm với hai hình vuông.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cutSquares(self, square1: List[int], square2: List[int]) -> List[float]:
        x1, y1 = square1[0] + square1[2] / 2, square1[1] + square1[2] / 2
        x2, y2 = square2[0] + square2[2] / 2, square2[1] + square2[2] / 2
        if x1 == x2:
            y3 = min(square1[1], square2[1])
            y4 = max(square1[1] + square1[2], square2[1] + square2[2])
            return [x1, y3, x2, y4]
        k = (y2 - y1) / (x2 - x1)
        b = y1 - k * x1
        if abs(k) > 1:
            y3 = min(square1[1], square2[1])
            x3 = (y3 - b) / k
            y4 = max(square1[1] + square1[2], square2[1] + square2[2])
            x4 = (y4 - b) / k
            if x3 > x4 or (x3 == x4 and y3 > y4):
                x3, y3, x4, y4 = x4, y4, x3, y3
        else:
            x3 = min(square1[0], square2[0])
            y3 = k * x3 + b
            x4 = max(square1[0] + square1[2], square2[0] + square2[2])
            y4 = k * x4 + b
        return [x3, y3, x4, y4]
```

#### Java

```java
class Solution {
    public double[] cutSquares(int[] square1, int[] square2) {
        double x1 = square1[0] + square1[2] / 2.0;
        double y1 = square1[1] + square1[2] / 2.0;
        double x2 = square2[0] + square2[2] / 2.0;
        double y2 = square2[1] + square2[2] / 2.0;
        if (x1 == x2) {
            double y3 = Math.min(square1[1], square2[1]);
            double y4 = Math.max(square1[1] + square1[2], square2[1] + square2[2]);
            return new double[] {x1, y3, x2, y4};
        }
        double k = (y2 - y1) / (x2 - x1);
        double b = y1 - k * x1;
        if (Math.abs(k) > 1) {
            double y3 = Math.min(square1[1], square2[1]);
            double x3 = (y3 - b) / k;
            double y4 = Math.max(square1[1] + square1[2], square2[1] + square2[2]);
            double x4 = (y4 - b) / k;
            if (x3 > x4 || (x3 == x4 && y3 > y4)) {
                return new double[] {x4, y4, x3, y3};
            }
            return new double[] {x3, y3, x4, y4};
        } else {
            double x3 = Math.min(square1[0], square2[0]);
            double y3 = k * x3 + b;
            double x4 = Math.max(square1[0] + square1[2], square2[0] + square2[2]);
            double y4 = k * x4 + b;
            return new double[] {x3, y3, x4, y4};
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<double> cutSquares(vector<int>& square1, vector<int>& square2) {
        double x1 = square1[0] + square1[2] / 2.0;
        double y1 = square1[1] + square1[2] / 2.0;
        double x2 = square2[0] + square2[2] / 2.0;
        double y2 = square2[1] + square2[2] / 2.0;
        if (x1 == x2) {
            double y3 = min(square1[1], square2[1]);
            double y4 = max(square1[1] + square1[2], square2[1] + square2[2]);
            return {x1, y3, x2, y4};
        }
        double k = (y2 - y1) / (x2 - x1);
        double b = y1 - k * x1;
        if (abs(k) > 1) {
            double y3 = min(square1[1], square2[1]);
            double x3 = (y3 - b) / k;
            double y4 = max(square1[1] + square1[2], square2[1] + square2[2]);
            double x4 = (y4 - b) / k;
            if (x3 > x4 || (x3 == x4 && y3 > y4)) {
                return {x4, y4, x3, y3};
            }
            return {x3, y3, x4, y4};
        } else {
            double x3 = min(square1[0], square2[0]);
            double y3 = k * x3 + b;
            double x4 = max(square1[0] + square1[2], square2[0] + square2[2]);
            double y4 = k * x4 + b;
            return {x3, y3, x4, y4};
        }
    }
};
```

#### Go

```go
func cutSquares(square1 []int, square2 []int) []float64 {
	x1, y1 := float64(square1[0])+float64(square1[2])/2, float64(square1[1])+float64(square1[2])/2
	x2, y2 := float64(square2[0])+float64(square2[2])/2, float64(square2[1])+float64(square2[2])/2
	if x1 == x2 {
		y3 := math.Min(float64(square1[1]), float64(square2[1]))
		y4 := math.Max(float64(square1[1]+square1[2]), float64(square2[1]+square2[2]))
		return []float64{x1, y3, x2, y4}
	}
	k := (y2 - y1) / (x2 - x1)
	b := y1 - k*x1
	if math.Abs(k) > 1 {
		y3 := math.Min(float64(square1[1]), float64(square2[1]))
		x3 := (y3 - b) / k
		y4 := math.Max(float64(square1[1]+square1[2]), float64(square2[1]+square2[2]))
		x4 := (y4 - b) / k
		if x3 > x4 || (x3 == x4 && y3 > y4) {
			return []float64{x4, y4, x3, y3}
		}
		return []float64{x3, y3, x4, y4}
	} else {
		x3 := math.Min(float64(square1[0]), float64(square2[0]))
		y3 := k*x3 + b
		x4 := math.Max(float64(square1[0]+square1[2]), float64(square2[0]+square2[2]))
		y4 := k*x4 + b
		return []float64{x3, y3, x4, y4}
	}
}
```

#### TypeScript

```ts
function cutSquares(square1: number[], square2: number[]): number[] {
    const x1 = square1[0] + square1[2] / 2;
    const y1 = square1[1] + square1[2] / 2;
    const x2 = square2[0] + square2[2] / 2;
    const y2 = square2[1] + square2[2] / 2;
    if (x1 === x2) {
        const y3 = Math.min(square1[1], square2[1]);
        const y4 = Math.max(square1[1] + square1[2], square2[1] + square2[2]);
        return [x1, y3, x2, y4];
    }
    const k = (y2 - y1) / (x2 - x1);
    const b = y1 - k * x1;
    if (Math.abs(k) > 1) {
        const y3 = Math.min(square1[1], square2[1]);
        const x3 = (y3 - b) / k;
        const y4 = Math.max(square1[1] + square1[2], square2[1] + square2[2]);
        const x4 = (y4 - b) / k;
        if (x3 > x4 || (x3 === x4 && y3 > y4)) {
            return [x4, y4, x3, y3];
        }
        return [x3, y3, x4, y4];
    } else {
        const x3 = Math.min(square1[0], square2[0]);
        const y3 = k * x3 + b;
        const x4 = Math.max(square1[0] + square1[2], square2[0] + square2[2]);
        const y4 = k * x4 + b;
        return [x3, y3, x4, y4];
    }
}
```

#### Swift

```swift
class Solution {
    func cutSquares(_ square1: [Int], _ square2: [Int]) -> [Double] {
        let x1 = Double(square1[0]) + Double(square1[2]) / 2.0
        let y1 = Double(square1[1]) + Double(square1[2]) / 2.0
        let x2 = Double(square2[0]) + Double(square2[2]) / 2.0
        let y2 = Double(square2[1]) + Double(square2[2]) / 2.0

        if x1 == x2 {
            let y3 = min(Double(square1[1]), Double(square2[1]))
            let y4 = max(Double(square1[1]) + Double(square1[2]), Double(square2[1]) + Double(square2[2]))
            return [x1, y3, x2, y4]
        }

        let k = (y2 - y1) / (x2 - x1)
        let b = y1 - k * x1

        if abs(k) > 1 {
            let y3 = min(Double(square1[1]), Double(square2[1]))
            let x3 = (y3 - b) / k
            let y4 = max(Double(square1[1]) + Double(square1[2]), Double(square2[1]) + Double(square2[2]))
            let x4 = (y4 - b) / k
            if x3 > x4 || (x3 == x4 && y3 > y4) {
                return [x4, y4, x3, y3]
            }
            return [x3, y3, x4, y4]
        } else {
            let x3 = min(Double(square1[0]), Double(square2[0]))
            let y3 = k * x3 + b
            let x4 = max(Double(square1[0]) + Double(square1[2]), Double(square2[0]) + Double(square2[2]))
            let y4 = k * x4 + b
            return [x3, y3, x4, y4]
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
