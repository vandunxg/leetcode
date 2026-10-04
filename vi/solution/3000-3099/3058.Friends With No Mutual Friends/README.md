---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3058. Friends With No Mutual Friends 🔒](https://leetcode.com/problems/friends-with-no-mutual-friends)

[中文文档](/solution/3000-3099/3058.Friends%20With%20No%20Mutual%20Friends/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Friends</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id1    | int  |
| user_id2    | int  |
+-------------+------+
(user_id1, user_id2) là khóa chính (kết hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa user id1 và user id2, hai người dùng là bạn của nhau.
</pre>

<p>Hãy viết lời giải để tìm <strong>tất cả</strong> <strong>cặp</strong> người dùng là bạn của nhau và <strong>không có</strong> bạn chung.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo </em><code>user_id1,</code> <code>user_id2</code><em> theo <strong>thứ tự</strong></em><em><strong> tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Friends:
+----------+----------+
| user_id1 | user_id2 |
+----------+----------+
| 1        | 2        |
| 2        | 3        |
| 2        | 4        |
| 1        | 5        |
| 6        | 7        |
| 3        | 4        |
| 2        | 5        |
| 8        | 9        |
+----------+----------+
<strong>Đầu ra:</strong>
+----------+----------+
| user_id1 | user_id2 |
+----------+----------+
| 6        | 7        |
| 8        | 9        |
+----------+----------+
<strong>Giải thích:</strong>
- Người dùng 1 và 2 là bạn của nhau, nhưng họ có bạn chung với user ID 5, nên cặp này không được đưa vào kết quả.
- Người dùng 2 và 3 là bạn, cả hai có bạn chung với user ID 4 nên bị loại; tương tự, người dùng 2 và 4 có bạn chung với user ID 3 nên cũng không được đưa vào kết quả.
- Người dùng 1 và 5 là bạn của nhau, nhưng họ có bạn chung với user ID 2, nên cặp này không được đưa vào kết quả.
- Người dùng 6 và 7, cũng như người dùng 8 và 9, là bạn của nhau và không có bạn chung, nên được đưa vào kết quả.
- Người dùng 3 và 4 là bạn của nhau, nhưng có kết nối chung với user ID 2 nên không được đưa vào kết quả; tương tự, người dùng 2 và 5 là bạn nhưng bị loại do có kết nối chung với user ID 1.
Bảng kết quả được sắp xếp theo user_id1 theo thứ tự tăng dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy vấn con

<!-- thinking:start -->

> **Tư duy**
>
> Giữ lại một quan hệ bạn bè vô hướng khi hai người dùng không có bạn chung. Bảng chỉ lưu mỗi cạnh một lần, nên trước hết ta thêm các cạnh ngược lại.
>
> Self-join trên người dùng ở giữa sẽ liệt kê mọi cặp có chung một hàng xóm. Các cạnh ban đầu không thuộc tập đó chính là đáp án.
>
> Nối thêm hai hướng, thực hiện self-join, rồi lọc các cặp ban đầu dựa trên việc chúng có thuộc tập này hay không.

<!-- thinking:end -->

Đầu tiên, ta liệt kê tất cả quan hệ bạn bè và ghi chúng vào bảng `T`. Sau đó, ta tìm các cặp bạn bè không có bạn chung.

Tiếp theo, ta có thể dùng một truy vấn con để tìm các cặp bạn bè không có bạn chung, tức là cặp bạn bè này không cùng xuất hiện trong danh sách bạn bè của bất kỳ người nào khác.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT user_id1, user_id2 FROM Friends
        UNION ALL
        SELECT user_id2, user_id1 FROM Friends
    )
SELECT user_id1, user_id2
FROM Friends
WHERE
    (user_id1, user_id2) NOT IN (
        SELECT t1.user_id1, t2.user_id1
        FROM
            T AS t1
            JOIN T AS t2 ON t1.user_id2 = t2.user_id2
    )
ORDER BY 1, 2;
```

#### Python3

```python
import pandas as pd


def friends_with_no_mutual_friends(friends: pd.DataFrame) -> pd.DataFrame:
    cp = friends.copy()
    t = cp[["user_id1", "user_id2"]].copy()
    t = pd.concat(
        [
            t,
            cp[["user_id2", "user_id1"]].rename(
                columns={"user_id2": "user_id1", "user_id1": "user_id2"}
            ),
        ]
    )
    merged = t.merge(t, left_on="user_id2", right_on="user_id2")
    ans = cp[
        ~cp.apply(
            lambda x: (x["user_id1"], x["user_id2"])
            in zip(merged["user_id1_x"], merged["user_id1_y"]),
            axis=1,
        )
    ]
    return ans.sort_values(by=["user_id1", "user_id2"])
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
