---
comments: true
difficulty: Medium
rating: 2258
source: Weekly Contest 380 Q3
tags:
    - Bit Manipulation
    - Math
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [3007. Maximum Number That Sum of the Prices Is Less Than or Equal to K](https://leetcode.com/problems/maximum-number-that-sum-of-the-prices-is-less-than-or-equal-to-k)

[Tài liệu tiếng Trung](/solution/3000-3099/3007.Maximum%20Number%20That%20Sum%20of%20the%20Prices%20Is%20Less%20Than%20or%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>k</code> và một số nguyên <code>x</code>. Giá của một số <code>num</code> được tính bằng số lượng <span data-keyword="set-bit">bit được đặt</span> tại các vị trí <code>x</code>, <code>2x</code>, <code>3x</code>, v.v. trong biểu diễn nhị phân của nó, bắt đầu từ bit ít quan trọng nhất. Bảng sau đây đưa ra các ví dụ về cách tính giá.</p>

<table border="1">
	<tbody>
		<tr>
			<th>x</th>
			<th>num</th>
			<th>Biểu diễn nhị phân</th>
			<th>Giá</th>
		</tr>
		<tr>
			<td>1</td>
			<td>13</td>
			<td><u>0</u><u>0</u><u>0</u><u>0</u><u>0</u><strong><u>1</u></strong><strong><u>1</u></strong><u>0</u><strong><u>1</u></strong></td>
			<td>3</td>
		</tr>
		<tr>
			<td>2</td>
			<td>13</td>
			<td>0<u>0</u>0<u>0</u>0<strong><u>1</u></strong>1<u>0</u>1</td>
			<td>1</td>
		</tr>
		<tr>
			<td>2</td>
			<td>233</td>
			<td>0<strong><u>1</u></strong>1<strong><u>1</u></strong>0<strong><u>1</u></strong>0<u>0</u>1</td>
			<td>3</td>
		</tr>
		<tr>
			<td>3</td>
			<td>13</td>
			<td><u>0</u>00<u>0</u>01<strong><u>1</u></strong>01</td>
			<td>1</td>
		</tr>
		<tr>
			<td>3</td>
			<td>362</td>
			<td><strong><u>1</u></strong>01<strong><u>1</u></strong>01<u>0</u>10</td>
			<td>2</td>
		</tr>
	</tbody>
</table>

<p><strong>Giá tích lũy</strong> của <code>num</code> là <b>tổng</b> giá của các số từ <code>1</code> đến <code>num</code>. <code>num</code> được coi là <strong>rẻ</strong> nếu giá tích lũy của nó nhỏ hơn hoặc bằng <code>k</code>.</p>

<p>Trả về số rẻ <b>lớn nhất</b>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 9, x = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Như trong bảng dưới đây, <code>6</code> là số rẻ lớn nhất.</p>

<table border="1">
	<tbody>
		<tr>
			<th>x</th>
			<th>num</th>
			<th>Biểu diễn nhị phân</th>
			<th>Giá</th>
			<th>Giá tích lũy</th>
		</tr>
		<tr>
			<td>1</td>
			<td>1</td>
			<td><u>0</u><u>0</u><strong><u>1</u></strong></td>
			<td>1</td>
			<td>1</td>
		</tr>
		<tr>
			<td>1</td>
			<td>2</td>
			<td><u>0</u><strong><u>1</u></strong><u>0</u></td>
			<td>1</td>
			<td>2</td>
		</tr>
		<tr>
			<td>1</td>
			<td>3</td>
			<td><u>0</u><strong><u>1</u></strong><strong><u>1</u></strong></td>
			<td>2</td>
			<td>4</td>
		</tr>
		<tr>
			<td>1</td>
			<td>4</td>
			<td><strong><u>1</u></strong><u>0</u><u>0</u></td>
			<td>1</td>
			<td>5</td>
		</tr>
		<tr>
			<td>1</td>
			<td>5</td>
			<td><strong><u>1</u></strong><u>0</u><strong><u>1</u></strong></td>
			<td>2</td>
			<td>7</td>
		</tr>
		<tr>
			<td>1</td>
			<td>6</td>
			<td><strong><u>1</u></strong><strong><u>1</u></strong><u>0</u></td>
			<td>2</td>
			<td>9</td>
		</tr>
		<tr>
			<td>1</td>
			<td>7</td>
			<td><strong><u>1</u></strong><strong><u>1</u></strong><strong><u>1</u></strong></td>
			<td>3</td>
			<td>12</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 7, x = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Như trong bảng dưới đây, <code>9</code> là số rẻ lớn nhất.</p>

<table border="1">
	<tbody>
		<tr>
			<th>x</th>
			<th>num</th>
			<th>Biểu diễn nhị phân</th>
			<th>Giá</th>
			<th>Giá tích lũy</th>
		</tr>
		<tr>
			<td>2</td>
			<td>1</td>
			<td><u>0</u>0<u>0</u>1</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td>2</td>
			<td>2</td>
			<td><u>0</u>0<strong><u>1</u></strong>0</td>
			<td>1</td>
			<td>1</td>
		</tr>
		<tr>
			<td>2</td>
			<td>3</td>
			<td><u>0</u>0<strong><u>1</u></strong>1</td>
			<td>1</td>
			<td>2</td>
		</tr>
		<tr>
			<td>2</td>
			<td>4</td>
			<td><u>0</u>1<u>0</u>0</td>
			<td>0</td>
			<td>2</td>
		</tr>
		<tr>
			<td>2</td>
			<td>5</td>
			<td><u>0</u>1<u>0</u>1</td>
			<td>0</td>
			<td>2</td>
		</tr>
		<tr>
			<td>2</td>
			<td>6</td>
			<td><u>0</u>1<strong><u>1</u></strong>0</td>
			<td>1</td>
			<td>3</td>
		</tr>
		<tr>
			<td>2</td>
			<td>7</td>
			<td><u>0</u>1<strong><u>1</u></strong>1</td>
			<td>1</td>
			<td>4</td>
		</tr>
		<tr>
			<td>2</td>
			<td>8</td>
			<td><strong><u>1</u></strong>0<u>0</u>0</td>
			<td>1</td>
			<td>5</td>
		</tr>
		<tr>
			<td>2</td>
			<td>9</td>
			<td><strong><u>1</u></strong>0<u>0</u>1</td>
			<td>1</td>
			<td>6</td>
		</tr>
		<tr>
			<td>2</td>
			<td>10</td>
			<td><strong><u>1</u></strong>0<strong><u>1</u></strong>0</td>
			<td>2</td>
			<td>8</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>15</sup></code></li>
	<li><code>1 &lt;= x &lt;= 8</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Tổng giá tăng đơn điệu theo $\textit{num}$, trong khi $k$ có thể đạt $10^{15}$, nên ta không thể cộng dồn từng số một.
>
> Kiểm tra tổng giá đến $\textit{mid}$ không vượt quá $k$ là một bài toán đếm bit: một bit chỉ được tính giá khi chỉ số của nó (bắt đầu từ $1$) là bội của $x$.
>
> Ta tìm kiếm nhị phân $\textit{num}$ và memoize theo vị trí bit, đồng thời cộng dồn số bit được tính giá đã được đặt trong một ràng buộc chặt.

<!-- thinking:end -->

Ta nhận thấy khi $\textit{num}$ tăng, tổng giá từ $1$ đến $\textit{num}$ cũng tăng. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm số rẻ lớn nhất.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 1$. Vì trong mỗi $2^x + 1$ số luôn có ít nhất một số có giá trị, và tổng giá không vượt quá $10^{15}$, ta có thể đặt biên phải của tìm kiếm nhị phân là $r = 10^{18}$.

Tiếp theo, ta thực hiện tìm kiếm nhị phân. Với mỗi $\textit{mid}$, ta dùng digit DP để tính tổng giá từ $1$ đến $\textit{mid}$. Nếu tổng giá không vượt quá $k$, điều đó có nghĩa $\textit{mid}$ là một số rẻ, và ta cập nhật biên trái $l$ thành $\textit{mid}$. Ngược lại, ta cập nhật biên phải $r$ thành $\textit{mid} - 1$.

Cuối cùng, ta trả về biên trái $l$.

Độ phức tạp thời gian là $O(\log^2 k)$, và độ phức tạp không gian là $O(\log k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaximumNumber(self, k: int, x: int) -> int:
        @cache
        def dfs(pos, limit, cnt):
            if pos == 0:
                return cnt
            ans = 0
            up = (self.num >> (pos - 1) & 1) if limit else 1
            for i in range(up + 1):
                ans += dfs(pos - 1, limit and i == up, cnt + (i == 1 and pos % x == 0))
            return ans

        l, r = 1, 10**18
        while l < r:
            mid = (l + r + 1) >> 1
            self.num = mid
            v = dfs(mid.bit_length(), True, 0)
            dfs.cache_clear()
            if v <= k:
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    private int x;
    private long num;
    private Long[][] f;

    public long findMaximumNumber(long k, int x) {
        this.x = x;
        long l = 1, r = (long) 1e17;
        while (l < r) {
            long mid = (l + r + 1) >>> 1;
            num = mid;
            f = new Long[65][65];
            int pos = 64 - Long.numberOfLeadingZeros(mid);
            if (dfs(pos, 0, true) <= k) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private long dfs(int pos, int cnt, boolean limit) {
        if (pos == 0) {
            return cnt;
        }
        if (!limit && f[pos][cnt] != null) {
            return f[pos][cnt];
        }
        long ans = 0;
        int up = limit ? (int) (num >> (pos - 1) & 1) : 1;
        for (int i = 0; i <= up; ++i) {
            ans += dfs(pos - 1, cnt + (i == 1 && pos % x == 0 ? 1 : 0), limit && i == up);
        }
        if (!limit) {
            f[pos][cnt] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long findMaximumNumber(long long k, int x) {
        using ll = long long;
        ll l = 1, r = 1e17;
        ll num = 0;
        ll f[65][65];
        auto dfs = [&](this auto&& dfs, int pos, int cnt, bool limit) -> ll {
            if (pos == 0) {
                return cnt;
            }
            if (!limit && f[pos][cnt] != -1) {
                return f[pos][cnt];
            }
            int up = limit ? num >> (pos - 1) & 1 : 1;
            ll ans = 0;
            for (int i = 0; i <= up; ++i) {
                ans += dfs(pos - 1, cnt + (i == 1 && pos % x == 0), limit && i == up);
            }
            if (!limit) {
                f[pos][cnt] = ans;
            }
            return ans;
        };
        while (l < r) {
            ll mid = (l + r + 1) >> 1;
            num = mid;
            memset(f, -1, sizeof(f));
            int pos = 64 - __builtin_clzll(mid);
            if (dfs(pos, 0, true) <= k) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func findMaximumNumber(k int64, x int) int64 {
	var l, r int64 = 1, 1e17
	var num int64
	var f [65][65]int64
	var dfs func(pos, cnt int, limit bool) int64
	dfs = func(pos, cnt int, limit bool) int64 {
		if pos == 0 {
			return int64(cnt)
		}
		if !limit && f[pos][cnt] != -1 {
			return f[pos][cnt]
		}
		var ans int64
		up := 1
		if limit {
			up = int(num >> (pos - 1) & 1)
		}
		for i := 0; i <= up; i++ {
			v := cnt
			if i == 1 && pos%x == 0 {
				v++
			}
			ans += dfs(pos-1, v, limit && i == up)
		}
		if !limit {
			f[pos][cnt] = ans
		}
		return ans
	}
	for l < r {
		mid := (l + r + 1) >> 1
		num = mid
		m := bits.Len(uint(num))
		for i := range f {
			for j := range f[i] {
				f[i][j] = -1
			}
		}
		if dfs(m, 0, true) <= k {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function findMaximumNumber(k: number, x: number): number {
    let [l, r] = [1n, 10n ** 17n];
    let num: bigint;
    const f: bigint[][] = Array.from({ length: 65 }, () => Array(65).fill(-1n));

    const dfs = (pos: number, cnt: number, limit: boolean): bigint => {
        if (pos === 0) {
            return BigInt(cnt);
        }
        if (!limit && f[pos][cnt] !== -1n) {
            return f[pos][cnt];
        }
        let ans: bigint = 0n;
        let up: number = 1;
        if (limit) {
            up = Number((num >> BigInt(pos - 1)) & 1n);
        }
        for (let i = 0; i <= up; i++) {
            let v: number = cnt;
            if (i === 1 && pos % x === 0) {
                v++;
            }
            ans += dfs(pos - 1, v, limit && i === up);
        }
        if (!limit) {
            f[pos][cnt] = ans;
        }
        return ans;
    };

    while (l < r) {
        let mid: bigint = (l + r + 1n) >> 1n;
        num = mid;
        let m: number = num.toString(2).length;
        for (let i = 0; i < f.length; i++) {
            f[i].fill(-1n);
        }
        if (dfs(m, 0, true) <= BigInt(k)) {
            l = mid;
        } else {
            r = mid - 1n;
        }
    }
    return Number(l);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
