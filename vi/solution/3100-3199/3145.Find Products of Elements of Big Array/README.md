---
comments: true
difficulty: Hard
rating: 2859
source: Biweekly Contest 130 Q4
tags:
    - Bit Manipulation
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3145. Find Products of Elements of Big Array](https://leetcode.com/problems/find-products-of-elements-of-big-array)

[中文文档](/solution/3100-3199/3145.Find%20Products%20of%20Elements%20of%20Big%20Array/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Mảng powerful</strong> của một số nguyên không âm <code>x</code> được định nghĩa là mảng ngắn nhất, đã sắp xếp, gồm các lũy thừa của hai có tổng bằng <code>x</code>. Bảng dưới đây minh họa cách xác định <strong>mảng powerful</strong>. Có thể chứng minh rằng mảng powerful của <code>x</code> là duy nhất.</p>

<table border="1">
	<tbody>
		<tr>
			<th>num</th>
			<th>Biểu diễn nhị phân</th>
			<th>mảng powerful</th>
		</tr>
		<tr>
			<td>1</td>
			<td>0000<u>1</u></td>
			<td>[1]</td>
		</tr>
		<tr>
			<td>8</td>
			<td>0<u>1</u>000</td>
			<td>[8]</td>
		</tr>
		<tr>
			<td>10</td>
			<td>0<u>1</u>0<u>1</u>0</td>
			<td>[2, 8]</td>
		</tr>
		<tr>
			<td>13</td>
			<td>0<u>11</u>0<u>1</u></td>
			<td>[1, 4, 8]</td>
		</tr>
		<tr>
			<td>23</td>
			<td><u>1</u>0<u>111</u></td>
			<td>[1, 2, 4, 16]</td>
		</tr>
	</tbody>
</table>

<p>Mảng <code>big_nums</code> được tạo bằng cách nối các <strong>mảng powerful</strong> của mọi số nguyên dương <code>i</code> theo thứ tự tăng dần: 1, 2, 3, ... Do đó, <code>big_nums</code> bắt đầu bằng <code>[<u>1</u>, <u>2</u>, <u>1, 2</u>, <u>4</u>, <u>1, 4</u>, <u>2, 4</u>, <u>1, 2, 4</u>, <u>8</u>, ...]</code>.</p>

<p>Cho ma trận số nguyên 2D <code>queries</code>. Với <code>queries[i] = [from<sub>i</sub>, to<sub>i</sub>, mod<sub>i</sub>]</code>, hãy tính <code>(big_nums[from<sub>i</sub>] * big_nums[from<sub>i</sub> + 1] * ... * big_nums[to<sub>i</sub>]) % mod<sub>i</sub></code><!-- notionvc: a71131cc-7b52-4786-9a4b-660d6d864f89 -->.</p>

<p>Trả về mảng số nguyên <code>answer</code> sao cho <code>answer[i]</code> là đáp án của truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = [[1,3,7]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có một truy vấn.</p>

<p><code>big_nums[1..3] = [2,1,2]</code>. Tích của các phần tử này là 4. Kết quả là <code>4 % 7 = 4.</code></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = [[2,5,3],[7,7,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có hai truy vấn.</p>

<p>Truy vấn thứ nhất: <code>big_nums[2..5] = [1,2,4,1]</code>. Tích của các phần tử này là 8. Kết quả là <code>8 % 3 = 2</code>.</p>

<p>Truy vấn thứ hai: <code>big_nums[7] = 2</code>. Kết quả là <code>2 % 4 = 2</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queries.length &lt;= 500</code></li>
	<li><code>queries[i].length == 3</code></li>
	<li><code>0 &lt;= queries[i][0] &lt;= queries[i][1] &lt;= 10<sup>15</sup></code></li>
	<li><code>1 &lt;= queries[i][2] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Mảng lớn nối các lũy thừa của hai tương ứng với các bit 1 của mọi số nguyên dương. Tích trên đoạn $[L,R]$ là một lũy thừa của hai, với số mũ bằng tổng trên đoạn, và các chỉ số có thể lên tới $10^{15}$.
>
> Tổng tiền tố của số mũ $f(i)$ cho đoạn cần tìm bằng $f(R+1)-f(L)$. Độ dài và tổng số mũ của các số từ $1..x$ có công thức đóng theo bit cao nhất, nên có thể tìm kiếm nhị phân số $x$ chứa chỉ số đó.
>
> Tiền xử lý số lượng và tổng trên tiền tố gồm $50$ bit, tìm $x$ lớn nhất sao cho độ dài mảng strong của nó nhỏ hơn $i$, sau đó cộng thêm các bit thấp còn lại. Mỗi truy vấn trả về $2^{f(R+1)-f(L)}\bmod \textit{mod}$.

<!-- thinking:end -->

Các số nguyên dương liên tiếp tương ứng với mảng số nguyên strong, tạo thành mảng $\textit{bignums}$. Bài toán yêu cầu tìm kết quả của tích của mảng con $\textit{bignums}[\textit{left}..\textit{right}]$ modulo $\textit{mod}$ cho mỗi truy vấn $[\textit{left}, \textit{right}, \textit{mod}]$. Vì mỗi phần tử của mảng con là một lũy thừa của 2, điều này tương đương với việc tìm tổng các số mũ $\textit{power}$ của mảng con, sau đó tính $2^{\textit{power}} \bmod \textit{mod}$. Ví dụ, với mảng con $[1, 4, 8]$, tức là $[2^0, 2^2, 2^3]$, tổng các số mũ là $0 + 2 + 3 = 5$, nên kết quả cần tìm là $2^5 \bmod \textit{mod}$.

Do đó, ta có thể chuyển $\textit{bignums}$ thành một mảng các số mũ. Ví dụ, mảng con $[1, 4, 8]$ được chuyển thành $[0, 2, 3]$. Khi đó, bài toán trở thành tìm tổng của mảng con các số mũ, tức là $\textit{power} = \textit{f}(\textit{right} + 1) - \textit{f}(\textit{left})$, trong đó $\textit{f}(i)$ biểu diễn tổng các số mũ của $\textit{bignums}[0..i)$, chính là tổng tiền tố.

Tiếp theo, ta tính giá trị của $\textit{f}(i)$ dựa trên chỉ số $i$. Ta có thể dùng tìm kiếm nhị phân để tìm số lớn nhất có độ dài mảng strong nhỏ hơn $i$, rồi tính tổng các số mũ của những số còn lại.

Theo mô tả đề bài, ta liệt kê các số strong từ $0..14$:

| $\textit{nums}$ | 8($2^3$)                              | 4($2^2$)                              | 2($2^1$)                              | ($2^0$)                               |
| --------------- | ------------------------------------- | ------------------------------------- | ------------------------------------- | ------------------------------------- |
| 0               | 0                                     | 0                                     | 0                                     | 0                                     |
| 1               | <span style="color: red;">0</span>    | <span style="color: red;">0</span>    | <span style="color: red;">0</span>    | <span style="color: red;">1</span>    |
| 2               | <span style="color: blue;">0</span>   | <span style="color: blue;">0</span>   | <span style="color: blue;">1</span>   | <span style="color: blue;">0</span>   |
| 3               | <span style="color: blue;">0</span>   | <span style="color: blue;">0</span>   | <span style="color: blue;">1</span>   | <span style="color: blue;">1</span>   |
| 4               | <span style="color: green;">0</span>  | <span style="color: green;">1</span>  | <span style="color: green;">0</span>  | <span style="color: green;">0</span>  |
| 5               | <span style="color: green;">0</span>  | <span style="color: green;">1</span>  | <span style="color: green;">0</span>  | <span style="color: green;">1</span>  |
| 6               | <span style="color: green;">0</span>  | <span style="color: green;">1</span>  | <span style="color: green;">1</span>  | <span style="color: green;">0</span>  |
| 7               | <span style="color: green;">0</span>  | <span style="color: green;">1</span>  | <span style="color: green;">1</span>  | <span style="color: green;">1</span>  |
| 8               | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">0</span> |
| 9               | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">1</span> |
| 10              | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> |
| 11              | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">1</span> | <span style="color: yellow;">1</span> |
| 12              | <span style="color: yellow;">1</span> | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">0</span> |
| 13              | <span style="color: yellow;">1</span> | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> | <span style="color: yellow;">1</span> |
| 14              | <span style="color: yellow;">1</span> | <span style="color: yellow;">1</span> | <span style="color: yellow;">1</span> | <span style="color: yellow;">0</span> |

Chia các số thành những nhóm màu khác nhau theo đoạn $[2^i, 2^{i+1}-1]$, ta thấy các số trong đoạn $[2^i, 2^{i+1}-1]$ tương đương với việc cộng $2^i$ vào mỗi số trong đoạn $[0, 2^i-1]$. Dựa trên quy luật này, ta có thể tính tổng số mảng strong $\textit{cnt}[i]$ và tổng các số mũ $\textit{s}[i]$ của $i$ nhóm số đầu tiên trong $\textit{bignums}$.

Tiếp theo, với một số bất kỳ, ta xét cách tính số lượng mảng strong và tổng các số mũ. Ta có thể dùng phương pháp nhị phân, tính từ bit cao nhất. Ví dụ, với số $13 = 2^3 + 2^2 + 2^0$, kết quả của $2^3$ số đầu tiên có thể lấy từ $\textit{cnt}[3]$ và $\textit{s}[3]$, còn kết quả của đoạn còn lại $[2^3, 13]$ tương đương với việc cộng $3$ vào tất cả các số trong $[0, 13-2^3]$, tức là $[0, 5]$. Bài toán được chuyển thành tính số lượng mảng strong và tổng các số mũ của $[0, 5]$. Theo cách này, ta có thể tính số lượng mảng strong và tổng các số mũ của bất kỳ số nào.

Cuối cùng, dựa trên giá trị của $\textit{power}$, ta dùng lũy thừa nhanh để tính kết quả của $2^{\textit{power}} \bmod \textit{mod}$.

Độ phức tạp thời gian là $O(q \times \log M)$, và độ phức tạp không gian là $O(\log M)$. Trong đó, $q$ là số lượng truy vấn, còn $M$ là cận trên của số cần xét, với $M \le 10^{15}$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
m = 50
cnt = [0] * (m + 1)
s = [0] * (m + 1)
p = 1
for i in range(1, m + 1):
    cnt[i] = cnt[i - 1] * 2 + p
    s[i] = s[i - 1] * 2 + p * (i - 1)
    p *= 2


def num_idx_and_sum(x: int) -> tuple:
    idx = 0
    total_sum = 0
    while x:
        i = x.bit_length() - 1
        idx += cnt[i]
        total_sum += s[i]
        x -= 1 << i
        total_sum += (x + 1) * i
        idx += x + 1
    return (idx, total_sum)


def f(i: int) -> int:
    l, r = 0, 1 << m
    while l < r:
        mid = (l + r + 1) >> 1
        idx, _ = num_idx_and_sum(mid)
        if idx < i:
            l = mid
        else:
            r = mid - 1

    total_sum = 0
    idx, total_sum = num_idx_and_sum(l)
    i -= idx
    x = l + 1
    for _ in range(i):
        y = x & -x
        total_sum += y.bit_length() - 1
        x -= y
    return total_sum


class Solution:
    def findProductsOfElements(self, queries: List[List[int]]) -> List[int]:
        return [pow(2, f(right + 1) - f(left), mod) for left, right, mod in queries]
```

#### Java

```java
class Solution {
    private static final int M = 50;
    private static final long[] cnt = new long[M + 1];
    private static final long[] s = new long[M + 1];

    static {
        long p = 1;
        for (int i = 1; i <= M; i++) {
            cnt[i] = cnt[i - 1] * 2 + p;
            s[i] = s[i - 1] * 2 + p * (i - 1);
            p *= 2;
        }
    }

    private static long[] numIdxAndSum(long x) {
        long idx = 0;
        long totalSum = 0;
        while (x > 0) {
            int i = Long.SIZE - Long.numberOfLeadingZeros(x) - 1;
            idx += cnt[i];
            totalSum += s[i];
            x -= 1L << i;
            totalSum += (x + 1) * i;
            idx += x + 1;
        }
        return new long[] {idx, totalSum};
    }

    private static long f(long i) {
        long l = 0;
        long r = 1L << M;
        while (l < r) {
            long mid = (l + r + 1) >> 1;
            long[] idxAndSum = numIdxAndSum(mid);
            long idx = idxAndSum[0];
            if (idx < i) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        long[] idxAndSum = numIdxAndSum(l);
        long totalSum = idxAndSum[1];
        long idx = idxAndSum[0];
        i -= idx;
        long x = l + 1;
        for (int j = 0; j < i; j++) {
            long y = x & -x;
            totalSum += Long.numberOfTrailingZeros(y);
            x -= y;
        }
        return totalSum;
    }

    public int[] findProductsOfElements(long[][] queries) {
        int n = queries.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; i++) {
            long left = queries[i][0];
            long right = queries[i][1];
            long mod = queries[i][2];
            long power = f(right + 1) - f(left);
            ans[i] = qpow(2, power, mod);
        }
        return ans;
    }

    private int qpow(long a, long n, long mod) {
        long ans = 1 % mod;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
using ll = long long;
const int m = 50;
ll cnt[m + 1];
ll s[m + 1];
ll p = 1;

auto init = [] {
    cnt[0] = 0;
    s[0] = 0;
    for (int i = 1; i <= m; ++i) {
        cnt[i] = cnt[i - 1] * 2 + p;
        s[i] = s[i - 1] * 2 + p * (i - 1);
        p *= 2;
    }
    return 0;
}();

pair<ll, ll> numIdxAndSum(ll x) {
    ll idx = 0;
    ll totalSum = 0;
    while (x > 0) {
        int i = 63 - __builtin_clzll(x);
        idx += cnt[i];
        totalSum += s[i];
        x -= 1LL << i;
        totalSum += (x + 1) * i;
        idx += x + 1;
    }
    return make_pair(idx, totalSum);
}

ll f(ll i) {
    ll l = 0;
    ll r = 1LL << m;
    while (l < r) {
        ll mid = (l + r + 1) >> 1;
        auto idxAndSum = numIdxAndSum(mid);
        ll idx = idxAndSum.first;
        if (idx < i) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }

    auto idxAndSum = numIdxAndSum(l);
    ll totalSum = idxAndSum.second;
    ll idx = idxAndSum.first;
    i -= idx;
    ll x = l + 1;
    for (int j = 0; j < i; ++j) {
        ll y = x & -x;
        totalSum += __builtin_ctzll(y);
        x -= y;
    }
    return totalSum;
}

ll qpow(ll a, ll n, ll mod) {
    ll ans = 1 % mod;
    a = a % mod;
    while (n > 0) {
        if (n & 1) {
            ans = ans * a % mod;
        }
        a = a * a % mod;
        n >>= 1;
    }
    return ans;
}

class Solution {
public:
    vector<int> findProductsOfElements(vector<vector<ll>>& queries) {
        int n = queries.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            ll left = queries[i][0];
            ll right = queries[i][1];
            ll mod = queries[i][2];
            ll power = f(right + 1) - f(left);
            if (power < 0) {
                power += mod;
            }
            ans[i] = static_cast<int>(qpow(2, power, mod));
        }
        return ans;
    }
};
```

#### Go

```go
const m = 50

var cnt [m + 1]int64
var s [m + 1]int64
var p int64 = 1

func init() {
	cnt[0] = 0
	s[0] = 0
	for i := 1; i <= m; i++ {
		cnt[i] = cnt[i-1]*2 + p
		s[i] = s[i-1]*2 + p*(int64(i)-1)
		p *= 2
	}
}

func numIdxAndSum(x int64) (int64, int64) {
	var idx, totalSum int64
	for x > 0 {
		i := 63 - bits.LeadingZeros64(uint64(x))
		idx += cnt[i]
		totalSum += s[i]
		x -= 1 << i
		totalSum += (x + 1) * int64(i)
		idx += x + 1
	}
	return idx, totalSum
}

func f(i int64) int64 {
	l, r := int64(0), int64(1)<<m
	for l < r {
		mid := (l + r + 1) >> 1
		idx, _ := numIdxAndSum(mid)
		if idx < i {
			l = mid
		} else {
			r = mid - 1
		}
	}

	_, totalSum := numIdxAndSum(l)
	idx, _ := numIdxAndSum(l)
	i -= idx
	x := l + 1
	for j := int64(0); j < i; j++ {
		y := x & -x
		totalSum += int64(bits.TrailingZeros64(uint64(y)))
		x -= y
	}
	return totalSum
}

func qpow(a, n, mod int64) int64 {
	ans := int64(1) % mod
	a = a % mod
	for n > 0 {
		if n&1 == 1 {
			ans = (ans * a) % mod
		}
		a = (a * a) % mod
		n >>= 1
	}
	return ans
}

func findProductsOfElements(queries [][]int64) []int {
	ans := make([]int, len(queries))
	for i, q := range queries {
		left, right, mod := q[0], q[1], q[2]
		power := f(right+1) - f(left)
		ans[i] = int(qpow(2, power, mod))
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
