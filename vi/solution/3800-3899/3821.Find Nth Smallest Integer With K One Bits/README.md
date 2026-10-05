---
comments: true
difficulty: Hard
rating: 2069
source: Weekly Contest 486 Q4
tags:
    - Bit Manipulation
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [3821. Find Nth Smallest Integer With K One Bits](https://leetcode.com/problems/find-nth-smallest-integer-with-k-one-bits)

[中文文档](/solution/3800-3899/3821.Find%20Nth%20Smallest%20Integer%20With%20K%20One%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>n</code> và <code>k</code>.</p>

<p>Hãy trả về một số nguyên biểu thị số nguyên dương nhỏ thứ <code>n<sup>th</sup></code> có <strong>chính xác</strong> <code>k</code> bit 1 trong biểu diễn nhị phân. Đảm bảo đáp án <strong>nhỏ hơn nghiêm ngặt</strong> <code>2<sup>50</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>4 số nguyên dương nhỏ nhất có chính xác <code>k = 2</code> bit 1 trong biểu diễn nhị phân là:</p>

<ul>
	<li><code>3 = 11<sub>2</sub></code></li>
	<li><code>5 = 101<sub>2</sub></code></li>
	<li><code>6 = 110<sub>2</sub></code></li>
	<li><code>9 = 1001<sub>2</sub></code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>3 số nguyên dương nhỏ nhất có chính xác <code>k = 1</code> bit 1 trong biểu diễn nhị phân là:</p>

<ul>
	<li><code>1 = 1<sub>2</sub></code></li>
	<li><code>2 = 10<sub>2</sub></code></li>
	<li><code>4 = 100<sub>2</sub></code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>50</sup></code></li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
	<li>Đáp án nhỏ hơn nghiêm ngặt <code>2<sup>50</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổ hợp + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên dương thứ $n$ có chính xác $k$ bit 1; số này nhỏ hơn $2^{50}$. Việc duyệt các số tự nhiên và đếm số bit là quá chậm.
>
> Hãy quyết định các bit từ cao xuống thấp: nếu số cách đặt $k$ bit 1 còn lại trong $i$ bit thấp hơn $n$, bit hiện tại phải là $1$.
>
> Tính trước $\binom{i}{k}$. Từ bit $49$ giảm dần, nếu $n>C(i,k)$ thì bật bit đó, trừ số cách này khỏi thứ hạng hiện tại và giảm $k$.
>
> Cách điền greedy này xác định duy nhất số thứ $n$ theo thứ tự số.

<!-- thinking:end -->

Ta cần tìm số nguyên dương nhỏ thứ $n$ có chính xác $k$ bit 1 trong biểu diễn nhị phân. Ta có thể xác định từng bit từ bit cao nhất đến bit thấp nhất, quyết định bit đó là $0$ hay $1$.

Giả sử ta đang xét bit thứ $i$ (từ $49$ xuống $0$). Nếu đặt bit này bằng $0$, thì $k$ bit 1 còn lại phải được chọn trong $i$ bit thấp hơn, và số tổ hợp có thể là $C(i, k)$. Nếu $n$ lớn hơn $C(i, k)$, điều đó có nghĩa là bit thứ $i$ của số thứ $n$ phải là $1$. Khi đó, ta đặt bit này bằng $1$, trừ $C(i, k)$ khỏi $n$ và giảm $k$ đi $1$ (vì ta đã dùng một bit $1$). Ngược lại, ta đặt bit này bằng $0$.

Ta lặp lại quy trình trên cho đến khi đã xét hết các bit hoặc $k$ trở thành $0$.

Độ phức tạp thời gian là $O(\log^2 M)$ và độ phức tạp không gian là $O(\log^2 M)$, trong đó $M$ là cận trên của đáp án, $2^{50}$.

<!-- tabs:start -->

#### Python3

```python
mx = 50
c = [[0] * (mx + 1) for _ in range(mx)]
for i in range(mx):
    c[i][0] = 1
    for j in range(1, i + 1):
        c[i][j] = c[i - 1][j - 1] + c[i - 1][j]


class Solution:
    def nthSmallest(self, n: int, k: int) -> int:
        ans = 0
        for i in range(49, -1, -1):
            if n > c[i][k]:
                n -= c[i][k]
                ans |= 1 << i
                k -= 1
                if k == 0:
                    break
        return ans
```

#### Java

```java
class Solution {
    private static final int MX = 50;
    private static final long[][] c = new long[MX][MX + 1];

    static {
        for (int i = 0; i < MX; i++) {
            c[i][0] = 1;
            for (int j = 1; j <= i; j++) {
                c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
            }
        }
    }

    public long nthSmallest(long n, int k) {
        long ans = 0;
        for (int i = 49; i >= 0; i--) {
            if (n > c[i][k]) {
                n -= c[i][k];
                ans |= 1L << i;
                k--;
                if (k == 0) {
                    break;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
constexpr int MX = 50;
long long c[MX][MX + 1];

auto init = [] {
    for (int i = 0; i < MX; i++) {
        c[i][0] = 1;
        for (int j = 1; j <= i; j++) {
            c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
        }
    }
    return 0;
}();

class Solution {
public:
    long long nthSmallest(long long n, int k) {
        long long ans = 0;
        for (int i = 49; i >= 0; i--) {
            if (n > c[i][k]) {
                n -= c[i][k];
                ans |= 1LL << i;
                if (--k == 0) {
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
const MX = 50

var c [MX][MX + 1]int64

func init() {
	for i := 0; i < MX; i++ {
		c[i][0] = 1
		for j := 1; j <= i; j++ {
			c[i][j] = c[i-1][j-1] + c[i-1][j]
		}
	}
}

func nthSmallest(n int64, k int) int64 {
	var ans int64 = 0
	for i := 49; i >= 0; i-- {
		if n > c[i][k] {
			n -= c[i][k]
			ans |= 1 << i
			k--
			if k == 0 {
				break
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
const MX = 50;
const c: bigint[][] = Array.from({ length: MX }, () => Array(MX + 1).fill(0n));

for (let i = 0; i < MX; i++) {
    c[i][0] = 1n;
    for (let j = 1; j <= i; j++) {
        c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
    }
}

function nthSmallest(n: number, k: number): number {
    let nn = BigInt(n);
    let ans = 0n;
    for (let i = 49; i >= 0; i--) {
        if (nn > c[i][k]) {
            nn -= c[i][k];
            ans |= 1n << BigInt(i);
            if (--k === 0) {
                break;
            }
        }
    }
    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
