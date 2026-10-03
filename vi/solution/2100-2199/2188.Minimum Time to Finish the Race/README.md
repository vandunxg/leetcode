---
comments: true
difficulty: Hard
rating: 2315
source: Weekly Contest 282 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2188. Minimum Time to Finish the Race](https://leetcode.com/problems/minimum-time-to-finish-the-race)

[中文文档](/solution/2100-2199/2188.Minimum%20Time%20to%20Finish%20the%20Race/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <strong>0-indexed</strong> <code>tires</code>, trong đó <code>tires[i] = [f<sub>i</sub>, r<sub>i</sub>]</code> cho biết chiếc lốp thứ <code>i<sup>th</sup></code> có thể hoàn thành vòng liên tiếp thứ <code>x<sup>th</sup></code> trong <code>f<sub>i</sub> * r<sub>i</sub><sup>(x-1)</sup></code> giây.</p>

<ul>
	<li>Ví dụ, nếu <code>f<sub>i</sub> = 3</code> và <code>r<sub>i</sub> = 2</code>, chiếc lốp sẽ hoàn thành vòng thứ <code>1<sup>st</sup></code> trong <code>3</code> giây, vòng thứ <code>2<sup>nd</sup></code> trong <code>3 * 2 = 6</code> giây, vòng thứ <code>3<sup>rd</sup></code> trong <code>3 * 2<sup>2</sup> = 12</code> giây, v.v.</li>
</ul>

<p>Bạn cũng được cho một số nguyên <code>changeTime</code> và một số nguyên <code>numLaps</code>.</p>

<p>Cuộc đua gồm <code>numLaps</code> vòng và bạn có thể bắt đầu cuộc đua với <strong>bất kỳ</strong> chiếc lốp nào. Bạn có nguồn cung <strong>không giới hạn</strong> cho mỗi loại lốp và sau mỗi vòng, bạn có thể <strong>đổi</strong> sang bất kỳ chiếc lốp nào (kể cả loại lốp hiện tại) nếu chờ <code>changeTime</code> giây.</p>

<p>Trả về<em> thời gian <strong>ít nhất</strong> để hoàn thành cuộc đua.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> tires = [[2,3],[3,4]], changeTime = 5, numLaps = 4
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong>
Vòng 1: Bắt đầu với lốp 0 và hoàn thành vòng trong 2 giây.
Vòng 2: Tiếp tục dùng lốp 0 và hoàn thành vòng trong 2 * 3 = 6 giây.
Vòng 3: Đổi sang một chiếc lốp 0 mới trong 5 giây, sau đó hoàn thành vòng trong thêm 2 giây.
Vòng 4: Tiếp tục dùng lốp 0 và hoàn thành vòng trong 2 * 3 = 6 giây.
Tổng thời gian = 2 + 6 + 5 + 2 + 6 = 21 giây.
Thời gian ít nhất để hoàn thành cuộc đua là 21 giây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tires = [[1,10],[2,2],[3,4]], changeTime = 6, numLaps = 5
<strong>Đầu ra:</strong> 25
<strong>Giải thích:</strong>
Vòng 1: Bắt đầu với lốp 1 và hoàn thành vòng trong 2 giây.
Vòng 2: Tiếp tục dùng lốp 1 và hoàn thành vòng trong 2 * 2 = 4 giây.
Vòng 3: Đổi sang một chiếc lốp 1 mới trong 6 giây, sau đó hoàn thành vòng trong thêm 2 giây.
Vòng 4: Tiếp tục dùng lốp 1 và hoàn thành vòng trong 2 * 2 = 4 giây.
Vòng 5: Đổi sang lốp 0 trong 6 giây, sau đó hoàn thành vòng trong thêm 1 giây.
Tổng thời gian = 2 + 4 + 6 + 2 + 4 + 6 + 1 = 25 giây.
Thời gian ít nhất để hoàn thành cuộc đua là 25 giây.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tires.length &lt;= 10<sup>5</sup></code></li>
	<li><code>tires[i].length == 2</code></li>
	<li><code>1 &lt;= f<sub>i</sub>, changeTime &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= r<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= numLaps &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khi dùng một chiếc lốp cho vòng liên tiếp thứ $i$, thời gian tăng theo cấp số nhân; khi thời gian cho vòng đó lớn hơn thời gian đổi lốp, ta nên dừng chuỗi vòng này. Vì vậy, độ dài chuỗi vòng hữu ích rất nhỏ (khoảng $17$). Với tối đa $10^3$ vòng, ta dùng quy hoạch động cho các điểm đổi lốp.
>
> Tính trước $\textit{cost}[i]$, là thời gian tốt nhất để chạy $i$ vòng bằng một chiếc lốp. Khi đó $f[i]$ là thời gian tốt nhất cho $i$ vòng, với chuỗi cuối cùng có độ dài $j$: $f[i]=f[i-j]+\textit{cost}[j]+\textit{changeTime}$.
>
> $f[0]=-\textit{changeTime}$ để triệt tiêu lần đổi lốp không tồn tại trước chuỗi vòng đầu tiên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumFinishTime(
        self, tires: List[List[int]], changeTime: int, numLaps: int
    ) -> int:
        cost = [inf] * 18
        for f, r in tires:
            i, s, t = 1, 0, f
            while t <= changeTime + f:
                s += t
                cost[i] = min(cost[i], s)
                t *= r
                i += 1
        f = [inf] * (numLaps + 1)
        f[0] = -changeTime
        for i in range(1, numLaps + 1):
            for j in range(1, min(18, i + 1)):
                f[i] = min(f[i], f[i - j] + cost[j])
            f[i] += changeTime
        return f[numLaps]
```

#### Java

```java
class Solution {
    public int minimumFinishTime(int[][] tires, int changeTime, int numLaps) {
        final int inf = 1 << 30;
        int[] cost = new int[18];
        Arrays.fill(cost, inf);
        for (int[] e : tires) {
            int f = e[0], r = e[1];
            int s = 0, t = f;
            for (int i = 1; t <= changeTime + f; ++i) {
                s += t;
                cost[i] = Math.min(cost[i], s);
                t *= r;
            }
        }
        int[] f = new int[numLaps + 1];
        Arrays.fill(f, inf);
        f[0] = -changeTime;
        for (int i = 1; i <= numLaps; ++i) {
            for (int j = 1; j < Math.min(18, i + 1); ++j) {
                f[i] = Math.min(f[i], f[i - j] + cost[j]);
            }
            f[i] += changeTime;
        }
        return f[numLaps];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumFinishTime(vector<vector<int>>& tires, int changeTime, int numLaps) {
        int cost[18];
        memset(cost, 0x3f, sizeof(cost));
        for (auto& e : tires) {
            int f = e[0], r = e[1];
            int s = 0;
            long long t = f;
            for (int i = 1; t <= changeTime + f; ++i) {
                s += t;
                cost[i] = min(cost[i], s);
                t *= r;
            }
        }
        int f[numLaps + 1];
        memset(f, 0x3f, sizeof(f));
        f[0] = -changeTime;
        for (int i = 1; i <= numLaps; ++i) {
            for (int j = 1; j < min(18, i + 1); ++j) {
                f[i] = min(f[i], f[i - j] + cost[j]);
            }
            f[i] += changeTime;
        }
        return f[numLaps];
    }
};
```

#### Go

```go
func minimumFinishTime(tires [][]int, changeTime int, numLaps int) int {
	const inf = 1 << 30
	cost := [18]int{}
	for i := range cost {
		cost[i] = inf
	}
	for _, e := range tires {
		f, r := e[0], e[1]
		s, t := 0, f
		for i := 1; t <= changeTime+f; i++ {
			s += t
			cost[i] = min(cost[i], s)
			t *= r
		}
	}
	f := make([]int, numLaps+1)
	for i := range f {
		f[i] = inf
	}
	f[0] = -changeTime
	for i := 1; i <= numLaps; i++ {
		for j := 1; j < min(18, i+1); j++ {
			f[i] = min(f[i], f[i-j]+cost[j])
		}
		f[i] += changeTime
	}
	return f[numLaps]
}
```

#### TypeScript

```ts
function minimumFinishTime(tires: number[][], changeTime: number, numLaps: number): number {
    const cost: number[] = Array(18).fill(Infinity);
    for (const [f, r] of tires) {
        let s = 0;
        let t = f;
        for (let i = 1; t <= changeTime + f; ++i) {
            s += t;
            cost[i] = Math.min(cost[i], s);
            t *= r;
        }
    }
    const f: number[] = Array(numLaps + 1).fill(Infinity);
    f[0] = -changeTime;
    for (let i = 1; i <= numLaps; ++i) {
        for (let j = 1; j < Math.min(18, i + 1); ++j) {
            f[i] = Math.min(f[i], f[i - j] + cost[j]);
        }
        f[i] += changeTime;
    }
    return f[numLaps];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
