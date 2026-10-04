---
comments: true
difficulty: Hard
rating: 2544
source: Biweekly Contest 156 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3544. Subtree Inversion Sum](https://leetcode.com/problems/subtree-inversion-sum)

[Tài liệu tiếng Trung](/solution/3500-3599/3544.Subtree%20Inversion%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p data-end="551" data-start="302">Bạn được cho một cây vô hướng có gốc tại nút <code>0</code>, gồm <code>n</code> nút được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh nối giữa các nút <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>.</p>

<p data-end="670" data-start="553">Bạn cũng được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums[i]</code> biểu thị giá trị tại nút <code>i</code>, cùng một số nguyên <code>k</code>.</p>

<p data-end="763" data-start="672">Bạn có thể thực hiện <strong>các phép đảo</strong> trên một tập con các nút theo những quy tắc sau:</p>

<ul data-end="1247" data-start="765">
    <li data-end="890" data-start="765">
    <p data-end="799" data-start="767"><strong data-end="799" data-start="767">Phép đảo cây con:</strong></p>

    <ul data-end="890" data-start="802">
         <li data-end="887" data-start="802">
         <p data-end="887" data-start="804">Khi đảo một nút, mọi giá trị trong <span data-keyword="subtree-of-node">cây con</span> có gốc tại nút đó sẽ được nhân với -1.</p>
         </li>
    </ul>
    </li>
    <li data-end="1247" data-start="891">
    <p data-end="931" data-start="893"><strong data-end="931" data-start="893">Ràng buộc khoảng cách giữa các phép đảo:</strong></p>

    <ul data-end="1247" data-start="934">
         <li data-end="1020" data-start="934">
         <p data-end="1020" data-start="936">Bạn chỉ có thể đảo một nút nếu nó &quot;đủ xa&quot; so với mọi nút khác đã được đảo.</p>
         </li>
         <li data-end="1247" data-start="1023">
         <p data-end="1247" data-start="1025">Cụ thể, nếu bạn đảo hai nút <code>a</code> và <code>b</code> sao cho một nút là tổ tiên của nút kia (tức là, nếu <code>LCA(a, b) = a</code> hoặc <code>LCA(a, b) = b</code>), thì khoảng cách (số cạnh trên đường đi duy nhất giữa chúng) phải ít nhất là <code>k</code>.</p>
         </li>
    </ul>
    </li>

</ul>

<p data-end="1358" data-start="1249">Hãy trả về <strong>tổng</strong> <strong>lớn nhất</strong> có thể có của các giá trị tại nút sau khi thực hiện <strong>các phép đảo</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], nums = [4,-8,-6,3,7,-2,5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">27</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3544.Subtree%20Inversion%20Sum/images/tree1-3.jpg" style="width: 311px; height: 202px;" /></p>

<ul>
    <li>Thực hiện các phép đảo tại các nút 0, 3, 4 và 6.</li>
    <li>Cuối cùng, mảng <code data-end="1726" data-start="1720">nums</code> là <code data-end="1760" data-start="1736">[-4, 8, 6, 3, 7, 2, 5]</code>, và tổng là 27.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[1,2],[2,3],[3,4]], nums = [-1,3,-2,4,-5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3544.Subtree%20Inversion%20Sum/images/tree2-1.jpg" style="width: 371px; height: 71px;" /></p>

<ul>
    <li>Thực hiện phép đảo tại nút 4.</li>
    <li data-end="2632" data-start="2483">Cuối cùng, mảng <code data-end="2569" data-start="2563">nums</code> trở thành <code data-end="2603" data-start="2584">[-1, 3, -2, 4, 5]</code>, và tổng là 9.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2]], nums = [0,-1,-2], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện các phép đảo tại các nút 1 và 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>edges.length == n - 1</code></li>
    <li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li><code>nums.length == n</code></li>
    <li><code>-5 * 10<sup>4</sup> &lt;= nums[i] &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>1 &lt;= k &lt;= 50</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đảo một cây con sẽ nhân mọi giá trị với $-1$, và hai phép đảo trên một cặp tổ tiên–hậu duệ phải cách nhau ít nhất $k$. Với $n \le 5 \cdot 10^4$ và $k \le 50$, ta có thể nghĩ đến tree DP với chiều khoảng cách.
>
> Tại mỗi nút, lưu tổng lớn nhất của cây con khi xét dấu hiện tại và khoảng cách còn lại kể từ phép đảo gần nhất của tổ tiên. Chuyển trạng thái quyết định có đảo tại đây hay không (nếu khoảng cách cho phép), sau đó cộng kết quả của các nút con.

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
