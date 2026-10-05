---
comments: true
difficulty: Medium
rating: 1546
source: Weekly Contest 479 Q2
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3770. Largest Prime from Consecutive Prime Sum](https://leetcode.com/problems/largest-prime-from-consecutive-prime-sum)

[中文文档](/solution/3700-3799/3770.Largest%20Prime%20from%20Consecutive%20Prime%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>.</p>

<p>Trả về <strong><span data-keyword="prime-number">số nguyên tố</span> lớn nhất</strong> nhỏ hơn hoặc bằng <code>n</code>, có thể biểu diễn dưới dạng <strong>tổng</strong> của một hoặc nhiều <strong>số nguyên tố liên tiếp</strong> bắt đầu từ 2. Nếu không tồn tại số nào như vậy, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 20</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên tố nhỏ hơn hoặc bằng <code>n = 20</code> có thể biểu diễn dưới dạng tổng các số nguyên tố liên tiếp là:</p>

<ul>
	<li>
		<p><code>2 = 2</code></p>
	</li>
	<li>
		<p><code>5 = 2 + 3</code></p>
	</li>
	<li>
		<p><code>17 = 2 + 3 + 5 + 7</code></p>
	</li>
</ul>

<p>Số lớn nhất là 17, nên đó là đáp án.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng các số nguyên tố liên tiếp duy nhất nhỏ hơn hoặc bằng 2 là chính 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 5\times 10^5$ và tổng phải bắt đầu từ $2$. Sau khi sàng các số nguyên tố đến giới hạn, ta cộng dồn các tổng tiền tố và giữ lại những tổng vẫn là số nguyên tố; với mỗi truy vấn, ta tìm kiếm nhị phân tổng lớn nhất không vượt quá $n$.

<!-- thinking:end -->

Ta có thể tiền xử lý danh sách tất cả số nguyên tố nhỏ hơn hoặc bằng $5 \times 10^5$, sau đó tính các tổng của những số nguyên tố liên tiếp bắt đầu từ 2 và lưu các tổng là số nguyên tố vào mảng $s$.

Với mỗi truy vấn, ta chỉ cần tìm kiếm nhị phân trên mảng $s$ để tìm giá trị lớn nhất nhỏ hơn hoặc bằng $n$.

Về độ phức tạp thời gian, tiền xử lý các số nguyên tố mất $O(M \log \log M)$, còn mỗi truy vấn mất $O(\log k)$, trong đó $M$ là giới hạn tiền xử lý và $k$ là độ dài của mảng $s$. Trong bài toán này, $k \leq 40$.

<!-- tabs:start -->

#### Python3

```python
mx = 500000
is_prime = [True] * (mx + 1)
is_prime[0] = is_prime[1] = False
primes = []
for i in range(2, mx + 1):
    if is_prime[i]:
        primes.append(i)
        for j in range(i * i, mx + 1, i):
            is_prime[j] = False
s = [0]
t = 0
for x in primes:
    t += x
    if t > mx:
        break
    if is_prime[t]:
        s.append(t)


class Solution:
    def largestPrime(self, n: int) -> int:
        i = bisect_right(s, n) - 1
        return s[i]
```

#### Java

```java
class Solution {
    private static final int MX = 500000;
    private static final boolean[] IS_PRIME = new boolean[MX + 1];
    private static final List<Integer> PRIMES = new ArrayList<>();
    private static final List<Integer> S = new ArrayList<>();

    static {
        Arrays.fill(IS_PRIME, true);
        IS_PRIME[0] = false;
        IS_PRIME[1] = false;

        for (int i = 2; i <= MX; i++) {
            if (IS_PRIME[i]) {
                PRIMES.add(i);
                if ((long) i * i <= MX) {
                    for (int j = i * i; j <= MX; j += i) {
                        IS_PRIME[j] = false;
                    }
                }
            }
        }

        S.add(0);
        int t = 0;
        for (int x : PRIMES) {
            t += x;
            if (t > MX) {
                break;
            }
            if (IS_PRIME[t]) {
                S.add(t);
            }
        }
    }

    public int largestPrime(int n) {
        int i = Collections.binarySearch(S, n + 1);
        if (i < 0) {
            i = ~i;
        }
        return S.get(i - 1);
    }
}
```

#### C++

```cpp
static const int MX = 500000;
static vector<bool> IS_PRIME(MX + 1, true);
static vector<int> PRIMES;
static vector<int> S;

auto init = [] {
    IS_PRIME[0] = false;
    IS_PRIME[1] = false;

    for (int i = 2; i <= MX; i++) {
        if (IS_PRIME[i]) {
            PRIMES.push_back(i);
            if (1LL * i * i <= MX) {
                for (int j = i * i; j <= MX; j += i) {
                    IS_PRIME[j] = false;
                }
            }
        }
    }

    S.push_back(0);
    int t = 0;
    for (int x : PRIMES) {
        t += x;
        if (t > MX) break;
        if (IS_PRIME[t]) {
            S.push_back(t);
        }
    }

    return 0;
}();

class Solution {
public:
    int largestPrime(int n) {
        auto it = upper_bound(S.begin(), S.end(), n);
        return *(it - 1);
    }
};
```

#### Go

```go
const MX = 500000

var (
	isPrime = make([]bool, MX+1)
	primes  []int
	S       []int
)

func init() {
	for i := range isPrime {
		isPrime[i] = true
	}
	isPrime[0] = false
	isPrime[1] = false

	for i := 2; i <= MX; i++ {
		if isPrime[i] {
			primes = append(primes, i)
			if i*i <= MX {
				for j := i * i; j <= MX; j += i {
					isPrime[j] = false
				}
			}
		}
	}

	S = append(S, 0)
	t := 0
	for _, x := range primes {
		t += x
		if t > MX {
			break
		}
		if isPrime[t] {
			S = append(S, t)
		}
	}
}

func largestPrime(n int) int {
	i := sort.SearchInts(S, n+1)
	return S[i-1]
}
```

#### TypeScript

```ts
const MX = 500000;

const isPrime: boolean[] = Array(MX + 1).fill(true);
isPrime[0] = false;
isPrime[1] = false;

const primes: number[] = [];
const s: number[] = [];

(function init() {
    for (let i = 2; i <= MX; i++) {
        if (isPrime[i]) {
            primes.push(i);
            if (i * i <= MX) {
                for (let j = i * i; j <= MX; j += i) {
                    isPrime[j] = false;
                }
            }
        }
    }

    s.push(0);
    let t = 0;
    for (const x of primes) {
        t += x;
        if (t > MX) break;
        if (isPrime[t]) {
            s.push(t);
        }
    }
})();

function largestPrime(n: number): number {
    const i = _.sortedIndex(s, n + 1) - 1;
    return s[i];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
