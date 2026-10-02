---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [608. Tree Node](https://leetcode.com/problems/tree-node)

[中文文档](/solution/0600-0699/0608.Tree%20Node/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Tree</code></p>

<pre>
+-------------+------+
| Tên cột    | Kiểu |
+-------------+------+
| id          | int  |
| p_id        | int  |
+-------------+------+
id là cột có giá trị duy nhất trong bảng này.
Mỗi hàng trong bảng chứa ID của một node và ID của node cha của nó trong cây.
Cấu trúc được cho luôn là một cây hợp lệ.
</pre>

<p>&nbsp;</p>

<p>Mỗi node trong cây thuộc một trong ba loại sau:</p>

<ul>
	<li><strong>&quot;Leaf&quot;</strong>: node là lá.</li>
	<li><strong>&quot;Root&quot;</strong>: node là gốc của cây.</li>
	<li><strong>&quot;Inner&quot;</strong>: node không phải lá cũng không phải gốc.</li>
</ul>

<p>Hãy viết truy vấn xác định loại của từng node trong cây.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0608.Tree%20Node/images/tree1.jpg" style="width: 304px; height: 224px;" />
<pre>
<strong>Đầu vào:</strong> 
Bảng Tree:
+----+------+
| id | p_id |
+----+------+
| 1  | null |
| 2  | 1    |
| 3  | 1    |
| 4  | 2    |
| 5  | 2    |
+----+------+
<strong>Đầu ra:</strong> 
+----+-------+
| id | type  |
+----+-------+
| 1  | Root  |
| 2  | Inner |
| 3  | Leaf  |
| 4  | Leaf  |
| 5  | Leaf  |
+----+-------+
<strong>Giải thích:</strong> 
Node 1 là node gốc vì node cha của nó là null và nó có hai node con là 2 và 3.
Node 2 là node trung gian vì nó có node cha là 1 và các node con là 4 và 5.
Các node 3, 4 và 5 là node lá vì chúng có node cha nhưng không có node con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0608.Tree%20Node/images/tree2.jpg" style="width: 64px; height: 65px;" />
<pre>
<strong>Đầu vào:</strong> 
Bảng Tree:
+----+------+
| id | p_id |
+----+------+
| 1  | null |
+----+------+
<strong>Đầu ra:</strong> 
+----+-------+
| id | type  |
+----+-------+
| 1  | Root  |
+----+-------+
<strong>Giải thích:</strong> Nếu cây chỉ có một node, bạn chỉ cần xuất thông tin node gốc đó.
</pre>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với <a href="https://leetcode.com/problems/binary-tree-nodes/description/" target="_blank"> 3054: Binary Tree Nodes.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu lệnh điều kiện + Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Loại của một node phụ thuộc vào việc nó có node cha hay không và nó có phải node cha của node khác hay không.
>
> `CASE` gán loại Root khi `p_id IS NULL`, Inner khi `id IN (SELECT p_id)`, còn lại là Leaf.

<!-- thinking:end -->

Ta có thể dùng câu lệnh điều kiện `CASE WHEN` để xác định loại của từng node như sau:

- Nếu `p_id` của một node là `NULL`, đó là node gốc.
- Nếu không, nếu một node là node cha của node khác (ta dùng subquery để xác định điều này), thì đó là node trung gian.
- Nếu không thì đó là node lá.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    id,
    CASE
        WHEN p_id IS NULL THEN 'Root'
        WHEN id IN (SELECT p_id FROM Tree) THEN 'Inner'
        ELSE 'Leaf'
    END AS type
FROM Tree;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
