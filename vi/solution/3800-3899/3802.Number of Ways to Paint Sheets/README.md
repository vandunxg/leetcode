---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [3802. Number of Ways to Paint Sheets 🔒](https://leetcode.com/problems/number-of-ways-to-paint-sheets)

[中文文档](/solution/3800-3899/3802.Number%20of%20Ways%20to%20Paint%20Sheets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số lượng tờ giấy.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>limit</code> có kích thước <code>m</code>, trong đó <code>limit[i]</code> là số lượng tờ giấy <strong>tối đa</strong> có thể được sơn bằng màu <code>i</code>.</p>

<p>Bạn phải sơn <strong>tất cả</strong> <code>n</code> tờ giấy theo các điều kiện sau:</p>

<ul>
	<li>Sử dụng <strong>chính xác hai màu phân biệt</strong>.</li>
	<li>Mỗi màu phải phủ một <strong>đoạn liên tiếp duy nhất</strong> của các tờ giấy.</li>
	<li>Số lượng tờ giấy được sơn bằng màu <code>i</code> không được vượt quá <code>limit[i]</code>.</li>
</ul>

<p>Trả về một số nguyên biểu thị số cách <strong>khác nhau</strong> để sơn tất cả các tờ giấy. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý:</strong> Hai cách được xem là khác nhau nếu <strong>ít nhất</strong> một tờ giấy được sơn bằng màu khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, limit = [3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>
Với mỗi cặp có thứ tự <code>(i, j)</code>, trong đó màu <code>i</code> được dùng cho đoạn thứ nhất và màu <code>j</code> cho đoạn thứ hai (<code>i != j</code>), cách chia thành <code>x</code> và <code>4 - x</code> là hợp lệ nếu <code>1 &lt;= x &lt;= limit[i]</code> và <code>1 &lt;= 4 - x &lt;= limit[j]</code>.

<p>Các cặp hợp lệ và số cách tương ứng:</p>

<ul>
	<li><code>(0, 1): x = 3</code></li>
	<li><code>(0, 2): x = 2, 3</code></li>
	<li><code>(1, 0): x = 1</code></li>
	<li><code>(2, 0): x = 1, 2</code></li>
</ul>

<p>Do đó, tổng cộng có 6 cách hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, limit = [1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi cặp có thứ tự <code>(i, j)</code>, trong đó màu <code>i</code> được dùng cho đoạn thứ nhất và màu <code>j</code> cho đoạn thứ hai (<code>i != j</code>), cách chia thành <code>x</code> và <code>3 - x</code> là hợp lệ nếu <code>1 &lt;= x &lt;= limit[i]</code> và <code>1 &lt;= 3 - x &lt;= limit[j]</code>.</p>

<p>Các cặp hợp lệ và số cách tương ứng:</p>

<ul>
	<li><code>(0, 1): x = 1</code></li>
	<li><code>(1, 0): x = 2</code></li>
</ul>

<p>Vì vậy, tổng cộng có 2 cách hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, limit = [2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi cặp có thứ tự <code>(i, j)</code>, trong đó màu <code>i</code> được dùng cho đoạn thứ nhất và màu <code>j</code> cho đoạn thứ hai (<code>i != j</code>), cách chia thành <code>x</code> và <code>3 - x</code> là hợp lệ nếu <code>1 &lt;= x &lt;= limit[i]</code> và <code>1 &lt;= 3 - x &lt;= limit[j]</code>.</p>

<p>Các cặp hợp lệ và số cách tương ứng:</p>

<ul>
	<li><code>(0, 1): x = 1, 2</code></li>
	<li><code>(1, 0): x = 1, 2</code></li>
</ul>

<p>Do đó, tổng cộng có 4 cách hợp lệ.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>2 &lt;= m == limit.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= limit[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải sử dụng chính xác hai màu, mỗi màu trên một khối liên tiếp và không vượt quá $\textit{limit}$. Vì $n \le 10^9$, không thể liệt kê từng vị trí cắt của các tờ giấy.
>
> Với một cặp có thứ tự $(i,j)$, các vị trí cắt hợp lệ $x$ tạo thành một đoạn nguyên được xác định bởi $n$ và hai giới hạn, có độ dài $O(1)$.
>
> Với $m \le 10^5$, ghép từng cặp màu sẽ tốn $O(m^2)$ ngay cả khi vị trí cắt đã có công thức đóng. Nút thắt nằm ở việc đếm đóng góp của nhiều giới hạn cùng lúc.
>
> Đóng góp của mỗi màu chỉ phụ thuộc vào vị trí của giới hạn tương ứng so với $n$. Sắp xếp $\textit{limit}$ và cộng dồn độ dài các đoạn bằng tổng tiền tố sẽ cho đáp án theo modulo $10^9+7$.

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
