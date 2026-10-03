---
comments: true
difficulty: Hard
rating: 2620
source: Biweekly Contest 50 Q4
tags:
    - Hash Table
    - Math
    - String
    - Combinatorics
    - Counting
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [1830. Minimum Number of Operations to Make String Sorted](https://leetcode.com/problems/minimum-number-of-operations-to-make-string-sorted)

[中文文档](/solution/1800-1899/1830.Minimum%20Number%20of%20Operations%20to%20Make%20String%20Sorted/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> (<strong>đánh chỉ số từ 0</strong>)​​​​​​. Hãy thực hiện thao tác sau trên <code>s</code>​​​​​​ cho đến khi nhận được một chuỗi đã sắp xếp:</p>

<ol>
	<li>Tìm <strong>chỉ số lớn nhất</strong> <code>i</code> sao cho <code>1 &lt;= i &lt; s.length</code> và <code>s[i] &lt; s[i - 1]</code>.</li>
	<li>Tìm <strong>chỉ số lớn nhất</strong> <code>j</code> sao cho <code>i &lt;= j &lt; s.length</code> và <code>s[k] &lt; s[i - 1]</code> với mọi giá trị có thể có của <code>k</code> trong đoạn <code>[i, j]</code> (bao gồm cả hai đầu).</li>
	<li>Hoán đổi hai ký tự tại các chỉ số <code>i - 1</code>​​​​ và <code>j</code>​​​​​.</li>
	<li>Đảo ngược hậu tố bắt đầu từ chỉ số <code>i</code>​​​​​​.</li>
</ol>

<p>Trả về <em>số thao tác cần thực hiện để chuỗi được sắp xếp</em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cba&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Quá trình mô phỏng diễn ra như sau:
Thao tác 1: i=2, j=2. Hoán đổi s[1] và s[2] để được s=&quot;cab&quot;, sau đó đảo ngược hậu tố bắt đầu từ 2. Khi đó, s=&quot;cab&quot;.
Thao tác 2: i=1, j=2. Hoán đổi s[0] và s[2] để được s=&quot;bac&quot;, sau đó đảo ngược hậu tố bắt đầu từ 1. Khi đó, s=&quot;bca&quot;.
Thao tác 3: i=2, j=2. Hoán đổi s[1] và s[2] để được s=&quot;bac&quot;, sau đó đảo ngược hậu tố bắt đầu từ 2. Khi đó, s=&quot;bac&quot;.
Thao tác 4: i=1, j=1. Hoán đổi s[0] và s[1] để được s=&quot;abc&quot;, sau đó đảo ngược hậu tố bắt đầu từ 1. Khi đó, s=&quot;acb&quot;.
Thao tác 5: i=2, j=2. Hoán đổi s[1] và s[2] để được s=&quot;abc&quot;, sau đó đảo ngược hậu tố bắt đầu từ 2. Khi đó, s=&quot;abc&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabaa&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Quá trình mô phỏng diễn ra như sau:
Thao tác 1: i=3, j=4. Hoán đổi s[2] và s[4] để được s=&quot;aaaab&quot;, sau đó đảo ngược chuỗi con bắt đầu từ 3. Khi đó, s=&quot;aaaba&quot;.
Thao tác 2: i=4, j=4. Hoán đổi s[3] và s[4] để được s=&quot;aaaab&quot;, sau đó đảo ngược chuỗi con bắt đầu từ 4. Khi đó, s=&quot;aaaab&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 3000</code></li>
	<li><code>s</code>​​​​​​ chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Hoán vị và tổ hợp + Tiền xử lý

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác đưa chuỗi về hoán vị trước đó, vì vậy đáp án là số hoán vị nhỏ hơn nghiêm ngặt $s$. Với $|s|\le 3000$, ta không thể liệt kê tất cả các hoán vị.
>
> Tại mỗi vị trí, chọn một chữ cái nhỏ hơn chữ cái hiện tại rồi nối với bất kỳ hoán vị nào của đa tập còn lại sẽ tạo thành một chuỗi nhỏ hơn. Giai thừa và nghịch đảo giai thừa được tính trước để tính $m\times(n-i-1)!/\prod n_c!$ theo modulo $10^9+7$; sau đó giảm số lần xuất hiện của chữ cái hiện tại. Tổng các phần đóng góp này chính là đáp án.

<!-- thinking:end -->

Thao tác trong đề thực chất là tìm hoán vị trước của hoán vị hiện tại theo thứ tự từ điển. Vì vậy, ta chỉ cần tìm số hoán vị nhỏ hơn hoán vị hiện tại, đó chính là đáp án.

Ở đây ta cần giải quyết bài toán: với số lần xuất hiện của mỗi chữ cái, ta có thể tạo ra bao nhiêu hoán vị khác nhau?

Giả sử có tổng cộng $n$ chữ cái, trong đó có $n_1$ chữ cái $a$, $n_2$ chữ cái $b$ và $n_3$ chữ cái $c$. Khi đó ta có thể tạo ra $\frac{n!}{n_1! \times n_2! \times n_3!}$ hoán vị khác nhau, với $n=n_1+n_2+n_3$.

Ta có thể tính trước mọi giai thừa $f$ và nghịch đảo của các giai thừa $g$ trong bước tiền xử lý. Nghịch đảo của giai thừa có thể được tính bằng định lý nhỏ Fermat.

Tiếp theo, ta duyệt chuỗi $s$ từ trái sang phải. Với mỗi vị trí $i$, ta cần tìm số chữ cái nhỏ hơn $s[i]$, ký hiệu là $m$. Khi đó, ta có thể tạo ra $m \times \frac{(n - i - 1)!}{n_1! \times n_2! \cdots \times n_k!}$ hoán vị khác nhau, trong đó $k$ là số loại chữ cái, rồi cộng số này vào đáp án. Sau đó, ta giảm số lần xuất hiện của $s[i]$ đi một và tiếp tục với vị trí kế tiếp.

Sau khi duyệt toàn bộ chuỗi, ta nhận được đáp án. Lưu ý thực hiện phép modulo cho đáp án.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $k$ lần lượt là độ dài chuỗi và số loại chữ cái.

<!-- tabs:start -->

#### Python3

```python
n = 3010
mod = 10**9 + 7
f = [1] + [0] * n
g = [1] + [0] * n

for i in range(1, n):
    f[i] = f[i - 1] * i % mod
    g[i] = pow(f[i], mod - 2, mod)


class Solution:
    def makeStringSorted(self, s: str) -> int:
        cnt = Counter(s)
        ans, n = 0, len(s)
        for i, c in enumerate(s):
            m = sum(v for a, v in cnt.items() if a < c)
            t = f[n - i - 1] * m
            for v in cnt.values():
                t = t * g[v] % mod
            ans = (ans + t) % mod
            cnt[c] -= 1
            if cnt[c] == 0:
                cnt.pop(c)
        return ans
```

#### Java

```java
class Solution {
    private static final int N = 3010;
    private static final int MOD = (int) 1e9 + 7;
    private static final long[] f = new long[N];
    private static final long[] g = new long[N];

    static {
        f[0] = 1;
        g[0] = 1;
        for (int i = 1; i < N; ++i) {
            f[i] = f[i - 1] * i % MOD;
            g[i] = qmi(f[i], MOD - 2);
        }
    }

    public static long qmi(long a, int k) {
        long res = 1;
        while (k != 0) {
            if ((k & 1) == 1) {
                res = res * a % MOD;
            }
            k >>= 1;
            a = a * a % MOD;
        }
        return res;
    }

    public int makeStringSorted(String s) {
        int[] cnt = new int[26];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            int m = 0;
            for (int j = s.charAt(i) - 'a' - 1; j >= 0; --j) {
                m += cnt[j];
            }
            long t = m * f[n - i - 1] % MOD;
            for (int v : cnt) {
                t = t * g[v] % MOD;
            }
            --cnt[s.charAt(i) - 'a'];
            ans = (ans + t + MOD) % MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
const int N = 3010;
const int MOD = 1e9 + 7;
long f[N];
long g[N];

long qmi(long a, int k) {
    long res = 1;
    while (k != 0) {
        if ((k & 1) == 1) {
            res = res * a % MOD;
        }
        k >>= 1;
        a = a * a % MOD;
    }
    return res;
}

int init = []() {
    f[0] = g[0] = 1;
    for (int i = 1; i < N; ++i) {
        f[i] = f[i - 1] * i % MOD;
        g[i] = qmi(f[i], MOD - 2);
    }
    return 0;
}();

class Solution {
public:
    int makeStringSorted(string s) {
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        int n = s.size();
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            int m = 0;
            for (int j = s[i] - 'a' - 1; ~j; --j) {
                m += cnt[j];
            }
            long t = m * f[n - i - 1] % MOD;
            for (int& v : cnt) {
                t = t * g[v] % MOD;
            }
            ans = (ans + t + MOD) % MOD;
            --cnt[s[i] - 'a'];
        }
        return ans;
    }
};
```

#### Go

```go
const n = 3010
const mod = 1e9 + 7

var f = make([]int, n)
var g = make([]int, n)

func qmi(a, k int) int {
	res := 1
	for k != 0 {
		if k&1 == 1 {
			res = res * a % mod
		}
		k >>= 1
		a = a * a % mod
	}
	return res
}

func init() {
	f[0], g[0] = 1, 1
	for i := 1; i < n; i++ {
		f[i] = f[i-1] * i % mod
		g[i] = qmi(f[i], mod-2)
	}
}

func makeStringSorted(s string) (ans int) {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	for i, c := range s {
		m := 0
		for j := int(c-'a') - 1; j >= 0; j-- {
			m += cnt[j]
		}
		t := m * f[len(s)-i-1] % mod
		for _, v := range cnt {
			t = t * g[v] % mod
		}
		ans = (ans + t + mod) % mod
		cnt[c-'a']--
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
