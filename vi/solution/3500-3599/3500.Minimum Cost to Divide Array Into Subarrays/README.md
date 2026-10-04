---
comments: true
difficulty: Hard
rating: 2569
source: Biweekly Contest 153 Q3
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
    - Li Chao Tree
---

<!-- problem:start -->

# [3500. Minimum Cost to Divide Array Into Subarrays](https://leetcode.com/problems/minimum-cost-to-divide-array-into-subarrays)

[中文文档](/solution/3500-3599/3500.Minimum%20Cost%20to%20Divide%20Array%20Into%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>nums</code> và <code>cost</code> có cùng kích thước, cùng một số nguyên <code>k</code>.</p>

<p>Bạn có thể chia <code>nums</code> thành các <span data-keyword="subarray-nonempty">mảng con</span>. Chi phí của mảng con thứ <code>i<sup>th</sup></code> gồm các phần tử <code>nums[l..r]</code> là:</p>

<ul>
    <li><code>(nums[0] + nums[1] + ... + nums[r] + k * i) * (cost[l] + cost[l + 1] + ... + cost[r])</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng <code>i</code> biểu thị thứ tự của mảng con: 1 cho mảng con đầu tiên, 2 cho mảng con thứ hai, v.v.</p>

<p>Trả về tổng chi phí <strong>nhỏ nhất</strong> có thể đạt được từ bất kỳ cách chia hợp lệ nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,4], cost = [4,6,6], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">110</span></p>

<p><strong>Giải thích:</strong></p>
Tổng chi phí nhỏ nhất có thể đạt được bằng cách chia <code>nums</code> thành các mảng con <code>[3, 1]</code> và <code>[4]</code>.

<ul>
    <li>Chi phí của mảng con đầu tiên <code>[3,1]</code> là <code>(3 + 1 + 1 * 1) * (4 + 6) = 50</code>.</li>
    <li>Chi phí của mảng con thứ hai <code>[4]</code> là <code>(3 + 1 + 4 + 1 * 2) * 6 = 60</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,8,5,1,14,2,2,12,1], cost = [7,2,8,4,2,2,1,1,2], k = 7</span></p>

<p><strong>Đầu ra:</strong> 985</p>

<p><strong>Giải thích:</strong></p>
Tổng chi phí nhỏ nhất có thể đạt được bằng cách chia <code>nums</code> thành các mảng con <code>[4, 8, 5, 1]</code>, <code>[14, 2, 2]</code> và <code>[12, 1]</code>.

<ul>
    <li>Chi phí của mảng con đầu tiên <code>[4, 8, 5, 1]</code> là <code>(4 + 8 + 5 + 1 + 7 * 1) * (7 + 2 + 8 + 4) = 525</code>.</li>
    <li>Chi phí của mảng con thứ hai <code>[14, 2, 2]</code> là <code>(4 + 8 + 5 + 1 + 14 + 2 + 2 + 7 * 2) * (2 + 2 + 1) = 250</code>.</li>
    <li>Chi phí của mảng con thứ ba <code>[12, 1]</code> là <code>(4 + 8 + 5 + 1 + 14 + 2 + 2 + 12 + 1 + 7 * 3) * (1 + 2) = 210</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 1000</code></li>
    <li><code>cost.length == nums.length</code></li>
    <li><code>1 &lt;= nums[i], cost[i] &lt;= 1000</code></li>
    <li><code>1 &lt;= k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có $2^{n-1}$ cách chia và $n \le 1000$, nên không thể duyệt qua mọi vị trí cắt. Chi phí của đoạn thứ $i$ $[l,r]$ là $(\textit{prefN}[r] + k \cdot i) \cdot (\textit{prefC}[r] - \textit{prefC}[l-1])$, chỉ phụ thuộc vào tổng tiền tố và chỉ số của đoạn.
>
> Xác định chi phí nhỏ nhất để chia $j$ phần tử đầu tiên và duyệt qua vị trí cắt trước đó; khi đó công thức truy hồi có độ phức tạp $O(n^2)$. Hạng tử $k \cdot i$ tuyến tính theo số đoạn và vẫn nằm trong cùng khuôn khổ tổng tiền tố.

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
