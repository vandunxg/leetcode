---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3054. Binary Tree Nodes 🔒](https://leetcode.com/problems/binary-tree-nodes)

[中文文档](/solution/3000-3099/3054.Binary%20Tree%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Tree</code></font></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| N           | int  |
| P           | int  |
+-------------+------+
N là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng gồm N và P, trong đó N biểu diễn giá trị của một node trong cây nhị phân, còn P là node cha của N.
</pre>

<p>Hãy viết lời giải để xác định loại node trong cây nhị phân. Với mỗi node, hãy xuất một trong các loại sau:</p>

<ul>
	<li><strong>Root</strong>: nếu node là node gốc.</li>
	<li><strong>Leaf</strong>: nếu node là node lá.</li>
	<li><strong>Inner</strong>: nếu node không phải node gốc cũng không phải node lá.</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo giá trị node theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Tree:
+---+------+
| N | P    |
+---+------+
| 1 | 2    |
| 3 | 2    |
| 6 | 8    |
| 9 | 8    |
| 2 | 5    |
| 8 | 5    |
| 5 | null |
+---+------+
<strong>Đầu ra:</strong>
+---+-------+
| N | Type  |
+---+-------+
| 1 | Leaf  |
| 2 | Inner |
| 3 | Leaf  |
| 5 | Root  |
| 6 | Leaf  |
| 8 | Inner |
| 9 | Leaf  |
+---+-------+
<strong>Giải thích:</strong>
- Node 5 là node gốc vì không có node cha.
- Các node 1, 3, 6 và 9 là node lá vì chúng không có node con nào.
- Các node 2 và 8 là node trung gian vì chúng là node cha của một số node trong cấu trúc.
</pre>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với <a href="https://leetcode.com/problems/tree-node/description/" target="_blank"> 608: Tree Node.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: LEFT JOIN

<!-- thinking:start -->

> **Tư duy**
>
> Vai trò của một node phụ thuộc vào việc node đó có node cha và node con hay không. Node cha nằm trong cột $P$; các node con là những hàng khác trỏ đến node này.
>
> Phép left self-join trên $t_1.N=t_2.P$ giúp phân biệt $t_1.P$ null (node gốc), $t_2$ null (node lá) và node trung gian.
>
> Phép join có thể tạo ra nhiều bản sao của node, nên chúng ta lấy $N$ distinct rồi sắp xếp.

<!-- thinking:end -->

Nếu parent của một node là null, node đó là node gốc; nếu một node không là parent của bất kỳ node nào, đó là node lá; nếu không, đó là node nội bộ.

Do đó, chúng ta dùng left join để join bảng `Tree` với chính nó, với điều kiện join là `t1.N = t2.P`. Nếu `t1.P` là null, thì `t1.N` là node gốc; nếu `t2.P` là null, thì `t1.N` là node lá; nếu không, `t1.N` là node nội bộ.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT
    t1.N AS N,
    IF(t1.P IS NULL, 'Root', IF(t2.P IS NULL, 'Leaf', 'Inner')) AS Type
FROM
    Tree AS t1
    LEFT JOIN Tree AS t2 ON t1.N = t2.p
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
