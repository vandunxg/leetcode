---
comments: true
difficulty: Hard
rating: 2434
source: Biweekly Contest 148 Q3
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3425. Longest Special Path](https://leetcode.com/problems/longest-special-path)

[中文文档](/solution/3400-3499/3425.Longest%20Special%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây vô hướng có gốc tại node <code>0</code> với <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>, được biểu diễn bằng một mảng 2D <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, length<sub>i</sub>]</code> cho biết một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code> với độ dài <code>length<sub>i</sub></code>. Bạn cũng được cho một mảng số nguyên <code>nums</code>, trong đó <code>nums[i]</code> biểu diễn giá trị tại node <code>i</code>.</p>

<p>Một <b data-stringify-type="bold">đường đi đặc biệt</b> được định nghĩa là một đường đi <b data-stringify-type="bold">đi xuống</b> từ một node tổ tiên đến một node hậu duệ sao cho tất cả giá trị của các node trên đường đi đều <b data-stringify-type="bold">khác nhau</b>.</p>

<p><strong>Lưu ý</strong> rằng một đường đi có thể bắt đầu và kết thúc tại cùng một node.</p>

<p>Trả về một mảng <code data-stringify-type="code">result</code> có kích thước 2, trong đó <code>result[0]</code> là <b data-stringify-type="bold">độ dài</b> của đường đi đặc biệt <strong>dài nhất</strong>, và <code>result[1]</code> là số node <b data-stringify-type="bold">nhỏ nhất</b> trong tất cả các đường đi đặc biệt <strong>dài nhất</strong> <i data-stringify-type="italic">có thể có</i>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1,2],[1,2,3],[1,3,5],[1,4,4],[2,5,6]], nums = [2,1,2,1,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,2]</span></p>

<p><strong>Giải thích:</strong></p>

<h4>Trong hình dưới đây, các node được tô màu theo giá trị tương ứng trong <code>nums</code></h4>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3425.Longest%20Special%20Path/images/tree3.jpeg" style="width: 250px; height: 350px;" /></p>

<p>Các đường đi đặc biệt dài nhất là <code>2 -&gt; 5</code> và <code>0 -&gt; 1 -&gt; 4</code>, cả hai đều có độ dài bằng 6. Số node nhỏ nhất trong tất cả các đường đi đặc biệt dài nhất là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[1,0,8]], nums = [2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3425.Longest%20Special%20Path/images/tree4.jpeg" style="width: 190px; height: 75px;" /></p>

<p>Các đường đi đặc biệt dài nhất là <code>0</code> và <code>1</code>, cả hai đều có độ dài bằng 0. Số node nhỏ nhất trong tất cả các đường đi đặc biệt dài nhất là 1.</p>
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
> Đường đi đặc biệt là một đường đi trên cây có các giá trị node khác nhau (hoặc, theo đề bài, cho phép lặp nhiều nhất một loại giá trị). $n\le 5\times 10^4$ khiến việc liệt kê các đường đi là không thể.
>
> Một cửa sổ đường đi được mô tả bởi độ sâu DFS và độ sâu xuất hiện gần nhất của từng giá trị: một giá trị lặp lại buộc điểm đầu bên trái phải vượt qua lần xuất hiện trước đó.
>
> Ta duyệt từ gốc, duy trì tổng prefix của trọng số cạnh và các vị trí xuất hiện gần nhất, đồng thời dùng two-pointer để xác định điểm bắt đầu hợp lệ trên stack. Cập nhật đồng thời độ dài và số node để tìm đường đi đặc biệt dài nhất với ít node nhất.

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
