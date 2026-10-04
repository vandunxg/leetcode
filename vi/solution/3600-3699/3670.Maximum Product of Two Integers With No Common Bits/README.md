---
comments: true
difficulty: Medium
rating: 2233
source: Weekly Contest 465 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3670. Maximum Product of Two Integers With No Common Bits](https://leetcode.com/problems/maximum-product-of-two-integers-with-no-common-bits)

[中文文档](/solution/3600-3699/3670.Maximum%20Product%20of%20Two%20Integers%20With%20No%20Common%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Nhiệm vụ của bạn là tìm hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code> sao cho tích <code>nums[i] * nums[j]</code> là <strong>lớn nhất</strong>, đồng thời biểu diễn nhị phân của <code>nums[i]</code> và <code>nums[j]</code> không có bit 1 nào được bật chung.</p>

<p>Trả về tích <strong>lớn nhất</strong> có thể đạt được của một cặp như vậy. Nếu không tồn tại cặp nào, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,6,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cặp tốt nhất là 3 (011) và 4 (100). Chúng không có bit 1 nào được bật chung và <code>3 * 4 = 12</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,6,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi cặp số đều có ít nhất một bit 1 được bật chung. Do đó, đáp án là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [64,8,32]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2048</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cặp số nào có bit chung, nên đáp án là tích của hai phần tử lớn nhất, 64 và 32 (<code>64 * 32 = 2048</code>).</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn hai số có AND bằng 0 và tối đa hóa tích của chúng. Việc liệt kê các cặp có độ phức tạp bậc hai; số lượng bit đủ nhỏ để áp dụng subset DP.
>
> Gọi $f[s]$ là giá trị lớn nhất trong input và là một submask của $s$. Với mỗi $x$, truy vấn mask bù của nó trong $f$ rồi cập nhật tích.
>
> Khởi tạo $f[x]$ từ input, sau đó thực hiện SOS-max trên các bit để $f[s]$ bao hàm mọi submask. Truy vấn mask bù đảm bảo các bit 1 không giao nhau.

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
