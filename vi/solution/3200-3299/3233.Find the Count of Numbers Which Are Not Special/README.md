---
comments: true
difficulty: Medium
rating: 1509
source: Weekly Contest 408 Q2
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3233. Find the Count of Numbers Which Are Not Special](https://leetcode.com/problems/find-the-count-of-numbers-which-are-not-special)

[Tài liệu tiếng Trung](/solution/3200-3299/3233.Find%20the%20Count%20of%20Numbers%20Which%20Are%20Not%20Special/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho 2 số nguyên <strong>dương</strong> <code>l</code> và <code>r</code>. Với mọi số <code>x</code>, tất cả các ước dương của <code>x</code> <em>ngoại trừ</em> <code>x</code> được gọi là <strong>ước thực sự</strong> của <code>x</code>.</p>

<p>Một số được gọi là <strong>đặc biệt</strong> nếu nó có đúng 2 <strong>ước thực sự</strong>. Ví dụ:</p>

<ul>
	<li>Số 4 là <em>đặc biệt</em> vì nó có các ước thực sự là 1 và 2.</li>
	<li>Số 6 <em>không đặc biệt</em> vì nó có các ước thực sự là 1, 2 và 3.</li>
</ul>

<p>Trả về số lượng các số trong đoạn <code>[l, r]</code> <strong>không</strong> <strong>đặc biệt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 5, r = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số đặc biệt nào trong đoạn <code>[5, 7]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 4, r = 16</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số đặc biệt trong đoạn <code>[4, 16]</code> là 4 và 9.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= l &lt;= r &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Một số đặc biệt có đúng ba ước dương, tức là bình phương của một số nguyên tố. Vì $l,r\le 10^9$, việc thử chia từng số trong đoạn là không khả thi.
>
> Các số đặc biệt trong $[l,r]$ tương ứng với các số nguyên tố trong $[\lceil\sqrt{l}\rceil,\lfloor\sqrt{r}\rfloor]$. Sau khi sàng đến $\sqrt{10^9}$, ta đếm các số nguyên tố đó rồi lấy độ dài đoạn trừ đi số lượng này.

<!-- thinking:end -->

Theo mô tả đề bài, ta có thể nhận thấy rằng chỉ các bình phương của số nguyên tố mới là số đặc biệt. Vì vậy, trước tiên ta có thể tiền xử lý tất cả các số nguyên tố nhỏ hơn hoặc bằng $\sqrt{10^9}$, sau đó duyệt qua đoạn $[\lceil\sqrt{l}\rceil, \lfloor\sqrt{r}\rfloor]$ và đếm số lượng số nguyên tố là $\textit{cnt}$ trong đoạn. Cuối cùng, ta trả về $r - l + 1 - \textit{cnt}$.

Độ phức tạp thời gian là $O(\sqrt{m})$, và độ phức tạp không gian là $O(\sqrt{m})$, trong đó $m = 10^9$.

<!-- tabs:start -->

#### Python3

```python
m = 31623
primes = [True] * (m + 1)
primes[0] = primes[1] = False
for i in range(2, m + 1):
    if primes[i]:
        for j in range(i + i, m + 1, i):
            primes[j] = False


class Solution:
    def nonSpecialCount(self, l: int, r: int) -> int:
        lo = ceil(sqrt(l))
        hi = floor(sqrt(r))
        cnt = sum(primes[i] for i in range(lo, hi + 1))
        return r - l + 1 - cnt
```

#### Java

```java
class Solution {
    static int m = 31623;
    static boolean[] primes = new boolean[m + 1];

    static {
        Arrays.fill(primes, true);
        primes[0] = primes[1] = false;
        for (int i = 2; i <= m; i++) {
            if (primes[i]) {
                for (int j = i + i; j <= m; j += i) {
                    primes[j] = false;
                }
            }
        }
    }

    public int nonSpecialCount(int l, int r) {
        int lo = (int) Math.ceil(Math.sqrt(l));
        int hi = (int) Math.floor(Math.sqrt(r));
        int cnt = 0;
        for (int i = lo; i <= hi; i++) {
            if (primes[i]) {
                cnt++;
            }
        }
        return r - l + 1 - cnt;
    }
}
```

#### C++

```cpp
const int m = 31623;
bool primes[m + 1];

auto init = [] {
    memset(primes, true, sizeof(primes));
    primes[0] = primes[1] = false;
    for (int i = 2; i <= m; ++i) {
        if (primes[i]) {
            for (int j = i * 2; j <= m; j += i) {
                primes[j] = false;
            }
        }
    }
    return 0;
}();

class Solution {
public:
    int nonSpecialCount(int l, int r) {
        int lo = ceil(sqrt(l));
        int hi = floor(sqrt(r));
        int cnt = 0;
        for (int i = lo; i <= hi; ++i) {
            if (primes[i]) {
                ++cnt;
            }
        }
        return r - l + 1 - cnt;
    }
};
```

#### Go

```go
const m = 31623

var primes [m + 1]bool

func init() {
	for i := range primes {
		primes[i] = true
	}
	primes[0] = false
	primes[1] = false
	for i := 2; i <= m; i++ {
		if primes[i] {
			for j := i * 2; j <= m; j += i {
				primes[j] = false
			}
		}
	}
}

func nonSpecialCount(l int, r int) int {
	lo := int(math.Ceil(math.Sqrt(float64(l))))
	hi := int(math.Floor(math.Sqrt(float64(r))))
	cnt := 0
	for i := lo; i <= hi; i++ {
		if primes[i] {
			cnt++
		}
	}
	return r - l + 1 - cnt
}
```

#### TypeScript

```ts
const m = 31623;
const primes: boolean[] = Array(m + 1).fill(true);

(() => {
    primes[0] = primes[1] = false;
    for (let i = 2; i <= m; ++i) {
        if (primes[i]) {
            for (let j = i * 2; j <= m; j += i) {
                primes[j] = false;
            }
        }
    }
})();

function nonSpecialCount(l: number, r: number): number {
    const lo = Math.ceil(Math.sqrt(l));
    const hi = Math.floor(Math.sqrt(r));
    let cnt = 0;
    for (let i = lo; i <= hi; ++i) {
        if (primes[i]) {
            ++cnt;
        }
    }
    return r - l + 1 - cnt;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
