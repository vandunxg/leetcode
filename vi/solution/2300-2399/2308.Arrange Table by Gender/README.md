---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2308. Arrange Table by Gender 🔒](https://leetcode.com/problems/arrange-table-by-gender)

[中文文档](/solution/2300-2399/2308.Arrange%20Table%20by%20Gender/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Genders</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| gender      | varchar |
+-------------+---------+
user_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
gender là ENUM (nhóm) có kiểu &#39;female&#39;, &#39;male&#39; hoặc &#39;other&#39;.
Mỗi hàng trong bảng này chứa ID và giới tính của một người dùng.
Bảng có số lượng &#39;female&#39;, &#39;male&#39; và &#39;other&#39; bằng nhau.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để sắp xếp lại bảng <code>Genders</code> sao cho các hàng lần lượt xen kẽ theo thứ tự <code>&#39;female&#39;</code>, <code>&#39;other&#39;</code> và <code>&#39;male&#39;</code>. Bảng phải được sắp xếp lại sao cho ID của mỗi giới tính được sắp xếp theo thứ tự tăng dần.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự đã nêu</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Genders:
+---------+--------+
| user_id | gender |
+---------+--------+
| 4       | male   |
| 7       | female |
| 2       | other  |
| 5       | male   |
| 3       | female |
| 8       | male   |
| 6       | other  |
| 1       | other  |
| 9       | female |
+---------+--------+
<strong>Đầu ra:</strong>
+---------+--------+
| user_id | gender |
+---------+--------+
| 3       | female |
| 1       | other  |
| 4       | male   |
| 7       | female |
| 2       | other  |
| 5       | male   |
| 9       | female |
| 6       | other  |
| 8       | male   |
+---------+--------+
<strong>Giải thích:</strong>
Giới tính female: các ID 3, 7 và 9.
Giới tính other: các ID 1, 2 và 6.
Giới tính male: các ID 4, 5 và 8.
Ta sắp xếp bảng xen kẽ giữa &#39;female&#39;, &#39;other&#39; và &#39;male&#39;.
Lưu ý rằng ID của mỗi giới tính được sắp xếp theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng phải xen kẽ theo giới tính, đồng thời mỗi giới tính phải giữ thứ tự theo $user\_id$. Nếu chỉ sắp xếp theo giới tính, ta sẽ nhận được ba nhóm liên tiếp.
>
> Hãy đánh hạng $user\_id$ trong từng giới tính, rồi ánh xạ female / other / male thành $0,1,2$. Sắp xếp theo hạng đó, sau đó theo ánh xạ, để ba giới tính ở cùng một hạng xuất hiện theo đúng thứ tự yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY gender
                ORDER BY user_id
            ) AS rk1,
            CASE
                WHEN gender = 'female' THEN 0
                WHEN gender = 'other' THEN 1
                ELSE 2
            END AS rk2
        FROM Genders
    )
SELECT user_id, gender
FROM t
ORDER BY rk1, rk2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tạo hai khóa sắp xếp trong CTE. Ta có thể đưa cùng phép đánh hạng cửa sổ vào $ORDER\ BY$: phân vùng theo giới tính, đánh hạng theo $user\_id$, sau đó dùng thứ tự từ điển của gender (female, male, other) làm khóa thứ hai, loại bỏ $CASE$ tường minh và phần $WITH$ bổ sung.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    user_id,
    gender
FROM Genders
ORDER BY
    (
        RANK() OVER (
            PARTITION BY gender
            ORDER BY user_id
        )
    ),
    2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
