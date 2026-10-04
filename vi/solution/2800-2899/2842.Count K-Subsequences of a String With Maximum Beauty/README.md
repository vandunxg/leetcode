---
comments: true
difficulty: Hard
rating: 2091
source: Biweekly Contest 112 Q4
tags:
    - Greedy
    - Hash Table
    - Math
    - String
    - Combinatorics
    - Sorting
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [2842. Count K-Subsequences of a String With Maximum Beauty](https://leetcode.com/problems/count-k-subsequences-of-a-string-with-maximum-beauty)

[中文文档](/solution/2800-2899/2842.Count%20K-Subsequences%20of%20a%20String%20With%20Maximum%20Beauty/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Một <strong>k-subsequence</strong> là một <strong>subsequence</strong> của <code>s</code>, có độ dài <code>k</code> và tất cả các ký tự đều <strong>khác nhau</strong>, <strong>tức là</strong> mỗi ký tự xuất hiện đúng một lần.</p>

<p>Gọi <code>f(c)</code> là số lần ký tự <code>c</code> xuất hiện trong <code>s</code>.</p>

<p><strong>Độ đẹp</strong> của một <strong>k-subsequence</strong> là <strong>tổng</strong> của <code>f(c)</code> với mọi ký tự <code>c</code> trong k-subsequence.</p>

<p>Ví dụ, xét <code>s = &quot;abbbdd&quot;</code> và <code>k = 2</code>:</p>

<ul>
	<li><code>f(&#39;a&#39;) = 1</code>, <code>f(&#39;b&#39;) = 3</code>, <code>f(&#39;d&#39;) = 2</code></li>
	<li>Một số k-subsequence của <code>s</code> là:
	<ul>
		<li><code>&quot;<u><strong>ab</strong></u>bbdd&quot;</code> -&gt; <code>&quot;ab&quot;</code> có độ đẹp bằng <code>f(&#39;a&#39;) + f(&#39;b&#39;) = 4</code></li>
		<li><code>&quot;<u><strong>a</strong></u>bbb<strong><u>d</u></strong>d&quot;</code> -&gt; <code>&quot;ad&quot;</code> có độ đẹp bằng <code>f(&#39;a&#39;) + f(&#39;d&#39;) = 3</code></li>
		<li><code>&quot;a<strong><u>b</u></strong>bb<u><strong>d</strong></u>d&quot;</code> -&gt; <code>&quot;bd&quot;</code> có độ đẹp bằng <code>f(&#39;b&#39;) + f(&#39;d&#39;) = 5</code></li>
	</ul>
	</li>
</ul>

<p>Trả về <em>một số nguyên biểu thị số lượng k-subsequence </em><em>có <strong>độ đẹp</strong> <strong>lớn nhất</strong> trong tất cả các <strong>k-subsequence</strong></em>. Vì đáp án có thể rất lớn, hãy trả về kết quả theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>Một subsequence của một chuỗi là một chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào) mà không làm thay đổi vị trí tương đối của các ký tự còn lại.</p>

<p><strong>Lưu ý</strong></p>

<ul>
	<li><code>f(c)</code> là số lần ký tự <code>c</code> xuất hiện trong <code>s</code>, không phải trong một k-subsequence.</li>
	<li>Hai k-subsequence được xem là khác nhau nếu một subsequence được tạo bởi một chỉ số không xuất hiện trong subsequence còn lại. Vì vậy, hai k-subsequence có thể tạo thành cùng một chuỗi.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bcca&quot;, k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> <span style="white-space: normal">Từ s, ta có f(&#39;a&#39;) = 1, f(&#39;b&#39;) = 1 và f(&#39;c&#39;) = 2.</span>
Các k-subsequence của s là:
<strong><u>bc</u></strong>ca có độ đẹp bằng f(&#39;b&#39;) + f(&#39;c&#39;) = 3
<strong><u>b</u></strong>c<u><strong>c</strong></u>a có độ đẹp bằng f(&#39;b&#39;) + f(&#39;c&#39;) = 3
<strong><u>b</u></strong>cc<strong><u>a</u></strong> có độ đẹp bằng f(&#39;b&#39;) + f(&#39;a&#39;) = 2
b<strong><u>c</u></strong>c<u><strong>a</strong></u><strong> </strong>có độ đẹp bằng f(&#39;c&#39;) + f(&#39;a&#39;) = 3
bc<strong><u>ca</u></strong> có độ đẹp bằng f(&#39;c&#39;) + f(&#39;a&#39;) = 3
Có 4 k-subsequence có độ đẹp lớn nhất là 3.
Vì vậy, đáp án là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbcd&quot;, k = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Từ s, ta có f(&#39;a&#39;) = 1, f(&#39;b&#39;) = 2, f(&#39;c&#39;) = 1 và f(&#39;d&#39;) = 1.
Các k-subsequence của s là:
<u><strong>ab</strong></u>b<strong><u>cd</u></strong> có độ đẹp bằng f(&#39;a&#39;) + f(&#39;b&#39;) + f(&#39;c&#39;) + f(&#39;d&#39;) = 5
<u style="white-space: normal;"><strong>a</strong></u>b<u><strong>bcd</strong></u> có độ đẹp bằng f(&#39;a&#39;) + f(&#39;b&#39;) + f(&#39;c&#39;) + f(&#39;d&#39;) = 5
Có 2 k-subsequence có độ đẹp lớn nhất là 5.
Vì vậy, đáp án là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Combinatorial Mathematics

<!-- thinking:start -->

> **Tư duy**
>
> Độ đẹp là tổng tần suất của $k$ ký tự phân biệt. Ta nên chọn các tần suất lớn nhất. Sau khi sắp xếp tần suất theo thứ tự giảm dần, mọi giá trị lớn hơn giá trị thứ $k$ đều phải được chọn; các ký tự có cùng tần suất tại ngưỡng này được chọn bằng hệ số nhị thức và đóng góp tần suất đó lũy thừa số vị trí còn lại.

<!-- thinking:end -->

Đầu tiên, chúng ta dùng một bảng băm $f$ để đếm số lần xuất hiện của mỗi ký tự trong chuỗi $s$, tức là $f[c]$ biểu thị số lần ký tự $c$ xuất hiện trong chuỗi $s$.

Vì một $k$-subsequence là một subsequence có độ dài $k$ trong chuỗi $s$ với các ký tự khác nhau, nếu số ký tự khác nhau trong $f$ nhỏ hơn $k$ thì không tồn tại $k$-subsequence, và ta có thể trả về $0$ ngay lập tức.

Ngược lại, để tối đa hóa độ đẹp của $k$-subsequence, ta cần đưa các ký tự có độ đẹp cao vào $k$-subsequence nhiều nhất có thể. Do đó, ta sắp xếp các giá trị trong $f$ theo thứ tự giảm dần để thu được một mảng $vs$.

Gọi số lần xuất hiện của ký tự thứ $k$ trong mảng $vs$ là $val$, và có $x$ ký tự xuất hiện $val$ lần.

Trước tiên, ta tìm các ký tự có số lần xuất hiện lớn hơn $val$, nhân số lần xuất hiện của từng ký tự để thu được đáp án ban đầu $ans$, đồng thời cập nhật số ký tự còn lại cần chọn về $k$. Ta cần chọn $k$ ký tự từ $x$ ký tự, nên đáp án cần được nhân với số tổ hợp $C_x^k$, rồi cuối cùng nhân với $val^k$, tức là $ans = ans \times C_x^k \times val^k$.

Lưu ý rằng ở đây ta cần sử dụng lũy thừa nhanh và phép modulo.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài chuỗi và $\Sigma$ là tập ký tự. Trong bài toán này, tập ký tự là các chữ cái viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKSubsequencesWithMaxBeauty(self, s: str, k: int) -> int:
        f = Counter(s)
        if len(f) < k:
            return 0
        mod = 10**9 + 7
        vs = sorted(f.values(), reverse=True)
        val = vs[k - 1]
        x = vs.count(val)
        ans = 1
        for v in vs:
            if v == val:
                break
            k -= 1
            ans = ans * v % mod
        ans = ans * comb(x, k) * pow(val, k, mod) % mod
        return ans
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int countKSubsequencesWithMaxBeauty(String s, int k) {
        int[] f = new int[26];
        int n = s.length();
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            if (++f[s.charAt(i) - 'a'] == 1) {
                ++cnt;
            }
        }
        if (cnt < k) {
            return 0;
        }
        Integer[] vs = new Integer[cnt];
        for (int i = 0, j = 0; i < 26; ++i) {
            if (f[i] > 0) {
                vs[j++] = f[i];
            }
        }
        Arrays.sort(vs, (a, b) -> b - a);
        long ans = 1;
        int val = vs[k - 1];
        int x = 0;
        for (int v : vs) {
            if (v == val) {
                ++x;
            }
        }
        for (int v : vs) {
            if (v == val) {
                break;
            }
            --k;
            ans = ans * v % mod;
        }
        int[][] c = new int[x + 1][x + 1];
        for (int i = 0; i <= x; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= i; ++j) {
                c[i][j] = (c[i - 1][j - 1] + c[i - 1][j]) % mod;
            }
        }
        ans = ((ans * c[x][k]) % mod) * qpow(val, k) % mod;
        return (int) ans;
    }

    private long qpow(long a, int n) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countKSubsequencesWithMaxBeauty(string s, int k) {
        int f[26]{};
        int cnt = 0;
        for (char& c : s) {
            if (++f[c - 'a'] == 1) {
                ++cnt;
            }
        }
        if (cnt < k) {
            return 0;
        }
        vector<int> vs(cnt);
        for (int i = 0, j = 0; i < 26; ++i) {
            if (f[i]) {
                vs[j++] = f[i];
            }
        }
        sort(vs.rbegin(), vs.rend());
        const int mod = 1e9 + 7;
        long long ans = 1;
        int val = vs[k - 1];
        int x = 0;
        for (int v : vs) {
            x += v == val;
        }
        for (int v : vs) {
            if (v == val) {
                break;
            }
            --k;
            ans = ans * v % mod;
        }
        int c[x + 1][x + 1];
        memset(c, 0, sizeof(c));
        for (int i = 0; i <= x; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= i; ++j) {
                c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
            }
        }
        auto qpow = [&](long long a, int n) {
            long long ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return ans;
        };
        ans = (ans * c[x][k] % mod) * qpow(val, k) % mod;
        return ans;
    }
};
```

#### Go

```go
func countKSubsequencesWithMaxBeauty(s string, k int) int {
	f := [26]int{}
	cnt := 0
	for _, c := range s {
		f[c-'a']++
		if f[c-'a'] == 1 {
			cnt++
		}
	}
	if cnt < k {
		return 0
	}
	vs := []int{}
	for _, x := range f {
		if x > 0 {
			vs = append(vs, x)
		}
	}
	sort.Slice(vs, func(i, j int) bool {
		return vs[i] > vs[j]
	})
	const mod int = 1e9 + 7
	ans := 1
	val := vs[k-1]
	x := 0
	for _, v := range vs {
		if v == val {
			x++
		}
	}
	for _, v := range vs {
		if v == val {
			break
		}
		k--
		ans = ans * v % mod
	}
	c := make([][]int, x+1)
	for i := range c {
		c[i] = make([]int, x+1)
		c[i][0] = 1
		for j := 1; j <= i; j++ {
			c[i][j] = (c[i-1][j-1] + c[i-1][j]) % mod
		}
	}
	qpow := func(a, n int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	ans = (ans * c[x][k] % mod) * qpow(val, k) % mod
	return ans
}
```

#### TypeScript

```ts
function countKSubsequencesWithMaxBeauty(s: string, k: number): number {
    const f: number[] = new Array(26).fill(0);
    let cnt = 0;
    for (const c of s) {
        const i = c.charCodeAt(0) - 97;
        if (++f[i] === 1) {
            ++cnt;
        }
    }
    if (cnt < k) {
        return 0;
    }
    const mod = BigInt(10 ** 9 + 7);
    const vs: number[] = f.filter(v => v > 0).sort((a, b) => b - a);
    const val = vs[k - 1];
    const x = vs.filter(v => v === val).length;
    let ans = 1n;
    for (const v of vs) {
        if (v === val) {
            break;
        }
        --k;
        ans = (ans * BigInt(v)) % mod;
    }
    const c: number[][] = new Array(x + 1).fill(0).map(() => new Array(k + 1).fill(0));
    for (let i = 0; i <= x; ++i) {
        c[i][0] = 1;
        for (let j = 1; j <= Math.min(i, k); ++j) {
            c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % Number(mod);
        }
    }
    const qpow = (a: bigint, n: number): bigint => {
        let ans = 1n;
        for (; n; n >>>= 1) {
            if (n & 1) {
                ans = (ans * a) % BigInt(mod);
            }
            a = (a * a) % BigInt(mod);
        }
        return ans;
    };
    ans = (((ans * BigInt(c[x][k])) % mod) * qpow(BigInt(val), k)) % mod;
    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
