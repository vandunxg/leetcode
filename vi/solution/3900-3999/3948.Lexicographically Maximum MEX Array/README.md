---
comments: true
difficulty: Hard
rating: 2122
source: Weekly Contest 504 Q4
tags:
    - Greedy
    - Queue
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3948. Lexicographically Maximum MEX Array](https://leetcode.com/problems/lexicographically-maximum-mex-array)

[中文文档](/solution/3900-3999/3948.Lexicographically%20Maximum%20MEX%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn muốn tạo một mảng <code>result</code> bằng cách lặp lại thao tác sau cho đến khi <code>nums</code> trở thành mảng rỗng:</p>

<ul>
	<li>Chọn một số nguyên <code>k</code> sao cho <code>1 &lt;= k &lt;= len(nums)</code>.</li>
	<li>Tính <strong>MEX</strong> của <code>k</code> phần tử đầu tiên của <code>nums</code>.</li>
	<li>Thêm <strong>MEX</strong> vào cuối <code>result</code>.</li>
	<li>Xóa <code>k</code> phần tử đầu tiên khỏi <code>nums</code>.</li>
</ul>

<p>Trả về mảng <code>result</code> <strong>lớn nhất theo thứ tự từ điển</strong> có thể nhận được sau khi thực hiện các thao tác.</p>

<p><strong>MEX</strong> của một mảng là số nguyên <strong>không âm nhỏ nhất</strong> không xuất hiện trong mảng.</p>

<p>Một mảng <code>a</code> được gọi là <strong>lớn hơn theo thứ tự từ điển</strong> mảng <code>b</code> nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, phần tử của mảng <code>a</code> lớn hơn phần tử tương ứng của <code>b</code>. Nếu <code>min(a.length, b.length)</code> phần tử đầu tiên không khác nhau, mảng dài hơn được xem là <strong>lớn hơn theo thứ tự từ điển</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 2</code> phần tử đầu tiên <code>[0, 1]</code>, có MEX = 2. Khi đó <code>result = [2]</code>.</li>
	<li>Mảng còn lại <code>[0]</code> có MEX = 1. Vì vậy, <code>result = [2, 1]</code> cuối cùng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 3</code> phần tử đầu tiên <code>[1, 0, 2]</code>, có MEX = 3.</li>
	<li><code><span class="example-io">nums</span></code> lúc này đã rỗng. Vì vậy, <code>result = [3]</code> cuối cùng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0]</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Chọn <code>k = 1</code>, phần tử đầu tiên <code>[3]</code> có MEX = 0. Khi đó <code>result = [0]</code>.</li>
	<li>Mảng còn lại <code>[1]</code> có MEX = 0. Vì vậy, <code>result = [0, 0]</code> cuối cùng.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước lấy MEX của một prefix rồi loại bỏ prefix đó. Với $n\le 10^5$, ta không thể tính lại MEX cho mọi $k$. Tính lớn nhất theo thứ tự từ điển yêu cầu MEX lớn nhất có thể xuất hiện càng sớm càng tốt, tức là cắt ngay khi MEX không còn tăng.
>
> Ta duy trì số lần xuất hiện và MEX của cửa sổ; cắt khi việc mở rộng không thể làm MEX tăng thêm (hoặc làm MEX giảm). Lặp lại cho đến khi mảng rỗng.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở thao tác cắt tham lam đó.

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
