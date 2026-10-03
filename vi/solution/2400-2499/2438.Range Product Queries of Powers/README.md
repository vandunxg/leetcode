---
comments: true
difficulty: Medium
rating: 1609
source: Biweekly Contest 89 Q2
tags:
    - Bit Manipulation
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2438. Range Product Queries of Powers](https://leetcode.com/problems/range-product-queries-of-powers)

[中文文档](/solution/2400-2499/2438.Range%20Product%20Queries%20of%20Powers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>, tồn tại một mảng <strong>có chỉ số bắt đầu từ 0</strong> có tên là <code>powers</code> được tạo thành từ <strong>số lượng nhỏ nhất</strong> các lũy thừa của <code>2</code> sao cho tổng của chúng bằng <code>n</code>. Mảng được sắp xếp theo thứ tự <strong>không giảm</strong>, và chỉ có <strong>một</strong> cách duy nhất để tạo ra mảng này.</p>

<p>Đồng thời, bạn được cung cấp một mảng số nguyên 2 chiều <code>queries</code> có <strong>chỉ số bắt đầu từ 0</strong>, trong đó <code>queries[i] = [left<sub>i</sub>, right<sub>i</sub>]</code>. Mỗi <code>queries[i]</code> biểu diễn một truy vấn yêu cầu tính tích của tất cả <code>powers[j]</code> với <code>left<sub>i</sub> &lt;= j &lt;= right<sub>i</sub></code>.</p>

<p>Trả về <em>một mảng </em><code>answers</code><em> có cùng độ dài với </em><code>queries</code><em>, trong đó </em><code>answers[i]</code><em> là đáp án của truy vấn thứ </em><code>i<sup>th</sup></code><em>. Vì đáp án của truy vấn thứ <code>i<sup>th</sup></code> có thể rất lớn, mỗi <code>answers[i]</code> cần được trả về theo </em><strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 15, queries = [[0,1],[2,2],[0,3]]
<strong>Đầu ra:</strong> [2,4,64]
<strong>Giải thích:</strong>
Với n = 15, powers = [1,2,4,8]. Có thể chứng minh rằng powers không thể có ít phần tử hơn.
Đáp án của truy vấn thứ nhất: powers[0] * powers[1] = 1 * 2 = 2.
Đáp án của truy vấn thứ hai: powers[2] = 4.
Đáp án của truy vấn thứ ba: powers[0] * powers[1] * powers[2] * powers[3] = 1 * 2 * 4 * 8 = 64.
Mỗi đáp án lấy modulo 10<sup>9</sup> + 7 đều cho cùng kết quả, nên trả về [2,4,64].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, queries = [[0,0]]
<strong>Đầu ra:</strong> [2]
<strong>Giải thích:</strong>
Với n = 2, powers = [2].
Đáp án của truy vấn duy nhất là powers[0] = 2. Đáp án lấy modulo 10<sup>9</sup> + 7 vẫn là chính nó, nên trả về [2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt; powers.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bit Manipulation + Simulation

<!-- thinking:start -->

> **Tư duy**
>
> Biểu diễn $n$ dưới dạng tổng các lũy thừa tăng dần của hai chính là các bit 1 của nó. Với $n\le 10^9$, ta liên tục lấy $n\mathbin{\&}-n$ để xây dựng $\textit{powers}$, có độ dài nhiều nhất là $30$.
>
> Có $\le 10^5$ truy vấn và mỗi đoạn có độ dài $\le 30$, nên ta có thể tính tích của từng đoạn theo modulo $10^9+7$. Không cần dùng tích tiền tố.

<!-- thinking:end -->

Ta có thể sử dụng phép thao tác bit (lowbit) để tạo mảng $\textit{powers}$, sau đó mô phỏng để tìm đáp án cho từng truy vấn.

Trước hết, với một số nguyên dương $n$, ta có thể nhanh chóng lấy giá trị tương ứng với bit $1$ thấp nhất trong biểu diễn nhị phân thông qua $n \& -n$. Đây là lũy thừa nhỏ nhất của $2$ trong số hiện tại. Bằng cách lặp lại thao tác này trên $n$ rồi trừ đi giá trị vừa lấy, ta lần lượt thu được tất cả các lũy thừa của $2$ tương ứng với các bit 1, tạo thành mảng $\textit{powers}$. Mảng này có thứ tự tăng dần, và độ dài của nó bằng số lượng $1$s trong biểu diễn nhị phân của $n$.

Tiếp theo, ta cần xử lý từng truy vấn. Với truy vấn hiện tại $(l, r)$, ta cần tính

$$
\textit{answers}[i] = \prod_{j=l}^{r} \textit{powers}[j]
$$

trong đó $\textit{answers}[i]$ là đáp án của truy vấn thứ $i$. Vì kết quả truy vấn có thể rất lớn, ta cần lấy modulo $10^9 + 7$ cho mỗi đáp án.

Độ phức tạp thời gian là $O(m \times \log n)$, trong đó $m$ là độ dài của mảng $\textit{queries}$. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(\log n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def productQueries(self, n: int, queries: List[List[int]]) -> List[int]:
        powers = []
        while n:
            x = n & -n
            powers.append(x)
            n -= x
        mod = 10**9 + 7
        ans = []
        for l, r in queries:
            x = 1
            for i in range(l, r + 1):
                x = x * powers[i] % mod
            ans.append(x)
        return ans
```

#### Java

```java
class Solution {
    public int[] productQueries(int n, int[][] queries) {
        int[] powers = new int[Integer.bitCount(n)];
        for (int i = 0; n > 0; ++i) {
            int x = n & -n;
            powers[i] = x;
            n -= x;
        }
        int m = queries.length;
        int[] ans = new int[m];
        final int mod = (int) 1e9 + 7;
        for (int i = 0; i < m; ++i) {
            int l = queries[i][0], r = queries[i][1];
            long x = 1;
            for (int j = l; j <= r; ++j) {
                x = x * powers[j] % mod;
            }
            ans[i] = (int) x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> productQueries(int n, vector<vector<int>>& queries) {
        vector<int> powers;
        while (n) {
            int x = n & -n;
            powers.push_back(x);
            n -= x;
        }
        vector<int> ans;
        const int mod = 1e9 + 7;
        for (const auto& q : queries) {
            int l = q[0], r = q[1];
            long long x = 1;
            for (int j = l; j <= r; ++j) {
                x = x * powers[j] % mod;
            }
            ans.push_back(x);
        }
        return ans;
    }
};
```

#### Go

```go
func productQueries(n int, queries [][]int) []int {
	var powers []int
	for n > 0 {
		x := n & -n
		powers = append(powers, x)
		n -= x
	}
	const mod = 1_000_000_007
	ans := make([]int, 0, len(queries))
	for _, q := range queries {
		l, r := q[0], q[1]
		x := 1
		for j := l; j <= r; j++ {
			x = x * powers[j] % mod
		}
		ans = append(ans, x)
	}
	return ans
}
```

#### TypeScript

```ts
function productQueries(n: number, queries: number[][]): number[] {
    const powers: number[] = [];
    while (n > 0) {
        const x = n & -n;
        powers.push(x);
        n -= x;
    }
    const mod = 1_000_000_007;
    const ans: number[] = [];
    for (const [l, r] of queries) {
        let x = 1;
        for (let j = l; j <= r; j++) {
            x = (x * powers[j]) % mod;
        }
        ans.push(x);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn product_queries(mut n: i32, queries: Vec<Vec<i32>>) -> Vec<i32> {
        let mut powers = Vec::new();
        while n > 0 {
            let x = n & -n;
            powers.push(x);
            n -= x;
        }
        let modulo = 1_000_000_007;
        let mut ans = Vec::with_capacity(queries.len());
        for q in queries {
            let l = q[0] as usize;
            let r = q[1] as usize;
            let mut x: i64 = 1;
            for j in l..=r {
                x = x * powers[j] as i64 % modulo;
            }
            ans.push(x as i32);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
