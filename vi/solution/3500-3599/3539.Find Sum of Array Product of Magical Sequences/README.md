---
comments: true
difficulty: Hard
rating: 2693
source: Weekly Contest 448 Q4
tags:
    - Bit Manipulation
    - Array
    - Math
    - Dynamic Programming
    - Bitmask
    - Combinatorics
---

<!-- problem:start -->

# [3539. Find Sum of Array Product of Magical Sequences](https://leetcode.com/problems/find-sum-of-array-product-of-magical-sequences)

[中文文档](/solution/3500-3599/3539.Find%20Sum%20of%20Array%20Product%20of%20Magical%20Sequences/README.md)

## Mô tả

<!-- description:start -->
<p>Cho hai số nguyên <code>m</code> và <code>k</code>, cùng một mảng số nguyên <code>nums</code>.</p>
Một dãy số nguyên <code>seq</code> được gọi là <strong>magical</strong> nếu:

<ul>
    <li><code>seq</code> có độ dài <code>m</code>.</li>
    <li><code>0 &lt;= seq[i] &lt; nums.length</code></li>
    <li><strong>Biểu diễn nhị phân</strong> của <code>2<sup>seq[0]</sup> + 2<sup>seq[1]</sup> + ... + 2<sup>seq[m - 1]</sup></code> có <code>k</code> <strong>bit bật</strong>.</li>
</ul>

<p><strong>Tích mảng</strong> của dãy này được định nghĩa là <code>prod(seq) = (nums[seq[0]] * nums[seq[1]] * ... * nums[seq[m - 1]])</code>.</p>

<p>Trả về <strong>tổng</strong> của <strong>tích mảng</strong> của tất cả các dãy <strong>magical</strong> hợp lệ.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Bit bật</strong> là bit trong biểu diễn nhị phân của một số có giá trị bằng 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 5, k = 5, nums = [1,10,100,10000,1000000]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">991600007</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi hoán vị của <code>[0, 1, 2, 3, 4]</code> đều là các dãy magical, mỗi dãy có tích mảng bằng 10<sup>13</sup>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, k = 2, nums = [5,4,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">170</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy magical là <code>[0, 1]</code>, <code>[0, 2]</code>, <code>[0, 3]</code>, <code>[0, 4]</code>, <code>[1, 0]</code>, <code>[1, 2]</code>, <code>[1, 3]</code>, <code>[1, 4]</code>, <code>[2, 0]</code>, <code>[2, 1]</code>, <code>[2, 3]</code>, <code>[2, 4]</code>, <code>[3, 0]</code>, <code>[3, 1]</code>, <code>[3, 2]</code>, <code>[3, 4]</code>, <code>[4, 0]</code>, <code>[4, 1]</code>, <code>[4, 2]</code> và <code>[4, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 1, k = 1, nums = [28]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">28</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy magical duy nhất là <code>[0]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= k &lt;= m &lt;= 30</code></li>
    <li><code>1 &lt;= nums.length &lt;= 50</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổ hợp + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy độ dài $m$ được chọn từ $\textit{nums}$; phần đóng góp của tích phụ thuộc vào popcount của vector tần suất sau khi thực hiện phép nhớ nhị phân. Việc liệt kê tất cả các dãy là bất khả thi.
>
> Gán số lần xuất hiện $t$ cho $\textit{nums}[i]$ với trọng số $\binom{j}{t} \cdot \textit{nums}[i]^t$. Cập nhật phần nhớ và popcount còn lại từ $t+\textit{st}$. Ghi nhớ kết quả của $\textit{dfs}(i,j,k,\textit{st})$ và dùng giai thừa nghịch đảo để tính các hệ số nhị thức.

<!-- thinking:end -->

Ta xây dựng hàm $\text{dfs}(i, j, k, st)$, biểu diễn số cách khi đang xử lý phần tử thứ $i$ của mảng $\textit{nums}$, còn cần chọn số từ $j$ vị trí còn lại để điền vào dãy magical, còn cần thỏa mãn có $k$ bit bật trong biểu diễn nhị phân, và phần nhớ hiện tại từ bit trước đó là $st$. Khi đó, đáp án là $\text{dfs}(0, m, k, 0)$.

Quy trình thực thi hàm $\text{dfs}(i, j, k, st)$ như sau:

Nếu $k < 0$ hoặc $i = n$ và $j > 0$, nghĩa là nghiệm hiện tại không hợp lệ, trả về $0$.

Nếu $i = n$, nghĩa là đã xử lý xong mảng $\textit{nums}$. Ta cần kiểm tra xem trong phần nhớ hiện tại $st$ còn bit bật nào không; nếu có, ta cần giảm $k$. Nếu tại thời điểm này $k = 0$, nghĩa là nghiệm hiện tại hợp lệ, trả về $1$; ngược lại, trả về $0$.

Trong các trường hợp còn lại, ta duyệt số $t$ phần tử được chọn tại vị trí $i$ để điền vào dãy magical ($0 \leq t \leq j$). Số cách điền $t$ phần tử vào dãy magical là $\binom{j}{t}$, tích mảng là $\textit{nums}[i]^t$, phần nhớ được cập nhật thành $(t + st) >> 1$, số bit bật cần thỏa mãn được cập nhật thành $k - ((t + st) \& 1)$, sau đó ta gọi đệ quy $\text{dfs}(i + 1, j - t, k - ((t + st) \& 1), (t + st) >> 1)$. Tổng của tất cả các nghiệm theo $t$ là $\text{dfs}(i, j, k, st)$.

Để tính hiệu quả hệ số nhị thức $\binom{m}{n}$, ta tiền xử lý mảng giai thừa $f$ và mảng giai thừa nghịch đảo $g$, trong đó $f[i] = i! \mod (10^9 + 7)$ và $g[i] = (i!)^{-1} \mod (10^9 + 7)$. Khi đó $\binom{m}{n} = f[m] \cdot g[n] \cdot g[m - n] \mod (10^9 + 7)$.

Độ phức tạp thời gian là $O(n \cdot m^3 \cdot k)$ và độ phức tạp không gian là $O(n \cdot m^2 \cdot k)$, trong đó $n$ là độ dài của mảng $\textit{nums}$, còn $m$ và $k$ là các tham số trong đề bài.

<!-- tabs:start -->

#### Python3

```python
mx = 30
mod = 10**9 + 7
f = [1] + [0] * mx
g = [1] + [0] * mx

for i in range(1, mx + 1):
    f[i] = f[i - 1] * i % mod
    g[i] = pow(f[i], mod - 2, mod)


def comb(m: int, n: int) -> int:
    return f[m] * g[n] * g[m - n] % mod


class Solution:
    def magicalSum(self, m: int, k: int, nums: List[int]) -> int:
        @cache
        def dfs(i: int, j: int, k: int, st: int) -> int:
            if k < 0 or (i == len(nums) and j > 0):
                return 0
            if i == len(nums):
                while st:
                    k -= st & 1
                    st >>= 1
                return int(k == 0)
            res = 0
            for t in range(j + 1):
                nt = t + st
                p = pow(nums[i], t, mod)
                nk = k - (nt & 1)
                res += comb(j, t) * p * dfs(i + 1, j - t, nk, nt >> 1)
                res %= mod
            return res

        ans = dfs(0, m, k, 0)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    static final int N = 31;
    static final long MOD = 1_000_000_007L;
    private static final long[] f = new long[N];
    private static final long[] g = new long[N];
    private Long[][][][] dp;

    static {
        f[0] = 1;
        g[0] = 1;
        for (int i = 1; i < N; ++i) {
            f[i] = f[i - 1] * i % MOD;
            g[i] = qpow(f[i], MOD - 2);
        }
    }

    public static long qpow(long a, long k) {
        long res = 1;
        while (k != 0) {
            if ((k & 1) == 1) {
                res = res * a % MOD;
            }
            a = a * a % MOD;
            k >>= 1;
        }
        return res;
    }

    public static long comb(int m, int n) {
        return f[m] * g[n] % MOD * g[m - n] % MOD;
    }

    public int magicalSum(int m, int k, int[] nums) {
        int n = nums.length;
        dp = new Long[n + 1][m + 1][k + 1][N];
        long ans = dfs(0, m, k, 0, nums);
        return (int) ans;
    }

    private long dfs(int i, int j, int k, int st, int[] nums) {
        if (k < 0 || (i == nums.length && j > 0)) {
            return 0;
        }
        if (i == nums.length) {
            while (st > 0) {
                k -= (st & 1);
                st >>= 1;
            }
            return k == 0 ? 1 : 0;
        }

        if (dp[i][j][k][st] != null) {
            return dp[i][j][k][st];
        }

        long res = 0;
        for (int t = 0; t <= j; t++) {
            int nt = t + st;
            int nk = k - (nt & 1);
            long p = qpow(nums[i], t);
            long tmp = comb(j, t) * p % MOD * dfs(i + 1, j - t, nk, nt >> 1, nums) % MOD;
            res = (res + tmp) % MOD;
        }

        return dp[i][j][k][st] = res;
    }
}
```

#### C++

```cpp
const int N = 31;
const long long MOD = 1'000'000'007;

long long f[N], g[N];

long long qpow(long long a, long long k) {
    long long res = 1;
    while (k) {
        if (k & 1) res = res * a % MOD;
        a = a * a % MOD;
        k >>= 1;
    }
    return res;
}

int init = []() {
    f[0] = g[0] = 1;
    for (int i = 1; i < N; ++i) {
        f[i] = f[i - 1] * i % MOD;
        g[i] = qpow(f[i], MOD - 2);
    }
    return 0;
}();

long long comb(int m, int n) {
    return f[m] * g[n] % MOD * g[m - n] % MOD;
}

class Solution {
    vector<vector<vector<vector<long long>>>> dp;

    long long dfs(int i, int j, int k, int st) {
        if (k < 0 || (i == nums.size() && j > 0)) {
            return 0;
        }
        if (i == nums.size()) {
            while (st > 0) {
                k -= (st & 1);
                st >>= 1;
            }
            return k == 0 ? 1 : 0;
        }

        long long& res = dp[i][j][k][st];
        if (res != -1) {
            return res;
        }

        res = 0;
        for (int t = 0; t <= j; ++t) {
            int nt = t + st;
            int nk = k - (nt & 1);
            long long p = qpow(nums[i], t);
            long long tmp = comb(j, t) * p % MOD * dfs(i + 1, j - t, nk, nt >> 1) % MOD;
            res = (res + tmp) % MOD;
        }
        return res;
    }

public:
    int magicalSum(int m, int k, vector<int>& nums) {
        int n = nums.size();
        this->nums = nums;
        dp.assign(n + 1, vector<vector<vector<long long>>>(m + 1, vector<vector<long long>>(k + 1, vector<long long>(N, -1))));
        return dfs(0, m, k, 0);
    }

private:
    vector<int> nums;
};
```

#### Go

```go
const N = 31
const MOD = 1_000_000_007

var f [N]int64
var g [N]int64

func init() {
    f[0], g[0] = 1, 1
    for i := 1; i < N; i++ {
        f[i] = f[i-1] * int64(i) % MOD
        g[i] = qpow(f[i], MOD-2)
    }
}

func qpow(a, k int64) int64 {
    res := int64(1)
    for k > 0 {
        if k&1 == 1 {
            res = res * a % MOD
        }
        a = a * a % MOD
        k >>= 1
    }
    return res
}

func comb(m, n int) int64 {
    if n < 0 || n > m {
        return 0
    }
    return f[m] * g[n] % MOD * g[m-n] % MOD
}

func magicalSum(m int, k int, nums []int) int {
    n := len(nums)
    dp := make([][][][]int64, n+1)
    for i := 0; i <= n; i++ {
        dp[i] = make([][][]int64, m+1)
        for j := 0; j <= m; j++ {
            dp[i][j] = make([][]int64, k+1)
            for l := 0; l <= k; l++ {
                dp[i][j][l] = make([]int64, N)
                for s := 0; s < N; s++ {
                    dp[i][j][l][s] = -1
                }
            }
        }
    }

    var dfs func(i, j, k, st int) int64
    dfs = func(i, j, k, st int) int64 {
        if k < 0 || (i == n && j > 0) {
            return 0
        }
        if i == n {
            for st > 0 {
                k -= st & 1
                st >>= 1
            }
            if k == 0 {
                return 1
            }
            return 0
        }
        if dp[i][j][k][st] != -1 {
            return dp[i][j][k][st]
        }
        res := int64(0)
        for t := 0; t <= j; t++ {
            nt := t + st
            nk := k - (nt & 1)
            p := qpow(int64(nums[i]), int64(t))
            tmp := comb(j, t) * p % MOD * dfs(i+1, j-t, nk, nt>>1) % MOD
            res = (res + tmp) % MOD
        }
        dp[i][j][k][st] = res
        return res
    }

    return int(dfs(0, m, k, 0))
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
