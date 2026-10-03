---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - Math
    - String
    - Combinatorics
    - Counting
---

<!-- problem:start -->

# [2539. Count the Number of Good Subsequences 🔒](https://leetcode.com/problems/count-the-number-of-good-subsequences)

[中文文档](/solution/2500-2599/2539.Count%20the%20Number%20of%20Good%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>dãy con</strong> của một chuỗi được gọi là good nếu nó không rỗng và tần suất xuất hiện của mỗi ký tự trong đó đều bằng nhau.</p>

<p>Cho một chuỗi <code>s</code>, hãy trả về <em>số lượng dãy con good của</em> <code>s</code>. Vì đáp án có thể rất lớn, hãy trả về kết quả theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Dãy con</strong> là một chuỗi có thể được tạo ra từ một chuỗi khác bằng cách xóa một số hoặc không xóa ký tự nào mà không thay đổi thứ tự của các ký tự còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabb&quot;
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Tổng số dãy con là <code>2<sup>4</sup>. </code>Có năm dãy con không good: &quot;<strong><u>aab</u></strong>b&quot;, &quot;a<u><strong>abb</strong></u>&quot;, &quot;<strong><u>a</u></strong>a<u><strong>bb</strong></u>&quot;, &quot;<u><strong>aa</strong></u>b<strong><u>b</u></strong>&quot; và dãy con rỗng. Do đó, số lượng dãy con good là <code>2<sup>4</sup>-5 = 11</code>.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leet&quot;
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Có bốn dãy con không good: &quot;<strong><u>l</u><em>ee</em></strong>t&quot;, &quot;l<u><strong>eet</strong></u>&quot;, &quot;<strong><u>leet</u></strong>&quot; và dãy con rỗng. Do đó, số lượng dãy con good là <code>2<sup>4</sup>-4 = 12</code>.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Mọi dãy con khác rỗng đều là dãy con good. Do đó, số lượng dãy con good là <code>2<sup>4</sup>-1 = 15</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con good là dãy con không rỗng và mọi ký tự xuất hiện trong đó đều có cùng tần suất. Có $2^n$ dãy con, nhưng chỉ có $26$ ký tự, nên tần suất của các ký tự là yếu tố quyết định.
>
> Ta liệt kê tần suất chung $i=1,\ldots,mx$. Một ký tự có số lần xuất hiện $v\ge i$ có thể đóng góp $\binom{v}{i}$ lựa chọn hoặc bị bỏ qua, tức là có $\binom{v}{i}+1$ lựa chọn; các ký tự có $v<i$ buộc phải bị loại. Ta nhân các lựa chọn này trên mọi ký tự, trừ đi lựa chọn rỗng, rồi cộng kết quả theo từng $i$. Giai thừa và nghịch đảo được tính trước để tính các hệ số nhị thức.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
N = 10001
MOD = 10**9 + 7
f = [1] * N
g = [1] * N
for i in range(1, N):
    f[i] = f[i - 1] * i % MOD
    g[i] = pow(f[i], MOD - 2, MOD)


def comb(n, k):
    return f[n] * g[k] * g[n - k] % MOD


class Solution:
    def countGoodSubsequences(self, s: str) -> int:
        cnt = Counter(s)
        ans = 0
        for i in range(1, max(cnt.values()) + 1):
            x = 1
            for v in cnt.values():
                if v >= i:
                    x = x * (comb(v, i) + 1) % MOD
            ans = (ans + x - 1) % MOD
        return ans
```

#### Java

```java
class Solution {
    private static final int N = 10001;
    private static final int MOD = (int) 1e9 + 7;
    private static final long[] F = new long[N];
    private static final long[] G = new long[N];

    static {
        F[0] = 1;
        G[0] = 1;
        for (int i = 1; i < N; ++i) {
            F[i] = F[i - 1] * i % MOD;
            G[i] = qmi(F[i], MOD - 2, MOD);
        }
    }

    public static long qmi(long a, long k, long p) {
        long res = 1;
        while (k != 0) {
            if ((k & 1) == 1) {
                res = res * a % p;
            }
            k >>= 1;
            a = a * a % p;
        }
        return res;
    }

    public static long comb(int n, int k) {
        return (F[n] * G[k] % MOD) * G[n - k] % MOD;
    }

    public int countGoodSubsequences(String s) {
        int[] cnt = new int[26];
        int mx = 1;
        for (int i = 0; i < s.length(); ++i) {
            mx = Math.max(mx, ++cnt[s.charAt(i) - 'a']);
        }
        long ans = 0;
        for (int i = 1; i <= mx; ++i) {
            long x = 1;
            for (int j = 0; j < 26; ++j) {
                if (cnt[j] >= i) {
                    x = x * (comb(cnt[j], i) + 1) % MOD;
                }
            }
            ans = (ans + x - 1) % MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
int N = 10001;
int MOD = 1e9 + 7;
long f[10001];
long g[10001];

long qmi(long a, long k, long p) {
    long res = 1;
    while (k != 0) {
        if ((k & 1) == 1) {
            res = res * a % p;
        }
        k >>= 1;
        a = a * a % p;
    }
    return res;
}

int init = []() {
    f[0] = 1;
    g[0] = 1;
    for (int i = 1; i < N; ++i) {
        f[i] = f[i - 1] * i % MOD;
        g[i] = qmi(f[i], MOD - 2, MOD);
    }
    return 0;
}();

int comb(int n, int k) {
    return (f[n] * g[k] % MOD) * g[n - k] % MOD;
}

class Solution {
public:
    int countGoodSubsequences(string s) {
        int cnt[26]{};
        int mx = 1;
        for (char& c : s) {
            mx = max(mx, ++cnt[c - 'a']);
        }
        long ans = 0;
        for (int i = 1; i <= mx; ++i) {
            long x = 1;
            for (int j = 0; j < 26; ++j) {
                if (cnt[j] >= i) {
                    x = (x * (comb(cnt[j], i) + 1)) % MOD;
                }
            }
            ans = (ans + x - 1) % MOD;
        }
        return ans;
    }
};
```

#### Go

```go
const n = 1e4 + 1
const mod = 1e9 + 7

var f = make([]int, n)
var g = make([]int, n)

func qmi(a, k, p int) int {
	res := 1
	for k != 0 {
		if k&1 == 1 {
			res = res * a % p
		}
		k >>= 1
		a = a * a % p
	}
	return res
}

func init() {
	f[0], g[0] = 1, 1
	for i := 1; i < n; i++ {
		f[i] = f[i-1] * i % mod
		g[i] = qmi(f[i], mod-2, mod)
	}
}

func comb(n, k int) int {
	return (f[n] * g[k] % mod) * g[n-k] % mod
}

func countGoodSubsequences(s string) (ans int) {
	cnt := [26]int{}
	mx := 1
	for _, c := range s {
		cnt[c-'a']++
		mx = max(mx, cnt[c-'a'])
	}
	for i := 1; i <= mx; i++ {
		x := 1
		for _, v := range cnt {
			if v >= i {
				x = (x * (comb(v, i) + 1)) % mod
			}
		}
		ans = (ans + x - 1) % mod
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
