---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3451. Find Invalid IP Addresses](https://leetcode.com/problems/find-invalid-ip-addresses)

[中文文档](/solution/3400-3499/3451.Find%20Invalid%20IP%20Addresses/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code> logs</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| log_id      | int     |
| ip          | varchar |
| status_code | int     |
+-------------+---------+
log_id là khóa duy nhất của bảng này.
Mỗi hàng chứa thông tin log truy cập máy chủ, bao gồm địa chỉ IP và mã trạng thái HTTP.
</pre>

<p>Viết lời giải để tìm <strong>các địa chỉ IP không hợp lệ</strong>. Một địa chỉ IPv4 không hợp lệ nếu thỏa mãn bất kỳ điều kiện nào sau đây:</p>

<ul>
	<li>Chứa các số <strong>lớn hơn</strong> <code>255</code> trong bất kỳ octet nào</li>
	<li>Có <strong>số 0 ở đầu</strong> trong bất kỳ octet nào (ví dụ: <code>01.02.03.04</code>)</li>
	<li>Có <strong>ít hơn hoặc nhiều hơn</strong> <code>4</code> octet</li>
</ul>

<p>Trả về <em>bảng kết quả</em> <em>được sắp xếp theo</em> <code>invalid_count</code>,&nbsp;<code>ip</code>&nbsp;<em>theo <strong>thứ tự giảm dần</strong> tương ứng</em>.&nbsp;</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng logs:</p>

<pre class="example-io">
+--------+---------------+-------------+
| log_id | ip            | status_code |
+--------+---------------+-------------+
| 1      | 192.168.1.1   | 200         |
| 2      | 256.1.2.3     | 404         |
| 3      | 192.168.001.1 | 200         |
| 4      | 192.168.1.1   | 200         |
| 5      | 192.168.1     | 500         |
| 6      | 256.1.2.3     | 404         |
| 7      | 192.168.001.1 | 200         |
+--------+---------------+-------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------------+--------------+
| ip            | invalid_count|
+---------------+--------------+
| 256.1.2.3     | 2            |
| 192.168.001.1 | 2            |
| 192.168.1     | 1            |
+---------------+--------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>256.1.2.3 không hợp lệ vì 256 &gt; 255</li>
	<li>192.168.001.1 không hợp lệ vì có số 0 ở đầu</li>
	<li>192.168.1 không hợp lệ vì chỉ có 3 octet</li>
</ul>

<p>Bảng kết quả được sắp xếp theo invalid_count, ip theo thứ tự giảm dần tương ứng.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta đếm các địa chỉ IPv4 không hợp lệ trong log. Một địa chỉ hợp lệ có bốn octet, mỗi octet là một số nguyên trong khoảng $0..255$ và không có số 0 ở đầu.
>
> Một regex SQL có thể biểu diễn điều này, nhưng việc xử lý số 0 ở đầu cùng các khoảng giá trị khá phức tạp; tách chuỗi trong Pandas rõ ràng hơn.
>
> Tách theo `.`, kiểm tra mỗi phần là một chuỗi chữ số trong khoảng cho phép và bằng $\textit{str}(\textit{int})$. Đếm các IP không hợp lệ rồi sắp xếp theo số lượng, sau đó theo địa chỉ, đều theo thứ tự giảm dần.

<!-- thinking:end -->

Chúng ta có thể xác định một địa chỉ IP không hợp lệ dựa trên các điều kiện sau:

1. Số lượng `.` trong địa chỉ IP không bằng $3$;
2. Bất kỳ octet nào trong địa chỉ IP bắt đầu bằng `0`;
3. Bất kỳ octet nào trong địa chỉ IP lớn hơn $255$.

Sau đó, chúng ta nhóm các địa chỉ IP không hợp lệ và đếm số lần xuất hiện của từng địa chỉ IP không hợp lệ trong `invalid_count`, cuối cùng sắp xếp theo `invalid_count` và `ip` theo thứ tự giảm dần.

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    ip,
    COUNT(*) AS invalid_count
FROM logs
WHERE
    LENGTH(ip) - LENGTH(REPLACE(ip, '.', '')) != 3
    OR SUBSTRING_INDEX(ip, '.', 1) REGEXP '^0[0-9]'
    OR SUBSTRING_INDEX(SUBSTRING_INDEX(ip, '.', 2), '.', -1) REGEXP '^0[0-9]'
    OR SUBSTRING_INDEX(SUBSTRING_INDEX(ip, '.', 3), '.', -1) REGEXP '^0[0-9]'
    OR SUBSTRING_INDEX(ip, '.', -1) REGEXP '^0[0-9]'
    OR SUBSTRING_INDEX(ip, '.', 1) > 255
    OR SUBSTRING_INDEX(SUBSTRING_INDEX(ip, '.', 2), '.', -1) > 255
    OR SUBSTRING_INDEX(SUBSTRING_INDEX(ip, '.', 3), '.', -1) > 255
    OR SUBSTRING_INDEX(ip, '.', -1) > 255
GROUP BY 1
ORDER BY 2 DESC, 1 DESC;
```

#### Pandas

```python
import pandas as pd


def find_invalid_ips(logs: pd.DataFrame) -> pd.DataFrame:
    def is_valid_ip(ip: str) -> bool:
        octets = ip.split(".")
        if len(octets) != 4:
            return False
        for octet in octets:
            if not octet.isdigit():
                return False
            value = int(octet)
            if not 0 <= value <= 255 or octet != str(value):
                return False
        return True

    logs["is_valid"] = logs["ip"].apply(is_valid_ip)
    invalid_ips = logs[~logs["is_valid"]]
    invalid_count = invalid_ips["ip"].value_counts().reset_index()
    invalid_count.columns = ["ip", "invalid_count"]
    result = invalid_count.sort_values(
        by=["invalid_count", "ip"], ascending=[False, False]
    )
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
