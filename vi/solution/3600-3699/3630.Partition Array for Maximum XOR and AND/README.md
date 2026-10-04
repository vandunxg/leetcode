---
comments: true
difficulty: Hard
rating: 2743
source: Weekly Contest 460 Q4
tags:
    - Greedy
    - Bit Manipulation
    - Array
    - Math
    - Enumeration
---

<!-- problem:start -->

# [3630. Partition Array for Maximum XOR and AND](https://leetcode.com/problems/partition-array-for-maximum-xor-and-and)

[中文文档](/solution/3600-3699/3630.Partition%20Array%20for%20Maximum%20XOR%20and%20AND/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Chia mảng thành <strong>ba</strong> <span data-keyword="subsequence-array">dãy con</span> <code>A</code>, <code>B</code> và <code>C</code> (có thể rỗng) sao cho mọi phần tử của <code>nums</code> thuộc <strong>chính xác</strong> một dãy con.</p>

<p>Mục tiêu của bạn là <strong>tối đa hóa</strong> giá trị: <code>XOR(A) + AND(B) + XOR(C)</code></p>

<p>Trong đó:</p>

<ul>
    <li><code>XOR(arr)</code> biểu diễn phép XOR bitwise của tất cả các phần tử trong <code>arr</code>. Nếu <code>arr</code> rỗng, giá trị được định nghĩa là 0.</li>
    <li><code>AND(arr)</code> biểu diễn phép AND bitwise của tất cả các phần tử trong <code>arr</code>. Nếu <code>arr</code> rỗng, giá trị được định nghĩa là 0.</li>
</ul>

<p>Trả về <strong>giá trị lớn nhất</strong> có thể đạt được.</p>

<p><strong>Lưu ý:</strong> Nếu nhiều cách chia cho cùng một tổng <strong>lớn nhất</strong>, bạn có thể chọn bất kỳ cách chia nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chia tối ưu là:</p>

<ul>
    <li><code>A = [3], XOR(A) = 3</code></li>
    <li><code>B = [2], AND(B) = 2</code></li>
    <li><code>C = [], XOR(C) = 0</code></li>
</ul>

<p>Giá trị lớn nhất của: <code>XOR(A) + AND(B) + XOR(C) = 3 + 2 + 0 = 5</code>. Do đó, đáp án là 5.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chia tối ưu là:</p>

<ul>
    <li><code>A = [1], XOR(A) = 1</code></li>
    <li><code>B = [2], AND(B) = 2</code></li>
    <li><code>C = [3], XOR(C) = 3</code></li>
</ul>

<p>Giá trị lớn nhất của: <code>XOR(A) + AND(B) + XOR(C) = 1 + 2 + 3 = 6</code>. Do đó, đáp án là 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,6,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chia tối ưu là:</p>

<ul>
    <li><code>A = [7], XOR(A) = 7</code></li>
    <li><code>B = [2,3], AND(B) = 2</code></li>
    <li><code>C = [6], XOR(C) = 6</code></li>
</ul>

<p>Giá trị lớn nhất của: <code>XOR(A) + AND(B) + XOR(C) = 7 + 2 + 6 = 15</code>. Do đó, đáp án là 15.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 19</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chia thành $A,B,C$ và tối đa hóa $(\mathrm{XOR}\,A)+(\mathrm{AND}\,B)+(\mathrm{XOR}\,C)$. Một cơ sở tuyến tính xử lý việc những giá trị XOR nào có thể tạo ra.
>
> XOR của toàn bộ mảng là cố định. $\mathrm{AND}\,B$ là phép AND bitwise của một tập con, nên ta liệt kê các mặt nạ AND ứng viên.
>
> Với một đóng góp AND cố định, ta đưa các giá trị còn lại vào một cơ sở tuyến tính. XOR lớn nhất mà cơ sở này có thể tạo ra, kết hợp với XOR của các phần tử còn lại, sẽ cho ra cặp $A,C$. Kết hợp việc liệt kê với cơ sở tuyến tính sẽ cho nghiệm tối ưu.

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
