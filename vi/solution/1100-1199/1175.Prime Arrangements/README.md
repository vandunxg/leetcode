---
comments: true
difficulty: Easy
rating: 1489
source: Weekly Contest 152 Q1
tags:
    - Math
    - Primality Test
    - Sieve
    - Sieve of Eratosthenes
---

<!-- problem:start -->

# [1175. Prime Arrangements](https://leetcode.com/problems/prime-arrangements)

[中文文档](/solution/1100-1199/1175.Prime%20Arrangements/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy trả về số hoán vị của các số từ 1 đến <code>n</code> sao cho số nguyên tố nằm ở các chỉ số nguyên tố (đánh số từ 1).</p>

<p><em>(Nhắc lại: một số nguyên là số nguyên tố khi và chỉ khi nó lớn hơn 1 và không thể biểu diễn thành tích của hai số nguyên dương đều nhỏ hơn nó.)</em></p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>theo modulo <code>10^9 + 7</code></strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 5
<strong>Output:</strong> 12
<strong>Giải thích:</strong> Ví dụ, [1,2,5,4,3] là một hoán vị hợp lệ, còn [5,2,3,4,1] thì không vì số nguyên tố 5 nằm ở chỉ số 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 100
<strong>Output:</strong> 682289015
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Số nguyên tố chỉ có thể nằm ở các chỉ số nguyên tố, còn hợp số chỉ nằm ở các chỉ số hợp số, nên số hoán vị là $cnt!\times(n-cnt)!$. Vì $n\le 100$, dùng sàng để đếm số nguyên tố trong đoạn $[1,n]$, rồi nhân hai giai thừa theo modulo $10^9+7$.

<!-- thinking:end -->

Trước tiên, đếm số nguyên tố trong đoạn $[1,n]$, gọi số lượng đó là $cnt$. Sau đó, tính tích của $cnt!$ và $(n-cnt)!$ để có đáp án, đồng thời lấy modulo.

Ở đây, ta dùng "Sieve of Eratosthenes" để đếm số nguyên tố.

Nếu $x$ là số nguyên tố thì các bội của $x$ lớn hơn $x$, chẳng hạn $2x$, $3x$, ..., chắc chắn không phải số nguyên tố. Vì vậy, ta có thể bắt đầu đánh dấu từ đây.

Gọi $primes[i]$ là giá trị cho biết $i$ có phải số nguyên tố hay không. Nếu là số nguyên tố thì giá trị là $true$, ngược lại là $false$.

Ta lần lượt duyệt từng số $i$ trong đoạn $[2,n]$. Nếu $i$ là số nguyên tố, tăng số lượng số nguyên tố thêm $1$, rồi đánh dấu tất cả bội số $j$ của nó là hợp số (ngoại trừ chính số nguyên tố đó), tức là $primes[j]=false$. Sau khi duyệt xong, ta biết được tổng số nguyên tố.

Độ phức tạp thời gian là $O(n \times \log \log n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numPrimeArrangements(self, n: int) -> int:
        def count(n):
            cnt = 0
            primes = [True] * (n + 1)
            for i in range(2, n + 1):
                if primes[i]:
                    cnt += 1
                    for j in range(i + i, n + 1, i):
                        primes[j] = False
            return cnt

        cnt = count(n)
        ans = factorial(cnt) * factorial(n - cnt)
        return ans % (10**9 + 7)
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int numPrimeArrangements(int n) {
        int cnt = count(n);
        long ans = f(cnt) * f(n - cnt);
        return (int) (ans % MOD);
    }

    private long f(int n) {
        long ans = 1;
        for (int i = 2; i <= n; ++i) {
            ans = (ans * i) % MOD;
        }
        return ans;
    }

    private int count(int n) {
        int cnt = 0;
        boolean[] primes = new boolean[n + 1];
        Arrays.fill(primes, true);
        for (int i = 2; i <= n; ++i) {
            if (primes[i]) {
                ++cnt;
                for (int j = i + i; j <= n; j += i) {
                    primes[j] = false;
                }
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
using ll = long long;
const int MOD = 1e9 + 7;

class Solution {
public:
    int numPrimeArrangements(int n) {
        int cnt = count(n);
        ll ans = f(cnt) * f(n - cnt);
        return (int) (ans % MOD);
    }

    ll f(int n) {
        ll ans = 1;
        for (int i = 2; i <= n; ++i) ans = (ans * i) % MOD;
        return ans;
    }

    int count(int n) {
        vector<bool> primes(n + 1, true);
        int cnt = 0;
        for (int i = 2; i <= n; ++i) {
            if (primes[i]) {
                ++cnt;
                for (int j = i + i; j <= n; j += i) primes[j] = false;
            }
        }
        return cnt;
    }
};
```

#### Go

```go
func numPrimeArrangements(n int) int {
	count := func(n int) int {
		cnt := 0
		primes := make([]bool, n+1)
		for i := range primes {
			primes[i] = true
		}
		for i := 2; i <= n; i++ {
			if primes[i] {
				cnt++
				for j := i + i; j <= n; j += i {
					primes[j] = false
				}
			}
		}
		return cnt
	}

	mod := int(1e9) + 7
	f := func(n int) int {
		ans := 1
		for i := 2; i <= n; i++ {
			ans = (ans * i) % mod
		}
		return ans
	}

	cnt := count(n)
	ans := f(cnt) * f(n-cnt)
	return ans % mod
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
