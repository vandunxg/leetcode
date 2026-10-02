---
comments: true
difficulty: Easy
rating: 1227
source: Biweekly Contest 4 Q1
tags:
    - Math
---

<!-- problem:start -->

# [1118. Number of Days in a Month 🔒](https://leetcode.com/problems/number-of-days-in-a-month)

[中文文档](/solution/1100-1199/1118.Number%20of%20Days%20in%20a%20Month/README.md)

## Mô tả

<!-- description:start -->

<p>Cho năm <code>year</code> và tháng <code>month</code>, hãy trả về <em>số ngày trong tháng đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> year = 1992, month = 7
<strong>Output:</strong> 31
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> year = 2000, month = 2
<strong>Output:</strong> 29
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Input:</strong> year = 1900, month = 2
<strong>Output:</strong> 28
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1583 &lt;= year &lt;= 2100</code></li>
	<li><code>1 &lt;= month &lt;= 12</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xác định năm nhuận

<!-- thinking:start -->

> **Tư duy**
>
> Số ngày của các tháng là cố định, ngoại trừ tháng Hai. Quy tắc năm nhuận thông thường (chia hết cho $4$ nhưng không chia hết cho $100$, hoặc chia hết cho $400$) xác định tháng Hai có $29$ hay $28$ ngày; sau đó tra bảng để lấy $days[month]$, không cần duyệt từng ngày trong tháng.

<!-- thinking:end -->

Trước tiên, xác định năm đã cho có phải năm nhuận hay không. Nếu năm chia hết cho $4$ nhưng không chia hết cho $100$, hoặc chia hết cho $400$, thì đó là năm nhuận.

Tháng Hai có $29$ ngày trong năm nhuận và $28$ ngày trong năm thường.

Có thể dùng mảng $days$ để lưu số ngày của từng tháng trong năm hiện tại, trong đó $days[0]=0$, còn $days[i]$ là số ngày của tháng thứ $i$. Khi đó, đáp án là $days[month]$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfDays(self, year: int, month: int) -> int:
        leap = (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)
        days = [0, 31, 29 if leap else 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
        return days[month]
```

#### Java

```java
class Solution {
    public int numberOfDays(int year, int month) {
        boolean leap = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
        int[] days = new int[] {0, 31, leap ? 29 : 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        return days[month];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfDays(int year, int month) {
        bool leap = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
        vector<int> days = {0, 31, leap ? 29 : 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        return days[month];
    }
};
```

#### Go

```go
func numberOfDays(year int, month int) int {
	leap := (year%4 == 0 && year%100 != 0) || (year%400 == 0)
	x := 28
	if leap {
		x = 29
	}
	days := []int{0, 31, x, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}
	return days[month]
}
```

#### TypeScript

```ts
function numberOfDays(year: number, month: number): number {
    const leap = (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0;
    const days = [0, 31, leap ? 29 : 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
    return days[month];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
