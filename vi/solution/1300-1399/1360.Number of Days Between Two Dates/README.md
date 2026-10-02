---
comments: true
difficulty: Easy
rating: 1421
source: Weekly Contest 177 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1360. Number of Days Between Two Dates](https://leetcode.com/problems/number-of-days-between-two-dates)

[中文文档](/solution/1300-1399/1360.Number%20of%20Days%20Between%20Two%20Dates/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết chương trình đếm số ngày giữa hai ngày.</p>

<p>Hai ngày được cho dưới dạng chuỗi, theo định dạng <code>YYYY-MM-DD</code> như trong các ví dụ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> date1 = "2019-06-29", date2 = "2019-06-30"
<strong>Đầu ra:</strong> 1
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> date1 = "2020-01-15", date2 = "2019-12-31"
<strong>Đầu ra:</strong> 15
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Các ngày được cho là ngày hợp lệ trong khoảng từ năm <code>1971</code> đến năm <code>2100</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính số ngày giữa hai ngày hợp lệ. Cộng độ dài các tháng thủ công dễ bỏ sót năm nhuận. Đổi mỗi ngày thành số ngày tính từ $1971$-$01$-$01$: cộng $365$ hoặc $366$ ngày cho mỗi năm, sau đó cộng số ngày theo bảng tháng (tháng Hai phụ thuộc vào quy tắc năm nhuận), rồi cộng ngày trong tháng. Giá trị chênh lệch tuyệt đối là đáp án.

<!-- thinking:end -->

Trước tiên, ta định nghĩa hàm `isLeapYear(year)` để xác định năm `year` đã cho có phải năm nhuận hay không. Nếu là năm nhuận thì trả về `true`, nếu không thì trả về `false`.

Tiếp theo, ta định nghĩa hàm `daysInMonth(year, month)` để tính số ngày trong tháng `month` của năm `year`. Ta có thể dùng mảng `days` để lưu số ngày của từng tháng, trong đó `days[1]` biểu thị số ngày của tháng Hai. Nếu là năm nhuận thì tháng này có $29$ ngày, nếu không thì có $28$ ngày.

Sau đó, ta định nghĩa hàm `calcDays(date)` để tính số ngày từ ngày `date` đã cho đến `1971-01-01`. Ta có thể dùng `date.split("-")` để tách `date` theo dấu `-` thành năm `year`, tháng `month` và ngày `day`. Tiếp theo, dùng vòng lặp để tính tổng số ngày từ năm `1971` đến `year`, rồi tính tổng số ngày từ tháng Một đến `month`, cuối cùng cộng thêm `day` ngày.

Cuối cùng, ta chỉ cần trả về giá trị tuyệt đối của `calcDays(date1) - calcDays(date2)`.

Độ phức tạp thời gian là $O(y + m)$, trong đó $y$ là số năm từ ngày đã cho đến `1971-01-01`, còn $m$ là số tháng của ngày đó. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def daysBetweenDates(self, date1: str, date2: str) -> int:
        def isLeapYear(year: int) -> bool:
            return year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)

        def daysInMonth(year: int, month: int) -> int:
            days = [
                31,
                28 + int(isLeapYear(year)),
                31,
                30,
                31,
                30,
                31,
                31,
                30,
                31,
                30,
                31,
            ]
            return days[month - 1]

        def calcDays(date: str) -> int:
            year, month, day = map(int, date.split("-"))
            days = 0
            for y in range(1971, year):
                days += 365 + int(isLeapYear(y))
            for m in range(1, month):
                days += daysInMonth(year, m)
            days += day
            return days

        return abs(calcDays(date1) - calcDays(date2))
```

#### Java

```java
class Solution {
    public int daysBetweenDates(String date1, String date2) {
        return Math.abs(calcDays(date1) - calcDays(date2));
    }

    private boolean isLeapYear(int year) {
        return year % 4 == 0 && (year % 100 != 0 || year % 400 == 0);
    }

    private int daysInMonth(int year, int month) {
        int[] days = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        days[1] += isLeapYear(year) ? 1 : 0;
        return days[month - 1];
    }

    private int calcDays(String date) {
        int year = Integer.parseInt(date.substring(0, 4));
        int month = Integer.parseInt(date.substring(5, 7));
        int day = Integer.parseInt(date.substring(8));
        int days = 0;
        for (int y = 1971; y < year; ++y) {
            days += isLeapYear(y) ? 366 : 365;
        }
        for (int m = 1; m < month; ++m) {
            days += daysInMonth(year, m);
        }
        days += day;
        return days;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int daysBetweenDates(string date1, string date2) {
        return abs(calcDays(date1) - calcDays(date2));
    }

    bool isLeapYear(int year) {
        return year % 4 == 0 && (year % 100 != 0 || year % 400 == 0);
    }

    int daysInMonth(int year, int month) {
        int days[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        days[1] += isLeapYear(year);
        return days[month - 1];
    }

    int calcDays(string date) {
        int year = stoi(date.substr(0, 4));
        int month = stoi(date.substr(5, 2));
        int day = stoi(date.substr(8, 2));
        int days = 0;
        for (int y = 1971; y < year; ++y) {
            days += 365 + isLeapYear(y);
        }
        for (int m = 1; m < month; ++m) {
            days += daysInMonth(year, m);
        }
        days += day;
        return days;
    }
};
```

#### Go

```go
func daysBetweenDates(date1 string, date2 string) int {
	return abs(calcDays(date1) - calcDays(date2))
}

func isLeapYear(year int) bool {
	return year%4 == 0 && (year%100 != 0 || year%400 == 0)
}

func daysInMonth(year, month int) int {
	days := [12]int{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}
	if isLeapYear(year) {
		days[1] = 29
	}
	return days[month-1]
}

func calcDays(date string) int {
	year, _ := strconv.Atoi(date[:4])
	month, _ := strconv.Atoi(date[5:7])
	day, _ := strconv.Atoi(date[8:])
	days := 0
	for y := 1971; y < year; y++ {
		days += 365
		if isLeapYear(y) {
			days++
		}
	}
	for m := 1; m < month; m++ {
		days += daysInMonth(year, m)
	}
	days += day
	return days
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function daysBetweenDates(date1: string, date2: string): number {
    return Math.abs(calcDays(date1) - calcDays(date2));
}

function isLeapYear(year: number): boolean {
    return year % 4 === 0 && (year % 100 !== 0 || year % 400 === 0);
}

function daysOfMonth(year: number, month: number): number {
    const days = [31, isLeapYear(year) ? 29 : 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
    return days[month - 1];
}

function calcDays(date: string): number {
    let days = 0;
    const [year, month, day] = date.split('-').map(Number);
    for (let y = 1971; y < year; ++y) {
        days += isLeapYear(y) ? 366 : 365;
    }
    for (let m = 1; m < month; ++m) {
        days += daysOfMonth(year, m);
    }
    days += day - 1;
    return days;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
