---
comments: true
difficulty: Hard
tags:
    - Geometry
    - Array
    - Math
    - Smallest Enclosing Circle
---

<!-- problem:start -->

# [1924. Erect the Fence II 🔒](https://leetcode.com/problems/erect-the-fence-ii)

[中文文档](/solution/1900-1999/1924.Erect%20the%20Fence%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>trees</code>, trong đó <code>trees[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn vị trí của cây thứ <code>i<sup>th</sup></code> trong khu vườn.</p>

<p>Bạn được yêu cầu rào toàn bộ khu vườn bằng sợi dây có độ dài nhỏ nhất có thể. Khu vườn chỉ được rào kín nếu <strong>tất cả các cây đều nằm bên trong</strong> và sợi dây được sử dụng <strong>tạo thành một đường tròn hoàn hảo</strong>. Một cây được xem là nằm bên trong nếu nó nằm bên trong hoặc trên đường biên của đường tròn.</p>

<p>Cụ thể hơn, bạn phải tạo một đường tròn bằng sợi dây với tâm <code>(x, y)</code> và bán kính <code>r</code>, sao cho tất cả các cây đều nằm bên trong hoặc trên đường tròn và <code>r</code> là <strong>nhỏ nhất</strong>.</p>

<p>Trả về <em>tâm và bán kính của đường tròn dưới dạng một mảng có độ dài 3 là </em><code>[x, y, r]</code><em>.</em>&nbsp;Các đáp án có sai số không quá <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1924.Erect%20the%20Fence%20II/images/trees1.png" style="width: 510px; height: 501px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> trees = [[1,1],[2,2],[2,0],[2,4],[3,3],[4,2]]
<strong>Đầu ra:</strong> [2.00000,2.00000,2.00000]
<strong>Giải thích:</strong> Hàng rào sẽ có tâm = (2, 2) và bán kính = 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1924.Erect%20the%20Fence%20II/images/trees2.png" style="width: 510px; height: 501px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> trees = [[1,2],[2,2],[4,2]]
<strong>Đầu ra:</strong> [2.50000,2.00000,1.50000]
<strong>Giải thích:</strong> Hàng rào sẽ có tâm = (2.5, 2) và bán kính = 1.5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= trees.length &lt;= 3000</code></li>
	<li><code>trees[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 3000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đường tròn bao nhỏ nhất của các điểm chính là đường tròn cần tìm. Thử mọi cặp hoặc bộ ba điểm có độ phức tạp là $O(n^3)$, không phù hợp với $n\le 3000$.
>
> Đường tròn được xác định bởi hai điểm ở hai đầu đường kính hoặc ba điểm trên đường biên. Phương pháp xây dựng tăng dần ngẫu nhiên của Welzl có độ phức tạp tuyến tính kỳ vọng: một điểm mới nằm bên trong đường tròn hiện tại thì không làm thay đổi kết quả; ngược lại, nó phải nằm trên đường biên tiếp theo và bài toán thu nhỏ lại.
>
> Hãy xáo trộn các điểm, duy trì tâm và bán kính hiện tại, đồng thời giữ sai số trong giới hạn cho phép là $10^{-5}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python

```

#### Java

```java

```

#### C++

```cpp

```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
