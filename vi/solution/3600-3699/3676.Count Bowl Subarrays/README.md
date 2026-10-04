---
comments: true
difficulty: Medium
rating: 1847
source: Weekly Contest 466 Q3
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [3676. Count Bowl Subarrays](https://leetcode.com/problems/count-bowl-subarrays)

[中文文档](/solution/3600-3699/3676.Count%20Bowl%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> gồm các phần tử <strong>phân biệt</strong>.</p>

<p>Một <span data-keyword="subarray">mảng con</span> <code>nums[l...r]</code> của <code>nums</code> được gọi là một <strong>bowl</strong> nếu:</p>

<ul>
	<li>Mảng con có độ dài ít nhất là 3. Nghĩa là, <code>r - l + 1 &gt;= 3</code>.</li>
	<li><strong>Giá trị nhỏ nhất</strong> trong hai đầu mút của nó <strong>lớn hơn nghiêm ngặt</strong> <strong>giá trị lớn nhất</strong> của tất cả các phần tử ở giữa. Nghĩa là <code>min(nums[l], nums[r]) &gt; max(nums[l + 1], ..., nums[r - 1])</code>.</li>
</ul>

<p>Hãy trả về số lượng mảng con <strong>bowl</strong> trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,3,1,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con bowl là <code>[3, 1, 4]</code> và <code>[5, 3, 1, 4]</code>.</p>

<ul>
	<li><code>[3, 1, 4]</code> là một bowl vì <code>min(3, 4) = 3 &gt; max(1) = 1</code>.</li>
	<li><code>[5, 3, 1, 4]</code> là một bowl vì <code>min(5, 4) = 4 &gt; max(3, 1) = 3</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con bowl là <code>[5, 1, 2]</code>, <code>[5, 1, 2, 3]</code> và <code>[5, 1, 2, 3, 4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = </span>[1000000000,999999999,999999998]</p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có mảng con nào là bowl.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>Các phần tử trong <code>nums</code> là các phần tử phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con bowl có hai đầu đều lớn hơn nghiêm ngặt mọi giá trị ở giữa. Việc ghép từng cặp đầu mút có độ phức tạp bậc hai. Hai đầu mút phải là hai giá trị lớn nhất của đoạn và nằm ở hai phía đối diện.
>
> Với mỗi chỉ số, các phần tử lớn hơn gần nhất ở bên trái và bên phải chính là hai thành. Một stack đơn điệu có thể tính các phần tử lân cận này trong một lượt duyệt.
>
> Mỗi cặp $(\textit{L}[i],\textit{R}[i])$ có span ít nhất $3$ là một bowl. Loại bỏ việc đếm trùng bằng cách gắn mỗi bowl với quan hệ phần tử lớn hơn gần nhất, để cùng một cặp thành không bị đếm hai lần.

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
