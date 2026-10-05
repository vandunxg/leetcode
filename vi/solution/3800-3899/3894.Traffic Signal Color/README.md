---
comments: true
difficulty: Easy
rating: 1222
source: Biweekly Contest 180 Q1
tags:
    - Math
    - String
    - Simulation
---

<!-- problem:start -->

# [3894. Traffic Signal Color](https://leetcode.com/problems/traffic-signal-color)

[中文文档](/solution/3800-3899/3894.Traffic%20Signal%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>timer</code> biểu thị thời gian còn lại (tính bằng giây) của một tín hiệu giao thông.</p>

<p>Tín hiệu tuân theo các quy tắc sau:</p>

<ul>
	<li>Nếu <code>timer == 0</code>, tín hiệu là <code>&quot;Green&quot;</code></li>
	<li>Nếu <code>timer == 30</code>, tín hiệu là <code>&quot;Orange&quot;</code></li>
	<li>Nếu <code>30 &lt; timer &lt;= 90</code>, tín hiệu là <code>&quot;Red&quot;</code></li>
</ul>

<p>Hãy trả về trạng thái hiện tại của tín hiệu. Nếu không thỏa mãn điều kiện nào ở trên, hãy trả về <code>&quot;Invalid&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">timer = 60</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;Red&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>timer = 60</code> và <code>30 &lt; timer &lt;= 90</code>, đáp án là <code>&quot;Red&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">timer = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;Invalid&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>timer = 5</code> không thỏa mãn điều kiện nào đã cho, đáp án là <code>&quot;Invalid&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= timer &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Trả về màu đèn dựa trên một vài phép kiểm tra rời rạc trên $\textit{timer}$, nếu không thì trả về $\texttt{Invalid}$.
>
> Các mệnh đề điều kiện không giao nhau; kiểm tra $0$, sau đó $30$, rồi đến $(30,90]$.
>
> Không cần trạng thái bổ sung.
>
> Thời gian hằng số.

<!-- thinking:end -->

Ta xác định đáp án dựa trên các điều kiện được mô tả trong đề bài và trả về chuỗi tương ứng.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def trafficSignal(self, timer: int) -> str:
        if timer == 0:
            return "Green"
        if timer == 30:
            return "Orange"
        if 30 < timer <= 90:
            return "Red"
        return "Invalid"
```

#### Java

```java
class Solution {
    public String trafficSignal(int timer) {
        if (timer == 0) {
            return "Green";
        }
        if (timer == 30) {
            return "Orange";
        }
        if (timer > 30 && timer <= 90) {
            return "Red";
        }
        return "Invalid";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string trafficSignal(int timer) {
        if (timer == 0) {
            return "Green";
        }
        if (timer == 30) {
            return "Orange";
        }
        if (timer > 30 && timer <= 90) {
            return "Red";
        }
        return "Invalid";
    }
};
```

#### Go

```go
func trafficSignal(timer int) string {
	switch {
	case timer == 0:
		return "Green"
	case timer == 30:
		return "Orange"
	case timer > 30 && timer <= 90:
		return "Red"
	default:
		return "Invalid"
	}
}
```

#### TypeScript

```ts
function trafficSignal(timer: number): string {
    if (timer === 0) {
        return 'Green';
    }
    if (timer === 30) {
        return 'Orange';
    }
    if (timer > 30 && timer <= 90) {
        return 'Red';
    }
    return 'Invalid';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
