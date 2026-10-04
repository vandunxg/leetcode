---
comments: true
difficulty: Medium
rating: 2227
source: Biweekly Contest 117 Q3
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [2930. Number of Strings Which Can Be Rearranged to Contain Substring](https://leetcode.com/problems/number-of-strings-which-can-be-rearranged-to-contain-substring)

[中文文档](/solution/2900-2999/2930.Number%20of%20Strings%20Which%20Can%20Be%20Rearranged%20to%20Contain%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một số nguyên <code>n</code>.</p>

<p>Một chuỗi <code>s</code> được gọi là <strong>tốt </strong> nếu nó chỉ chứa các ký tự tiếng Anh viết thường <strong>và</strong> có thể sắp xếp lại các ký tự của <code>s</code> sao cho chuỗi mới chứa <code>&quot;leet&quot;</code> dưới dạng một <strong>chuỗi con</strong>.</p>

<p>Ví dụ:</p>

<ul>
	<li>Chuỗi <code>&quot;lteer&quot;</code> là chuỗi tốt vì ta có thể sắp xếp lại thành <code>&quot;leetr&quot;</code> .</li>
	<li><code>&quot;letl&quot;</code> không phải chuỗi tốt vì ta không thể sắp xếp lại nó để chứa <code>&quot;leet&quot;</code> như một chuỗi con.</li>
</ul>

<p>Trả về <em><strong>tổng số</strong> chuỗi tốt có độ dài </em><code>n</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo </strong><code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<div class="notranslate" style="all: initial;">&nbsp;</div>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> 12 chuỗi có thể được sắp xếp lại để chứa &quot;leet&quot; như một chuỗi con là: &quot;eelt&quot;, &quot;eetl&quot;, &quot;elet&quot;, &quot;elte&quot;, &quot;etel&quot;, &quot;etle&quot;, &quot;leet&quot;, &quot;lete&quot;, &quot;ltee&quot;, &quot;teel&quot;, &quot;tele&quot; và &quot;tlee&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 83943898
<strong>Giải thích:</strong> Số chuỗi có độ dài 10 có thể được sắp xếp lại để chứa &quot;leet&quot; như một chuỗi con là 526083947580. Do đó, đáp án là 526083947580 % (10<sup>9</sup> + 7) = 83943898.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi chữ thường độ dài $n$ có thể sắp xếp lại để chứa $leet$ khi và chỉ khi có ít nhất một $l$, hai $e$ và một $t$. Nếu xây dựng các chuỗi được đếm rồi nhân với số hoán vị, ta sẽ đếm trùng. Ta có thể điền từng vị trí và theo dõi việc ba điều kiện này đã được thỏa mãn hay chưa, khi đó có $n \times 2 \times 3 \times 2$ trạng thái.
>
> $dfs(i,l,e,t)$ thử các lựa chọn “khác”, $l$, $e$ hoặc $t$, đồng thời giới hạn ba giá trị cuối lần lượt ở $1,2,1$. Với $n \le 10^5$, kích thước bộ nhớ đệm tương ứng với không gian trạng thái.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i, l, e, t)$, biểu thị số chuỗi tốt có thể tạo ra khi phần còn lại của chuỗi có độ dài $i$, đồng thời đã có ít nhất $l$ ký tự 'l', $e$ ký tự 'e' và $t$ ký tự 't'. Đáp án là $dfs(n, 0, 0, 0)$.

Logic thực thi của hàm $dfs(i, l, e, t)$ như sau:

Nếu $i = 0$, nghĩa là chuỗi hiện tại đã được xây dựng xong. Nếu $l = 1$, $e = 2$ và $t = 1$, thì chuỗi hiện tại là chuỗi tốt, trả về $1$; ngược lại trả về $0$.

Nếu không, ta có thể xét việc thêm một chữ cái viết thường bất kỳ khác 'l', 'e', 't' vào vị trí hiện tại. Có tổng cộng 23 lựa chọn, nên số phương án nhận được lúc này là $dfs(i - 1, l, e, t) \times 23$.

Ta cũng có thể xét việc thêm 'l' vào vị trí hiện tại, khi đó số phương án nhận được là $dfs(i - 1, \min(1, l + 1), e, t)$. Tương tự, số phương án khi thêm 'e' và 't' lần lượt là $dfs(i - 1, l, \min(2, e + 1), t)$ và $dfs(i - 1, l, e, \min(1, t + 1))$. Cộng các phương án này lại và lấy modulo $10^9 + 7$ để nhận được giá trị của $dfs(i, l, e, t)$.

Để tránh tính toán lặp lại, ta có thể sử dụng tìm kiếm có nhớ.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stringCount(self, n: int) -> int:
        @cache
        def dfs(i: int, l: int, e: int, t: int) -> int:
            if i == 0:
                return int(l == 1 and e == 2 and t == 1)
            a = dfs(i - 1, l, e, t) * 23 % mod
            b = dfs(i - 1, min(1, l + 1), e, t)
            c = dfs(i - 1, l, min(2, e + 1), t)
            d = dfs(i - 1, l, e, min(1, t + 1))
            return (a + b + c + d) % mod

        mod = 10**9 + 7
        return dfs(n, 0, 0, 0)
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;
    private Long[][][][] f;

    public int stringCount(int n) {
        f = new Long[n + 1][2][3][2];
        return (int) dfs(n, 0, 0, 0);
    }

    private long dfs(int i, int l, int e, int t) {
        if (i == 0) {
            return l == 1 && e == 2 && t == 1 ? 1 : 0;
        }
        if (f[i][l][e][t] != null) {
            return f[i][l][e][t];
        }
        long a = dfs(i - 1, l, e, t) * 23 % mod;
        long b = dfs(i - 1, Math.min(1, l + 1), e, t);
        long c = dfs(i - 1, l, Math.min(2, e + 1), t);
        long d = dfs(i - 1, l, e, Math.min(1, t + 1));
        return f[i][l][e][t] = (a + b + c + d) % mod;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stringCount(int n) {
        const int mod = 1e9 + 7;
        using ll = long long;
        ll f[n + 1][2][3][2];
        memset(f, -1, sizeof(f));
        function<ll(int, int, int, int)> dfs = [&](int i, int l, int e, int t) -> ll {
            if (i == 0) {
                return l == 1 && e == 2 && t == 1 ? 1 : 0;
            }
            if (f[i][l][e][t] != -1) {
                return f[i][l][e][t];
            }
            ll a = dfs(i - 1, l, e, t) * 23 % mod;
            ll b = dfs(i - 1, min(1, l + 1), e, t) % mod;
            ll c = dfs(i - 1, l, min(2, e + 1), t) % mod;
            ll d = dfs(i - 1, l, e, min(1, t + 1)) % mod;
            return f[i][l][e][t] = (a + b + c + d) % mod;
        };
        return dfs(n, 0, 0, 0);
    }
};
```

#### Go

```go
func stringCount(n int) int {
	const mod int = 1e9 + 7
	f := make([][2][3][2]int, n+1)
	for i := range f {
		for j := range f[i] {
			for k := range f[i][j] {
				for l := range f[i][j][k] {
					f[i][j][k][l] = -1
				}
			}
		}
	}
	var dfs func(i, l, e, t int) int
	dfs = func(i, l, e, t int) int {
		if i == 0 {
			if l == 1 && e == 2 && t == 1 {
				return 1
			}
			return 0
		}
		if f[i][l][e][t] == -1 {
			a := dfs(i-1, l, e, t) * 23 % mod
			b := dfs(i-1, min(1, l+1), e, t)
			c := dfs(i-1, l, min(2, e+1), t)
			d := dfs(i-1, l, e, min(1, t+1))
			f[i][l][e][t] = (a + b + c + d) % mod
		}
		return f[i][l][e][t]
	}
	return dfs(n, 0, 0, 0)
}
```

#### TypeScript

```ts
function stringCount(n: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[][][][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: 2 }, () =>
            Array.from({ length: 3 }, () => Array.from({ length: 2 }, () => -1)),
        ),
    );
    const dfs = (i: number, l: number, e: number, t: number): number => {
        if (i === 0) {
            return l === 1 && e === 2 && t === 1 ? 1 : 0;
        }
        if (f[i][l][e][t] !== -1) {
            return f[i][l][e][t];
        }
        const a = (dfs(i - 1, l, e, t) * 23) % mod;
        const b = dfs(i - 1, Math.min(1, l + 1), e, t);
        const c = dfs(i - 1, l, Math.min(2, e + 1), t);
        const d = dfs(i - 1, l, e, Math.min(1, t + 1));
        return (f[i][l][e][t] = (a + b + c + d) % mod);
    };
    return dfs(n, 0, 0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tư duy ngược + Nguyên lý bao hàm - loại trừ

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 duyệt theo độ dài và thực hiện số chuyển trạng thái tuyến tính. Phần bù ngắn gọn hơn: lấy $26^n$ trừ đi các chuỗi thiếu $l$, thiếu $t$ hoặc có ít hơn hai $e$, rồi khôi phục các giao của chúng bằng nguyên lý bao hàm - loại trừ.
>
> Mỗi tập hợp đều cấm một số ký tự hoặc giới hạn số lượng $e$, nên có thể dùng lũy thừa nhanh để tính $25^n$, $24^n$ và các hạng tử “không có hoặc chỉ có một $e$”. Với $n$ lớn, cách này gọn hơn quy hoạch động theo từng bước.

<!-- thinking:end -->

Ta có thể tư duy ngược, tức là tính số chuỗi không chứa chuỗi con "leet", sau đó lấy tổng số chuỗi trừ đi số này.

Ta chia thành các trường hợp sau:

- Trường hợp $a$: biểu thị số phương án mà chuỗi không chứa ký tự 'l', nên $a = 25^n$.
- Trường hợp $b$: tương tự $a$, biểu thị số phương án mà chuỗi không chứa ký tự 't', nên $b = 25^n$.
- Trường hợp $c$: biểu thị số phương án mà chuỗi không chứa ký tự 'e' hoặc chỉ chứa một ký tự 'e', nên $c = 25^n + n \times 25^{n - 1}$.
- Trường hợp $ab$: biểu thị số phương án mà chuỗi không chứa các ký tự 'l' và 't', nên $ab = 24^n$.
- Trường hợp $ac$: biểu thị số phương án mà chuỗi không chứa các ký tự 'l' và 'e', hoặc chỉ chứa một ký tự 'e', nên $ac = 24^n + n \times 24^{n - 1}$.
- Trường hợp $bc$: tương tự $ac$, biểu thị số phương án mà chuỗi không chứa các ký tự 't' và 'e', hoặc chỉ chứa một ký tự 'e', nên $bc = 24^n + n \times 24^{n - 1}$.
- Trường hợp $abc$: biểu thị số phương án mà chuỗi không chứa các ký tự 'l', 't' và 'e', hoặc chỉ chứa một ký tự 'e', nên $abc = 23^n + n \times 23^{n - 1}$.

Theo nguyên lý bao hàm - loại trừ, $a + b + c - ab - ac - bc + abc$ là số chuỗi không chứa chuỗi con "leet".

Tổng số chuỗi là $tot = 26^n$, vậy đáp án là $tot - (a + b + c - ab - ac - bc + abc)$. Đừng quên lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stringCount(self, n: int) -> int:
        mod = 10**9 + 7
        a = b = pow(25, n, mod)
        c = pow(25, n, mod) + n * pow(25, n - 1, mod)
        ab = pow(24, n, mod)
        ac = bc = (pow(24, n, mod) + n * pow(24, n - 1, mod)) % mod
        abc = (pow(23, n, mod) + n * pow(23, n - 1, mod)) % mod
        tot = pow(26, n, mod)
        return (tot - (a + b + c - ab - ac - bc + abc)) % mod
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int stringCount(int n) {
        long a = qpow(25, n);
        long b = a;
        long c = (qpow(25, n) + n * qpow(25, n - 1) % mod) % mod;
        long ab = qpow(24, n);
        long ac = (qpow(24, n) + n * qpow(24, n - 1) % mod) % mod;
        long bc = ac;
        long abc = (qpow(23, n) + n * qpow(23, n - 1) % mod) % mod;
        long tot = qpow(26, n);
        return (int) ((tot - (a + b + c - ab - ac - bc + abc)) % mod + mod) % mod;
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
    int stringCount(int n) {
        const int mod = 1e9 + 7;
        using ll = long long;
        auto qpow = [&](ll a, int n) {
            ll ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return ans;
        };
        ll a = qpow(25, n);
        ll b = a;
        ll c = (qpow(25, n) + n * qpow(25, n - 1) % mod) % mod;
        ll ab = qpow(24, n);
        ll ac = (qpow(24, n) + n * qpow(24, n - 1) % mod) % mod;
        ll bc = ac;
        ll abc = (qpow(23, n) + n * qpow(23, n - 1) % mod) % mod;
        ll tot = qpow(26, n);
        return ((tot - (a + b + c - ab - ac - bc + abc)) % mod + mod) % mod;
    }
};
```

#### Go

```go
func stringCount(n int) int {
	const mod int = 1e9 + 7
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
	a := qpow(25, n)
	b := a
	c := qpow(25, n) + n*qpow(25, n-1)
	ab := qpow(24, n)
	ac := (qpow(24, n) + n*qpow(24, n-1)) % mod
	bc := ac
	abc := (qpow(23, n) + n*qpow(23, n-1)) % mod
	tot := qpow(26, n)
	return ((tot-(a+b+c-ab-ac-bc+abc))%mod + mod) % mod
}
```

#### TypeScript

```ts
function stringCount(n: number): number {
    const mod = BigInt(10 ** 9 + 7);
    const qpow = (a: bigint, n: number): bigint => {
        let ans = 1n;
        for (; n; n >>>= 1) {
            if (n & 1) {
                ans = (ans * a) % mod;
            }
            a = (a * a) % mod;
        }
        return ans;
    };
    const a = qpow(25n, n);
    const b = a;
    const c = (qpow(25n, n) + ((BigInt(n) * qpow(25n, n - 1)) % mod)) % mod;
    const ab = qpow(24n, n);
    const ac = (qpow(24n, n) + ((BigInt(n) * qpow(24n, n - 1)) % mod)) % mod;
    const bc = ac;
    const abc = (qpow(23n, n) + ((BigInt(n) * qpow(23n, n - 1)) % mod)) % mod;
    const tot = qpow(26n, n);
    return Number((((tot - (a + b + c - ab - ac - bc + abc)) % mod) + mod) % mod);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
