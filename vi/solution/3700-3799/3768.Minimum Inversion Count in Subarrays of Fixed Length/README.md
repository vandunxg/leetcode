---
comments: true
difficulty: Hard
rating: 2157
source: Biweekly Contest 171 Q4
tags:
    - Segment Tree
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [3768. Minimum Inversion Count in Subarrays of Fixed Length](https://leetcode.com/problems/minimum-inversion-count-in-subarrays-of-fixed-length)

[中文文档](/solution/3700-3799/3768.Minimum%20Inversion%20Count%20in%20Subarrays%20of%20Fixed%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Một <strong>nghịch thế</strong> là một cặp chỉ số <code>(i, j)</code> trong <code>nums</code> sao cho <code>i &lt; j</code> và <code>nums[i] &gt; nums[j]</code>.</p>

<p><strong>Số lượng nghịch thế</strong> của một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> là số nghịch thế bên trong mảng con đó.</p>

<p>Hãy trả về <strong>số lượng nghịch thế nhỏ nhất</strong> trong tất cả các <strong>mảng con</strong> của <code>nums</code> có độ dài <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2,5,4], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xét tất cả các mảng con có độ dài <code>k = 3</code> (các chỉ số bên dưới là chỉ số tương đối trong mỗi mảng con):</p>

<ul>
	<li><code>[3, 1, 2]</code> có 2 nghịch thế: <code>(0, 1)</code> và <code>(0, 2)</code>.</li>
	<li><code>[1, 2, 5]</code> có 0 nghịch thế.</li>
	<li><code>[2, 5, 4]</code> có 1 nghịch thế: <code>(1, 2)</code>.</li>
</ul>

<p>Số lượng nghịch thế nhỏ nhất trong tất cả các mảng con có độ dài <code>3</code> là 0, đạt được với mảng con <code>[1, 2, 5]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,3,2,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một mảng con có độ dài <code>k = 4</code>: <code>[5, 3, 2, 1]</code>.<br />
Trong mảng con này, các nghịch thế là: <code>(0, 1)</code>, <code>(0, 2)</code>, <code>(0, 3)</code>, <code>(1, 2)</code>, <code>(1, 3)</code> và <code>(2, 3)</code>.<br />
Tổng số nghịch thế là 6, nên số lượng nghịch thế nhỏ nhất là 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các mảng con có độ dài <code>k = 1</code> chỉ chứa một phần tử, nên không thể có nghịch thế nào.<br />
Do đó, số lượng nghịch thế nhỏ nhất là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có $n-k+1$ cửa sổ độ dài $k$, và $n\le 10^5$ khiến việc đếm lại nghịch thế từ đầu là không thể. Hai cửa sổ liên tiếp chỉ khác nhau ở một phần tử được thêm vào và một phần tử bị xóa đi. Fenwick tree trên các giá trị trong cửa sổ có thể cập nhật số lượng nghịch thế khi một phần tử mới lớn hơn các phần tử bên trái hoặc nhỏ hơn các phần tử bên phải được thêm vào, sau đó ta lấy giá trị nhỏ nhất.

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
