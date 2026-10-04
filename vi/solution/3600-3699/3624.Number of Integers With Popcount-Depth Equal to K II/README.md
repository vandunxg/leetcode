---
comments: true
difficulty: Hard
rating: 2085
source: Weekly Contest 459 Q3
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Divide and Conquer
---

<!-- problem:start -->

# [3624. Number of Integers With Popcount-Depth Equal to K II](https://leetcode.com/problems/number-of-integers-with-popcount-depth-equal-to-k-ii)

[中文文档](/solution/3600-3699/3624.Number%20of%20Integers%20With%20Popcount-Depth%20Equal%20to%20K%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Với mọi số nguyên dương <code>x</code>, định nghĩa dãy sau:</p>

<ul>
    <li><code>p<sub>0</sub> = x</code></li>
    <li><code>p<sub>i+1</sub> = popcount(p<sub>i</sub>)</code> với mọi <code>i &gt;= 0</code>, trong đó <code>popcount(y)</code> là số lượng bit 1 trong biểu diễn nhị phân của <code>y</code>.</li>
</ul>

<p>Dãy này cuối cùng sẽ đạt đến giá trị 1.</p>

<p><strong>Độ sâu popcount</strong> của <code>x</code> được định nghĩa là số nguyên <strong>nhỏ nhất</strong> <code>d &gt;= 0</code> sao cho <code>p<sub>d</sub> = 1</code>.</p>

<p>Ví dụ, nếu <code>x = 7</code> (biểu diễn nhị phân <code>&quot;111&quot;</code>). Khi đó, dãy là: <code>7 &rarr; 3 &rarr; 2 &rarr; 1</code>, nên độ sâu popcount của 7 là 3.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code>, trong đó mỗi <code>queries[i]</code> là một trong các loại sau:</p>

<ul>
    <li><code>[1, l, r, k]</code> - <strong>Xác định</strong> số lượng chỉ số <code>j</code> sao cho <code>l &lt;= j &lt;= r</code> và <strong>độ sâu popcount</strong> của <code>nums[j]</code> bằng <code>k</code>.</li>
    <li><code>[2, idx, val]</code> - <strong>Cập nhật</strong> <code>nums[idx]</code> thành <code>val</code>.</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là số lượng chỉ số của truy vấn thứ <code>i<sup>th</sup></code> có dạng <code>[1, l, r, k]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4], queries = [[1,0,1,1],[2,1,1],[1,0,1,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>queries[i]</code></th>
            <th style="border: 1px solid black;"><code>nums</code></th>
            <th style="border: 1px solid black;">nhị phân(<code>nums</code>)</th>
            <th style="border: 1px solid black;">độ sâu popcount-<br />
            độ sâu</th>
            <th style="border: 1px solid black;"><code>[l, r]</code></th>
            <th style="border: 1px solid black;"><code>k</code></th>
            <th style="border: 1px solid black;">Hợp lệ<br />
            <code>nums[j]</code></th>
            <th style="border: 1px solid black;">sau cập nhật<br />
            <code>nums</code></th>
            <th style="border: 1px solid black;">Đáp án</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">[1,0,1,1]</td>
            <td style="border: 1px solid black;">[2,4]</td>
            <td style="border: 1px solid black;">[10, 100]</td>
            <td style="border: 1px solid black;">[1, 1]</td>
            <td style="border: 1px solid black;">[0, 1]</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">[0, 1]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">[2,1,1]</td>
            <td style="border: 1px solid black;">[2,4]</td>
            <td style="border: 1px solid black;">[10, 100]</td>
            <td style="border: 1px solid black;">[1, 1]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">[2,1]</td>
            <td style="border: 1px solid black;">&mdash;</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">[1,0,1,0]</td>
            <td style="border: 1px solid black;">[2,1]</td>
            <td style="border: 1px solid black;">[10, 1]</td>
            <td style="border: 1px solid black;">[1, 0]</td>
            <td style="border: 1px solid black;">[0, 1]</td>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">[1]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">1</td>
        </tr>
    </tbody>
</table>

<p>Vì vậy, <code>answer</code> cuối cùng là <code>[2, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,6], queries = [[1,0,2,2],[2,1,4],[1,1,2,1],[1,0,1,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>queries[i]</code></th>
            <th style="border: 1px solid black;"><code>nums</code></th>
            <th style="border: 1px solid black;">nhị phân(<code>nums</code>)</th>
            <th style="border: 1px solid black;">độ sâu popcount-<br />
            độ sâu</th>
            <th style="border: 1px solid black;"><code>[l, r]</code></th>
            <th style="border: 1px solid black;"><code>k</code></th>
            <th style="border: 1px solid black;">Hợp lệ<br />
            <code>nums[j]</code></th>
            <th style="border: 1px solid black;">sau cập nhật<br />
            <code>nums</code></th>
            <th style="border: 1px solid black;">Đáp án</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">[1,0,2,2]</td>
            <td style="border: 1px solid black;">[3, 5, 6]</td>
            <td style="border: 1px solid black;">[11, 101, 110]</td>
            <td style="border: 1px solid black;">[2, 2, 2]</td>
            <td style="border: 1px solid black;">[0, 2]</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">[0, 1, 2]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">3</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">[2,1,4]</td>
            <td style="border: 1px solid black;">[3, 5, 6]</td>
            <td style="border: 1px solid black;">[11, 101, 110]</td>
            <td style="border: 1px solid black;">[2, 2, 2]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">[3, 4, 6]</td>
            <td style="border: 1px solid black;">&mdash;</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">[1,1,2,1]</td>
            <td style="border: 1px solid black;">[3, 4, 6]</td>
            <td style="border: 1px solid black;">[11, 100, 110]</td>
            <td style="border: 1px solid black;">[2, 1, 2]</td>
            <td style="border: 1px solid black;">[1, 2]</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">[1]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">1</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">[1,0,1,0]</td>
            <td style="border: 1px solid black;">[3, 4, 6]</td>
            <td style="border: 1px solid black;">[11, 100, 110]</td>
            <td style="border: 1px solid black;">[2, 1, 2]</td>
            <td style="border: 1px solid black;">[0, 1]</td>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">[]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">0</td>
        </tr>
    </tbody>
</table>

<p>Vì vậy, <code>answer</code> cuối cùng là <code>[3, 1, 0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2], queries = [[1,0,1,1],[2,0,3],[1,0,0,1],[1,0,0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>queries[i]</code></th>
            <th style="border: 1px solid black;"><code>nums</code></th>
            <th style="border: 1px solid black;">nhị phân(<code>nums</code>)</th>
            <th style="border: 1px solid black;">độ sâu popcount-<br />
            độ sâu</th>
            <th style="border: 1px solid black;"><code>[l, r]</code></th>
            <th style="border: 1px solid black;"><code>k</code></th>
            <th style="border: 1px solid black;">Hợp lệ<br />
            <code>nums[j]</code></th>
            <th style="border: 1px solid black;">sau cập nhật<br />
            <code>nums</code></th>
            <th style="border: 1px solid black;">Đáp án</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">[1,0,1,1]</td>
            <td style="border: 1px solid black;">[1, 2]</td>
            <td style="border: 1px solid black;">[1, 10]</td>
            <td style="border: 1px solid black;">[0, 1]</td>
            <td style="border: 1px solid black;">[0, 1]</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">[1]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">1</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">[2,0,3]</td>
            <td style="border: 1px solid black;">[1, 2]</td>
            <td style="border: 1px solid black;">[1, 10]</td>
            <td style="border: 1px solid black;">[0, 1]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">[3, 2]</td>
            <td style="border: 1px solid black;">&nbsp;</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">[1,0,0,1]</td>
            <td style="border: 1px solid black;">[3, 2]</td>
            <td style="border: 1px solid black;">[11, 10]</td>
            <td style="border: 1px solid black;">[2, 1]</td>
            <td style="border: 1px solid black;">[0, 0]</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">[]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">0</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">[1,0,0,2]</td>
            <td style="border: 1px solid black;">[3, 2]</td>
            <td style="border: 1px solid black;">[11, 10]</td>
            <td style="border: 1px solid black;">[2, 1]</td>
            <td style="border: 1px solid black;">[0, 0]</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">[0]</td>
            <td style="border: 1px solid black;">&mdash;</td>
            <td style="border: 1px solid black;">1</td>
        </tr>
    </tbody>
</table>

<p>Vì vậy, <code>answer</code> cuối cùng là <code>[1, 0, 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>15</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i].length == 3</code> hoặc <code>4</code>
    <ul>
        <li><code>queries[i] == [1, l, r, k]</code> hoặc,</li>
        <li><code>queries[i] == [2, idx, val]</code></li>
        <li><code>0 &lt;= l &lt;= r &lt;= n - 1</code></li>
        <li><code>0 &lt;= k &lt;= 5</code></li>
        <li><code>0 &lt;= idx &lt;= n - 1</code></li>
        <li><code>1 &lt;= val &lt;= 10<sup>15</sup></code></li>
    </ul>
    </li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với các phép cập nhật từng phần tử và đếm trong đoạn theo một độ sâu popcount cho trước, cùng $n,q\le 10^5$ và $k\le 5$, ta không thể quét lại đoạn.
>
> Khoảng độ sâu rất nhỏ, nên dùng một Fenwick tree hoặc segment tree cho mỗi $k$ để lưu các vị trí xuất hiện của độ sâu đó. Truy vấn $[l,r]$ là hiệu của hai tổng tiền tố trên cây $k$.
>
> Các giá trị có thể đạt tới $10^{15}$, vì vậy độ sâu được tính bằng một vòng lặp popcount ngắn.

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
