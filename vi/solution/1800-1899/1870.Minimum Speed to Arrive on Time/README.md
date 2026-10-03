---
comments: true
difficulty: Medium
rating: 1675
source: Weekly Contest 242 Q2
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1870. Minimum Speed to Arrive on Time](https://leetcode.com/problems/minimum-speed-to-arrive-on-time)

[中文文档](/solution/1800-1899/1870.Minimum%20Speed%20to%20Arrive%20on%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số thực <code>hour</code>, biểu thị khoảng thời gian bạn có để đến văn phòng. Để đi đến văn phòng, bạn phải đi <code>n</code> chuyến tàu theo đúng thứ tự. Bạn cũng được cho một mảng số nguyên <code>dist</code> có độ dài <code>n</code>, trong đó <code>dist[i]</code> biểu thị quãng đường (tính bằng kilômét) của chuyến tàu thứ <code>i<sup>th</sup></code>.</p>

<p>Mỗi chuyến tàu chỉ có thể khởi hành vào một giờ nguyên, vì vậy bạn có thể phải chờ giữa các chuyến.</p>

<ul>
	<li>Ví dụ, nếu chuyến tàu <code>1<sup>st</sup></code> mất <code>1.5</code> giờ, bạn phải chờ thêm <code>0.5</code> giờ để có thể lên chuyến tàu <code>2<sup>nd</sup></code> vào thời điểm 2 giờ.</li>
</ul>

<p>Trả về <em>vận tốc là <strong>số nguyên dương nhỏ nhất</strong> <strong>(tính bằng kilômét trên giờ)</strong> mà tất cả các chuyến tàu phải chạy để bạn đến văn phòng đúng giờ, hoặc </em><code>-1</code><em> nếu không thể đến đúng giờ</em>.</p>

<p>Dữ liệu kiểm thử được tạo sao cho đáp án không vượt quá <code>10<sup>7</sup></code> và <code>hour</code> có <strong>nhiều nhất hai chữ số sau dấu thập phân</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [1,3,2], hour = 6
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>Với vận tốc 1:
- Chuyến tàu đầu tiên mất 1/1 = 1 giờ.
- Vì đang ở một giờ nguyên, ta khởi hành ngay vào thời điểm 1 giờ. Chuyến tàu thứ hai mất 3/1 = 3 giờ.
- Vì đang ở một giờ nguyên, ta khởi hành ngay vào thời điểm 4 giờ. Chuyến tàu thứ ba mất 2/1 = 2 giờ.
- Ta sẽ đến nơi đúng vào thời điểm 6 giờ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [1,3,2], hour = 2.7
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Với vận tốc 3:
- Chuyến tàu đầu tiên mất 1/3 = 0.33333 giờ.
- Vì chưa đến một giờ nguyên, ta chờ đến thời điểm 1 giờ mới khởi hành. Chuyến tàu thứ hai mất 3/3 = 1 giờ.
- Vì đang ở một giờ nguyên, ta khởi hành ngay vào thời điểm 2 giờ. Chuyến tàu thứ ba mất 2/3 = 0.66667 giờ.
- Ta sẽ đến nơi vào thời điểm 2.66667 giờ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [1,3,2], hour = 1.9
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể đến đúng giờ vì chuyến tàu thứ ba sớm nhất chỉ có thể khởi hành vào thời điểm 2 giờ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == dist.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= dist[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= hour &lt;= 10<sup>9</sup></code></li>
	<li><code>hour</code> có nhiều nhất hai chữ số sau dấu thập phân.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Tất cả chuyến đi trừ chuyến cuối đều được làm tròn lên; ta cần vận tốc nguyên nhỏ nhất để hoàn thành trong $hour$. Vận tốc lớn hơn chỉ có lợi. Nếu số chuyến tàu lớn hơn $\lceil hour\rceil$, thì ngay cả mỗi chuyến mất một giờ cũng vẫn quá chậm.
>
> Tìm kiếm nhị phân vận tốc trong $[1,10^7]$: tính tổng $d/v$ (làm tròn lên trừ chuyến cuối) rồi so sánh với $hour$. Trả về $-1$ nếu không có vận tốc nào phù hợp.

<!-- thinking:end -->

Ta nhận thấy nếu vận tốc $v$ cho phép đến nơi trong thời gian quy định, thì với mọi $v' > v$, ta chắc chắn cũng đến nơi trong thời gian đó. Đây là tính đơn điệu, vì vậy ta có thể dùng tìm kiếm nhị phân để tìm vận tốc nhỏ nhất thỏa mãn điều kiện.

Trước khi tìm kiếm nhị phân, trước hết cần xác định liệu có thể đến nơi trong thời gian quy định hay không. Nếu số chuyến tàu lớn hơn giá trị làm tròn lên của thời gian quy định, thì chắc chắn không thể đến nơi đúng hạn, và ta trả về trực tiếp $-1$.

Tiếp theo, ta đặt biên trái và phải của tìm kiếm nhị phân lần lượt là $l = 1$, $r = 10^7 + 1$, rồi mỗi lần lấy giá trị giữa $\textit{mid} = \frac{l + r}{2}$ để kiểm tra điều kiện. Nếu thỏa mãn, ta đưa biên phải về $\textit{mid}$; ngược lại, ta đưa biên trái về $\textit{mid} + 1$.

Bài toán được chuyển thành việc xác định liệu vận tốc $v$ có cho phép ta đến nơi trong thời gian quy định hay không. Ta duyệt từng chuyến tàu, tính thời gian đi mỗi chuyến $t = \frac{d}{v}$; nếu là chuyến cuối thì cộng trực tiếp $t$, còn không thì làm tròn lên rồi cộng $t$. Cuối cùng, ta kiểm tra tổng thời gian có nhỏ hơn hoặc bằng thời gian quy định hay không; nếu có thì điều kiện được thỏa mãn.

Sau khi kết thúc tìm kiếm nhị phân, nếu biên trái lớn hơn $10^7$, nghĩa là ta không thể đến nơi trong thời gian quy định và trả về $-1$; ngược lại, trả về biên trái.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là số chuyến tàu và cận trên của vận tốc. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSpeedOnTime(self, dist: List[int], hour: float) -> int:
        def check(v: int) -> bool:
            s = 0
            for i, d in enumerate(dist):
                t = d / v
                s += t if i == len(dist) - 1 else ceil(t)
            return s <= hour

        if len(dist) > ceil(hour):
            return -1
        r = 10**7 + 1
        ans = bisect_left(range(1, r), True, key=check) + 1
        return -1 if ans == r else ans
```

#### Java

```java
class Solution {
    public int minSpeedOnTime(int[] dist, double hour) {
        if (dist.length > Math.ceil(hour)) {
            return -1;
        }
        final int m = (int) 1e7;
        int l = 1, r = m + 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(dist, mid, hour)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > m ? -1 : l;
    }

    private boolean check(int[] dist, int v, double hour) {
        double s = 0;
        int n = dist.length;
        for (int i = 0; i < n; ++i) {
            double t = dist[i] * 1.0 / v;
            s += i == n - 1 ? t : Math.ceil(t);
        }
        return s <= hour;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSpeedOnTime(vector<int>& dist, double hour) {
        if (dist.size() > ceil(hour)) {
            return -1;
        }
        const int m = 1e7;
        int l = 1, r = m + 1;
        int n = dist.size();
        auto check = [&](int v) {
            double s = 0;
            for (int i = 0; i < n; ++i) {
                double t = dist[i] * 1.0 / v;
                s += i == n - 1 ? t : ceil(t);
            }
            return s <= hour;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > m ? -1 : l;
    }
};
```

#### Go

```go
func minSpeedOnTime(dist []int, hour float64) int {
	if float64(len(dist)) > math.Ceil(hour) {
		return -1
	}
	const m int = 1e7
	n := len(dist)
	ans := sort.Search(m+1, func(v int) bool {
		v++
		s := 0.0
		for i, d := range dist {
			t := float64(d) / float64(v)
			if i == n-1 {
				s += t
			} else {
				s += math.Ceil(t)
			}
		}
		return s <= hour
	}) + 1
	if ans > m {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minSpeedOnTime(dist: number[], hour: number): number {
    if (dist.length > Math.ceil(hour)) {
        return -1;
    }
    const n = dist.length;
    const m = 10 ** 7;
    const check = (v: number): boolean => {
        let s = 0;
        for (let i = 0; i < n; ++i) {
            const t = dist[i] / v;
            s += i === n - 1 ? t : Math.ceil(t);
        }
        return s <= hour;
    };
    let [l, r] = [1, m + 1];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l > m ? -1 : l;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_speed_on_time(dist: Vec<i32>, hour: f64) -> i32 {
        if dist.len() as f64 > hour.ceil() {
            return -1;
        }
        const M: i32 = 10_000_000;
        let (mut l, mut r) = (1, M + 1);
        let n = dist.len();
        let check = |v: i32| -> bool {
            let mut s = 0.0;
            for i in 0..n {
                let t = dist[i] as f64 / v as f64;
                s += if i == n - 1 { t } else { t.ceil() };
            }
            s <= hour
        };
        while l < r {
            let mid = (l + r) / 2;
            if check(mid) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        if l > M {
            -1
        } else {
            l
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} dist
 * @param {number} hour
 * @return {number}
 */
var minSpeedOnTime = function (dist, hour) {
    if (dist.length > Math.ceil(hour)) {
        return -1;
    }
    const n = dist.length;
    const m = 10 ** 7;
    const check = v => {
        let s = 0;
        for (let i = 0; i < n; ++i) {
            const t = dist[i] / v;
            s += i === n - 1 ? t : Math.ceil(t);
        }
        return s <= hour;
    };
    let [l, r] = [1, m + 1];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l > m ? -1 : l;
};
```

#### Kotlin

```kotlin
class Solution {
    fun minSpeedOnTime(dist: IntArray, hour: Double): Int {
        val n = dist.size
        if (n > Math.ceil(hour)) {
            return -1
        }
        val m = 1e7.toInt()
        var left = 1
        var right = m + 1
        while (left < right) {
            val middle = (left + right) / 2
            var time = 0.0
            dist.forEachIndexed { i, item ->
                val t = item.toDouble() / middle
                time += if (i == n - 1) t else Math.ceil(t)
            }
            if (time > hour) {
                left = middle + 1
            } else {
                right = middle
            }
        }
        return if (left > m) -1 else left
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
