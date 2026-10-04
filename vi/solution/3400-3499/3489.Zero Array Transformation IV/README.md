---
comments: true
difficulty: Medium
rating: 2068
source: Weekly Contest 441 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3489. Zero Array Transformation IV](https://leetcode.com/problems/zero-array-transformation-iv)

[中文文档](/solution/3400-3499/3489.Zero%20Array%20Transformation%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng 2 chiều <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, val<sub>i</sub>]</code>.</p>

<p>Mỗi <code>queries[i]</code> biểu diễn thao tác sau trên <code>nums</code>:</p>

<ul>
    <li>Chọn một <span data-keyword="subset">tập con</span> các chỉ số trong phạm vi <code>[l<sub>i</sub>, r<sub>i</sub>]</code> của <code>nums</code>.</li>
    <li>Giảm giá trị tại mỗi chỉ số được chọn đi <strong>đúng</strong> <code>val<sub>i</sub></code>.</li>
</ul>

<p><strong>Mảng toàn số 0</strong> là một mảng có tất cả phần tử bằng 0.</p>

<p>Trả về giá trị <strong>không âm</strong> <strong>nhỏ nhất</strong> của <code>k</code> sao cho sau khi thực hiện <strong>tuần tự</strong> <code>k</code> truy vấn đầu tiên, <code>nums</code> trở thành <strong>Mảng toàn số 0</strong>. Nếu không tồn tại <code>k</code> như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,0,2], queries = [[0,2,1],[0,2,1],[1,1,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Với truy vấn 0 (l = 0, r = 2, val = 1):</strong>

    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[0, 2]</code> đi 1.</li>
        <li>Mảng trở thành <code>[1, 0, 1]</code>.</li>
    </ul>
    </li>
    <li><strong>Với truy vấn 1 (l = 0, r = 2, val = 1):</strong>
    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[0, 2]</code> đi 1.</li>
        <li>Mảng trở thành <code>[0, 0, 0]</code>, đây là Mảng toàn số 0. Vì vậy, giá trị nhỏ nhất của <code>k</code> là 2.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,2,1], queries = [[1,3,2],[0,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể biến nums thành Mảng toàn số 0 ngay cả sau khi thực hiện tất cả truy vấn.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,2,1], queries = [[0,1,1],[1,2,1],[2,3,2],[3,4,1],[4,4,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Với truy vấn 0 (l = 0, r = 1, val = 1):</strong>

    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[0, 1]</code> đi <code><font face="monospace">1</font></code>.</li>
        <li>Mảng trở thành <code>[0, 1, 3, 2, 1]</code>.</li>
    </ul>
    </li>
    <li><strong>Với truy vấn 1 (l = 1, r = 2, val = 1):</strong>
    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[1, 2]</code> đi 1.</li>
        <li>Mảng trở thành <code>[0, 0, 2, 2, 1]</code>.</li>
    </ul>
    </li>
    <li><strong>Với truy vấn 2 (l = 2, r = 3, val = 2):</strong>
    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[2, 3]</code> đi 2.</li>
        <li>Mảng trở thành <code>[0, 0, 0, 0, 1]</code>.</li>
    </ul>
    </li>
    <li><strong>Với truy vấn 3 (l = 3, r = 4, val = 1):</strong>
    <ul>
        <li>Giảm giá trị tại chỉ số 4 đi 1.</li>
        <li>Mảng trở thành <code>[0, 0, 0, 0, 0]</code>. Vì vậy, giá trị nhỏ nhất của <code>k</code> là 4.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,2,6], queries = [[0,1,1],[0,2,1],[1,4,2],[4,4,4],[3,4,1],[4,4,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10</code></li>
    <li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
    <li><code>1 &lt;= queries.length &lt;= 1000</code></li>
    <li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, val<sub>i</sub>]</code></li>
    <li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; nums.length</code></li>
    <li><code>1 &lt;= val<sub>i</sub> &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn có thể trừ $val$ khỏi một tập con bất kỳ của $[l,r]$. Vì $n\le 10$ và có nhiều nhất $1000$ truy vấn, ta có thể xem mỗi chỉ số là một bài toán knapsack riêng.
>
> Với mỗi chỉ số $i$, cần tạo giá trị $\textit{nums}[i]$ từ các giá trị $val$ của những truy vấn bao phủ chỉ số đó. Các truy vấn có tính chất đóng theo tiền tố: ta cần tìm tiền tố ngắn nhất có thể đạt được cho mọi chỉ số.
>
> Với mỗi chỉ số, một mảng khả năng đạt được kiểu boolean lần lượt tiếp nhận từng $val$ của các truy vấn bao phủ nó. Tiền tố đầu tiên có thể tạo ra mọi giá trị đích là đáp án; nếu không, trả về $-1$.

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
