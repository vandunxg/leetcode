---
comments: true
difficulty: Hard
rating: 1984
source: Biweekly Contest 166 Q4
tags:
    - Greedy
    - Union Find
    - Array
    - Sorting
---

<!-- problem:start -->

# [3695. Maximize Alternating Sum Using Swaps](https://leetcode.com/problems/maximize-alternating-sum-using-swaps)

[中文文档](/solution/3600-3699/3695.Maximize%20Alternating%20Sum%20Using%20Swaps/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn muốn tối đa hóa <strong>tổng xen kẽ</strong> của <code>nums</code>, được định nghĩa là giá trị thu được bằng cách <strong>cộng</strong> các phần tử ở chỉ số chẵn và <strong>trừ</strong> các phần tử ở chỉ số lẻ. Cụ thể, <code>nums[0] - nums[1] + nums[2] - nums[3]...</code></p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>swaps</code>, trong đó <code>swaps[i] = [p<sub>i</sub>, q<sub>i</sub>]</code>. Với mỗi cặp <code>[p<sub>i</sub>, q<sub>i</sub>]</code> trong <code>swaps</code>, bạn được phép hoán đổi các phần tử ở chỉ số <code>p<sub>i</sub></code> và <code>q<sub>i</sub></code>. Có thể thực hiện các phép hoán đổi này bao nhiêu lần tùy ý và theo bất kỳ thứ tự nào.</p>

<p>Trả về <strong>tổng xen kẽ</strong> lớn nhất có thể của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], swaps = [[0,2],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng xen kẽ lớn nhất đạt được khi <code>nums</code> là <code>[2, 1, 3]</code> hoặc <code>[3, 1, 2]</code>. Chẳng hạn, có thể thu được <code>nums = [2, 1, 3]</code> như sau.</p>

<ul>
	<li>Hoán đổi <code>nums[0]</code> và <code>nums[2]</code>. Khi đó <code>nums</code> là <code>[3, 2, 1]</code>.</li>
	<li>Hoán đổi <code>nums[1]</code> và <code>nums[2]</code>. Khi đó <code>nums</code> là <code>[3, 1, 2]</code>.</li>
	<li>Hoán đổi <code>nums[0]</code> và <code>nums[2]</code>. Khi đó <code>nums</code> là <code>[2, 1, 3]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], swaps = [[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng xen kẽ lớn nhất đạt được khi không thực hiện phép hoán đổi nào.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1000000000,1,1000000000,1,1000000000], swaps = []</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-2999999997</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì không thể thực hiện phép hoán đổi nào, tổng xen kẽ lớn nhất đạt được khi không thực hiện phép hoán đổi nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= swaps.length &lt;= 10<sup>5</sup></code></li>
	<li><code>swaps[i] = [p<sub>i</sub>, q<sub>i</sub>]</code></li>
	<li><code>0 &lt;= p<sub>i</sub> &lt; q<sub>i</sub> &lt;= nums.length - 1</code></li>
	<li><code>[p<sub>i</sub>, q<sub>i</sub>] != [p<sub>j</sub>, q<sub>j</sub>]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tổng xen kẽ là tổng các phần tử ở chỉ số chẵn trừ đi tổng các phần tử ở chỉ số lẻ. Các phép hoán đổi cho phép ta tráo đổi các giá trị trong cùng một component liên thông của các chỉ số.
>
> Union-find xây dựng các component đó. Với một component có $e$ chỉ số chẵn, ta gán $e$ giá trị lớn nhất cho các vị trí chẵn và các giá trị còn lại cho vị trí lẻ.
>
> Các component độc lập với nhau, nên tổng của chúng chính là tổng xen kẽ lớn nhất. Có thể thực hiện việc gán này bằng cách sort (hoặc chọn phần tử lớn thứ $e$).

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
