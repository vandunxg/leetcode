---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2669. Count Artist Occurrences On Spotify Ranking List 🔒](https://leetcode.com/problems/count-artist-occurrences-on-spotify-ranking-list)

[中文文档](/solution/2600-2699/2669.Count%20Artist%20Occurrences%20On%20Spotify%20Ranking%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Spotify</font></code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| id          | int     |
| track_name  | varchar |
| artist      | varchar |
+-------------+---------+
<code>id</code> là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng chứa id, track_name và artist.
</pre>

<p>Hãy viết lời giải để tìm số lần mỗi nghệ sĩ xuất hiện trong bảng xếp hạng Spotify.</p>

<p>Trả về bảng kết quả gồm tên nghệ sĩ và số lần xuất hiện tương ứng, được sắp xếp theo số lần xuất hiện theo <strong>thứ tự giảm dần</strong>. Nếu số lần xuất hiện bằng nhau, sắp xếp tên nghệ sĩ theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng bảng kết quả như trong ví dụ sau​​​​​.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>Bảng Spotify:
+---------+--------------------+------------+
| id      | track_name         | artist     |
+---------+--------------------+------------+
| 303651  | Heart Won&#39;t Forget | Sia        |
| 1046089 | Shape of you       | Ed Sheeran |
| 33445   | I&#39;m the one        | DJ Khalid  |
| 811266  | Young Dumb &amp; Broke | DJ Khalid  |
| 505727  | Happier            | Ed Sheeran |
+---------+--------------------+------------+
<strong>Đầu ra:
</strong>+------------+-------------+
| artist     | occurrences |
+------------+-------------+
| DJ Khalid  | 2           |
| Ed Sheeran | 2           |
| Sia        | 1           |
+------------+-------------+

<strong>Giải thích: </strong>Số lần xuất hiện được liệt kê theo thứ tự giảm dần dưới tên cột &quot;occurrences&quot;. Nếu số lần xuất hiện bằng nhau, tên nghệ sĩ được sắp xếp theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số hàng theo từng nghệ sĩ, sau đó sắp xếp theo số lần đếm giảm dần và tên nghệ sĩ tăng dần. `GROUP BY artist` với `COUNT`, tiếp theo là `ORDER BY occurrences DESC, artist`, khớp với thứ tự yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    artist,
    COUNT(1) AS occurrences
FROM Spotify
GROUP BY artist
ORDER BY occurrences DESC, artist;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
