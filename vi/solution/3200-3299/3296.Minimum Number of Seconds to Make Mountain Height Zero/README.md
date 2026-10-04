---
comments: true
difficulty: Medium
rating: 1694
source: Weekly Contest 416 Q2
tags:
    - Greedy
    - Array
    - Math
    - Binary Search
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3296. Minimum Number of Seconds to Make Mountain Height Zero](https://leetcode.com/problems/minimum-number-of-seconds-to-make-mountain-height-zero)

[中文文档](/solution/3200-3299/3296.Minimum%20Number%20of%20Seconds%20to%20Make%20Mountain%20Height%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>mountainHeight</code> biểu thị chiều cao của một ngọn núi.</p>

<p>Đồng thời, bạn được cho một mảng số nguyên <code>workerTimes</code> biểu thị thời gian làm việc của các worker tính bằng <strong>giây</strong>.</p>

<p data-end="203" data-start="76">Mỗi worker có thể giảm chiều cao của ngọn núi một lượng <strong>nguyên không âm</strong> bất kỳ. Nếu worker <code data-end="170" data-start="167">i</code> giảm chiều cao đi <code data-end="196" data-start="193">x</code>, thì:</p>

<ul data-end="415" data-start="208">
	<li data-end="275" data-section-id="66oopy" data-start="208">giảm đơn vị chiều cao đầu tiên mất <code data-end="266" data-start="250">workerTimes[i]</code> giây,</li>
	<li data-end="340" data-section-id="9o9grm" data-start="278">giảm đơn vị thứ hai mất <code data-end="331" data-start="311">workerTimes[i] * 2</code> giây,</li>
	<li data-end="348" data-section-id="1o23ba" data-start="343">...</li>
	<li data-end="413" data-section-id="1brl21f" data-start="351">giảm đơn vị thứ <code data-end="369" data-start="366">x</code> mất <code data-end="404" data-start="384">workerTimes[i] * x</code> giây.</li>
</ul>

<p data-end="516" data-start="418">Tổng thời gian worker <code data-end="452" data-start="449">i</code> cần là tổng thời gian để giảm tất cả <code data-end="497" data-start="494">x</code> đơn vị mà worker đó thực hiện.&nbsp;Vì tất cả worker làm việc đồng thời, tổng thời gian cần thiết là thời gian <strong>lớn nhất</strong> mà bất kỳ worker nào cần.</p>

<p>Trả về một số nguyên biểu thị <strong>thời gian tối thiểu</strong> tính bằng giây để các worker đưa chiều cao ngọn núi về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mountainHeight = 4, workerTimes = [2,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách để giảm chiều cao ngọn núi về 0 là:</p>

<ul>
	<li>Worker 0 giảm chiều cao đi 1, mất <code>workerTimes[0] = 2</code> giây.</li>
	<li>Worker 1 giảm chiều cao đi 2, mất <code>workerTimes[1] + workerTimes[1] * 2 = 3</code> giây.</li>
	<li>Worker 2 giảm chiều cao đi 1, mất <code>workerTimes[2] = 1</code> giây.</li>
</ul>

<p>Vì họ làm việc đồng thời, thời gian tối thiểu cần thiết là <code>max(2, 3, 1) = 3</code> giây.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mountainHeight = 10, workerTimes = [3,2,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Worker 0 giảm chiều cao đi 2, mất <code>workerTimes[0] + workerTimes[0] * 2 = 9</code> giây.</li>
	<li>Worker 1 giảm chiều cao đi 3, mất <code>workerTimes[1] + workerTimes[1] * 2 + workerTimes[1] * 3 = 12</code> giây.</li>
	<li>Worker 2 giảm chiều cao đi 3, mất <code>workerTimes[2] + workerTimes[2] * 2 + workerTimes[2] * 3 = 12</code> giây.</li>
	<li>Worker 3 giảm chiều cao đi 2, mất <code>workerTimes[3] + workerTimes[3] * 2 = 12</code> giây.</li>
</ul>

<p>Thời gian cần thiết là <code>max(9, 12, 12, 12) = 12</code> giây.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mountainHeight = 5, workerTimes = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong ví dụ này chỉ có một worker, nên đáp án là <code>workerTimes[0] + workerTimes[0] * 2 + workerTimes[0] * 3 + workerTimes[0] * 4 + workerTimes[0] * 5 = 15</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= mountainHeight &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= workerTimes.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= workerTimes[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Worker $i$ cần $wt_i\cdot h(h+1)/2$ để loại bỏ $h$ lớp; các worker làm việc song song. $H\le 10^5$ và $10^4$ worker khiến việc duyệt qua các mốc thời gian là không khả thi. Thời gian càng lớn thì chiều cao được loại bỏ càng nhiều, vì vậy ta dùng tìm kiếm nhị phân.
>
> $\textit{check}(t)$ giải phương trình bậc hai cho số lớp mỗi worker có thể loại bỏ trong $t$, rồi cộng các giá trị đó. Cận trên của phép tìm kiếm là một hằng số lớn, chẳng hạn $10^{16}$; `bisect_left` trả về $t$ nhỏ nhất thỏa mãn.

<!-- thinking:end -->

Ta nhận thấy nếu tất cả worker có thể đưa chiều cao ngọn núi về $0$ trong $t$ giây, thì với mọi $t' > t$, các worker cũng có thể đưa chiều cao ngọn núi về $0$ trong $t'$ giây. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm $t$ nhỏ nhất sao cho các worker có thể đưa chiều cao ngọn núi về $0$ trong $t$ giây.

Ta định nghĩa hàm $\textit{check}(t)$, cho biết liệu các worker có thể đưa chiều cao ngọn núi về $0$ trong $t$ giây hay không. Cụ thể, ta duyệt qua từng worker. Với worker hiện tại có $\textit{workerTimes}[i]$, giả sử worker đó giảm chiều cao đi $h'$ trong $t$ giây, ta có bất đẳng thức:

$$
\left(1 + h'\right) \cdot \frac{h'}{2} \cdot \textit{workerTimes}[i] \leq t
$$

Giải bất đẳng thức, ta được:

$$
h' \leq \left\lfloor \sqrt{\frac{2t}{\textit{workerTimes}[i]} + \frac{1}{4}} - \frac{1}{2} \right\rfloor
$$

Ta có thể cộng tất cả giá trị $h'$ của các worker để nhận được tổng chiều cao $h$ đã giảm. Nếu $h \geq \textit{mountainHeight}$, điều đó có nghĩa là các worker có thể đưa chiều cao ngọn núi về $0$ trong $t$ giây.

Tiếp theo, ta xác định cận trái của phép tìm kiếm nhị phân là $l = 1$. Vì có ít nhất một worker và thời gian làm việc của mỗi worker không vượt quá $10^6$, để đưa chiều cao ngọn núi về $0$, cần ít nhất $(1 + \textit{mountainHeight}) \cdot \textit{mountainHeight} / 2 \cdot \textit{workerTimes}[i] \leq 10^{16}$ giây. Do đó, ta có thể đặt cận phải của phép tìm kiếm nhị phân là $r = 10^{16}$. Sau đó, ta liên tục chia đôi đoạn $[l, r]$ cho đến khi $l = r$. Khi đó, $l$ là đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là số worker và $M$ là cận phải của phép tìm kiếm nhị phân, bằng $10^{16}$ trong bài này. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumberOfSeconds(self, mountainHeight: int, workerTimes: List[int]) -> int:
        def check(t: int) -> bool:
            h = 0
            for wt in workerTimes:
                h += int(sqrt(2 * t / wt + 1 / 4) - 1 / 2)
            return h >= mountainHeight

        return bisect_left(range(10**16), True, key=check)
```

#### Java

```java
class Solution {
    private int mountainHeight;
    private int[] workerTimes;

    public long minNumberOfSeconds(int mountainHeight, int[] workerTimes) {
        this.mountainHeight = mountainHeight;
        this.workerTimes = workerTimes;
        long l = 1, r = (long) 1e16;
        while (l < r) {
            long mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(long t) {
        long h = 0;
        for (int wt : workerTimes) {
            h += (long) (Math.sqrt(t * 2.0 / wt + 0.25) - 0.5);
        }
        return h >= mountainHeight;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minNumberOfSeconds(int mountainHeight, vector<int>& workerTimes) {
        using ll = long long;
        ll l = 1, r = 1e16;
        auto check = [&](ll t) -> bool {
            ll h = 0;
            for (int& wt : workerTimes) {
                h += (long long) (sqrt(t * 2.0 / wt + 0.25) - 0.5);
            }
            return h >= mountainHeight;
        };
        while (l < r) {
            ll mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minNumberOfSeconds(mountainHeight int, workerTimes []int) int64 {
	return int64(sort.Search(1e16, func(t int) bool {
		var h int64
		for _, wt := range workerTimes {
			h += int64(math.Sqrt(float64(t)*2.0/float64(wt)+0.25) - 0.5)
		}
		return h >= int64(mountainHeight)
	}))
}
```

#### TypeScript

```ts
function minNumberOfSeconds(mountainHeight: number, workerTimes: number[]): number {
    const check = (t: bigint): boolean => {
        let h = BigInt(0);
        for (const wt of workerTimes) {
            h += BigInt(Math.floor(Math.sqrt((Number(t) * 2.0) / wt + 0.25) - 0.5));
        }
        return h >= BigInt(mountainHeight);
    };

    let l = BigInt(1);
    let r = BigInt(1e16);

    while (l < r) {
        const mid = (l + r) >> BigInt(1);
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1n;
        }
    }

    return Number(l);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_number_of_seconds(mountain_height: i32, worker_times: Vec<i32>) -> i64 {
        let mut l: i64 = 1;
        let mut r: i64 = 10_i64.pow(16);

        let check = |t: i64| -> bool {
            let mut h: i64 = 0;
            for &wt in &worker_times {
                let wt = wt as f64;
                let t_f = t as f64;
                let val = ((t_f * 2.0 / wt + 0.25).sqrt() - 0.5).floor() as i64;
                h += val;
                if h >= mountain_height as i64 {
                    return true;
                }
            }
            h >= mountain_height as i64
        };

        while l < r {
            let mid = (l + r) >> 1;
            if check(mid) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }

        l
    }
}
```

#### JavaScript

```js
/**
 * @param {number} mountainHeight
 * @param {number[]} workerTimes
 * @return {number}
 */
var minNumberOfSeconds = function (mountainHeight, workerTimes) {
    const check = t => {
        let h = 0n;
        for (const wt of workerTimes) {
            h += BigInt(Math.floor(Math.sqrt((Number(t) * 2.0) / wt + 0.25) - 0.5));
        }
        return h >= BigInt(mountainHeight);
    };

    let l = 1n;
    let r = 10000000000000000n;

    while (l < r) {
        const mid = (l + r) >> 1n;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1n;
        }
    }

    return Number(l);
};
```

#### C#

```cs
public class Solution {
    public long MinNumberOfSeconds(int mountainHeight, int[] workerTimes) {
        long l = 1, r = (long)1e16;

        bool Check(long t) {
            long h = 0;
            foreach (int wt in workerTimes) {
                long val = (long)(Math.Sqrt(t * 2.0 / wt + 0.25) - 0.5);
                h += val;
                if (h >= mountainHeight) {
                    return true;
                }
            }
            return h >= mountainHeight;
        }

        while (l < r) {
            long mid = (l + r) >> 1;
            if (Check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }

        return l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
