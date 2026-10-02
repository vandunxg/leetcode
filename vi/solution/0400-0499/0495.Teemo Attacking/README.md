---
comments: true
difficulty: Easy
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [495. Teemo Attacking](https://leetcode.com/problems/teemo-attacking)

[中文文档](/solution/0400-0499/0495.Teemo%20Attacking/README.md)

## Mô tả

<!-- description:start -->

<p>Teemo tấn công Ashe bằng độc. Mỗi lần bị Teemo tấn công, Ashe sẽ trúng độc trong đúng <code>duration</code> giây. Cụ thể, nếu bị tấn công tại giây <code>t</code>, Ashe sẽ trúng độc trong khoảng thời gian <strong>bao gồm cả hai đầu mút</strong> <code>[t, t + duration - 1]</code>. Nếu Teemo tấn công lần nữa <strong>trước khi</strong> hiệu ứng độc kết thúc, thời gian sẽ được <strong>đặt lại</strong>, và hiệu ứng độc kết thúc sau lần tấn công mới <code>duration</code> giây.</p>

<p>Cho mảng số nguyên <strong>không giảm</strong> <code>timeSeries</code>, trong đó <code>timeSeries[i]</code> cho biết Teemo tấn công Ashe tại giây <code>timeSeries[i]</code>, cùng số nguyên <code>duration</code>.</p>

<p>Hãy trả về <em><strong>tổng</strong> số giây Ashe bị trúng độc</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> timeSeries = [1,4], duration = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các lần Teemo tấn công Ashe diễn ra như sau:
- Tại giây thứ 1, Teemo tấn công và Ashe bị trúng độc trong giây thứ 1 và 2.
- Tại giây thứ 4, Teemo tấn công và Ashe bị trúng độc trong giây thứ 4 và 5.
Ashe bị trúng độc trong các giây thứ 1, 2, 4 và 5, tổng cộng 4 giây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> timeSeries = [1,2], duration = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các lần Teemo tấn công Ashe diễn ra như sau:
- Tại giây thứ 1, Teemo tấn công và Ashe bị trúng độc trong giây thứ 1 và 2.
- Tuy nhiên, tại giây thứ 2, Teemo tấn công lần nữa và đặt lại thời gian hiệu ứng độc. Ashe bị trúng độc trong giây thứ 2 và 3.
Ashe bị trúng độc trong các giây thứ 1, 2 và 3, tổng cộng 3 giây.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= timeSeries.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= timeSeries[i], duration &lt;= 10<sup>7</sup></code></li>
	<li><code>timeSeries</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần tấn công làm mới hiệu ứng độc trong $\textit{duration}$ giây; khoảng thời gian chồng lấp chỉ được tính một lần. Không cần mô phỏng từng giây.
>
> Lần tấn công cuối cùng luôn đóng góp đủ $\textit{duration}$ giây. Với hai lần tấn công liên tiếp, lần trước đóng góp khoảng cách giữa chúng nếu khoảng cách đó nhỏ hơn $\textit{duration}$; nếu không thì đóng góp đủ $\textit{duration}$ giây.
>
> Cộng $\min(\textit{duration},b-a)$ cho từng cặp lần tấn công liên tiếp sẽ tính toàn bộ thời gian trong một lượt duyệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPoisonedDuration(self, timeSeries: List[int], duration: int) -> int:
        ans = duration
        for a, b in pairwise(timeSeries):
            ans += min(duration, b - a)
        return ans
```

#### Java

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        int n = timeSeries.length;
        int ans = duration;
        for (int i = 1; i < n; ++i) {
            ans += Math.min(duration, timeSeries[i] - timeSeries[i - 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        int ans = duration;
        int n = timeSeries.size();
        for (int i = 1; i < n; ++i) {
            ans += min(duration, timeSeries[i] - timeSeries[i - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func findPoisonedDuration(timeSeries []int, duration int) (ans int) {
	ans = duration
	for i, x := range timeSeries[1:] {
		ans += min(duration, x-timeSeries[i])
	}
	return
}
```

#### TypeScript

```ts
function findPoisonedDuration(timeSeries: number[], duration: number): number {
    const n = timeSeries.length;
    let ans = duration;
    for (let i = 1; i < n; ++i) {
        ans += Math.min(duration, timeSeries[i] - timeSeries[i - 1]);
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int FindPoisonedDuration(int[] timeSeries, int duration) {
        int ans = duration;
        int n = timeSeries.Length;
        for (int i = 1; i < n; ++i) {
            ans += Math.Min(duration, timeSeries[i] - timeSeries[i - 1]);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
