---
comments: true
difficulty: Hard
rating: 2336
source: Weekly Contest 435 Q3
tags:
    - Bit Manipulation
    - Array
    - Math
    - Dynamic Programming
    - Bitmask
    - Number Theory
---

<!-- problem:start -->

# [3444. Minimum Increments for Target Multiples in an Array](https://leetcode.com/problems/minimum-increments-for-target-multiples-in-an-array)

[中文文档](/solution/3400-3499/3444.Minimum%20Increments%20for%20Target%20Multiples%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng <code>nums</code> và <code>target</code>.</p>

<p>Trong một thao tác, bạn có thể tăng bất kỳ phần tử nào của <code>nums</code> thêm 1.</p>

<p>Hãy trả về <strong>số thao tác nhỏ nhất</strong> cần thực hiện để mỗi phần tử trong <code>target</code> có <strong>ít nhất</strong> một bội số trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], target = [4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số thao tác nhỏ nhất cần thực hiện để thỏa mãn điều kiện là 1.</p>

<ul>
	<li>Tăng 3 thành 4 chỉ với một thao tác, khi đó 4 là bội số của chính nó.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,4], target = [10,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số thao tác nhỏ nhất cần thực hiện để thỏa mãn điều kiện là 2.</p>

<ul>
	<li>Tăng 8 thành 10 với 2 thao tác, khi đó 10 là bội số của cả 5 và 10.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,9,10], target = [7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Target 7 đã có một bội số trong nums, nên không cần thực hiện thêm thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= target.length &lt;= 4</code></li>
	<li><code>target.length &lt;= nums.length</code></li>
	<li><code>1 &lt;= nums[i], target[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi target phải chia hết cho ít nhất một phần tử của mảng; ta chỉ có thể tăng các phần tử. Số lượng target nhỏ, nhưng có tới $10^5$ phần tử.
>
> Một phần tử có thể thỏa mãn nhiều target nếu tăng nó lên một bội chung nhỏ nhất của các target đó. Ta tính số lần tăng cho từng tập con, sau đó thỏa mãn các target qua các phần tử.
>
> Dùng quy hoạch động trên bitmask: $f[s]$ là số lần tăng nhỏ nhất để thỏa mãn tập $s$. Mỗi phần tử cung cấp một chi phí cho mọi tập con $t$ và cập nhật $f$. Cách này phù hợp vì $|target|\le 4$.

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
