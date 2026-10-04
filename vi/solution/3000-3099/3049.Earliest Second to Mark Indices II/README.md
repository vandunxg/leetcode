---
comments: true
difficulty: Hard
rating: 3111
source: Weekly Contest 386 Q4
tags:
    - Greedy
    - Array
    - Binary Search
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3049. Earliest Second to Mark Indices II](https://leetcode.com/problems/earliest-second-to-mark-indices-ii)

[中文文档](/solution/3000-3099/3049.Earliest%20Second%20to%20Mark%20Indices%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>được đánh số từ 1</strong>, <code>nums</code> và <code>changeIndices</code>, có độ dài lần lượt là <code>n</code> và <code>m</code>.</p>

<p>Ban đầu, tất cả các chỉ số trong <code>nums</code> đều chưa được đánh dấu. Nhiệm vụ của bạn là đánh dấu <strong>tất cả</strong> các chỉ số trong <code>nums</code>.</p>

<p>Trong mỗi giây <code>s</code>, theo thứ tự từ <code>1</code> đến <code>m</code> (<strong>bao gồm cả hai đầu</strong>), bạn có thể thực hiện <strong>một</strong> trong các thao tác sau:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong khoảng <code>[1, n]</code> và <strong>giảm</strong> <code>nums[i]</code> đi <code>1</code>.</li>
	<li>Đặt <code>nums[changeIndices[s]]</code> thành một giá trị <strong>không âm</strong> bất kỳ.</li>
	<li>Chọn một chỉ số <code>i</code> trong khoảng <code>[1, n]</code>, sao cho <code>nums[i]</code> <strong>bằng</strong> <code>0</code>, và <strong>đánh dấu</strong> chỉ số <code>i</code>.</li>
	<li>Không làm gì.</li>
</ul>

<p>Trả về <em>một số nguyên biểu thị <strong>giây sớm nhất</strong> trong khoảng </em><code>[1, m]</code><em> mà <strong>tất cả</strong> các chỉ số trong </em><code>nums</code><em> có thể được đánh dấu bằng cách lựa chọn thao tác tối ưu, hoặc </em><code>-1</code><em> nếu không thể.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,3], changeIndices = [1,3,2,2,2,2,3]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Trong ví dụ này, có 7 giây. Có thể thực hiện các thao tác sau để đánh dấu tất cả các chỉ số:
Giây 1: Đặt nums[changeIndices[1]] thành 0. nums trở thành [0,2,3].
Giây 2: Đặt nums[changeIndices[2]] thành 0. nums trở thành [0,2,0].
Giây 3: Đặt nums[changeIndices[3]] thành 0. nums trở thành [0,0,0].
Giây 4: Đánh dấu chỉ số 1 vì nums[1] bằng 0.
Giây 5: Đánh dấu chỉ số 2 vì nums[2] bằng 0.
Giây 6: Đánh dấu chỉ số 3 vì nums[3] bằng 0.
Bây giờ tất cả các chỉ số đã được đánh dấu.
Có thể chứng minh rằng không thể đánh dấu tất cả các chỉ số sớm hơn giây thứ 6.
Do đó, đáp án là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,1,2], changeIndices = [1,2,1,2,1,2,1,2]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Trong ví dụ này, có 8 giây. Có thể thực hiện các thao tác sau để đánh dấu tất cả các chỉ số:
Giây 1: Đánh dấu chỉ số 1 vì nums[1] bằng 0.
Giây 2: Đánh dấu chỉ số 2 vì nums[2] bằng 0.
Giây 3: Giảm chỉ số 4 đi một đơn vị. nums trở thành [0,0,1,1].
Giây 4: Giảm chỉ số 4 đi một đơn vị. nums trở thành [0,0,1,0].
Giây 5: Giảm chỉ số 3 đi một đơn vị. nums trở thành [0,0,0,0].
Giây 6: Đánh dấu chỉ số 3 vì nums[3] bằng 0.
Giây 7: Đánh dấu chỉ số 4 vì nums[4] bằng 0.
Bây giờ tất cả các chỉ số đã được đánh dấu.
Có thể chứng minh rằng không thể đánh dấu tất cả các chỉ số sớm hơn giây thứ 7.
Do đó, đáp án là 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], changeIndices = [1,2,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích: </strong>Trong ví dụ này, có thể chứng minh rằng không thể đánh dấu tất cả các chỉ số vì không có đủ thời gian.
Do đó, đáp án là -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 5000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m == changeIndices.length &lt;= 5000</code></li>
	<li><code>1 &lt;= changeIndices[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> So với phần I, ta có thể đặt một chỉ số về 0 trong một giây, và $n,m \le 5000$. Tính khả thi vẫn đơn điệu, nhưng mỗi chỉ số có thể được đưa về 0 bằng thao tác đặt lại hoặc bằng các lần giảm thông thường.
>
> Thao tác đặt lại có lợi hơn với $nums[i]$ lớn, và mỗi chỉ số chỉ cần dùng nhiều nhất một vị trí đặt lại sớm nhất của nó. Khi kiểm tra một ứng viên $t$, ta cần một heap để cân đối giữa số lần giảm được tiết kiệm và số giây phải dùng cho các thao tác đặt lại.
>
> Ta tìm kiếm nhị phân $t$ và duyệt ngược $t$ giây đầu tiên. Lần xuất hiện đầu tiên của một $nums[i]$ dương được đưa vào heap; ta lấy phần tử khỏi heap khi việc giảm giá trị trở nên có lợi hơn, để số giây còn lại đủ đánh dấu mọi chỉ số.

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
