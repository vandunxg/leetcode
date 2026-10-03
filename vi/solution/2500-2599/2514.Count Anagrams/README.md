---
comments: true
difficulty: Hard
rating: 2069
source: Biweekly Contest 94 Q4
tags:
    - Hash Table
    - Math
    - String
    - Combinatorics
    - Counting
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [2514. Count Anagrams](https://leetcode.com/problems/count-anagrams)

[中文文档](/solution/2500-2599/2514.Count%20Anagrams/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chứa một hoặc nhiều từ. Mỗi cặp từ liên tiếp được ngăn cách bởi một dấu cách duy nhất <code>&#39; &#39;</code>.</p>

<p>Chuỗi <code>t</code> là một <strong>anagram</strong> của chuỗi <code>s</code> nếu từ ở vị trí <code>i<sup>th</sup></code> của <code>t</code> là một <strong>hoán vị</strong> của từ ở vị trí <code>i<sup>th</sup></code> của <code>s</code>.</p>

<ul>
	<li>Ví dụ, <code>&quot;acb dfe&quot;</code> là một anagram của <code>&quot;abc def&quot;</code>, nhưng <code>&quot;def cab&quot;</code>&nbsp;và <code>&quot;adc bef&quot;</code> thì không.</li>
</ul>

<p>Trả về <em>số lượng <strong>anagram khác nhau</strong> của </em><code>s</code>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;too hot&quot;
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Một số anagram của chuỗi đã cho là &quot;too hot&quot;, &quot;oot hot&quot;, &quot;oto toh&quot;, &quot;too toh&quot; và &quot;too oht&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aa&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có một anagram có thể tạo thành từ chuỗi đã cho.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và dấu cách <code>&#39; &#39;</code>.</li>
	<li>Giữa hai từ liên tiếp có đúng một dấu cách.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi được tách thành các từ theo dấu cách; đáp án là tích số anagram khác nhau của từng từ, lấy modulo $10^9+7$. Tổng độ dài là $10^5$, nên không thể liệt kê tất cả các hoán vị.
>
> Một từ $w$ có $|w|!\,/\,\prod(c_i!)$ anagram, trong đó $c_i$ là số lần xuất hiện của các chữ cái. Khi duyệt, $\textit{ans}$ nhân với chỉ số hiện tại (tức giai thừa), còn $\textit{mul}$ nhân với số lần xuất hiện tích lũy của chữ cái hiện tại (tức mẫu số). Nhân với nghịch đảo modulo của $\textit{mul}$ sẽ hoàn tất việc tính cho mọi từ chỉ trong một lượt duyệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countAnagrams(self, s: str) -> int:
        mod = 10**9 + 7
        ans = mul = 1
        for w in s.split():
            cnt = Counter()
            for i, c in enumerate(w, 1):
                cnt[c] += 1
                mul = mul * cnt[c] % mod
                ans = ans * i % mod
        return ans * pow(mul, -1, mod) % mod
```

#### Java

```java
import java.math.BigInteger;

class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countAnagrams(String s) {
        int n = s.length();
        long[] f = new long[n + 1];
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            f[i] = f[i - 1] * i % MOD;
        }
        long p = 1;
        for (String w : s.split(" ")) {
            int[] cnt = new int[26];
            for (int i = 0; i < w.length(); ++i) {
                ++cnt[w.charAt(i) - 'a'];
            }
            p = p * f[w.length()] % MOD;
            for (int v : cnt) {
                p = p * BigInteger.valueOf(f[v]).modInverse(BigInteger.valueOf(MOD)).intValue()
                    % MOD;
            }
        }
        return (int) p;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countAnagrams(string s) {
        stringstream ss(s);
        string w;
        long ans = 1, mul = 1;
        while (ss >> w) {
            int cnt[26] = {0};
            for (int i = 1; i <= w.size(); ++i) {
                int c = w[i - 1] - 'a';
                ++cnt[c];
                ans = ans * i % mod;
                mul = mul * cnt[c] % mod;
            }
        }
        return ans * pow(mul, mod - 2) % mod;
    }

    long pow(long x, int n) {
        long res = 1L;
        for (; n; n /= 2) {
            if (n % 2) res = res * x % mod;
            x = x * x % mod;
        }
        return res;
    }
};
```

#### Go

```go
const mod int = 1e9 + 7

func countAnagrams(s string) int {
	ans, mul := 1, 1
	for _, w := range strings.Split(s, " ") {
		cnt := [26]int{}
		for i, c := range w {
			i++
			cnt[c-'a']++
			ans = ans * i % mod
			mul = mul * cnt[c-'a'] % mod
		}
	}
	return ans * pow(mul, mod-2) % mod
}

func pow(x, n int) int {
	res := 1
	for ; n > 0; n >>= 1 {
		if n&1 > 0 {
			res = res * x % mod
		}
		x = x * x % mod
	}
	return res
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
