---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3204. Bitwise User Permissions Analysis 🔒](https://leetcode.com/problems/bitwise-user-permissions-analysis)

[中文文档](/solution/3200-3299/3204.Bitwise%20User%20Permissions%20Analysis/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>user_permissions</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| permissions | int     |
+-------------+---------+
user_id là khóa chính.
Mỗi hàng trong bảng này chứa ID người dùng và các quyền của họ được mã hóa dưới dạng số nguyên.
</pre>

<p>Coi mỗi bit trong số nguyên <code>permissions</code> biểu diễn một cấp độ truy cập hoặc tính năng khác nhau mà người dùng có.</p>

<p>Hãy viết lời giải để tính các giá trị sau:</p>

<ul>
    <li>common_perms: Cấp độ truy cập được cấp cho <strong>tất cả người dùng</strong>. Giá trị này được tính bằng phép <strong>AND bit</strong> trên cột <code>permissions</code>.</li>
    <li>any_perms: Cấp độ truy cập được cấp cho <strong>bất kỳ người dùng nào</strong>. Giá trị này được tính bằng phép <strong>OR bit</strong> trên cột <code>permissions</code>.</li>
</ul>

<p>Trả về <em>bảng kết quả theo <strong>bất kỳ</strong> thứ tự nào</em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng user_permissions:</p>

<pre class="example-io">
+---------+-------------+
| user_id | permissions |
+---------+-------------+
| 1       | 5           |
| 2       | 12          |
| 3       | 7           |
| 4       | 3           |
+---------+-------------+
 </pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+--------------+
| common_perms | any_perms   |
+--------------+-------------+
| 0            | 15          |
+--------------+-------------+
    </pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>common_perms:</strong> Biểu diễn kết quả AND bit của tất cả các quyền:

    <ul>
        <li>Với người dùng 1 (5): 5 (nhị phân 0101)</li>
        <li>Với người dùng 2 (12): 12 (nhị phân 1100)</li>
        <li>Với người dùng 3 (7): 7 (nhị phân 0111)</li>
        <li>Với người dùng 4 (3): 3 (nhị phân 0011)</li>
        <li>AND bit: 5 &amp; 12 &amp; 7 &amp; 3 = 0 (nhị phân 0000)</li>
    </ul>
    </li>
    <li><strong>any_perms:</strong> Biểu diễn kết quả OR bit của tất cả các quyền:
    <ul>
        <li>OR bit: 5 | 12 | 7 | 3 = 15 (nhị phân 1111)</li>
    </ul>
    </li>

</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép toán bit

<!-- thinking:start -->

> **Tư duy**
>
> Một bit mà mọi người dùng đều có chính là phép AND bit của cả cột; một bit mà ít nhất một người dùng có chính là phép OR bit. Có thể dùng một vòng lặp tuần tự, nhưng chỉ cần một lần quét bằng hàm aggregate là đủ.
>
> `BIT_AND(permissions)` cho ra các bit chung của mọi người dùng, còn `BIT_OR(permissions)` cho ra các bit xuất hiện ở ít nhất một người dùng, không cần self-join hay window function.

<!-- thinking:end -->

Có thể sử dụng các hàm `BIT_AND` và `BIT_OR` để tính `common_perms` và `any_perms`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    BIT_AND(permissions) AS common_perms,
    BIT_OR(permissions) AS any_perms
FROM user_permissions;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
