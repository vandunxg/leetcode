---
comments: true
difficulty: Medium
rating: 2073
source: Weekly Contest 467 Q3
tags:
    - Array
    - Two Pointers
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3685. Subsequence Sum After Capping Elements](https://leetcode.com/problems/subsequence-sum-after-capping-elements)

[中文文档](/solution/3600-3699/3685.Subsequence%20Sum%20After%20Capping%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p data-end="320" data-start="259">Bạn được cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code> và một số nguyên dương <code>k</code>.</p>

<p data-end="294" data-start="163">Mảng được <strong>giới hạn</strong> bởi giá trị <code>x</code> được tạo ra bằng cách thay mỗi phần tử <code>nums[i]</code> bằng <code>min(nums[i], x)</code>.</p>

<p data-end="511" data-start="296">Với mỗi số nguyên <code data-end="316" data-start="313">x</code> từ 1 đến <code data-end="332" data-start="329">n</code>, hãy xác định liệu có thể chọn một <strong><span data-keyword="subsequence-array-nonempty">subsequence</span></strong> từ mảng được giới hạn bởi <code>x</code> sao cho tổng các phần tử được chọn <strong>chính xác</strong> bằng <code data-end="510" data-start="507">k</code> hay không.</p>

<p data-end="788" data-start="649">Trả về một mảng boolean <code data-end="680" data-start="672">answer</code> <strong>được đánh chỉ số từ 0</strong> và có kích thước <code data-end="694" data-start="691">n</code>, trong đó <code data-end="713" data-start="702">answer[i]</code> là <code data-end="723" data-start="717">true</code> nếu có thể thực hiện được khi sử dụng <code data-end="764" data-start="753">x = i + 1</code>, và là <code data-end="777" data-start="770">false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,2,4], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false,false,true,true]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>x = 1</code>, mảng được giới hạn là <code>[1, 1, 1, 1]</code>. Các tổng có thể tạo thành là <code>1, 2, 3, 4</code>, nên không thể tạo tổng bằng <code>5</code>.</li>
	<li>Với <code>x = 2</code>, mảng được giới hạn là <code>[2, 2, 2, 2]</code>. Các tổng có thể tạo thành là <code>2, 4, 6, 8</code>, nên không thể tạo tổng bằng <code>5</code>.</li>
	<li>Với <code>x = 3</code>, mảng được giới hạn là <code>[3, 3, 2, 3]</code>. Một subsequence <code>[2, 3]</code> có tổng bằng <code>5</code>, nên có thể thực hiện được.</li>
	<li>Với <code>x = 4</code>, mảng được giới hạn là <code>[4, 3, 2, 4]</code>. Một subsequence <code>[3, 2]</code> có tổng bằng <code>5</code>, nên có thể thực hiện được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,true,true,true,true]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mọi giá trị của <code>x</code>, luôn có thể chọn một subsequence từ mảng được giới hạn sao cho tổng chính xác bằng <code>3</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 4000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= n</code></li>
	<li><code>1 &lt;= k &lt;= 4000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi giới hạn $x=1\ldots n$, coi các phần tử lớn hơn là $x$ rồi kiểm tra xem có subsequence nào có tổng bằng $k$ hay không. Thực hiện một bài toán knapsack mới cho mỗi $x$ có độ phức tạp $O(n^2k)$, quá chậm với $n\le 4000$.
>
> Các giá trị đã $\le x$ tạo thành một bài toán knapsack $0$-$1$; $c$ giá trị lớn hơn $x$ trở thành $c$ bản sao của $x$. Khi tăng $x$, ta chèn mỗi giá trị vừa được bỏ giới hạn đúng một lần.
>
> Trên tập các tổng có thể đạt được hiện tại, kiểm tra xem có $t\le k$ nào để phần còn lại $k-t$ có thể tạo thành từ nhiều nhất $c$ bản sao của $x$ hay không. Bitset giúp mỗi truy vấn với $x$ có độ phức tạp $O(k/w)$.

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
