---
comments: true
difficulty: Hard
rating: 2257
source: Weekly Contest 447 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [3533. Concatenated Divisibility](https://leetcode.com/problems/concatenated-divisibility)

[中文文档](/solution/3500-3599/3533.Concatenated%20Divisibility/README.md)

## Mô tả

<!-- description:start -->

<p data-end="378" data-start="31">Bạn được cho một mảng các số nguyên dương <code data-end="85" data-start="79">nums</code> và một số nguyên dương <code data-end="112" data-start="109">k</code>.</p>

<p data-end="378" data-start="31">Một <span data-keyword="permutation-array">hoán vị</span> của <code data-end="137" data-start="131">nums</code> được gọi là tạo thành một <strong data-end="183" data-start="156">phép nối chia hết</strong> nếu khi <em>nối</em> <em>các biểu diễn thập phân</em> của các số theo thứ tự được chỉ định bởi hoán vị, số nhận được <strong>chia hết cho</strong> <code data-end="359" data-start="356">k</code>.</p>

<p data-end="561" data-start="380">Trả về hoán vị <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> (khi được xem như một danh sách các số nguyên) tạo thành một <strong>phép nối chia hết</strong>. Nếu không tồn tại hoán vị như vậy, trả về một danh sách rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,12,45], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,12,45]</span></p>

<p><strong>Giải thích:</strong></p>

<table data-end="896" data-start="441" node="[object Object]" style="border: 1px solid black;">
    <thead data-end="497" data-start="441">
        <tr data-end="497" data-start="441">
            <th data-end="458" data-start="441" style="border: 1px solid black;">Hoán vị</th>
            <th data-end="479" data-start="458" style="border: 1px solid black;">Giá trị nối</th>
            <th data-end="497" data-start="479" style="border: 1px solid black;">Chia hết cho 5</th>
        </tr>
    </thead>
    <tbody data-end="896" data-start="555">
        <tr data-end="611" data-start="555">
            <td style="border: 1px solid black;">[3, 12, 45]</td>
            <td style="border: 1px solid black;">31245</td>
            <td style="border: 1px solid black;">Có</td>
        </tr>
        <tr data-end="668" data-start="612">
            <td style="border: 1px solid black;">[3, 45, 12]</td>
            <td style="border: 1px solid black;">34512</td>
            <td style="border: 1px solid black;">Không</td>
        </tr>
        <tr data-end="725" data-start="669">
            <td style="border: 1px solid black;">[12, 3, 45]</td>
            <td style="border: 1px solid black;">12345</td>
            <td style="border: 1px solid black;">Có</td>
        </tr>
        <tr data-end="782" data-start="726">
            <td style="border: 1px solid black;">[12, 45, 3]</td>
            <td style="border: 1px solid black;">12453</td>
            <td style="border: 1px solid black;">Không</td>
        </tr>
        <tr data-end="839" data-start="783">
            <td style="border: 1px solid black;">[45, 3, 12]</td>
            <td style="border: 1px solid black;">45312</td>
            <td style="border: 1px solid black;">Không</td>
        </tr>
        <tr data-end="896" data-start="840">
            <td style="border: 1px solid black;">[45, 12, 3]</td>
            <td style="border: 1px solid black;">45123</td>
            <td style="border: 1px solid black;">Không</td>
        </tr>
    </tbody>
</table>

<p data-end="1618" data-start="1525">Hoán vị nhỏ nhất theo thứ tự từ điển tạo thành một phép nối chia hết là <code>[3,12,45]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,5], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,10]</span></p>

<p><strong>Giải thích:</strong></p>

<table data-end="1421" data-start="1200" node="[object Object]" style="border: 1px solid black;">
    <thead data-end="1255" data-start="1200">
        <tr data-end="1255" data-start="1200">
            <th data-end="1216" data-start="1200" style="border: 1px solid black;">Hoán vị</th>
            <th data-end="1237" data-start="1216" style="border: 1px solid black;">Giá trị nối</th>
            <th data-end="1255" data-start="1237" style="border: 1px solid black;">Chia hết cho 10</th>
        </tr>
    </thead>
    <tbody data-end="1421" data-start="1312">
        <tr data-end="1366" data-start="1312">
            <td style="border: 1px solid black;">[5, 10]</td>
            <td style="border: 1px solid black;">510</td>
            <td style="border: 1px solid black;">Có</td>
        </tr>
        <tr data-end="1421" data-start="1367">
            <td style="border: 1px solid black;">[10, 5]</td>
            <td style="border: 1px solid black;">105</td>
            <td style="border: 1px solid black;">Không</td>
        </tr>
    </tbody>
</table>

<p data-end="2011" data-start="1921">Hoán vị nhỏ nhất theo thứ tự từ điển tạo thành một phép nối chia hết là <code>[5,10]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì không có hoán vị nào của <code data-end="177" data-start="171">nums</code> tạo thành một phép nối chia hết hợp lệ, trả về một danh sách rỗng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 13</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 13$ cho phép duyệt qua các hoán vị, nhưng việc xây dựng lại số nối từ đầu sẽ lặp lại nhiều công việc. Đồng thời, ta cần hoán vị nhỏ nhất theo thứ tự từ điển.
>
> Tính trước lũy thừa của 10 tương ứng với mỗi giá trị. DP trên tập con lưu tập các phần tử đã dùng và phần dư hiện tại modulo $k$, đồng thời khôi phục đáp án theo nhánh nhỏ hơn theo thứ tự từ điển. Nếu không có trạng thái nào khả thi, trả về một danh sách rỗng.

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
