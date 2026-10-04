---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3401. Find Circular Gift Exchange Chains 🔒](https://leetcode.com/problems/find-circular-gift-exchange-chains)

[中文文档](/solution/3400-3499/3401.Find%20Circular%20Gift%20Exchange%20Chains/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>SecretSanta</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| giver_id    | int  |
| receiver_id | int  |
| gift_value  | int  |
+-------------+------+
(giver_id, receiver_id) is the unique key for this table.
Each row represents a record of a gift exchange between two employees, giver_id represents the employee who gives a gift, receiver_id represents the employee who receives the gift and gift_value represents the value of the gift given.
</pre>

<p>Hãy viết lời giải để tìm <strong>tổng giá trị quà tặng</strong> và <strong>độ dài</strong> của các <strong>chuỗi vòng</strong> trao đổi quà tặng Secret Santa:</p>

<p>Một <strong>chuỗi vòng</strong> được định nghĩa là một chuỗi các lượt trao đổi trong đó:</p>

<ul>
    <li>Mỗi nhân viên tặng quà cho <strong>chính xác một</strong> nhân viên khác.</li>
    <li>Mỗi nhân viên nhận quà <strong>từ chính xác</strong> một nhân viên khác.</li>
    <li>Các lượt trao đổi tạo thành một <strong>vòng lặp</strong> liên tục (ví dụ: nhân viên A tặng quà cho B, B tặng quà cho C và C tặng lại cho A).</li>
</ul>

<p>Trả về <em>kết quả được sắp xếp theo độ dài chuỗi và tổng giá trị quà tặng của chuỗi theo thứ tự&nbsp;<strong>giảm dần</strong></em>.&nbsp;</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng SecretSanta:</p>

<pre class="example-io">
+----------+-------------+------------+
| giver_id | receiver_id | gift_value |
+----------+-------------+------------+
| 1        | 2           | 20         |
| 2        | 3           | 30         |
| 3        | 1           | 40         |
| 4        | 5           | 25         |
| 5        | 4           | 35         |
+----------+-------------+------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+----------+--------------+------------------+
| chain_id | chain_length | total_gift_value |
+----------+--------------+------------------+
| 1        | 3            | 90               |
| 2        | 2            | 60               |
+----------+--------------+------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Chuỗi 1</strong> gồm các nhân viên 1, 2 và 3:

    <ul>
        <li>Nhân viên 1 tặng quà cho 2, nhân viên 2 tặng quà cho 3 và nhân viên 3 tặng quà cho 1.</li>
        <li>Tổng giá trị quà tặng của chuỗi này = 20 + 30 + 40 = 90.</li>
    </ul>
    </li>
    <li><strong>Chuỗi 2</strong> gồm các nhân viên 4 và 5:
    <ul>
        <li>Nhân viên 4 tặng quà cho 5 và nhân viên 5 tặng quà cho 4.</li>
        <li>Tổng giá trị quà tặng của chuỗi này = 25 + 35 = 60.</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo độ dài chuỗi và tổng giá trị quà tặng của chuỗi theo thứ tự giảm dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì mỗi nhân viên tặng và nhận chính xác một món quà, các lượt trao đổi tạo thành những chu trình có hướng rời nhau. Việc duyệt các cạnh trong code ứng dụng sẽ khiến việc gom nhóm chu trình nằm ngoài SQL, trong khi bài toán yêu cầu một query duy nhất báo cáo độ dài và tổng giá trị quà tặng của từng chu trình.
>
> Đồ thị chỉ lớn bằng bảng trao đổi. Khó khăn là gán mọi cạnh thuộc cùng một chu trình vào một nhóm và chỉ xuất mỗi chu trình một lần.
>
> Một CTE đệ quy có thể lần theo $\textit{giver\_id}\to\textit{receiver\_id}$, chọn mã nhân viên nhỏ nhất trong chu trình làm $\textit{chain\_id}$, tổng hợp độ dài và $\textit{gift\_value}$, rồi sắp xếp theo độ dài và tổng giá trị theo thứ tự giảm dần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
