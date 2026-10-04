---
comments: true
difficulty: Hard
rating: 3077
source: Weekly Contest 424 Q4
tags:
    - Greedy
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3357. Minimize the Maximum Adjacent Element Difference](https://leetcode.com/problems/minimize-the-maximum-adjacent-element-difference)

[中文文档](/solution/3300-3399/3357.Minimize%20the%20Maximum%20Adjacent%20Element%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Một số giá trị trong <code>nums</code> <strong>bị thiếu</strong> và được biểu diễn bằng -1.</p>

<p>Bạn phải chọn một cặp số nguyên <strong>dương</strong> <code>(x, y)</code> <strong>đúng một lần</strong> và thay mỗi phần tử <strong>bị thiếu</strong> bằng <em>một trong hai giá trị</em> <code>x</code> hoặc <code>y</code>.</p>

<p>Bạn cần <strong>tối thiểu hóa</strong><strong> </strong>độ<strong> lớn nhất</strong> <strong>độ chênh lệch tuyệt đối</strong> giữa các phần tử <em>kề nhau</em> của <code>nums</code> sau khi thay thế.</p>

<p>Trả về <strong>độ chênh lệch</strong> nhỏ nhất có thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,-1,10,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bằng cách chọn cặp số là <code>(6, 7)</code>, ta có thể biến đổi nums thành <code>[1, 2, 6, 10, 8]</code>.</p>

<p>Độ chênh lệch tuyệt đối giữa các phần tử kề nhau là:</p>

<ul>
    <li><code>|1 - 2| == 1</code></li>
    <li><code>|2 - 6| == 4</code></li>
    <li><code>|6 - 10| == 4</code></li>
    <li><code>|10 - 8| == 2</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-1,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bằng cách chọn cặp số là <code>(4, 4)</code>, ta có thể biến đổi nums thành <code>[4, 4, 4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,10,-1,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bằng cách chọn cặp số là <code>(11, 9)</code>, ta có thể biến đổi nums thành <code>[11, 10, 9, 8]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>nums[i]</code> là -1 hoặc nằm trong khoảng <code>[1, 10<sup>9</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta thay mỗi $-1$ bằng một giá trị trong $[1,\textit{limit}]$ để tối thiểu hóa độ chênh lệch lớn nhất giữa hai phần tử kề nhau. Với $n \le 10^5$, ta tìm kiếm nhị phân giá trị lớn nhất này.
>
> Các phần tử kề nhau đã biết tạo ra một cận dưới. Các khoảng trống là những đoạn liên tiếp gồm $-1$, được điền bằng nhiều nhất hai hằng số; ta kiểm tra xem các hằng số đó có thể nối với cả hai đầu mút dưới ngưỡng $d$ hay không.
>
> Nếu một đoạn quá dài hoặc hai đầu mút của nó chênh lệch lớn hơn $2d$, ta loại $d$. Giá trị $d$ nhỏ nhất khả thi là đáp án.

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
