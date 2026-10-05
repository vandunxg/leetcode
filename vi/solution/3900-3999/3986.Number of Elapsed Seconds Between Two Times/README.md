---
comments: true
difficulty: Easy
rating: 1205
source: Weekly Contest 510 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [3986. Number of Elapsed Seconds Between Two Times](https://leetcode.com/problems/number-of-elapsed-seconds-between-two-times)

[中文文档](/solution/3900-3999/3986.Number%20of%20Elapsed%20Seconds%20Between%20Two%20Times/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai thời điểm hợp lệ <code>startTime</code> và <code>endTime</code>, mỗi thời điểm được biểu diễn dưới dạng chuỗi theo định dạng <code>&quot;HH:MM:SS&quot;</code>.</p>

<p>Trả về số giây đã trôi qua từ <code>startTime</code> đến <code>endTime</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">startTime = &quot;01:00:00&quot;, endTime = &quot;01:00:25&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>
<code>endTime</code> muộn hơn <code>startTime</code> 25 giây.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">startTime = &quot;12:34:56&quot;, endTime = &quot;13:00:00&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1504</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>endTime</code> muộn hơn <code>startTime</code> 25 phút 4 giây, tương đương 1504 giây.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>startTime.length == 8</code></li>
	<li><code>endTime.length == 8</code></li>
	<li><code>startTime</code> và <code>endTime</code> là các thời điểm hợp lệ theo định dạng <code>&quot;HH:MM:SS&quot;</code></li>
	<li><code>00 &lt;= HH &lt;= 23</code></li>
	<li><code>00 &lt;= MM &lt;= 59</code></li>
	<li><code>00 &lt;= SS &lt;= 59</code></li>
	<li><code>endTime</code> không sớm hơn <code>startTime</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{endTime}$ không bao giờ sớm hơn $\textit{startTime}$, nên ta chuyển cả hai thành số giây tính từ nửa đêm rồi lấy hiệu. Mỗi chuỗi có dạng $HH\cdot 3600+MM\cdot 60+SS$.
>
> Không cần xử lý việc vượt qua nửa đêm.

<!-- thinking:end -->

Chuyển mỗi chuỗi thời gian thành số giây đã trôi qua kể từ $00$:$00$:$00$, tức là $HH \times 3600 + MM \times 60 + SS$, sau đó trả về hiệu giữa hai giá trị.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondsBetweenTimes(self, startTime: str, endTime: str) -> int:
        def f(s: str) -> int:
            return int(s[:2]) * 3600 + int(s[3:5]) * 60 + int(s[6:])

        return f(endTime) - f(startTime)
```

#### Java

```java
class Solution {
    public int secondsBetweenTimes(String startTime, String endTime) {
        return f(endTime) - f(startTime);
    }

    private int f(String s) {
        return Integer.parseInt(s.substring(0, 2)) * 3600 + Integer.parseInt(s.substring(3, 5)) * 60
            + Integer.parseInt(s.substring(6));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int secondsBetweenTimes(string startTime, string endTime) {
        return f(endTime) - f(startTime);
    }

private:
    int f(const string& s) {
        return stoi(s.substr(0, 2)) * 3600
            + stoi(s.substr(3, 2)) * 60
            + stoi(s.substr(6));
    }
};
```

#### Go

```go
func secondsBetweenTimes(startTime string, endTime string) int {
	return f(endTime) - f(startTime)
}

func f(s string) int {
	h, _ := strconv.Atoi(s[:2])
	m, _ := strconv.Atoi(s[3:5])
	sec, _ := strconv.Atoi(s[6:])
	return h*3600 + m*60 + sec
}
```

#### TypeScript

```ts
function secondsBetweenTimes(startTime: string, endTime: string): number {
    return f(endTime) - f(startTime);
}

function f(s: string): number {
    return parseInt(s.slice(0, 2)) * 3600 + parseInt(s.slice(3, 5)) * 60 + parseInt(s.slice(6));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
