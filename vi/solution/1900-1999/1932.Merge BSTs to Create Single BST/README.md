---
comments: true
difficulty: Hard
rating: 2483
source: Weekly Contest 249 Q4
tags:
    - Tree
    - Depth-First Search
    - Binary Search Tree
    - Array
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [1932. Merge BSTs to Create Single BST](https://leetcode.com/problems/merge-bsts-to-create-single-bst)

[中文文档](/solution/1900-1999/1932.Merge%20BSTs%20to%20Create%20Single%20BST/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>n</code> <strong>node gốc của các BST (cây tìm kiếm nhị phân)</strong> thuộc <code>n</code> BST riêng biệt, được lưu trong một mảng <code>trees</code> (<strong>đánh chỉ số từ 0</strong>). Mỗi BST trong <code>trees</code> có <strong>nhiều nhất 3 node</strong>, và không có hai node gốc nào có cùng giá trị. Trong một thao tác, bạn có thể:</p>

<ul>
	<li>Chọn hai chỉ số <strong>phân biệt</strong> <code>i</code> và <code>j</code> sao cho giá trị được lưu tại một trong các <strong>node lá</strong> của <code>trees[i]</code> bằng với <strong>giá trị node gốc</strong> của <code>trees[j]</code>.</li>
	<li>Thay node lá trong <code>trees[i]</code> bằng <code>trees[j]</code>.</li>
	<li>Xóa <code>trees[j]</code> khỏi <code>trees</code>.</li>
</ul>

<p>Hãy trả về <em><strong>node gốc</strong> của BST thu được nếu có thể tạo thành một BST hợp lệ sau khi thực hiện </em><code>n - 1</code><em> thao tác, hoặc</em><em> </em><code>null</code> <i>nếu không thể tạo thành một BST hợp lệ</i>.</p>

<p>Một BST (cây tìm kiếm nhị phân) là một cây nhị phân mà mỗi node thỏa mãn các tính chất sau:</p>

<ul>
	<li>Mọi node trong cây con trái của node đó có giá trị <strong>nhỏ hơn nghiêm ngặt</strong> giá trị của node đó.</li>
	<li>Mọi node trong cây con phải của node đó có giá trị <strong>lớn hơn nghiêm ngặt</strong> giá trị của node đó.</li>
</ul>

<p>Node lá là node không có node con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1932.Merge%20BSTs%20to%20Create%20Single%20BST/images/d1.png" style="width: 450px; height: 163px;" />
<pre>
<strong>Đầu vào:</strong> trees = [[2,1],[3,2,5],[5,4]]
<strong>Đầu ra:</strong> [3,2,5,1,null,4]
<strong>Giải thích:</strong>
Trong thao tác đầu tiên, chọn i=1 và j=0, rồi gộp trees[0] vào trees[1].
Xóa trees[0], khi đó trees = [[3,2,5,1],[5,4]].
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1932.Merge%20BSTs%20to%20Create%20Single%20BST/images/diagram.png" style="width: 450px; height: 181px;" />
Trong thao tác thứ hai, chọn i=0 và j=1, rồi gộp trees[1] vào trees[0].
Xóa trees[1], khi đó trees = [[3,2,5,1,null,4]].
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1932.Merge%20BSTs%20to%20Create%20Single%20BST/images/diagram-2.png" style="width: 220px; height: 165px;" />
Cây thu được, được minh họa ở trên, là một BST hợp lệ, nên trả về node gốc của nó.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1932.Merge%20BSTs%20to%20Create%20Single%20BST/images/d2.png" style="width: 450px; height: 171px;" />
<pre>
<strong>Đầu vào:</strong> trees = [[5,3,8],[3,2,6]]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong>
Chọn i=0 và j=1, rồi gộp trees[1] vào trees[0].
Xóa trees[1], khi đó trees = [[5,3,8,2,6]].
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1932.Merge%20BSTs%20to%20Create%20Single%20BST/images/diagram-3.png" style="width: 240px; height: 196px;" />
Cây thu được được minh họa ở trên. Đây là thao tác hợp lệ duy nhất có thể thực hiện, nhưng cây thu được không phải là một BST hợp lệ, nên trả về null.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1932.Merge%20BSTs%20to%20Create%20Single%20BST/images/d3.png" style="width: 430px; height: 168px;" />
<pre>
<strong>Đầu vào:</strong> trees = [[5,4],[3]]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không thể thực hiện bất kỳ thao tác nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == trees.length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li>Số node trong mỗi cây nằm trong khoảng <code>[1, 3]</code>.</li>
	<li>Mỗi node trong đầu vào có thể có node con nhưng không có node cháu.</li>
	<li>Không có hai node gốc nào của <code>trees</code> có cùng giá trị.</li>
	<li>Tất cả các cây trong đầu vào đều là <strong>BST hợp lệ</strong>.</li>
	<li><code>1 &lt;= TreeNode.val &lt;= 5 * 10<sup>4</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cây có nhiều nhất ba node và một lần gộp sẽ gắn một node gốc vào một node lá. Việc thử tất cả các cách ghép là không thể với $n\le 5\times 10^4$.
>
> Một node lá có giá trị bằng với node gốc của một cây khác thì phải nhận cây đó. Cuối cùng phải còn lại đúng một node gốc (giá trị của nó không xuất hiện ở node lá). Sau khi lắp ghép duy nhất này, ta kiểm tra thứ tự inorder để xác nhận BST hợp lệ.
>
> Một hash map ánh xạ các node gốc; ta gắn cây vào các node lá tương ứng và trả về không hợp lệ nếu node gốc không duy nhất, nếu không thể gắn, hoặc nếu dãy inorder không tăng nghiêm ngặt.

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
