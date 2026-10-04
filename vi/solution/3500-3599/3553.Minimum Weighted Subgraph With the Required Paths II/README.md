---
comments: true
difficulty: Hard
rating: 2410
source: Weekly Contest 450 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3553. Minimum Weighted Subgraph With the Required Paths II](https://leetcode.com/problems/minimum-weighted-subgraph-with-the-required-paths-ii)

[中文文档](/solution/3500-3599/3553.Minimum%20Weighted%20Subgraph%20With%20the%20Required%20Paths%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây <strong>vô hướng có trọng số</strong> gồm <code data-end="51" data-start="48">n</code> node, được đánh số từ <code data-end="75" data-start="72">0</code> đến <code data-end="86" data-start="79">n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code data-end="129" data-start="122">edges</code> có độ dài <code data-end="147" data-start="140">n - 1</code>, trong đó <code data-end="185" data-start="160">edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> cho biết có một cạnh nối các node <code data-end="236" data-start="232">u<sub>i</sub></code> và <code data-end="245" data-start="241">v<sub>i</sub></code> với trọng số <code data-end="262" data-start="258">w<sub>i</sub></code>.​</p>

<p>Ngoài ra, bạn được cho một mảng số nguyên 2 chiều <code data-end="56" data-start="47">queries</code>, trong đó <code data-end="105" data-start="69">queries[j] = [src1<sub>j</sub>, src2<sub>j</sub>, dest<sub>j</sub>]</code>.</p>

<p>Trả về một mảng <code data-end="24" data-start="16">answer</code> có độ dài bằng <code data-end="60" data-start="44">queries.length</code>, trong đó <code data-end="79" data-start="68">answer[j]</code> là <strong>tổng trọng số nhỏ nhất</strong> của một cây con sao cho có thể đi đến <code data-end="174" data-start="167">dest<sub>j</sub></code> từ cả <code data-end="192" data-start="185">src1<sub>j</sub></code> và <code data-end="204" data-start="197">src2<sub>j</sub></code> bằng các cạnh trong cây con đó.</p>

<p>Ở đây, <strong data-end="2287" data-start="2276">cây con</strong> là bất kỳ tập con liên thông nào của các node và cạnh trong cây ban đầu, tạo thành một cây hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1,2],[1,2,3],[1,3,5],[1,4,4],[2,5,6]], queries = [[2,3,4],[0,2,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[12,11]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cạnh màu xanh biểu diễn một trong những cây con cho ra đáp án tối ưu.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3553.Minimum%20Weighted%20Subgraph%20With%20the%20Required%20Paths%20II/images/tree1-4.jpg" style="width: 531px; height: 322px;" /></p>

<ul>
    <li data-end="118" data-start="0">
    <p data-end="118" data-start="2"><code>answer[0]</code>: Tổng trọng số của cây con được chọn, đảm bảo có đường đi từ <code>src1 = 2</code> và <code>src2 = 3</code> đến <code>dest = 4</code>, là <code>3 + 5 + 4 = 12</code>.</p>
    </li>
    <li data-end="235" data-start="119">
    <p data-end="235" data-start="121"><code>answer[1]</code>: Tổng trọng số của cây con được chọn, đảm bảo có đường đi từ <code>src1 = 0</code> và <code>src2 = 2</code> đến <code>dest = 5</code>, là <code>2 + 3 + 6 = 11</code>.</p>
    </li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[1,0,8],[0,2,7]], queries = [[0,1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[15]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3553.Minimum%20Weighted%20Subgraph%20With%20the%20Required%20Paths%20II/images/tree1-5.jpg" style="width: 270px; height: 80px;" /></p>

<ul>
    <li><code>answer[0]</code>: Tổng trọng số của cây con được chọn, đảm bảo có đường đi từ <code>src1 = 0</code> và <code>src2 = 1</code> đến <code>dest = 2</code>, là <code>8 + 7 = 15</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="36" data-start="20"><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li data-end="62" data-start="39"><code>edges.length == n - 1</code></li>
    <li data-end="87" data-start="65"><code>edges[i].length == 3</code></li>
    <li data-end="107" data-start="90"><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li data-end="127" data-start="110"><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
    <li data-end="159" data-start="130"><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li data-end="186" data-start="162"><code>queries[j].length == 3</code></li>
    <li data-end="219" data-start="189"><code>0 &lt;= src1<sub>j</sub>, src2<sub>j</sub>, dest<sub>j</sub> &lt; n</code></li>
    <li><code>src1<sub>j</sub></code>, <code>src2<sub>j</sub></code> và <code>dest<sub>j</sub></code> đôi một khác nhau.</li>
    <li>Đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu cây con có tổng trọng số nhỏ nhất chứa cả $src1 \to dest$ và $src2 \to dest$, tức là hợp của ba đường đi. Với $n,q \le 10^5$, không thể thực hiện BFS cho từng truy vấn.
>
> Trọng số của hợp của hai đường đi có thể tính bằng $(\textit{dist}(a,b)+\textit{dist}(a,c)+\textit{dist}(b,c))/2$. Sau khi có độ sâu, các prefix có trọng số và LCA, mỗi khoảng cách được tính trong $O(\log n)$, rồi công thức ba khoảng cách sẽ trả lời truy vấn.

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
