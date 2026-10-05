---
comments: true
difficulty: Medium
rating: 1899
source: Weekly Contest 520 Q3
---

<!-- problem:start -->

# [4058. Maximum Pulse Value After One Subarray Rotation](https://leetcode.com/problems/maximum-pulse-value-after-one-subarray-rotation)

[Tài liệu tiếng Trung](/solution/4000-4099/4058.Maximum%20Pulse%20Value%20After%20One%20Subarray%20Rotation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Định nghĩa <strong>giá trị pulse</strong> của một mảng số nguyên <code>arr</code> là <strong>tổng xen kẽ</strong> bắt đầu từ chỉ số 0: <code>pulse(arr) = arr[0] - arr[1] + arr[2] - arr[3] + ...</code>.</p>

<p>Bạn có thể thực hiện <strong>nhiều nhất</strong> một thao tác trên <code>nums</code>:</p>

<ul>
	<li>Chọn hai chỉ số <code>l</code> và <code>r</code> sao cho <code>0 &lt;= l &lt; r &lt; n</code>.</li>
	<li><strong>Xoay trái</strong> <span data-keyword="subarray-nonempty">mảng con</span> <code>nums[l..r]</code> <strong>đúng</strong> một vị trí. Ví dụ, <code>[a, b, c, d]</code> trở thành <code>[b, c, d, a]</code>.</li>
</ul>

<p>Trả về <strong>giá trị pulse lớn nhất</strong> có thể đạt được sau khi thực hiện <strong>nhiều nhất</strong> một thao tác như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị pulse ban đầu là <code>1 - 5 + 2 = -2</code>.</li>
	<li>Xoay mảng con <code>nums[0..1]</code> từ <code>[1, 5]</code> thành <code>[5, 1]</code>.</li>
	<li>Mảng kết quả là <code>[5, 1, 2]</code> và giá trị pulse của nó là <code>5 - 1 + 2 = 6</code>, đây là giá trị lớn nhất có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị pulse ban đầu là <code>6 - 4 + 3 = 5</code>.</li>
	<li>Xoay mảng con <code>nums[1..2]</code> từ <code>[4, 3]</code> thành <code>[3, 4]</code>.</li>
	<li>Mảng kết quả là <code>[6, 3, 4]</code> và giá trị pulse của nó là <code>6 - 3 + 4 = 7</code>, đây là giá trị lớn nhất có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giá trị pulse ban đầu là <code>9 - 7 = 2</code>, vốn đã là giá trị lớn nhất. Vì vậy, không cần xoay.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

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
