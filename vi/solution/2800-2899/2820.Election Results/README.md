---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2820. Election Results 🔒](https://leetcode.com/problems/election-results)

[中文文档](/solution/2800-2899/2820.Election%20Results/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Votes</font></code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| voter       | varchar |
| candidate   | varchar |
+-------------+---------+
(voter, candidate) là khóa chính (tổ hợp các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng chứa tên của cử tri và ứng viên mà họ chọn.
</pre>

<p>Cuộc bầu cử được tổ chức tại một thành phố, nơi mọi người có thể bỏ phiếu cho <strong>một hoặc nhiều</strong> ứng viên hoặc chọn <strong>không</strong> bỏ phiếu. Mỗi người có <code>1</code><strong> phiếu</strong>, vì vậy nếu họ bỏ phiếu cho nhiều ứng viên thì phiếu của họ được chia đều cho các ứng viên đó. Ví dụ, nếu một người bỏ phiếu cho <code>2</code> ứng viên thì mỗi ứng viên nhận được tương đương <code>0.5</code>&nbsp;phiếu.</p>

<p>Hãy viết một lời giải để tìm <code>candidate</code> nhận được nhiều phiếu nhất và chiến thắng cuộc bầu cử. Xuất tên của <strong>candidate</strong> hoặc nếu nhiều ứng viên có <strong>số phiếu bằng nhau</strong>, hãy hiển thị tên của tất cả các ứng viên đó.</p>

<p><em>Trả về bảng kết quả được sắp xếp</em> <em>theo</em> <code>candidate</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Votes:
+----------+-----------+
| voter    | candidate |
+----------+-----------+
| Kathy    | null      |
| Charles  | Ryan      |
| Charles  | Christine |
| Charles  | Kathy     |
| Benjamin | Christine |
| Anthony  | Ryan      |
| Edward   | Ryan      |
| Terry    | null      |
| Evelyn   | Kathy     |
| Arthur   | Christine |
+----------+-----------+
<strong>Đầu ra:</strong>
+-----------+
| candidate |
+-----------+
| Christine |
| Ryan      |
+-----------+
<strong>Giải thích:</strong>
- Kathy và Terry chọn không tham gia bỏ phiếu, nên số phiếu của họ được ghi nhận là 0. Charles chia phiếu của mình cho ba ứng viên, tương đương 0.33 phiếu cho mỗi ứng viên. Trong khi đó, Benjamin, Arthur, Anthony, Edward và Evelyn mỗi người bỏ phiếu cho một ứng viên duy nhất.
- Tổng cộng, ứng viên Ryan và Christine nhận được 2.33 phiếu, còn Kathy nhận được tổng cộng 1.33 phiếu.
Vì Ryan và Christine nhận được số phiếu bằng nhau, chúng ta sẽ hiển thị tên của họ theo thứ tự tăng dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ + Thống kê nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Phiếu của mỗi cử tri được chia đều cho các ứng viên khác null mà cử tri đó chọn. Một `COUNT` trên cửa sổ cho trọng số $1/\mathrm{cnt}$, `SUM` theo nhóm tính tổng cho từng ứng viên, còn `RANK` giữ lại mọi ứng viên đứng đầu và sắp xếp tên theo thứ tự bảng chữ cái.

<!-- thinking:end -->

Ta có thể dùng hàm cửa sổ `count` để tính số phiếu mà mỗi cử tri dành cho các ứng viên, sau đó dùng hàm thống kê nhóm `sum` để tính tổng số phiếu của từng ứng viên. Tiếp theo, ta dùng hàm cửa sổ `rank` để tính thứ hạng của mỗi ứng viên, rồi lọc ra ứng viên đứng đầu.

Lưu ý rằng có thể có nhiều ứng viên cùng đứng đầu trong tập kết quả, vì vậy ta cần dùng `order by` để sắp xếp các ứng viên.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT candidate, SUM(vote) AS tot
        FROM
            (
                SELECT
                    candidate,
                    1 / (COUNT(candidate) OVER (PARTITION BY voter)) AS vote
                FROM Votes
                WHERE candidate IS NOT NULL
            ) AS t
        GROUP BY 1
    ),
    P AS (
        SELECT
            candidate,
            RANK() OVER (ORDER BY tot DESC) AS rk
        FROM T
    )
SELECT candidate
FROM P
WHERE rk = 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
