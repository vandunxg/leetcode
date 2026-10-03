---
comments: true
difficulty: Hard
rating: 2235
source: Biweekly Contest 95 Q4
tags:
    - Greedy
    - Queue
    - Array
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [2528. Maximize the Minimum Powered City](https://leetcode.com/problems/maximize-the-minimum-powered-city)

[中文文档](/solution/2500-2599/2528.Maximize%20the%20Minimum%20Powered%20City/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>stations</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code>, trong đó <code>stations[i]</code> là số lượng trạm điện tại thành phố thứ <code>i<sup>th</sup></code>.</p>

<p>Mỗi trạm điện có thể cung cấp điện cho mọi thành phố trong một <strong>phạm vi</strong> cố định. Nói cách khác, nếu phạm vi được ký hiệu là <code>r</code>, thì trạm điện tại thành phố <code>i</code> có thể cung cấp điện cho mọi thành phố <code>j</code> sao cho <code>|i - j| &lt;= r</code> và <code>0 &lt;= i, j &lt;= n - 1</code>.</p>

<ul>
	<li>Lưu ý rằng <code>|x|</code> là giá trị <strong>tuyệt đối</strong>. Ví dụ, <code>|7 - 5| = 2</code> và <code>|3 - 10| = 7</code>.</li>
</ul>

<p><strong>Công suất</strong> của một thành phố là tổng số trạm điện đang cung cấp điện cho thành phố đó.</p>

<p>Chính phủ cho phép xây dựng thêm <code>k</code> trạm điện, mỗi trạm có thể được xây dựng tại bất kỳ thành phố nào và có cùng phạm vi với các trạm điện hiện có.</p>

<p>Cho hai số nguyên <code>r</code> và <code>k</code>, hãy trả về <em><strong>công suất tối thiểu lớn nhất có thể đạt được</strong> của một thành phố nếu xây dựng các trạm điện bổ sung một cách tối ưu.</em></p>

<p><strong>Lưu ý</strong> rằng có thể xây dựng <code>k</code> trạm điện tại nhiều thành phố khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stations = [1,2,4,5,0], r = 1, k = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Một cách tối ưu là xây dựng cả hai trạm điện tại thành phố 1.
Khi đó stations trở thành [1,4,4,5,0].
- Thành phố 0 nhận được điện từ 1 + 4 = 5 trạm điện.
- Thành phố 1 nhận được điện từ 1 + 4 + 4 = 9 trạm điện.
- Thành phố 2 nhận được điện từ 4 + 4 + 5 = 13 trạm điện.
- Thành phố 3 nhận được điện từ 5 + 4 = 9 trạm điện.
- Thành phố 4 nhận được điện từ 5 + 0 = 5 trạm điện.
Vì vậy, công suất nhỏ nhất của một thành phố là 5.
Do không thể đạt được công suất lớn hơn, ta trả về 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stations = [4,4,4,4], r = 0, k = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Có thể chứng minh rằng không thể làm cho công suất nhỏ nhất của một thành phố lớn hơn 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == stations.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= stations[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= r&nbsp;&lt;= n - 1</code></li>
	<li><code>0 &lt;= k&nbsp;&lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Mảng hiệu + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi trạm điện phủ một bán kính $r$, và có thể xây dựng nhiều nhất $k$ trạm bổ sung. Ta muốn tối đa hóa công suất nhỏ nhất giữa các thành phố. Việc thử các cách phân bổ là không khả thi khi $n\le 10^5$.
>
> Khả năng đạt được một mức công suất tối thiểu là đơn điệu theo $k$, nên ta tìm kiếm nhị phân một giá trị đích $x$. Trước tiên, mảng hiệu tính công suất ban đầu của từng thành phố. Khi kiểm tra, ta duyệt từ trái sang phải: nếu thành phố $i$ còn thiếu công suất để đạt $x$, ta đặt số trạm thiếu ở vị trí xa nhất về bên phải nhưng vẫn phủ được $i$, nhờ đó các thành phố phía sau cũng được hưởng lợi, đồng thời ghi nhận phép cộng trên đoạn bằng một mảng hiệu khác. Nếu vượt quá số trạm $k$ còn lại thì loại $x$.

<!-- thinking:end -->

Theo mô tả bài toán, công suất nhỏ nhất của các thành phố tăng khi giá trị của $k$ tăng. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm công suất nhỏ nhất lớn nhất, đồng thời đảm bảo số trạm điện bổ sung cần dùng không vượt quá $k$.

Đầu tiên, ta sử dụng mảng hiệu và tổng tiền tố để tính số trạm điện ban đầu tại mỗi thành phố, rồi lưu vào mảng $s$, trong đó $s[i]$ là số trạm điện tại thành phố thứ $i$.

Tiếp theo, ta đặt cận trái của tìm kiếm nhị phân là $0$ và cận phải là $2^{40}$. Sau đó, ta cài đặt hàm $check(x, k)$ để xác định liệu công suất nhỏ nhất của các thành phố có thể đạt $x$ hay không, sao cho số trạm điện bổ sung cần dùng không vượt quá $k$.

Logic cài đặt của hàm $check(x, k)$ như sau:

Duyệt qua từng thành phố. Nếu số trạm điện tại thành phố hiện tại $i$ nhỏ hơn $x$, ta tham lam xây dựng các trạm điện tại vị trí xa nhất về bên phải có thể, $j = \min(i + r, n - 1)$, để phủ được nhiều thành phố nhất. Trong quá trình này, ta có thể dùng mảng hiệu để cộng một giá trị vào một đoạn liên tiếp. Nếu số trạm điện bổ sung cần dùng vượt quá $k$, thì $x$ không thỏa mãn điều kiện và ta trả về `false`. Ngược lại, sau khi duyệt xong, ta trả về `true`.

Độ phức tạp thời gian là $O(n \times \log M)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số thành phố và $M$ được cố định ở $2^{40}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPower(self, stations: List[int], r: int, k: int) -> int:
        def check(x, k):
            d = [0] * (n + 1)
            t = 0
            for i in range(n):
                t += d[i]
                dist = x - (s[i] + t)
                if dist > 0:
                    if k < dist:
                        return False
                    k -= dist
                    j = min(i + r, n - 1)
                    left, right = max(0, j - r), min(j + r, n - 1)
                    d[left] += dist
                    d[right + 1] -= dist
                    t += dist
            return True

        n = len(stations)
        d = [0] * (n + 1)
        for i, v in enumerate(stations):
            left, right = max(0, i - r), min(i + r, n - 1)
            d[left] += v
            d[right + 1] -= v
        s = list(accumulate(d))
        left, right = 0, 1 << 40
        while left < right:
            mid = (left + right + 1) >> 1
            if check(mid, k):
                left = mid
            else:
                right = mid - 1
        return left
```

#### Java

```java
class Solution {
    private long[] s;
    private long[] d;
    private int n;

    public long maxPower(int[] stations, int r, int k) {
        n = stations.length;
        d = new long[n + 1];
        s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            int left = Math.max(0, i - r), right = Math.min(i + r, n - 1);
            d[left] += stations[i];
            d[right + 1] -= stations[i];
        }
        s[0] = d[0];
        for (int i = 1; i < n + 1; ++i) {
            s[i] = s[i - 1] + d[i];
        }
        long left = 0, right = 1l << 40;
        while (left < right) {
            long mid = (left + right + 1) >>> 1;
            if (check(mid, r, k)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

    private boolean check(long x, int r, int k) {
        Arrays.fill(d, 0);
        long t = 0;
        for (int i = 0; i < n; ++i) {
            t += d[i];
            long dist = x - (s[i] + t);
            if (dist > 0) {
                if (k < dist) {
                    return false;
                }
                k -= dist;
                int j = Math.min(i + r, n - 1);
                int left = Math.max(0, j - r), right = Math.min(j + r, n - 1);
                d[left] += dist;
                d[right + 1] -= dist;
                t += dist;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxPower(vector<int>& stations, int r, int k) {
        int n = stations.size();
        long long d[n + 1];
        memset(d, 0, sizeof d);
        for (int i = 0; i < n; ++i) {
            int left = max(0, i - r), right = min(i + r, n - 1);
            d[left] += stations[i];
            d[right + 1] -= stations[i];
        }
        long long s[n + 1];
        s[0] = d[0];
        for (int i = 1; i < n + 1; ++i) {
            s[i] = s[i - 1] + d[i];
        }
        auto check = [&](long long x, int k) {
            memset(d, 0, sizeof d);
            long long t = 0;
            for (int i = 0; i < n; ++i) {
                t += d[i];
                long long dist = x - (s[i] + t);
                if (dist > 0) {
                    if (k < dist) {
                        return false;
                    }
                    k -= dist;
                    int j = min(i + r, n - 1);
                    int left = max(0, j - r), right = min(j + r, n - 1);
                    d[left] += dist;
                    d[right + 1] -= dist;
                    t += dist;
                }
            }
            return true;
        };
        long long left = 0, right = 1e12;
        while (left < right) {
            long long mid = (left + right + 1) >> 1;
            if (check(mid, k)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func maxPower(stations []int, r int, k int) int64 {
	n := len(stations)
	d := make([]int, n+1)
	s := make([]int, n+1)
	for i, v := range stations {
		left, right := max(0, i-r), min(i+r, n-1)
		d[left] += v
		d[right+1] -= v
	}
	s[0] = d[0]
	for i := 1; i < n+1; i++ {
		s[i] = s[i-1] + d[i]
	}
	check := func(x, k int) bool {
		d := make([]int, n+1)
		t := 0
		for i := range stations {
			t += d[i]
			dist := x - (s[i] + t)
			if dist > 0 {
				if k < dist {
					return false
				}
				k -= dist
				j := min(i+r, n-1)
				left, right := max(0, j-r), min(j+r, n-1)
				d[left] += dist
				d[right+1] -= dist
				t += dist
			}
		}
		return true
	}
	left, right := 0, 1<<40
	for left < right {
		mid := (left + right + 1) >> 1
		if check(mid, k) {
			left = mid
		} else {
			right = mid - 1
		}
	}
	return int64(left)
}
```

#### TypeScript

```ts
function maxPower(stations: number[], r: number, k: number): number {
    function check(x: bigint, k: bigint): boolean {
        d.fill(0n);
        let t = 0n;
        for (let i = 0; i < n; ++i) {
            t += d[i];
            const dist = x - (s[i] + t);
            if (dist > 0) {
                if (k < dist) {
                    return false;
                }
                k -= dist;
                const j = Math.min(i + r, n - 1);
                const left = Math.max(0, j - r);
                const right = Math.min(j + r, n - 1);
                d[left] += dist;
                d[right + 1] -= dist;
                t += dist;
            }
        }
        return true;
    }
    const n = stations.length;
    const d: bigint[] = new Array(n + 1).fill(0n);
    const s: bigint[] = new Array(n + 1).fill(0n);

    for (let i = 0; i < n; ++i) {
        const left = Math.max(0, i - r);
        const right = Math.min(i + r, n - 1);
        d[left] += BigInt(stations[i]);
        d[right + 1] -= BigInt(stations[i]);
    }

    s[0] = d[0];
    for (let i = 1; i < n + 1; ++i) {
        s[i] = s[i - 1] + d[i];
    }

    let left = 0n,
        right = 1n << 40n;
    while (left < right) {
        const mid = (left + right + 1n) >> 1n;
        if (check(mid, BigInt(k))) {
            left = mid;
        } else {
            right = mid - 1n;
        }
    }
    return Number(left);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_power(stations: Vec<i32>, r: i32, k: i32) -> i64 {
        let n = stations.len();
        let mut d = vec![0i64; n + 2];
        for i in 0..n {
            let left = i.saturating_sub(r as usize);
            let right = (i + r as usize).min(n - 1);
            d[left] += stations[i] as i64;
            d[right + 1] -= stations[i] as i64;
        }

        let mut s = vec![0i64; n + 1];
        s[0] = d[0];
        for i in 1..=n {
            s[i] = s[i - 1] + d[i];
        }

        let check = |x: i64, mut k: i64| -> bool {
            let mut d = vec![0i64; n + 2];
            let mut t = 0i64;
            for i in 0..n {
                t += d[i];
                let dist = x - (s[i] + t);
                if dist > 0 {
                    if k < dist {
                        return false;
                    }
                    k -= dist;
                    let j = (i + r as usize).min(n - 1);
                    let left = j.saturating_sub(r as usize);
                    let right = (j + r as usize).min(n - 1);
                    d[left] += dist;
                    d[right + 1] -= dist;
                    t += dist;
                }
            }
            true
        };

        let (mut left, mut right) = (0i64, 1_000_000_000_000i64);
        while left < right {
            let mid = (left + right + 1) >> 1;
            if check(mid, k as i64) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        left
    }
}
```

#### C#

```cs
public class Solution {
    private long[] s;
    private long[] d;
    private int n;

    public long MaxPower(int[] stations, int r, int k) {
        n = stations.Length;
        d = new long[n + 1];
        s = new long[n + 1];

        for (int i = 0; i < n; ++i) {
            int left = Math.Max(0, i - r);
            int right = Math.Min(i + r, n - 1);
            d[left] += stations[i];
            d[right + 1] -= stations[i];
        }

        s[0] = d[0];
        for (int i = 1; i < n + 1; ++i) {
            s[i] = s[i - 1] + d[i];
        }

        long leftBound = 0, rightBound = 1L << 40;
        while (leftBound < rightBound) {
            long mid = (leftBound + rightBound + 1) >> 1;
            if (Check(mid, r, k)) {
                leftBound = mid;
            } else {
                rightBound = mid - 1;
            }
        }

        return leftBound;
    }

    private bool Check(long x, int r, long k) {
        Array.Fill(d, 0L);
        long t = 0;

        for (int i = 0; i < n; ++i) {
            t += d[i];
            long dist = x - (s[i] + t);
            if (dist > 0) {
                if (k < dist) {
                    return false;
                }
                k -= dist;
                int j = Math.Min(i + r, n - 1);
                int left = Math.Max(0, j - r);
                int right = Math.Min(j + r, n - 1);
                d[left] += dist;
                d[right + 1] -= dist;
                t += dist;
            }
        }

        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
