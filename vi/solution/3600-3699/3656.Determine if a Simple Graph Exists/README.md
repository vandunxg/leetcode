---
comments: true
difficulty: Medium
tags:
    - Graph
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [3656. Determine if a Simple Graph Exists 🔒](https://leetcode.com/problems/determine-if-a-simple-graph-exists)

[中文文档](/solution/3600-3699/3656.Determine%20if%20a%20Simple%20Graph%20Exists/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>degrees</code>, trong đó <code>degrees[i]</code> biểu diễn bậc mong muốn của đỉnh thứ <code>i<sup>th</sup></code>.</p>

<p>Nhiệm vụ của bạn là xác định xem có tồn tại một đồ thị <strong>vô hướng đơn</strong> có <strong>chính xác</strong> các bậc đỉnh này hay không.</p>

<p>Một đồ thị <strong>đơn</strong> không có self-loop hoặc cạnh song song giữa cùng một cặp đỉnh.</p>

<p>Trả về <code>true</code> nếu tồn tại đồ thị như vậy, nếu không thì trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">degrees = [3,1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3656.Determine%20if%20a%20Simple%20Graph%20Exists/images/screenshot-2025-08-13-at-24347-am.png" style="width: 200px; height: 132px;" />​​​​​​​</p>

<p>Một đồ thị vô hướng đơn khả dĩ là:</p>

<ul>
	<li>Các cạnh: <code>(0, 1), (0, 2), (0, 3), (2, 3)</code></li>
	<li>Bậc: <code>deg(0) = 3</code>, <code>deg(1) = 1</code>, <code>deg(2) = 2</code>, <code>deg(3) = 2</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">degrees = [1,3,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li><code>degrees[1] = 3</code> và <code>degrees[2] = 3</code> có nghĩa là chúng phải được nối với tất cả các đỉnh khác.</li>
	<li>Điều này yêu cầu <code>degrees[0]</code> và <code>degrees[3]</code> phải ít nhất bằng 2, nhưng cả hai đều bằng 1, mâu thuẫn với yêu cầu.</li>
	<li>Vì vậy, đáp án là <code>false</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == degrees.length &lt;= 10<sup>​​​​​​​5</sup></code></li>
	<li><code>0 &lt;= degrees[i] &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cho một dãy bậc và cần quyết định xem có tồn tại một đồ thị vô hướng đơn hay không. Havel–Hakimi phải sắp xếp lại sau mỗi bước nên quá chậm với $n\le 10^5$.
>
> Định lý Erdős–Gállai thay thế vòng lặp đó bằng các bất đẳng thức prefix cùng với điều kiện tổng bậc là số chẵn. Sau khi sắp xếp, ta dùng các tổng prefix để kiểm tra các bất đẳng thức trong thời gian tuyến tính.
>
> Loại ngay khi tổng là số lẻ hoặc có bậc lớn hơn $n-1$, sau đó so sánh tổng của $k$ bậc lớn nhất với vế phải đã cắt ngưỡng của định lý.

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
