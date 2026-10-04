---
comments: true
difficulty: Easy
rating: 1168
source: Weekly Contest 342 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2651. Calculate Delayed Arrival Time](https://leetcode.com/problems/calculate-delayed-arrival-time)

[中文文档](/solution/2600-2699/2651.Calculate%20Delayed%20Arrival%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>arrivalTime</code> biểu thị thời điểm tàu đến tính theo giờ, và một số nguyên dương <code>delayedTime</code> biểu thị số giờ bị trễ.</p>

<p>Hãy trả về <em>thời điểm tàu sẽ đến ga.</em></p>

<p>Lưu ý rằng thời gian trong bài toán này được biểu diễn theo định dạng 24 giờ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arrivalTime = 15, delayedTime = 5
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Tàu dự kiến đến lúc 15:00. Tàu bị trễ 5 giờ. Do đó, tàu sẽ đến lúc 15+5 = 20 (20:00).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arrivalTime = 13, delayedTime = 11
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tàu dự kiến đến lúc 13:00. Tàu bị trễ 11 giờ. Do đó, tàu sẽ đến lúc 13+11=24 (được biểu diễn là 00:00 theo định dạng 24 giờ, nên trả về 0).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arrivaltime &lt;&nbsp;24</code></li>
	<li><code>1 &lt;= delayedTime &lt;= 24</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thời điểm đến quay vòng theo đồng hồ 24 giờ. Tổng trực tiếp có thể vượt quá $23$; lấy phần dư modulo $24$ sẽ đưa kết quả về một giờ hợp lệ mà không cần mô phỏng việc chuyển sang ngày mới.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDelayedArrivalTime(self, arrivalTime: int, delayedTime: int) -> int:
        return (arrivalTime + delayedTime) % 24
```

#### Java

```java
class Solution {
    public int findDelayedArrivalTime(int arrivalTime, int delayedTime) {
        return (arrivalTime + delayedTime) % 24;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findDelayedArrivalTime(int arrivalTime, int delayedTime) {
        return (arrivalTime + delayedTime) % 24;
    }
};
```

#### Go

```go
func findDelayedArrivalTime(arrivalTime int, delayedTime int) int {
	return (arrivalTime + delayedTime) % 24
}
```

#### TypeScript

```ts
function findDelayedArrivalTime(arrivalTime: number, delayedTime: number): number {
    return (arrivalTime + delayedTime) % 24;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_delayed_arrival_time(arrival_time: i32, delayed_time: i32) -> i32 {
        (arrival_time + delayed_time) % 24
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
