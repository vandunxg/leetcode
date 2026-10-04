---
comments: true
difficulty: Hard
rating: 2924
source: Biweekly Contest 152 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3486. Longest Special Path II](https://leetcode.com/problems/longest-special-path-ii)

[中文文档](/solution/3400-3499/3486.Longest%20Special%20Path%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây vô hướng có gốc tại node <code>0</code>, với <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng 2D <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, length<sub>i</sub>]</code> cho biết một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code> với độ dài <code>length<sub>i</sub></code>. Bạn cũng được cho một mảng số nguyên <code>nums</code>, trong đó <code>nums[i]</code> biểu diễn giá trị tại node <code>i</code>.</p>

<p>Một <strong>đường đi đặc biệt</strong> được định nghĩa là một đường đi <strong>đi xuống</strong> từ một node tổ tiên đến một node hậu duệ, trong đó tất cả giá trị của các node đều <strong>khác nhau</strong>, ngoại trừ <strong>nhiều nhất</strong> một giá trị có thể xuất hiện hai lần.</p>

<p>Trả về một mảng <code data-stringify-type="code">result</code> có kích thước 2, trong đó <code>result[0]</code> là <b data-stringify-type="bold">độ dài</b> của đường đi đặc biệt <strong>dài nhất</strong>, và <code>result[1]</code> là số node <b data-stringify-type="bold">nhỏ nhất</b> trong tất cả các đường đi đặc biệt <strong>dài nhất</strong> <i data-stringify-type="italic">có thể có</i>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1,1],[1,2,3],[1,3,1],[2,4,6],[4,7,2],[3,5,2],[3,6,5],[6,8,3]], nums = [1,1,0,3,1,2,1,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[9,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong hình dưới đây, các node được tô màu theo giá trị tương ứng trong <code>nums</code>.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3486.Longest%20Special%20Path%20II/images/e1.png" style="width: 190px; height: 270px;" /></p>

<p>Các đường đi đặc biệt dài nhất là <code>1 -&gt; 2 -&gt; 4</code> và <code>1 -&gt; 3 -&gt; 6 -&gt; 8</code>, cả hai đều có độ dài bằng 9. Số node nhỏ nhất trong tất cả các đường đi đặc biệt dài nhất là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[1,0,3],[0,2,4],[0,3,5]], nums = [1,1,0,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3486.Longest%20Special%20Path%20II/images/e2.png" style="width: 150px; height: 110px;" /></p>

<p>Đường đi dài nhất là <code>0 -&gt; 3</code>, gồm 2 node và có độ dài bằng 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 5 * 10<sup><span style="font-size: 10.8333px;">4</span></sup></code></li>
    <li><code>edges.length == n - 1</code></li>
    <li><code>edges[i].length == 3</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li><code>1 &lt;= length<sub>i</sub> &lt;= 10<sup>3</sup></code></li>
    <li><code>nums.length == n</code></li>
    <li><code>0 &lt;= nums[i] &lt;= 5 * 10<sup>4</sup></code></li>
    <li>Đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tương tự phần I, nhưng giờ một giá trị có thể xuất hiện hai lần còn các giá trị khác phải giữ nguyên tính duy nhất. Với $n\le 5\times 10^4$, ta vẫn cần dùng sliding window trên cây.
>
> Đầu bên trái giờ bị chi phối bởi lần lặp thứ hai: lần trùng đầu tiên được phép giữ lại, còn một lần lặp tiếp theo sẽ đẩy cửa sổ vượt qua vị trí xuất hiện trước đó.
>
> DFS duy trì các danh sách vị trí xuất hiện gần nhất và tổng prefix của trọng số cạnh. Một cờ “đã dùng giá trị trùng lặp” nhiều nhất sẽ thu hẹp đầu bên trái, đồng thời ta cập nhật đường đi dài nhất và số node ít nhất của nó.

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
