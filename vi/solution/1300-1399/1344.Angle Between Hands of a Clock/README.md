---
comments: true
difficulty: Medium
rating: 1324
source: Biweekly Contest 19 Q3
tags:
    - Math
---

<!-- problem:start -->

# [1344. Angle Between Hands of a Clock](https://leetcode.com/problems/angle-between-hands-of-a-clock)

[中文文档](/solution/1300-1399/1344.Angle%20Between%20Hands%20of%20a%20Clock/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số <code>hour</code> và <code>minutes</code>, hãy trả về <em>góc nhỏ hơn (tính theo độ) tạo bởi kim </em><code>hour</code><em> và kim </em><code>minute</code><em>.</em></p>

<p>Các đáp án sai khác không quá <code>10<sup>-5</sup></code> so với giá trị thực sẽ được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1344.Angle%20Between%20Hands%20of%20a%20Clock/images/sample_1_1673.png" style="width: 300px; height: 296px;" />
<pre>
<strong>Đầu vào:</strong> hour = 12, minutes = 30
<strong>Đầu ra:</strong> 165
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1344.Angle%20Between%20Hands%20of%20a%20Clock/images/sample_2_1673.png" style="width: 300px; height: 301px;" />
<pre>
<strong>Đầu vào:</strong> hour = 3, minutes = 30
<strong>Đầu ra:</strong> 75
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1344.Angle%20Between%20Hands%20of%20a%20Clock/images/sample_3_1673.png" style="width: 300px; height: 301px;" />
<pre>
<strong>Đầu vào:</strong> hour = 3, minutes = 15
<strong>Đầu ra:</strong> 7.5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= hour &lt;= 12</code></li>
	<li><code>0 &lt;= minutes &lt;= 59</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Góc nhỏ hơn giữa kim giờ và kim phút. Kim giờ quay $30^\circ$ mỗi giờ và thêm $0.5^\circ$ mỗi phút; kim phút quay $6^\circ$ mỗi phút. Đáp án là giá trị nhỏ hơn giữa độ chênh lệch tuyệt đối của hai góc và $360^\circ$ trừ đi độ chênh lệch đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def angleClock(self, hour: int, minutes: int) -> float:
        h = 30 * hour + 0.5 * minutes
        m = 6 * minutes
        diff = abs(h - m)
        return min(diff, 360 - diff)
```

#### Java

```java
class Solution {
    public double angleClock(int hour, int minutes) {
        double h = 30 * hour + 0.5 * minutes;
        double m = 6 * minutes;
        double diff = Math.abs(h - m);
        return Math.min(diff, 360 - diff);
    }
}
```

#### C++

```cpp
class Solution {
public:
    double angleClock(int hour, int minutes) {
        double h = 30 * hour + 0.5 * minutes;
        double m = 6 * minutes;
        double diff = abs(h - m);
        return min(diff, 360 - diff);
    }
};
```

#### Go

```go
func angleClock(hour int, minutes int) float64 {
	h := 30*float64(hour) + 0.5*float64(minutes)
	m := 6 * float64(minutes)
	diff := math.Abs(h - m)
	return math.Min(diff, 360-diff)
}
```

#### TypeScript

```ts
function angleClock(hour: number, minutes: number): number {
    const h = 30 * hour + 0.5 * minutes;
    const m = 6 * minutes;
    const diff = Math.abs(h - m);
    return Math.min(diff, 360 - diff);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
