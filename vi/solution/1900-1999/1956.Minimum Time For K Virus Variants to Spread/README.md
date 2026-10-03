---
comments: true
difficulty: Hard
tags:
    - Geometry
    - Array
    - Math
    - Binary Search
    - Enumeration
    - Ordered Set
    - Sliding Window
---

<!-- problem:start -->

# [1956. Minimum Time For K Virus Variants to Spread 🔒](https://leetcode.com/problems/minimum-time-for-k-virus-variants-to-spread)

[中文文档](/solution/1900-1999/1956.Minimum%20Time%20For%20K%20Virus%20Variants%20to%20Spread/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> biến thể virus <strong>khác nhau</strong> trên một lưới 2D vô hạn. Cho một mảng 2 chiều <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn một virus bắt nguồn tại <code>(x<sub>i</sub>, y<sub>i</sub>)</code> vào ngày <code>0</code>. Lưu ý rằng <strong>nhiều</strong> biến thể virus có thể bắt nguồn tại <strong>cùng một</strong> điểm.</p>

<p>Mỗi ngày, mỗi ô bị nhiễm bởi một biến thể virus sẽ truyền virus đến <strong>tất cả</strong> các điểm lân cận theo <strong>bốn</strong> hướng chính (lên, xuống, trái và phải). Nếu một ô có nhiều biến thể, tất cả các biến thể sẽ lan truyền mà không cản trở lẫn nhau.</p>

<p>Cho một số nguyên <code>k</code>, hãy trả về <em>số ngày nguyên <strong>nhỏ nhất</strong> để <strong>bất kỳ</strong> điểm nào chứa <strong>ít nhất</strong> </em><code>k</code><em> biến thể virus khác nhau</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1956.Minimum%20Time%20For%20K%20Virus%20Variants%20to%20Spread/images/case-1.png" style="width: 421px; height: 256px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,1],[6,1]], k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Vào ngày 3, các điểm (3,1) và (4,1) sẽ chứa cả hai biến thể virus. Lưu ý rằng đây không phải là những điểm duy nhất chứa cả hai biến thể virus.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1956.Minimum%20Time%20For%20K%20Virus%20Variants%20to%20Spread/images/case-2.png" style="width: 416px; height: 257px;" />
<pre>
<strong>Đầu vào:</strong> points = [[3,3],[1,2],[9,2]], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Vào ngày 2, các điểm (1,3), (2,3), (2,2) và (3,2) sẽ chứa hai virus đầu tiên. Lưu ý rằng đây không phải là những điểm duy nhất chứa cả hai biến thể virus.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1956.Minimum%20Time%20For%20K%20Virus%20Variants%20to%20Spread/images/case-2.png" style="width: 416px; height: 257px;" />
<pre>
<strong>Đầu vào:</strong> points = [[3,3],[1,2],[9,2]], k = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Vào ngày 4, điểm (5,2) sẽ chứa cả 3 virus. Lưu ý rằng đây không phải là điểm duy nhất chứa cả 3 biến thể virus.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == points.length</code></li>
	<li><code>2 &lt;= n &lt;= 50</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
	<li><code>2 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi virus lan truyền theo khoảng cách Manhattan; ta cần tìm ngày đầu tiên có một ô chứa ít nhất $k$ virus. Vì $n\le 50$, ta có thể dùng tìm kiếm nhị phân trên số ngày.
>
> Ở ngày $t$, mỗi virus tạo ra một hình thoi bán kính $t$. Có thể kiểm tra một điểm có nằm trong ít nhất $k$ hình thoi hay không bằng cách xoay sang $(x+y,x-y)$ rồi dùng kỹ thuật quét hoặc liệt kê các ứng viên giao nhau.
>
> Tìm kiếm nhị phân ngày $t$ nhỏ nhất; mỗi lần kiểm tra đều đủ nhanh với giá trị $n$ này.

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
