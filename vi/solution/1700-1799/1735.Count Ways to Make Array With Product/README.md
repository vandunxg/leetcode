---
comments: true
difficulty: Hard
rating: 2499
source: Biweekly Contest 44 Q4
tags:
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
    - Number Theory
    - Prime Factorization
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [1735. Count Ways to Make Array With Product](https://leetcode.com/problems/count-ways-to-make-array-with-product)

[中文文档](/solution/1700-1799/1735.Count%20Ways%20to%20Make%20Array%20With%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên hai chiều <code>queries</code>. Với mỗi <code>queries[i]</code>, trong đó <code>queries[i] = [n<sub>i</sub>, k<sub>i</sub>]</code>, hãy tìm số cách khác nhau để đặt các số nguyên dương vào mảng có kích thước <code>n<sub>i</sub></code> sao cho tích các số bằng <code>k<sub>i</sub></code>. Vì số cách có thể rất lớn, đáp án của truy vấn thứ <code>i<sup>th</sup></code> là số cách <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Trả về <em>mảng số nguyên </em><code>answer</code><em> sao cho </em><code>answer.length == queries.length</code><em>, và </em><code>answer[i]</code><em> là đáp án của truy vấn </em><code>i<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> queries = [[2,6],[5,1],[73,660]]
<strong>Output:</strong> [4,1,50734910]
<strong>Giải thích:</strong>&nbsp;Mỗi truy vấn độc lập.
[2,6]: Có 4 cách điền mảng kích thước 2 có tích bằng 6: [1,6], [2,3], [3,2], [6,1].
[5,1]: Có 1 cách điền mảng kích thước 5 có tích bằng 1: [1,1,1,1,1].
[73,660]: Có 1050734917 cách điền mảng kích thước 73 có tích bằng 660. 1050734917 modulo 10<sup>9</sup> + 7 = 50734910.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> queries = [[1,1],[2,2],[3,3],[4,4],[5,5]]
<strong>Output:</strong> [1,2,3,10,5]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>4</sup> </code></li>
	<li><code>1 &lt;= n<sub>i</sub>, k<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích thừa số nguyên tố + Toán tổ hợp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn biểu diễn $k$ dưới dạng tích của $n$ số nguyên dương. Liệt kê các cách phân tích sẽ quá chậm khi có nhiều truy vấn với $k\le 10^4$.
>
> Sau khi phân tích $k$, các số mũ độc lập với nhau: đặt $x$ bản sao của một số nguyên tố vào $n$ vị trí có thể rỗng có $C_{x+n-1}^{n-1}$ cách.
>
> Tính trước giai thừa, giai thừa nghịch đảo và danh sách số mũ. Nhân các tổ hợp tương ứng với từng số mũ theo modulo $10^9+7$.

<!-- thinking:end -->

Ta có thể phân tích thừa số nguyên tố của $k$, tức là $k = p_1^{x_1} \times p_2^{x_2} \times \cdots \times p_m^{x_m}$, trong đó $p_i$ là số nguyên tố và $x_i$ là số mũ của $p_i$. Bài toán tương đương với việc lần lượt đặt $x_1$ bản sao của $p_1$, $x_2$ bản sao của $p_2$, $\cdots$, $x_m$ bản sao của $p_m$ vào $n$ vị trí, trong đó mỗi vị trí có thể rỗng. Ta cần đếm số cách.

Theo toán tổ hợp, có hai trường hợp khi đặt $x$ quả bóng vào $n$ hộp:

Nếu hộp không được rỗng, số cách là $C_{x-1}^{n-1}$. Đây là phương pháp vách ngăn: có tổng cộng $x$ quả bóng và ta đặt $n-1$ vách ngăn vào $x-1$ vị trí, chia $x$ quả bóng thành $n$ nhóm.

Nếu hộp có thể rỗng, ta thêm $n$ quả bóng ảo rồi dùng phương pháp vách ngăn: có tổng cộng $x+n$ quả bóng và đặt $n-1$ vách ngăn vào $x+n-1$ vị trí, chia $x$ quả bóng thật thành $n$ nhóm và cho phép hộp rỗng. Vì vậy, số cách là $C_{x+n-1}^{n-1}$.

Do đó, với mỗi truy vấn $queries[i]$, trước tiên ta phân tích thừa số nguyên tố của $k$ để lấy các số mũ $x_1, x_2, \cdots, x_m$, sau đó tính $C_{x_1+n-1}^{n-1}, C_{x_2+n-1}^{n-1}, \cdots, C_{x_m+n-1}^{n-1}$ và cuối cùng nhân tất cả số cách.

Như vậy, bài toán trở thành tính nhanh $C_m^n$. Theo công thức $C_m^n = \frac{m!}{n!(m-n)!}$, ta có thể tính trước $m!$, rồi dùng phần tử nghịch đảo để tính nhanh $C_m^n$.

Độ phức tạp thời gian là $O(K \times \log \log K + N + m \times \log K)$, còn độ phức tạp không gian là $O(N)$.

<!-- tabs:start -->

#### Python3

```python
N = 10020
MOD = 10**9 + 7
f = [1] * N
g = [1] * N
p = defaultdict(list)
for i in range(1, N):
    f[i] = f[i - 1] * i % MOD
    g[i] = pow(f[i], MOD - 2, MOD)
    x = i
    j = 2
    while j <= x // j:
        if x % j == 0:
            cnt = 0
            while x % j == 0:
                cnt += 1
                x //= j
            p[i].append(cnt)
        j += 1
    if x > 1:
        p[i].append(1)


def comb(n, k):
    return f[n] * g[k] * g[n - k] % MOD


class Solution:
    def waysToFillArray(self, queries: List[List[int]]) -> List[int]:
        ans = []
        for n, k in queries:
            t = 1
            for x in p[k]:
                t = t * comb(x + n - 1, n - 1) % MOD
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    private static final int N = 10020;
    private static final int MOD = (int) 1e9 + 7;
    private static final long[] F = new long[N];
    private static final long[] G = new long[N];
    private static final List<Integer>[] P = new List[N];

    static {
        F[0] = 1;
        G[0] = 1;
        Arrays.setAll(P, k -> new ArrayList<>());
        for (int i = 1; i < N; ++i) {
            F[i] = F[i - 1] * i % MOD;
            G[i] = qmi(F[i], MOD - 2, MOD);
            int x = i;
            for (int j = 2; j <= x / j; ++j) {
                if (x % j == 0) {
                    int cnt = 0;
                    while (x % j == 0) {
                        ++cnt;
                        x /= j;
                    }
                    P[i].add(cnt);
                }
            }
            if (x > 1) {
                P[i].add(1);
            }
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

    public int[] waysToFillArray(int[][] queries) {
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int n = queries[i][0], k = queries[i][1];
            long t = 1;
            for (int x : P[k]) {
                t = t * comb(x + n - 1, n - 1) % MOD;
            }
            ans[i] = (int) t;
        }
        return ans;
    }
}
```

#### C++

```cpp
int N = 10020;
int MOD = 1e9 + 7;
long f[10020];
long g[10020];
vector<int> p[10020];

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
        int x = i;
        for (int j = 2; j <= x / j; ++j) {
            if (x % j == 0) {
                int cnt = 0;
                while (x % j == 0) {
                    ++cnt;
                    x /= j;
                }
                p[i].push_back(cnt);
            }
        }
        if (x > 1) {
            p[i].push_back(1);
        }
    }
    return 0;
}();

int comb(int n, int k) {
    return (f[n] * g[k] % MOD) * g[n - k] % MOD;
}

class Solution {
public:
    vector<int> waysToFillArray(vector<vector<int>>& queries) {
        vector<int> ans;
        for (auto& q : queries) {
            int n = q[0], k = q[1];
            long long t = 1;
            for (int x : p[k]) {
                t = t * comb(x + n - 1, n - 1) % MOD;
            }
            ans.push_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
const n = 1e4 + 20
const mod = 1e9 + 7

var f = make([]int, n)
var g = make([]int, n)
var p = make([][]int, n)

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
		x := i
		for j := 2; j <= x/j; j++ {
			if x%j == 0 {
				cnt := 0
				for x%j == 0 {
					cnt++
					x /= j
				}
				p[i] = append(p[i], cnt)
			}
		}
		if x > 1 {
			p[i] = append(p[i], 1)
		}
	}
}

func comb(n, k int) int {
	return (f[n] * g[k] % mod) * g[n-k] % mod
}

func waysToFillArray(queries [][]int) (ans []int) {
	for _, q := range queries {
		n, k := q[0], q[1]
		t := 1
		for _, x := range p[k] {
			t = t * comb(x+n-1, n-1) % mod
		}
		ans = append(ans, t)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
