---
comments: true
difficulty: Medium
rating: 1588
source: Weekly Contest 361 Q2
tags:
    - Greedy
    - Math
    - String
    - Enumeration
---

<!-- problem:start -->

# [2844. Minimum Operations to Make a Special Number](https://leetcode.com/problems/minimum-operations-to-make-a-special-number)

[中文文档](/solution/2800-2899/2844.Minimum%20Operations%20to%20Make%20a%20Special%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>num</code> được <strong>đánh chỉ số từ 0</strong>, biểu diễn một số nguyên không âm.</p>

<p>Trong một thao tác, bạn có thể chọn và xóa một chữ số bất kỳ của <code>num</code>. Lưu ý rằng nếu xóa tất cả chữ số của <code>num</code>, <code>num</code> sẽ trở thành <code>0</code>.</p>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để biến</em> <code>num</code> <i>thành một số đặc biệt</i>.</p>

<p>Một số nguyên <code>x</code> được xem là <strong>đặc biệt</strong> nếu chia hết cho <code>25</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;2245047&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Xóa các chữ số num[5] và num[6]. Số thu được là &quot;22450&quot;, đây là số đặc biệt vì chia hết cho 25.
Có thể chứng minh rằng 2 là số thao tác nhỏ nhất cần thực hiện để thu được một số đặc biệt.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;2908305&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Xóa các chữ số num[3], num[4] và num[6]. Số thu được là &quot;2900&quot;, đây là số đặc biệt vì chia hết cho 25.
Có thể chứng minh rằng 3 là số thao tác nhỏ nhất cần thực hiện để thu được một số đặc biệt.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;10&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Xóa chữ số num[0]. Số thu được là &quot;0&quot;, đây là số đặc biệt vì chia hết cho 25.
Có thể chứng minh rằng 1 là số thao tác nhỏ nhất cần thực hiện để thu được một số đặc biệt.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 100</code></li>
	<li><code>num</code> chỉ gồm các chữ số từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.</li>
	<li><code>num</code> không chứa số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Một số đặc biệt có phần dư bằng $0$ khi chia cho $25$, và ta có thể xóa tùy ý các chữ số. $dfs(i,k)$ là số chữ số ít nhất cần xóa từ chỉ số $i$ khi phần dư hiện tại là $k$: xóa chữ số thì giữ nguyên $k$, còn giữ chữ số thì chuyển sang $(k\cdot 10+d)\bmod 25$.

<!-- thinking:end -->

Ta nhận thấy một số nguyên $x$ chia hết cho $25$ khi $x \bmod 25 = 0$. Vì vậy, ta có thể xây dựng hàm $dfs(i, k)$, biểu diễn số chữ số ít nhất cần xóa để biến số thành một số đặc biệt, bắt đầu từ chữ số thứ $i$ của chuỗi $num$, khi phần dư của số hiện tại khi chia cho $25$ là $k$. Đáp án là $dfs(0, 0)$.

Logic thực thi của hàm $dfs(i, k)$ như sau:

- Nếu $i = n$, tức là đã xử lý hết chuỗi $num$, thì nếu $k = 0$, số hiện tại chia hết cho $25$ nên trả về $0$, ngược lại trả về $n$;
- Nếu không, ta có thể xóa chữ số thứ $i$, khi đó cần xóa một chữ số, tức là $dfs(i + 1, k) + 1$; nếu không xóa chữ số thứ $i$, giá trị của $k$ sẽ trở thành $(k \times 10 + \textit{num}[i]) \bmod 25$, tức là $dfs(i + 1, (k \times 10 + \textit{num}[i]) \bmod 25)$. Lấy giá trị nhỏ hơn trong hai trường hợp này.

Để tránh tính toán lặp lại, ta có thể dùng memoization để tối ưu độ phức tạp thời gian.

Độ phức tạp thời gian là $O(n \times 25)$, và độ phức tạp không gian là $O(n \times 25)$. Trong đó, $n$ là độ dài của chuỗi $num$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, num: str) -> int:
        @cache
        def dfs(i: int, k: int) -> int:
            if i == n:
                return 0 if k == 0 else n
            ans = dfs(i + 1, k) + 1
            ans = min(ans, dfs(i + 1, (k * 10 + int(num[i])) % 25))
            return ans

        n = len(num)
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private String num;
    private int n;

    public int minimumOperations(String num) {
        n = num.length();
        this.num = num;
        f = new Integer[n][25];
        return dfs(0, 0);
    }

    private int dfs(int i, int k) {
        if (i == n) {
            return k == 0 ? 0 : n;
        }
        if (f[i][k] != null) {
            return f[i][k];
        }
        f[i][k] = dfs(i + 1, k) + 1;
        f[i][k] = Math.min(f[i][k], dfs(i + 1, (k * 10 + num.charAt(i) - '0') % 25));
        return f[i][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(string num) {
        int n = num.size();
        int f[n][25];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int k) -> int {
            if (i == n) {
                return k == 0 ? 0 : n;
            }
            if (f[i][k] != -1) {
                return f[i][k];
            }
            f[i][k] = dfs(i + 1, k) + 1;
            f[i][k] = min(f[i][k], dfs(i + 1, (k * 10 + num[i] - '0') % 25));
            return f[i][k];
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func minimumOperations(num string) int {
	n := len(num)
	f := make([][25]int, n)
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, k int) int
	dfs = func(i, k int) int {
		if i == n {
			if k == 0 {
				return 0
			}
			return n
		}
		if f[i][k] != -1 {
			return f[i][k]
		}
		f[i][k] = dfs(i+1, k) + 1
		f[i][k] = min(f[i][k], dfs(i+1, (k*10+int(num[i]-'0'))%25))
		return f[i][k]
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function minimumOperations(num: string): number {
    const n = num.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: 25 }, () => -1));
    const dfs = (i: number, k: number): number => {
        if (i === n) {
            return k === 0 ? 0 : n;
        }
        if (f[i][k] !== -1) {
            return f[i][k];
        }
        f[i][k] = dfs(i + 1, k) + 1;
        f[i][k] = Math.min(f[i][k], dfs(i + 1, (k * 10 + Number(num[i])) % 25));
        return f[i][k];
    };
    return dfs(0, 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_operations(num: String) -> i32 {
        let n = num.len();
        let bytes = num.as_bytes();

        let mut memo = vec![vec![-1; 25]; n];

        fn dfs(
            i: usize,
            k: usize,
            n: usize,
            bytes: &[u8],
            memo: &mut Vec<Vec<i32>>,
        ) -> i32 {
            if i == n {
                return if k == 0 { 0 } else { n as i32 };
            }

            if memo[i][k] != -1 {
                return memo[i][k];
            }

            // delete current digit
            let mut res = dfs(i + 1, k, n, bytes, memo) + 1;

            // keep current digit
            let digit = (bytes[i] - b'0') as usize;
            let nk = (k * 10 + digit) % 25;
            res = res.min(dfs(i + 1, nk, n, bytes, memo));

            memo[i][k] = res;
            res
        }

        dfs(0, 0, n, &bytes, &mut memo)
    }
}
```

#### C#

```cs
public class Solution {
    private int[,] memo;
    private string num;
    private int n;

    public int MinimumOperations(string num) {
        this.num = num;
        n = num.Length;
        memo = new int[n, 25];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 25; j++) {
                memo[i, j] = -1;
            }
        }

        return Dfs(0, 0);
    }

    private int Dfs(int i, int k) {
        if (i == n) {
            return k == 0 ? 0 : n;
        }

        if (memo[i, k] != -1) {
            return memo[i, k];
        }

        // delete current digit
        int res = Dfs(i + 1, k) + 1;

        // keep current digit
        int digit = num[i] - '0';
        int nk = (k * 10 + digit) % 25;
        res = Math.Min(res, Dfs(i + 1, nk));

        memo[i, k] = res;
        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
