---
comments: true
difficulty: Easy
rating: 1382
source: Weekly Contest 153 Q2
tags:
    - Math
---

<!-- problem:start -->

# [1185. Day of the Week](https://leetcode.com/problems/day-of-the-week)

[中文文档](/solution/1100-1199/1185.Day%20of%20the%20Week/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ngày tháng, hãy trả về thứ tương ứng với ngày đó.</p>

<p>Đầu vào gồm ba số nguyên lần lượt biểu thị <code>day</code>, <code>month</code> và <code>year</code>.</p>

<p>Trả về kết quả dưới dạng một trong các giá trị sau:&nbsp;<code>{&quot;Sunday&quot;, &quot;Monday&quot;, &quot;Tuesday&quot;, &quot;Wednesday&quot;, &quot;Thursday&quot;, &quot;Friday&quot;, &quot;Saturday&quot;}</code>.</p>

<p><strong>Lưu ý:</strong> Ngày 1 tháng 1 năm 1971 là thứ Sáu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> day = 31, month = 8, year = 2019
<strong>Đầu ra:</strong> &quot;Saturday&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> day = 18, month = 7, year = 1999
<strong>Đầu ra:</strong> &quot;Sunday&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> day = 15, month = 8, year = 1993
<strong>Đầu ra:</strong> &quot;Sunday&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Các ngày được cho đều hợp lệ và nằm trong khoảng từ năm <code>1971</code> đến năm <code>2100</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm thư viện

<!-- thinking:start -->

> **Tư duy**
>
> Thư viện chuẩn đã có sẵn chức năng xác định thứ trong tuần từ ngày theo lịch Gregorian. Chỉ cần tạo ngày và định dạng tên thứ, không cần tự xử lý năm nhuận hay số ngày trong tháng.

<!-- thinking:end -->

Cách đơn giản nhất là dùng thư viện ngày tháng của ngôn ngữ để lấy thứ trong tuần từ năm, tháng và ngày đã cho.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dayOfTheWeek(self, day: int, month: int, year: int) -> str:
        return datetime.date(year, month, day).strftime('%A')
```

#### Java

```java
import java.util.Calendar;

class Solution {
    private static final String[] WEEK
        = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};

    public static String dayOfTheWeek(int day, int month, int year) {
        Calendar calendar = Calendar.getInstance();
        calendar.set(year, month - 1, day);
        return WEEK[calendar.get(Calendar.DAY_OF_WEEK) - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Công thức đồng dư Zeller

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 cần thư viện ngày tháng. Công thức đồng dư Zeller tính thứ trong tuần dựa trên thế kỷ, năm trong thế kỷ, tháng và ngày; tháng 1 và tháng 2 được xem lần lượt là tháng $13$ và $14$ của năm trước đó. Không cần dùng kiểu dữ liệu ngày tháng.

<!-- thinking:end -->

Ta có thể dùng công thức đồng dư Zeller để tính thứ trong tuần. Công thức như sau:

$$
w = (\left \lfloor \frac{c}{4} \right \rfloor - 2c + y + \left \lfloor \frac{y}{4} \right \rfloor + \left \lfloor \frac{13(m+1)}{5} \right \rfloor + d - 1) \bmod 7
$$

Trong đó:

- `w`: Thứ trong tuần (bắt đầu từ Chủ nhật)
- `c`: Hai chữ số đầu của năm
- `y`: Hai chữ số cuối của năm
- `m`: Tháng (m nằm trong khoảng từ 3 đến 14. Theo công thức đồng dư Zeller, tháng 1 và tháng 2 của một năm được xem là tháng 13 và 14 của năm trước. Ví dụ, ngày 1 tháng 1 năm 2003 được xem là ngày thứ 1 của tháng 13 năm 2002.)
- `d`: Ngày
- `⌊⌋`: Hàm floor (làm tròn xuống)
- `mod`: Phép modulo

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dayOfTheWeek(self, d: int, m: int, y: int) -> str:
        if m < 3:
            m += 12
            y -= 1
        c = y // 100
        y = y % 100
        w = (c // 4 - 2 * c + y + y // 4 + 13 * (m + 1) // 5 + d - 1) % 7
        return [
            "Sunday",
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday",
            "Saturday",
        ][w]
```

#### Java

```java
class Solution {
    public String dayOfTheWeek(int d, int m, int y) {
        if (m < 3) {
            m += 12;
            y -= 1;
        }
        int c = y / 100;
        y %= 100;
        int w = (c / 4 - 2 * c + y + y / 4 + 13 * (m + 1) / 5 + d - 1) % 7;
        return new String[] {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
            "Saturday"}[(w + 7) % 7];
    }
}
```

#### C++

```cpp
class Solution {
public:
    string dayOfTheWeek(int d, int m, int y) {
        if (m < 3) {
            m += 12;
            y -= 1;
        }
        int c = y / 100;
        y %= 100;
        int w = (c / 4 - 2 * c + y + y / 4 + 13 * (m + 1) / 5 + d - 1) % 7;
        vector<string> weeks = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
        return weeks[(w + 7) % 7];
    }
};
```

#### Go

```go
func dayOfTheWeek(d int, m int, y int) string {
	if m < 3 {
		m += 12
		y -= 1
	}
	c := y / 100
	y %= 100
	w := (c/4 - 2*c + y + y/4 + 13*(m+1)/5 + d - 1) % 7
	weeks := []string{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}
	return weeks[(w+7)%7]
}
```

#### TypeScript

```ts
function dayOfTheWeek(d: number, m: number, y: number): string {
    if (m < 3) {
        m += 12;
        y -= 1;
    }
    const c: number = (y / 100) | 0;
    y %= 100;
    const w = (((c / 4) | 0) - 2 * c + y + ((y / 4) | 0) + (((13 * (m + 1)) / 5) | 0) + d - 1) % 7;
    const weeks: string[] = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
    ];
    return weeks[(w + 7) % 7];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
