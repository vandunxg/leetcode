---
comments: true
difficulty: Hard
rating: 2451
source: Biweekly Contest 162 Q4
tags:
    - Array
    - Hash Table
    - Binary Search
    - Divide and Conquer
    - Counting
    - Prefix Sum
---

<!-- problem:start -->

# [3636. Threshold Majority Queries](https://leetcode.com/problems/threshold-majority-queries)

[中文文档](/solution/3600-3699/3636.Threshold%20Majority%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, threshold<sub>i</sub>]</code>.</p>

<p>Trả về một mảng số nguyên <code data-end="33" data-start="28">ans</code>, trong đó <code data-end="48" data-start="40">ans[i]</code> là phần tử trong mảng con <code data-end="102" data-start="89">nums[l<sub>i</sub>...r<sub>i</sub>]</code> xuất hiện <strong>ít nhất</strong> <code data-end="137" data-start="125">threshold<sub>i</sub></code> lần, chọn phần tử có tần suất <strong>cao nhất</strong> (nếu hòa thì chọn phần tử <strong>nhỏ nhất</strong>), hoặc -1 nếu <em>không tồn tại</em> phần tử nào thỏa mãn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,2,1,1], queries = [[0,5,4],[0,3,3],[2,3,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,-1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th align="left" style="border: 1px solid black;">Truy vấn</th>
            <th align="left" style="border: 1px solid black;">Mảng con</th>
            <th align="left" style="border: 1px solid black;">Ngưỡng</th>
            <th align="left" style="border: 1px solid black;">Bảng tần suất</th>
            <th align="left" style="border: 1px solid black;">Đáp án</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td align="left" style="border: 1px solid black;">[0, 5, 4]</td>
            <td align="left" style="border: 1px solid black;">[1, 1, 2, 2, 1, 1]</td>
            <td align="left" style="border: 1px solid black;">4</td>
            <td align="left" style="border: 1px solid black;">1 &rarr; 4, 2 &rarr; 2</td>
            <td align="left" style="border: 1px solid black;">1</td>
        </tr>
        <tr>
            <td align="left" style="border: 1px solid black;">[0, 3, 3]</td>
            <td align="left" style="border: 1px solid black;">[1, 1, 2, 2]</td>
            <td align="left" style="border: 1px solid black;">3</td>
            <td align="left" style="border: 1px solid black;">1 &rarr; 2, 2 &rarr; 2</td>
            <td align="left" style="border: 1px solid black;">-1</td>
        </tr>
        <tr>
            <td align="left" style="border: 1px solid black;">[2, 3, 2]</td>
            <td align="left" style="border: 1px solid black;">[2, 2]</td>
            <td align="left" style="border: 1px solid black;">2</td>
            <td align="left" style="border: 1px solid black;">2 &rarr; 2</td>
            <td align="left" style="border: 1px solid black;">2</td>
        </tr>
    </tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,3,2,3,2,3], queries = [[0,6,4],[1,5,2],[2,4,1],[3,3,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,2,3,2]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th align="left" style="border: 1px solid black;">Truy vấn</th>
            <th align="left" style="border: 1px solid black;">Mảng con</th>
            <th align="left" style="border: 1px solid black;">Ngưỡng</th>
            <th align="left" style="border: 1px solid black;">Bảng tần suất</th>
            <th align="left" style="border: 1px solid black;">Đáp án</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td align="left" style="border: 1px solid black;">[0, 6, 4]</td>
            <td align="left" style="border: 1px solid black;">[3, 2, 3, 2, 3, 2, 3]</td>
            <td align="left" style="border: 1px solid black;">4</td>
            <td align="left" style="border: 1px solid black;">3 &rarr; 4, 2 &rarr; 3</td>
            <td align="left" style="border: 1px solid black;">3</td>
        </tr>
        <tr>
            <td align="left" style="border: 1px solid black;">[1, 5, 2]</td>
            <td align="left" style="border: 1px solid black;">[2, 3, 2, 3, 2]</td>
            <td align="left" style="border: 1px solid black;">2</td>
            <td align="left" style="border: 1px solid black;">2 &rarr; 3, 3 &rarr; 2</td>
            <td align="left" style="border: 1px solid black;">2</td>
        </tr>
        <tr>
            <td align="left" style="border: 1px solid black;">[2, 4, 1]</td>
            <td align="left" style="border: 1px solid black;">[3, 2, 3]</td>
            <td align="left" style="border: 1px solid black;">1</td>
            <td align="left" style="border: 1px solid black;">3 &rarr; 2, 2 &rarr; 1</td>
            <td align="left" style="border: 1px solid black;">3</td>
        </tr>
        <tr>
            <td align="left" style="border: 1px solid black;">[3, 3, 1]</td>
            <td align="left" style="border: 1px solid black;">[2]</td>
            <td align="left" style="border: 1px solid black;">1</td>
            <td align="left" style="border: 1px solid black;">2 &rarr; 1</td>
            <td align="left" style="border: 1px solid black;">2</td>
        </tr>
    </tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="51" data-start="19"><code data-end="49" data-start="19">1 &lt;= nums.length == n &lt;= 10<sup>4</sup></code></li>
    <li data-end="82" data-start="54"><code data-end="80" data-start="54">1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li data-end="120" data-start="85"><code data-end="118" data-start="85">1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
    <li data-end="195" data-start="123"><code data-end="193" data-is-only-node="" data-start="155">queries[i] = [l<sub>i</sub>, r<sub>i</sub>, threshold<sub>i</sub>]</code></li>
    <li data-end="221" data-start="198"><code data-end="219" data-start="198">0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; n</code></li>
    <li data-end="259" data-is-last-node="" data-start="224"><code data-end="259" data-is-last-node="" data-start="224">1 &lt;= threshold<sub>i</sub> &lt;= r<sub>i</sub> - l<sub>i</sub> + 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu tìm phần tử xuất hiện nhiều nhất trong một đoạn, chỉ xét các giá trị xuất hiện ít nhất $\textit{threshold}$ lần và nếu hòa thì ưu tiên giá trị lớn hơn. Với $n\le 10^4$ và $q\le 5\times 10^4$, việc đếm lại một cách ngây thơ là khá tốn kém.
>
> Sau khi nén các giá trị, thuật toán Mo di chuyển một cửa sổ đồng thời duy trì tần suất và các ứng viên đạt ngưỡng. Vì ngưỡng khác nhau giữa các truy vấn, ta có thể gom nhóm theo ngưỡng hoặc rollback bên trong một block.
>
> Một lựa chọn khác là Fenwick/chairman-tree để tìm kiếm nhị phân ngưỡng tần suất. Dù theo cách nào, đáp án cũng được tạo ra bằng cách tăng và giảm tần suất khi các con trỏ di chuyển.

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
