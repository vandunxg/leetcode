---
comments: true
difficulty: Hard
rating: 2854
source: Weekly Contest 432 Q4
tags:
    - Stack
    - Segment Tree
    - Queue
    - Array
    - Sliding Window
    - Monotonic Queue
    - Monotonic Stack
---

<!-- problem:start -->

# [3420. Count Non-Decreasing Subarrays After K Operations](https://leetcode.com/problems/count-non-decreasing-subarrays-after-k-operations)

[中文文档](/solution/3400-3499/3420.Count%20Non-Decreasing%20Subarrays%20After%20K%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm <code>n</code> số nguyên và một số nguyên <code>k</code>.</p>

<p>Với mỗi mảng con của <code>nums</code>, bạn có thể thực hiện <strong>tối đa</strong> <code>k</code> thao tác trên mảng con đó. Trong mỗi thao tác, bạn tăng một phần tử bất kỳ của mảng con lên 1.</p>

<p><strong>Lưu ý</strong> rằng mỗi mảng con được xét độc lập, nghĩa là các thay đổi trên một mảng con không được áp dụng cho các mảng con khác.</p>

<p>Trả về số lượng mảng con mà bạn có thể biến thành <strong>không giảm</strong> ​​​​​ sau khi thực hiện nhiều nhất <code>k</code> thao tác.</p>

<p>Một mảng được gọi là <strong>không giảm</strong> nếu mỗi phần tử lớn hơn hoặc bằng phần tử đứng trước nó, nếu phần tử đó tồn tại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,3,1,2,4,4], k = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong tổng số 21 mảng con có thể có của <code>nums</code>, chỉ các mảng con <code>[6, 3, 1]</code>, <code>[6, 3, 1, 2]</code>, <code>[6, 3, 1, 2, 4]</code> và <code>[6, 3, 1, 2, 4, 4]</code> không thể được biến thành không giảm sau khi thực hiện tối đa k = 7 thao tác. Vì vậy, số lượng mảng con không giảm là <code>21 - 4 = 17</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,3,1,3,6], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[3, 1, 3, 6]</code> cùng với mọi mảng con của <code>nums</code> có không quá ba phần tử, ngoại trừ <code>[6, 3, 1]</code>, đều có thể được biến thành không giảm sau <code>k</code> thao tác. Có 5 mảng con gồm một phần tử, 4 mảng con gồm hai phần tử và 2 mảng con gồm ba phần tử, ngoại trừ <code>[6, 3, 1]</code>, nên có <code>1 + 5 + 4 + 2 = 12</code> mảng con có thể được biến thành không giảm.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác tăng một phần tử lên 1. Để biến một mảng con thành không giảm, ta cần nâng từng vị trí lên giá trị lớn nhất của tiền tố bên trái; chi phí là tổng lượng tăng. $n\le 10^5$ khiến việc tính lại cho mọi mảng con là không khả thi.
>
> Các mảng con dài hơn không bao giờ có chi phí thấp hơn, vì vậy với mỗi đầu phải, tồn tại một đầu trái xa nhất sao cho chi phí vẫn $\le k$.
>
> Một monotonic stack lưu các đoạn đóng vai trò là giá trị lớn nhất của tiền tố. Con trỏ trái loại bỏ các đoạn đã hết hiệu lực và hoàn lại chi phí tương ứng. Cộng số đầu trái hợp lệ với mỗi đầu phải sẽ cho đáp án.

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
