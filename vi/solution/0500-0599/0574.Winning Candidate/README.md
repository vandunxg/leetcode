---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [574. Winning Candidate 🔒](https://leetcode.com/problems/winning-candidate)

[中文文档](/solution/0500-0599/0574.Winning%20Candidate/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Candidate</code></p>

<pre>
+-------------+----------+
| Tên cột     | Kiểu     |
+-------------+----------+
| id          | int      |
| name        | varchar  |
+-------------+----------+
id là cột có giá trị duy nhất trong bảng này.
Mỗi hàng trong bảng chứa thông tin về id và tên của một ứng viên.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Vote</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| id          | int  |
| candidateId | int  |
+-------------+------+
id là khóa chính tự tăng (cột có giá trị duy nhất).
candidateId là khóa ngoại (cột tham chiếu) đến id trong bảng Candidate.
Mỗi hàng trong bảng xác định ứng viên nhận được phiếu bầu thứ i trong cuộc bầu cử.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm tên ứng viên chiến thắng (tức ứng viên nhận được nhiều phiếu bầu nhất).</p>

<p>Các test case được tạo sao cho <strong>chỉ có đúng một ứng viên chiến thắng</strong> trong cuộc bầu cử.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Candidate:
+----+------+
| id | name |
+----+------+
| 1  | A    |
| 2  | B    |
| 3  | C    |
| 4  | D    |
| 5  | E    |
+----+------+
Bảng Vote:
+----+-------------+
| id | candidateId |
+----+-------------+
| 1  | 2           |
| 2  | 4           |
| 3  | 3           |
| 4  | 2           |
| 5  | 5           |
+----+-------------+
<strong>Đầu ra:</strong> 
+------+
| name |
+------+
| B    |
+------+
<strong>Giải thích:</strong> 
Ứng viên B có 2 phiếu bầu. Mỗi ứng viên C, D và E có 1 phiếu.
Ứng viên B là người chiến thắng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ứng viên chiến thắng là người có nhiều phiếu nhất. Đếm theo `CandidateId`, lấy id có số phiếu cao nhất rồi join để lấy tên.
>
> Truy vấn con gom nhóm và đếm, kết hợp `ORDER BY COUNT DESC LIMIT 1`, sẽ trả về id; truy vấn bên ngoài join để lấy `Name`. Đề bài đảm bảo không có trường hợp hòa.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    Name
FROM
    (
        SELECT
            CandidateId AS id
        FROM Vote
        GROUP BY CandidateId
        ORDER BY COUNT(id) DESC
        LIMIT 1
    ) AS t
    INNER JOIN Candidate AS c ON t.id = c.id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tổng hợp trước rồi mới join. Ở cách này, bắt đầu từ danh sách ứng viên, left join với phiếu bầu, gom nhóm theo ứng viên rồi sắp xếp theo số phiếu.
>
> `COUNT(1)` sau phép left join là tổng số phiếu; bằng $0$ nếu ứng viên không có phiếu nào. Cách này dùng ít hơn một truy vấn con và vẫn tìm được cùng ứng viên chiến thắng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT name
FROM
    Candidate AS c
    LEFT JOIN Vote AS v ON c.id = v.candidateId
GROUP BY c.id
ORDER BY COUNT(1) DESC
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
