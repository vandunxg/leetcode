---
comments: true
difficulty: Hard
rating: 2344
source: Weekly Contest 320 Q4
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2478. Number of Beautiful Partitions](https://leetcode.com/problems/number-of-beautiful-partitions)

[中文文档](/solution/2400-2499/2478.Number%20of%20Beautiful%20Partitions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>, cùng hai số nguyên <code>k</code> và <code>minLength</code>.</p>

<p>Một phép phân hoạch <code>s</code> được gọi là <strong>đẹp</strong> nếu:</p>

<ul>
	<li><code>s</code> được chia thành <code>k</code> chuỗi con không giao nhau.</li>
	<li>Mỗi chuỗi con có độ dài <strong>ít nhất</strong> <code>minLength</code>.</li>
	<li>Mỗi chuỗi con bắt đầu bằng một chữ số <strong>nguyên tố</strong> và kết thúc bằng một chữ số <strong>không nguyên tố</strong>. Các chữ số nguyên tố là <code>&#39;2&#39;</code>, <code>&#39;3&#39;</code>, <code>&#39;5&#39;</code> và <code>&#39;7&#39;</code>, các chữ số còn lại là không nguyên tố.</li>
</ul>

<p>Hãy trả về <em>số lượng phép phân hoạch <strong>đẹp</strong> của </em><code>s</code>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;23542185131&quot;, k = 3, minLength = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có ba cách tạo một phép phân hoạch đẹp:
&quot;2354 | 218 | 5131&quot;
&quot;2354 | 21851 | 31&quot;
&quot;2354218 | 51 | 31&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;23542185131&quot;, k = 3, minLength = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có một cách tạo phép phân hoạch đẹp: &quot;2354 | 218 | 5131&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;3312958&quot;, k = 3, minLength = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có một cách tạo phép phân hoạch đẹp: &quot;331 | 29 | 58&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k, minLength &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đoạn bắt đầu bằng một chữ số nguyên tố và kết thúc bằng một chữ số không nguyên tố, có độ dài ít nhất $\textit{minLength}$, tổng cộng có $k$ đoạn. Với $n\le 1000$, $f[i][j]$ là số cách chia $i$ ký tự đầu tiên thành $j$ đoạn. Một vị trí chỉ có thể là điểm kết thúc hợp lệ nếu ký tự tại đó không nguyên tố và ký tự tiếp theo là số nguyên tố (hoặc đó là cuối chuỗi).
>
> Mảng tổng tiền tố $g$ gộp các điểm kết thúc trước đó thành $g[i-\textit{minLength}][j-1]$. Nếu chữ số đầu tiên không nguyên tố hoặc chữ số cuối cùng là số nguyên tố, đáp án là $0$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách chia $i$ ký tự đầu tiên thành $j$ đoạn. Khởi tạo $f[0][0] = 1$, các giá trị còn lại $f[i][j] = 0$.

Trước tiên, ta cần xác định liệu ký tự thứ $i$ có thể là ký tự cuối của đoạn thứ $j$ hay không. Ký tự đó cần đồng thời thỏa mãn các điều kiện sau:

1. Ký tự thứ $i$ là một số không nguyên tố;
1. Ký tự thứ $i+1$ là một số nguyên tố, hoặc ký tự thứ $i$ là ký tự cuối cùng của toàn bộ chuỗi.

Nếu ký tự thứ $i$ không thể là ký tự cuối của đoạn thứ $j$, thì $f[i][j]=0$. Ngược lại, ta có:

$$
f[i][j]=\sum_{t=0}^{i-minLength}f[t][j-1]
$$

Nói cách khác, ta cần duyệt qua ký tự là điểm kết thúc của đoạn trước đó. Ở đây, ta sử dụng mảng tổng tiền tố $g[i][j] = \sum_{t=0}^{i}f[t][j]$ để tối ưu độ phức tạp thời gian của việc duyệt.

Khi đó:

$$
f[i][j]=g[i-minLength][j-1]
$$

Độ phức tạp thời gian là $O(n \times k)$, và độ phức tạp không gian là $O(n \times k)$. Trong đó, $n$ và $k$ lần lượt là độ dài chuỗi $s$ và số đoạn cần chia.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautifulPartitions(self, s: str, k: int, minLength: int) -> int:
        primes = '2357'
        if s[0] not in primes or s[-1] in primes:
            return 0
        mod = 10**9 + 7
        n = len(s)
        f = [[0] * (k + 1) for _ in range(n + 1)]
        g = [[0] * (k + 1) for _ in range(n + 1)]
        f[0][0] = g[0][0] = 1
        for i, c in enumerate(s, 1):
            if i >= minLength and c not in primes and (i == n or s[i] in primes):
                for j in range(1, k + 1):
                    f[i][j] = g[i - minLength][j - 1]
            for j in range(k + 1):
                g[i][j] = (g[i - 1][j] + f[i][j]) % mod
        return f[n][k]
```

#### Java

```java
class Solution {
    public int beautifulPartitions(String s, int k, int minLength) {
        int n = s.length();
        if (!prime(s.charAt(0)) || prime(s.charAt(n - 1))) {
            return 0;
        }
        int[][] f = new int[n + 1][k + 1];
        int[][] g = new int[n + 1][k + 1];
        f[0][0] = 1;
        g[0][0] = 1;
        final int mod = (int) 1e9 + 7;
        for (int i = 1; i <= n; ++i) {
            if (i >= minLength && !prime(s.charAt(i - 1)) && (i == n || prime(s.charAt(i)))) {
                for (int j = 1; j <= k; ++j) {
                    f[i][j] = g[i - minLength][j - 1];
                }
            }
            for (int j = 0; j <= k; ++j) {
                g[i][j] = (g[i - 1][j] + f[i][j]) % mod;
            }
        }
        return f[n][k];
    }

    private boolean prime(char c) {
        return c == '2' || c == '3' || c == '5' || c == '7';
    }
}
```

#### C++

```cpp
class Solution {
public:
    int beautifulPartitions(string s, int k, int minLength) {
        int n = s.size();
        auto prime = [](char c) {
            return c == '2' || c == '3' || c == '5' || c == '7';
        };
        if (!prime(s[0]) || prime(s[n - 1])) return 0;
        vector<vector<int>> f(n + 1, vector<int>(k + 1));
        vector<vector<int>> g(n + 1, vector<int>(k + 1));
        f[0][0] = g[0][0] = 1;
        const int mod = 1e9 + 7;
        for (int i = 1; i <= n; ++i) {
            if (i >= minLength && !prime(s[i - 1]) && (i == n || prime(s[i]))) {
                for (int j = 1; j <= k; ++j) {
                    f[i][j] = g[i - minLength][j - 1];
                }
            }
            for (int j = 0; j <= k; ++j) {
                g[i][j] = (g[i - 1][j] + f[i][j]) % mod;
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func beautifulPartitions(s string, k int, minLength int) int {
	prime := func(c byte) bool {
		return c == '2' || c == '3' || c == '5' || c == '7'
	}
	n := len(s)
	if !prime(s[0]) || prime(s[n-1]) {
		return 0
	}
	const mod int = 1e9 + 7
	f := make([][]int, n+1)
	g := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
		g[i] = make([]int, k+1)
	}
	f[0][0], g[0][0] = 1, 1
	for i := 1; i <= n; i++ {
		if i >= minLength && !prime(s[i-1]) && (i == n || prime(s[i])) {
			for j := 1; j <= k; j++ {
				f[i][j] = g[i-minLength][j-1]
			}
		}
		for j := 0; j <= k; j++ {
			g[i][j] = (g[i-1][j] + f[i][j]) % mod
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function beautifulPartitions(s: string, k: number, minLength: number): number {
    const prime = (c: string): boolean => {
        return c === '2' || c === '3' || c === '5' || c === '7';
    };

    const n: number = s.length;
    if (!prime(s[0]) || prime(s[n - 1])) return 0;

    const f: number[][] = new Array(n + 1).fill(0).map(() => new Array(k + 1).fill(0));
    const g: number[][] = new Array(n + 1).fill(0).map(() => new Array(k + 1).fill(0));
    const mod: number = 1e9 + 7;

    f[0][0] = g[0][0] = 1;

    for (let i = 1; i <= n; ++i) {
        if (i >= minLength && !prime(s[i - 1]) && (i === n || prime(s[i]))) {
            for (let j = 1; j <= k; ++j) {
                f[i][j] = g[i - minLength][j - 1];
            }
        }
        for (let j = 0; j <= k; ++j) {
            g[i][j] = (g[i - 1][j] + f[i][j]) % mod;
        }
    }

    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
