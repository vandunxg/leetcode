---
comments: true
difficulty: Medium
tags:
    - Geometry
    - Math
    - Rejection Sampling
    - Randomized
---

<!-- problem:start -->

# [478. Generate Random Point in a Circle](https://leetcode.com/problems/generate-random-point-in-a-circle)

[中文文档](/solution/0400-0499/0478.Generate%20Random%20Point%20in%20a%20Circle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho bán kính và tọa độ tâm của một hình tròn, hãy triển khai hàm <code>randPoint</code> để tạo ngẫu nhiên một điểm phân bố đều bên trong hình tròn.</p>

<p>Hãy triển khai class <code>Solution</code>:</p>

<ul>
	<li><code>Solution(double radius, double x_center, double y_center)</code> khởi tạo object với bán kính hình tròn <code>radius</code> và tọa độ tâm <code>(x_center, y_center)</code>.</li>
	<li><code>randPoint()</code> trả về một điểm ngẫu nhiên bên trong hình tròn. Điểm nằm trên đường tròn cũng được tính là nằm trong hình tròn. Kết quả được trả về dưới dạng mảng <code>[x, y]</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Solution&quot;, &quot;randPoint&quot;, &quot;randPoint&quot;, &quot;randPoint&quot;]
[[1.0, 0.0, 0.0], [], [], []]
<strong>Đầu ra</strong>
[null, [-0.02493, -0.38077], [0.82314, 0.38945], [0.36572, 0.17248]]

<strong>Giải thích</strong>
Solution solution = new Solution(1.0, 0.0, 0.0);
solution.randPoint(); // return [-0.02493, -0.38077]
solution.randPoint(); // return [0.82314, 0.38945]
solution.randPoint(); // return [0.36572, 0.17248]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;&nbsp;radius &lt;= 10<sup>8</sup></code></li>
	<li><code>-10<sup>7</sup> &lt;= x_center, y_center &lt;= 10<sup>7</sup></code></li>
	<li>Sẽ có tối đa <code>3 * 10<sup>4</sup></code> lần gọi <code>randPoint</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần lấy một điểm phân bố đều trong hình tròn. Nếu bán kính và góc đều được chọn ngẫu nhiên theo phân bố đều, các điểm sẽ tập trung nhiều hơn gần tâm. Phần tử diện tích là $r\,dr\,d\theta$, nên $r^2$ cần có phân bố đều.
>
> Lấy $\textit{length}=\sqrt{U(0,R^2)}$ và góc $U(0,2\pi)$, đổi sang tọa độ Cartesian rồi cộng tọa độ tâm.
>
> Chọn bình phương bán kính theo phân bố đều rồi lấy căn bậc hai giúp xác suất tỉ lệ với diện tích vành tròn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def __init__(self, radius: float, x_center: float, y_center: float):
        self.radius = radius
        self.x_center = x_center
        self.y_center = y_center

    def randPoint(self) -> List[float]:
        length = math.sqrt(random.uniform(0, self.radius**2))
        degree = random.uniform(0, 1) * 2 * math.pi
        x = self.x_center + length * math.cos(degree)
        y = self.y_center + length * math.sin(degree)
        return [x, y]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
