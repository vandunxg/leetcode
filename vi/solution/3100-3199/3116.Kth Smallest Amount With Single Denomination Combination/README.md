---
comments: true
difficulty: Hard
rating: 2387
source: Weekly Contest 393 Q3
tags:
    - Bit Manipulation
    - Array
    - Math
    - Binary Search
    - Combinatorics
    - Number Theory
---

<!-- problem:start -->

# [3116. Kth Smallest Amount With Single Denomination Combination](https://leetcode.com/problems/kth-smallest-amount-with-single-denomination-combination)

[中文文档](/solution/3100-3199/3116.Kth%20Smallest%20Amount%20With%20Single%20Denomination%20Combination/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>coins</code> biểu diễn các đồng xu có mệnh giá khác nhau và một số nguyên <code>k</code>.</p>

<p>Bạn có vô hạn đồng xu ở mỗi mệnh giá. Tuy nhiên, <strong>không được phép</strong> kết hợp các đồng xu khác mệnh giá.</p>

<p>Hãy trả về số tiền <code>k<sup>th</sup></code> <strong>nhỏ nhất</strong> có thể tạo ra bằng các đồng xu này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">coins = [3,6,9], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
"> 9</span></p>

<p><strong>Giải thích:</strong> Các đồng xu đã cho có thể tạo ra các số tiền sau:<br />
Đồng xu 3 tạo ra các bội của 3: 3, 6, 9, 12, 15, v.v.<br />
Đồng xu 6 tạo ra các bội của 6: 6, 12, 18, 24, v.v.<br />
Đồng xu 9 tạo ra các bội của 9: 9, 18, 27, 36, v.v.<br />
Tổng hợp tất cả các đồng xu, ta có: 3, 6, <u><strong>9</strong></u>, 12, 15, v.v.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong><span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
"> coins = [5,2], k = 7</span></p>

<p><strong>Đầu ra:</strong><span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
"> 12 </span></p>

<p><strong>Giải thích:</strong> Các đồng xu đã cho có thể tạo ra các số tiền sau:<br />
Đồng xu 5 tạo ra các bội của 5: 5, 10, 15, 20, v.v.<br />
Đồng xu 2 tạo ra các bội của 2: 2, 4, 6, 8, 10, 12, v.v.<br />
Tổng hợp tất cả các đồng xu, ta có: 2, 4, 5, 6, 8, 10, <u><strong>12</strong></u>, 14, 15, v.v.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= coins.length &lt;= 15</code></li>
	<li><code>1 &lt;= coins[i] &lt;= 25</code></li>
	<li><code>1 &lt;= k &lt;= 2 * 10<sup>9</sup></code></li>
	<li><code>coins</code> chứa các số nguyên đôi một khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Nguyên lý bao hàm-loại trừ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi số tiền là một bội dương của duy nhất một đồng xu. Không thể sinh các bội theo thứ tự: $k$ có thể là $10^{15}$ và bội chung nhỏ nhất có thể khiến dãy trở nên rất lớn.
>
> Số lượng số tiền hợp lệ không vượt quá $x$ là hàm đơn điệu theo $x$, nên ta tìm kiếm nhị phân giá trị $x$ nhỏ nhất sao cho số lượng này ít nhất là $k$. Số lượng được tính bằng nguyên lý bao hàm-loại trừ trên các bội chung nhỏ nhất của các tập con đồng xu.
>
> Có nhiều nhất $15$ đồng xu, nên có thể liệt kê mọi tập con và cộng $\lfloor x/\mathrm{lcm}\rfloor$ với dấu phù hợp. Tìm kiếm trong khoảng đến khoảng $10^{11}$ sẽ cho số tiền thứ $k$.

<!-- thinking:end -->

Ta có thể chuyển bài toán thành: tìm số nguyên dương $x$ nhỏ nhất sao cho số lượng số nhỏ hơn hoặc bằng $x$ và thỏa mãn điều kiện chính xác bằng $k$. Nếu $x$ thỏa mãn điều kiện, thì với mọi $x' > x$, $x'$ cũng thỏa mãn điều kiện. Điều này cho thấy tính đơn điệu, vì vậy ta có thể dùng tìm kiếm nhị phân để tìm $x$ nhỏ nhất thỏa mãn điều kiện.

Ta định nghĩa hàm `check(x)` để xác định xem số lượng số nhỏ hơn hoặc bằng $x$ và thỏa mãn điều kiện có lớn hơn hoặc bằng $k$ hay không. Ta cần tính có bao nhiêu số có thể tạo ra từ mảng $coins$.

Giả sử $coins = [a, b]$, theo nguyên lý bao hàm-loại trừ, số lượng số nhỏ hơn hoặc bằng $x$ và thỏa mãn điều kiện là:

$$
\left\lfloor \frac{x}{a} \right\rfloor + \left\lfloor \frac{x}{b} \right\rfloor - \left\lfloor \frac{x}{lcm(a, b)} \right\rfloor
$$

Nếu $coins = [a, b, c]$, số lượng số nhỏ hơn hoặc bằng $x$ và thỏa mãn điều kiện là:

$$
\left\lfloor \frac{x}{a} \right\rfloor + \left\lfloor \frac{x}{b} \right\rfloor + \left\lfloor \frac{x}{c} \right\rfloor - \left\lfloor \frac{x}{lcm(a, b)} \right\rfloor - \left\lfloor \frac{x}{lcm(a, c)} \right\rfloor - \left\lfloor \frac{x}{lcm(b, c)} \right\rfloor + \left\lfloor \frac{x}{lcm(a, b, c)} \right\rfloor
$$

Như vậy, ta cần cộng tất cả các trường hợp có số phần tử lẻ và trừ tất cả các trường hợp có số phần tử chẵn.

Vì $n \leq 15$, ta có thể dùng liệt kê nhị phân để liệt kê mọi tập con và tính số lượng số thỏa mãn điều kiện, ký hiệu là $cnt$. Nếu $cnt \geq k$, ta cần tìm $x$ nhỏ nhất sao cho `check(x)` là đúng.

Khi bắt đầu tìm kiếm nhị phân, ta đặt biên trái $l=1$ và biên phải $r={10}^{11}$. Sau đó, ta liên tục đưa giá trị giữa $mid$ vào hàm `check`. Nếu `check(mid)` là đúng, ta cập nhật biên phải $r$ thành $mid$, ngược lại cập nhật biên trái $l$ thành $mid+1$. Cuối cùng, ta trả về $l$.

Độ phức tạp thời gian là $O(n \times 2^n \times \log (k \times M))$, trong đó $n$ là độ dài của mảng $coins$, còn $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKthSmallest(self, coins: List[int], k: int) -> int:
        def check(mx: int) -> bool:
            cnt = 0
            for i in range(1, 1 << len(coins)):
                v = 1
                for j, x in enumerate(coins):
                    if i >> j & 1:
                        v = lcm(v, x)
                        if v > mx:
                            break
                m = i.bit_count()
                if m & 1:
                    cnt += mx // v
                else:
                    cnt -= mx // v
            return cnt >= k

        return bisect_left(range(10**11), True, key=check)
```

#### Java

```java
class Solution {
    private int[] coins;
    private int k;

    public long findKthSmallest(int[] coins, int k) {
        this.coins = coins;
        this.k = k;
        long l = 1, r = (long) 1e11;
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

    private boolean check(long mx) {
        long cnt = 0;
        int n = coins.length;
        for (int i = 1; i < 1 << n; ++i) {
            long v = 1;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    v = lcm(v, coins[j]);
                    if (v > mx) {
                        break;
                    }
                }
            }
            int m = Integer.bitCount(i);
            if (m % 2 == 1) {
                cnt += mx / v;
            } else {
                cnt -= mx / v;
            }
        }
        return cnt >= k;
    }

    private long lcm(long a, long b) {
        return a * b / gcd(a, b);
    }

    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long findKthSmallest(vector<int>& coins, int k) {
        using ll = long long;
        ll l = 1, r = 1e11;
        int n = coins.size();

        auto check = [&](ll mx) {
            ll cnt = 0;
            for (int i = 1; i < 1 << n; ++i) {
                ll v = 1;
                for (int j = 0; j < n; ++j) {
                    if (i >> j & 1) {
                        v = lcm(v, coins[j]);
                        if (v > mx) {
                            break;
                        }
                    }
                }
                int m = __builtin_popcount(i);
                if (m & 1) {
                    cnt += mx / v;
                } else {
                    cnt -= mx / v;
                }
            }
            return cnt >= k;
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
func findKthSmallest(coins []int, k int) int64 {
	var r int = 1e11
	n := len(coins)
	ans := sort.Search(r, func(mx int) bool {
		cnt := 0
		for i := 1; i < 1<<n; i++ {
			v := 1
			for j, x := range coins {
				if i>>j&1 == 1 {
					v = lcm(v, x)
					if v > mx {
						break
					}
				}
			}
			m := bits.OnesCount(uint(i))
			if m%2 == 1 {
				cnt += mx / v
			} else {
				cnt -= mx / v
			}
		}
		return cnt >= k
	})
	return int64(ans)
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}

func lcm(a, b int) int {
	return a * b / gcd(a, b)
}
```

#### TypeScript

```ts
function findKthSmallest(coins: number[], k: number): number {
    let [l, r] = [1n, BigInt(1e11)];
    const n = coins.length;
    const check = (mx: bigint): boolean => {
        let cnt = 0n;
        for (let i = 1; i < 1 << n; ++i) {
            let v = 1n;
            for (let j = 0; j < n; ++j) {
                if ((i >> j) & 1) {
                    v = lcm(v, BigInt(coins[j]));
                    if (v > mx) {
                        break;
                    }
                }
            }
            const m = bitCount(i);
            if (m & 1) {
                cnt += mx / v;
            } else {
                cnt -= mx / v;
            }
        }
        return cnt >= BigInt(k);
    };
    while (l < r) {
        const mid = (l + r) >> 1n;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1n;
        }
    }
    return Number(l);
}

function gcd(a: bigint, b: bigint): bigint {
    return b === 0n ? a : gcd(b, a % b);
}

function lcm(a: bigint, b: bigint): bigint {
    return (a * b) / gcd(a, b);
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
