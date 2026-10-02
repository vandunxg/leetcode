---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
---

<!-- problem:start -->

# [1516. Move Sub-Tree of N-Ary Tree 🔒](https://leetcode.com/problems/move-sub-tree-of-n-ary-tree)

[中文文档](/solution/1500-1599/1516.Move%20Sub-Tree%20of%20N-Ary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một <span data-keyword="n-ary-tree">cây N-ary</span> có các giá trị duy nhất, cùng hai node <code>p</code> và <code>q</code> của cây.</p>

<p>Hãy di chuyển cây con của node <code>p</code> để trở thành node con trực tiếp của node <code>q</code>. Nếu <code>p</code> đã là node con trực tiếp của <code>q</code>, không thay đổi gì. Node <code>p</code> <strong>phải là</strong> node con cuối cùng trong danh sách con của node <code>q</code>.</p>

<p>Trả về <em>root của cây</em> sau khi điều chỉnh.</p>

<p>&nbsp;</p>

<p>Có 3 trường hợp đối với node <code>p</code> và <code>q</code>:</p>

<ol>
	<li>Node <code>q</code> nằm trong cây con của node <code>p</code>.</li>
	<li>Node <code>p</code> nằm trong cây con của node <code>q</code>.</li>
	<li>Node <code>p</code> không nằm trong cây con của node <code>q</code> và node <code>q</code> cũng không nằm trong cây con của node <code>p</code>.</li>
</ol>

<p>Trong trường hợp 2 và 3, chỉ cần di chuyển <code><span>p</span></code> (cùng cây con của nó) để trở thành con của <code>q</code>, nhưng trong trường hợp 1 cây có thể bị tách rời, vì vậy cần nối lại cây. <strong>Hãy đọc kỹ các ví dụ trước khi giải bài này.</strong></p>

<p>&nbsp;</p>

<p><em>Dữ liệu vào của cây N-ary được tuần tự hóa theo thứ tự duyệt level-order, mỗi nhóm node con được ngăn cách bởi giá trị null (xem các ví dụ).</em></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1516.Move%20Sub-Tree%20of%20N-Ary%20Tree/images/sample_4_964.png" style="width: 296px; height: 241px;" /></p>

<p>For example, the above tree is serialized as <code>[1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1516.Move%20Sub-Tree%20of%20N-Ary%20Tree/images/move_e1.jpg" style="width: 450px; height: 188px;" />
<pre>
<strong>Input:</strong> root = [1,null,2,3,null,4,5,null,6,null,7,8], p = 4, q = 1
<strong>Output:</strong> [1,null,2,3,4,null,5,null,6,null,7,8]
<strong>Explanation:</strong> Ví dụ này thuộc trường hợp thứ hai vì node p nằm trong cây con của node q. Ta di chuyển node p cùng cây con của nó để trở thành node con trực tiếp của node q.
Lưu ý rằng node 4 là node con cuối cùng của node 1.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1516.Move%20Sub-Tree%20of%20N-Ary%20Tree/images/move_e2.jpg" style="width: 281px; height: 281px;" />
<pre>
<strong>Input:</strong> root = [1,null,2,3,null,4,5,null,6,null,7,8], p = 7, q = 4
<strong>Output:</strong> [1,null,2,3,null,4,5,null,6,null,7,8]
<strong>Explanation:</strong> Node 7 đã là node con trực tiếp của node 4. Ta không thay đổi gì.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1516.Move%20Sub-Tree%20of%20N-Ary%20Tree/images/move_e3.jpg" style="width: 450px; height: 331px;" />
<pre>
<strong>Input:</strong> root = [1,null,2,3,null,4,5,null,6,null,7,8], p = 3, q = 8
<strong>Output:</strong> [1,null,2,null,4,5,null,7,8,null,null,null,3,null,6]
<strong>Explanation:</strong> Ví dụ này thuộc trường hợp 3 vì node p không nằm trong cây con của node q và ngược lại. Ta có thể di chuyển node 3 cùng cây con của nó và đặt làm node con của node 8.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1516.Move%20Sub-Tree%20of%20N-Ary%20Tree/images/untitled-diagramdrawio.png" style="width: 500px; height: 175px;" />
<pre>
<strong>Input:</strong> root = [1,null,2,3,null,4], p = 1, q = 4
<strong>Output:</strong> [4,null,1,null,2,3]
<strong>Explanation:</strong> Ví dụ này thuộc trường hợp 1 vì node q nằm trong cây con của node p. Tách node 4 khỏi node cha của nó, rồi di chuyển node 1 cùng cây con và đặt làm node con của node 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Tổng số node nằm trong khoảng <code>[2, 1000]</code>.</li>
	<li>Mỗi node có một giá trị <strong>duy nhất</strong>.</li>
	<li><code>p != null</code></li>
	<li><code>q != null</code></li>
	<li><code>p</code> and <code>q</code> are two different nodes (i.e. <code>p != q</code>).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải di chuyển cây con gốc $p$ để $p$ trở thành con của $q$. Nếu nối lại các con trỏ mà không xét quan hệ tổ tiên, cây sẽ bị hỏng khi $q$ nằm trong cây con của $p$: tách $p$ cũng làm $q$ bị ngắt khỏi phần còn lại của cây.
>
> Tìm $p$, $q$ và các node cha của chúng, rồi kiểm tra quan hệ tổ tiên. Nếu $q$ nằm dưới $p$, trước tiên gắn $q$ vào node cha ban đầu của $p$, sau đó gắn $p$ dưới $q$; nếu không, tách $p$ khỏi node cha rồi treo nó dưới $q$. Cây đủ nhỏ nên chỉ cần vài lần duyệt để tìm node và nối lại các liên kết.

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
