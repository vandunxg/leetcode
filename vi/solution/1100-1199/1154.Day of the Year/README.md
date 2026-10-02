---
comments: true
difficulty: Easy
rating: 1199
source: Weekly Contest 149 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1154. Day of the Year](https://leetcode.com/problems/day-of-the-year)

[中文文档](/solution/1100-1199/1154.Day%20of%20the%20Year/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>date</code> biểu diễn một ngày theo <a href="https://en.wikipedia.org/wiki/Gregorian_calendar" target="_blank">lịch Gregorian</a>, có định dạng <code>YYYY-MM-DD</code>. Hãy trả về <em>thứ tự ngày đó trong năm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> date = &quot;2019-01-09&quot;
<strong>Output:</strong> 9
<strong>Giải thích:</strong> Ngày đã cho là ngày thứ 9 của năm 2019.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> date = &quot;2019-02-10&quot;
<strong>Output:</strong> 41
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>date.length == 10</code></li>
	<li><code>date[4] == date[7] == &#39;-&#39;</code>, các <code>date[i]</code> còn lại đều là chữ số.</li>
	<li><code>date</code> biểu diễn một ngày trong khoảng từ ngày 1 tháng 1 năm 1900 đến ngày 31 tháng 12 năm 2019.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự ngày trong năm bằng tổng số ngày của các tháng trước cộng với ngày hiện tại. Tách $y,m,d$, xác định số ngày của tháng 2 theo quy tắc năm nhuận, rồi cộng $days[0..m-2]$ với $d$. Không cần tạo đối tượng ngày tháng đầy đủ.

<!-- thinking:end -->

Theo đề bài, ngày đã cho thuộc lịch Gregorian, nên ta có thể tính trực tiếp thứ tự ngày đó trong năm.

Trước tiên, lấy năm, tháng và ngày từ ngày đã cho, lần lượt ký hiệu là $y$, $m$, $d$.

Sau đó, tính số ngày của tháng 2 trong năm đó theo quy tắc năm nhuận của lịch Gregorian. Tháng 2 có $29$ ngày trong năm nhuận và $28$ ngày trong năm thường.

> Quy tắc xác định năm nhuận: năm đó chia hết cho $400$, hoặc chia hết cho $4$ nhưng không chia hết cho $100$.

Cuối cùng, tính thứ tự ngày trong năm bằng cách cộng số ngày của các tháng trước với ngày hiện tại.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dayOfYear(self, date: str) -> int:
        y, m, d = (int(s) for s in date.split('-'))
        v = 29 if y % 400 == 0 or (y % 4 == 0 and y % 100) else 28
        days = [31, v, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
        return sum(days[: m - 1]) + d
```

#### Java

```java
class Solution {
    public int dayOfYear(String date) {
        int y = Integer.parseInt(date.substring(0, 4));
        int m = Integer.parseInt(date.substring(5, 7));
        int d = Integer.parseInt(date.substring(8));
        int v = y % 400 == 0 || (y % 4 == 0 && y % 100 != 0) ? 29 : 28;
        int[] days = {31, v, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        int ans = d;
        for (int i = 0; i < m - 1; ++i) {
            ans += days[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int dayOfYear(string date) {
        int y, m, d;
        sscanf(date.c_str(), "%d-%d-%d", &y, &m, &d);
        int v = y % 400 == 0 || (y % 4 == 0 && y % 100) ? 29 : 28;
        int days[] = {31, v, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        int ans = d;
        for (int i = 0; i < m - 1; ++i) {
            ans += days[i];
        }
        return ans;
    }
};
```

#### Go

```go
func dayOfYear(date string) (ans int) {
	var y, m, d int
	fmt.Sscanf(date, "%d-%d-%d", &y, &m, &d)
	days := []int{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}
	if y%400 == 0 || (y%4 == 0 && y%100 != 0) {
		days[1] = 29
	}
	ans += d
	for _, v := range days[:m-1] {
		ans += v
	}
	return
}
```

#### TypeScript

```ts
function dayOfYear(date: string): number {
    const y = +date.slice(0, 4);
    const m = +date.slice(5, 7);
    const d = +date.slice(8);
    const v = y % 400 == 0 || (y % 4 == 0 && y % 100) ? 29 : 28;
    const days = [31, v, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
    return days.slice(0, m - 1).reduce((a, b) => a + b, d);
}
```

#### JavaScript

```js
/**
 * @param {string} date
 * @return {number}
 */
var dayOfYear = function (date) {
    const y = +date.slice(0, 4);
    const m = +date.slice(5, 7);
    const d = +date.slice(8);
    const v = y % 400 == 0 || (y % 4 == 0 && y % 100) ? 29 : 28;
    const days = [31, v, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
    return days.slice(0, m - 1).reduce((a, b) => a + b, d);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
