---
comments: true
difficulty: Hard
rating: 2343
source: Weekly Contest 449 Q3
tags:
    - Greedy
    - Graph
    - Math
---

<!-- problem:start -->

# [3547. Maximum Sum of Edge Values in a Graph](https://leetcode.com/problems/maximum-sum-of-edge-values-in-a-graph)

[中文文档](/solution/3500-3599/3547.Maximum%20Sum%20of%20Edge%20Values%20in%20a%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một đồ thị <strong>vô hướng liên thông</strong> gồm <code>n</code> đỉnh, được đánh số từ <code>0</code> đến <code>n - 1</code>. Mỗi đỉnh nối với <strong>nhiều nhất</strong> 2 đỉnh khác.</p>

<p>Đồ thị gồm <code>m</code> cạnh, được biểu diễn bằng một mảng 2 chiều <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối đỉnh <code>a<sub>i</sub></code> với đỉnh <code>b<sub>i</sub></code>.</p>

<p data-end="502" data-start="345">Bạn phải gán cho mỗi đỉnh một giá trị <strong>khác nhau</strong> từ <code data-end="391" data-start="388">1</code> đến <code data-end="398" data-start="395">n</code>. Giá trị của một cạnh là <strong>tích</strong> của các giá trị được gán cho hai đỉnh mà cạnh đó nối.</p>

<p data-end="502" data-start="345">Điểm số là tổng giá trị của tất cả các cạnh trong đồ thị.</p>

<p>Hãy trả về điểm số <strong>lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3547.Maximum%20Sum%20of%20Edge%20Values%20in%20a%20Graph/images/screenshot-from-2025-05-13-01-27-52.png" style="width: 411px; height: 123px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges =&nbsp;</span>[[0,1],[1,2],[2,3]]</p>

<p><strong>Đầu ra:</strong> 23</p>

<p><strong>Giải thích:</strong></p>

<p>Hình minh họa phía trên thể hiện một cách gán giá trị tối ưu cho các đỉnh. Tổng giá trị của các cạnh là: <code>(1 * 3) + (3 * 4) + (4 * 2) = 23</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3547.Maximum%20Sum%20of%20Edge%20Values%20in%20a%20Graph/images/graphproblemex2drawio.png" style="width: 220px; height: 255px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, edges = [[0,3],[4,5],[2,0],[1,3],[2,4],[1,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">82</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hình minh họa phía trên thể hiện một cách gán giá trị tối ưu cho các đỉnh. Tổng giá trị của các cạnh là: <code>(1 * 2) + (2 * 4) + (4 * 6) + (6 * 5) + (5 * 3) + (3 * 1) = 82</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>m == edges.length</code></li>
    <li><code>1 &lt;= m &lt;= n</code></li>
    <li><code>edges[i].length == 2</code></li>
    <li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
    <li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
    <li>Không có cạnh trùng lặp.</li>
    <li>Đồ thị liên thông.</li>
    <li>Mỗi đỉnh nối với nhiều nhất 2 đỉnh khác.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đỉnh có bậc không quá $2$, vì vậy đồ thị là hợp rời nhau của các đường đi và chu trình. Giá trị của một cạnh là tích của hai đầu mút; các số nguyên lớn hơn nên được đặt ở những đỉnh nối với nhiều cạnh hơn.
>
> Hãy phân loại từng thành phần thành đường đi hoặc chu trình, rồi đặt $n,\ldots,1$ sao cho các giá trị lớn nằm cạnh nhau. Các thành phần khác nhau không có cạnh chung, vì vậy việc gán có thể thực hiện cục bộ trên từng khối.

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
