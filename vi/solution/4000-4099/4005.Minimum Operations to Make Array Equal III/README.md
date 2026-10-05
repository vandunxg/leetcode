---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Math
    - Counting
    - Number Theory
---

<!-- problem:start -->

# [4005. Minimum Operations to Make Array Equal III 🔒](https://leetcode.com/problems/minimum-operations-to-make-array-equal-iii)

[中文文档](/solution/4000-4099/4005.Minimum%20Operations%20to%20Make%20Array%20Equal%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Trong một thao tác, bạn có thể chọn <strong>bất kỳ</strong> phần tử <code>nums[i]</code> nào và thực hiện một trong các thao tác sau:</p>

<ul>
\t<li><strong>Nhân</strong> <code>nums[i]</code> với một số nguyên <code>k</code>, trong đó <code>k &gt;= 2</code>.</li>
\t<li><strong>Chia</strong> <code>nums[i]</code> cho một số nguyên <code>k</code>, trong đó <code>2 &lt;= k &lt; nums[i]</code>, với điều kiện <code>nums[i]</code> chia hết cho <code>k</code>.</li>
</ul>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thiết để mọi phần tử của <code>nums</code> <strong>bằng nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,12,8]</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thực hiện các thao tác sau để đưa mọi số về 6:</p>

<ul>
\t<li>Chia <code>nums[1] = 12</code> cho 2 để được 6.</li>
\t<li>Chia <code>nums[2] = 8</code> cho 4 để được 2.</li>
\t<li>Nhân <code>nums[2] = 2</code> với 3 để được 6.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,15,20]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thực hiện các thao tác sau để đưa mọi số về 5:</p>

<ul>
\t<li>Chia <code>nums[1] = 15</code> cho 3 để được 5.</li>
\t<li>Chia <code>nums[2] = 20</code> cho 4 để được 5.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,7,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi phần tử đã bằng nhau nên không cần thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
\t<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
\t<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với $n$ có thể lên đến $10^5$ và các giá trị lên đến $10^9$, ta không thể mô phỏng phép nhân và phép chia trên từng cặp, cũng không thể thử mọi số nguyên có thể dùng làm đích chung.
>
> Một phép nhân hoặc phép chia chính xác có thể đưa một số đến bất kỳ bội số hoặc ước thực sự nào, nên chi phí để gặp nhau tại một đích phụ thuộc vào các thừa số chung và những thừa số mà mỗi giá trị cần thêm hoặc loại bỏ, chứ không phụ thuộc vào số lượng số nguyên trung gian.
>
> Vì vậy, ta nén mỗi số bằng cách phân tích thừa số của nó và cộng dồn số thao tác nhỏ nhất trên một tập ứng viên nhỏ hơn rất nhiều so với $10^9$.

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
