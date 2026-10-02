---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee)

[中文文档](/solution/0700-0799/0714.Best%20Time%20to%20Buy%20and%20Sell%20Stock%20with%20Transaction%20Fee/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>prices</code>, trong đó <code>prices[i]</code> là giá cổ phiếu vào ngày thứ <code>i<sup>th</sup></code>, và số nguyên <code>fee</code> biểu thị phí giao dịch.</p>

<p>Hãy tìm lợi nhuận tối đa có thể đạt được. Bạn có thể thực hiện bao nhiêu giao dịch tùy ý, nhưng phải trả phí cho mỗi giao dịch.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Không được thực hiện nhiều giao dịch cùng lúc (tức là phải bán cổ phiếu trước khi mua lại).</li>
	<li>Phí giao dịch chỉ được tính một lần cho mỗi lần mua và bán cổ phiếu.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,3,2,8,4,9], fee = 2
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Có thể đạt lợi nhuận tối đa bằng cách:
- Mua cổ phiếu với giá prices[0] = 1
- Bán cổ phiếu với giá prices[3] = 8
- Mua cổ phiếu với giá prices[4] = 4
- Bán cổ phiếu với giá prices[5] = 9
Tổng lợi nhuận là ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,3,7,5,10,3], fee = 3
<strong>Đầu ra:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt; 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= fee &lt; 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Có thể giao dịch nhiều lần và trả $fee$ cho mỗi lần bán hoàn tất. Vì $n \le 5\times 10^4$, tìm kiếm không memoization sẽ lặp lại các trạng thái qua từng ngày.
>
> Mỗi ngày có hai trạng thái: không giữ cổ phiếu hoặc đang giữ. Khi không giữ, ta mua hoặc bỏ qua; khi đang giữ, ta bán (trừ $fee$) hoặc tiếp tục giữ. Lợi nhuận tối ưu tại ngày $i$ ở trạng thái $j$ chỉ phụ thuộc vào hai trạng thái kế tiếp đó.
>
> Memoize $dfs(i,j)$; sau ngày cuối cùng, lợi nhuận là $0$. Đáp án là $dfs(0,0)$. Có $O(n)$ trạng thái.

<!-- thinking:end -->

Định nghĩa hàm $dfs(i, j)$ là lợi nhuận tối đa có thể đạt được khi bắt đầu từ ngày $i$ với trạng thái $j$. Ở đây, $j$ có thể là $0$ hoặc $1$, lần lượt biểu thị không giữ và đang giữ cổ phiếu. Đáp án là $dfs(0, 0)$.

Logic thực thi của hàm $dfs(i, j)$ như sau:

Nếu $i \geq n$, không còn ngày giao dịch nào nên trả về $0$.

Ngược lại, ta có thể không giao dịch, khi đó $dfs(i, j) = dfs(i + 1, j)$. Ta cũng có thể giao dịch. Nếu $j \gt 0$, nghĩa là đang giữ cổ phiếu và có thể bán; khi đó $dfs(i, j) = prices[i] + dfs(i + 1, 0) - fee$. Nếu $j = 0$, nghĩa là hiện không giữ cổ phiếu và có thể mua; khi đó $dfs(i, j) = -prices[i] + dfs(i + 1, 1)$. Hàm $dfs(i, j)$ trả về giá trị lớn nhất.

Đáp án là $dfs(0, 0)$.

Để tránh tính toán lặp lại, dùng memoization để lưu giá trị trả về của $dfs(i, j)$ trong mảng $f$. Nếu $f[i][j]$ khác $-1$, nghĩa là giá trị đã được tính và có thể trả về trực tiếp $f[i][j]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $prices$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= len(prices):
                return 0
            ans = dfs(i + 1, j)
            if j:
                ans = max(ans, prices[i] + dfs(i + 1, 0) - fee)
            else:
                ans = max(ans, -prices[i] + dfs(i + 1, 1))
            return ans

        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[] prices;
    private int fee;

    public int maxProfit(int[] prices, int fee) {
        f = new Integer[prices.length][2];
        this.prices = prices;
        this.fee = fee;
        return dfs(0, 0);
    }

    private int dfs(int i, int j) {
        if (i >= prices.length) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = dfs(i + 1, j);
        if (j > 0) {
            ans = Math.max(ans, prices[i] + dfs(i + 1, 0) - fee);
        } else {
            ans = Math.max(ans, -prices[i] + dfs(i + 1, 1));
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        int f[n][2];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i >= prices.size()) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int ans = dfs(i + 1, j);
            if (j) {
                ans = max(ans, prices[i] + dfs(i + 1, 0) - fee);
            } else {
                ans = max(ans, -prices[i] + dfs(i + 1, 1));
            }
            return f[i][j] = ans;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func maxProfit(prices []int, fee int) int {
	n := len(prices)
	f := make([][2]int, n)
	for i := range f {
		f[i] = [2]int{-1, -1}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i >= n {
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		ans := dfs(i+1, j)
		if j > 0 {
			ans = max(ans, prices[i]+dfs(i+1, 0)-fee)
		} else {
			ans = max(ans, -prices[i]+dfs(i+1, 1))
		}
		f[i][j] = ans
		return ans
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function maxProfit(prices: number[], fee: number): number {
    const n = prices.length;
    const f: number[][] = Array.from({ length: n }, () => [-1, -1]);
    const dfs = (i: number, j: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        let ans = dfs(i + 1, j);
        if (j) {
            ans = Math.max(ans, prices[i] + dfs(i + 1, 0) - fee);
        } else {
            ans = Math.max(ans, -prices[i] + dfs(i + 1, 1));
        }
        return (f[i][j] = ans);
    };
    return dfs(0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã có độ phức tạp tuyến tính, nhưng đệ quy vẫn dùng stack và bảng kích thước $O(n)$. Có thể viết cùng các bước chuyển theo chiều xuôi qua từng ngày.
>
> Gọi $f[i][0/1]$ là lợi nhuận tối đa sau ngày $i$ khi không giữ hoặc đang giữ cổ phiếu. Mỗi hàng chỉ phụ thuộc vào hàng trước; đáp án là $f[n-1][0]$.

<!-- thinking:end -->

Định nghĩa $f[i][j]$ là lợi nhuận tối đa có thể đạt được đến hết ngày $i$ với trạng thái $j$. Ở đây, $j$ có thể là $0$ hoặc $1$, lần lượt biểu thị không giữ và đang giữ cổ phiếu. Khởi tạo $f[0][0] = 0$ và $f[0][1] = -prices[0]$.

Với $i \geq 1$, nếu cuối ngày hiện tại không giữ cổ phiếu thì $f[i][0]$ có thể chuyển từ $f[i - 1][0]$ hoặc $f[i - 1][1] + prices[i] - fee$, tức $f[i][0] = \max(f[i - 1][0], f[i - 1][1] + prices[i] - fee)$. Nếu đang giữ cổ phiếu thì $f[i][1]$ có thể chuyển từ $f[i - 1][1]$ hoặc $f[i - 1][0] - prices[i]$, tức $f[i][1] = \max(f[i - 1][1], f[i - 1][0] - prices[i])$. Đáp án cuối cùng là $f[n - 1][0]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $prices$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        f = [[0] * 2 for _ in range(n)]
        f[0][1] = -prices[0]
        for i in range(1, n):
            f[i][0] = max(f[i - 1][0], f[i - 1][1] + prices[i] - fee)
            f[i][1] = max(f[i - 1][1], f[i - 1][0] - prices[i])
        return f[n - 1][0]
```

#### Java

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int[][] f = new int[n][2];
        f[0][1] = -prices[0];
        for (int i = 1; i < n; ++i) {
            f[i][0] = Math.max(f[i - 1][0], f[i - 1][1] + prices[i] - fee);
            f[i][1] = Math.max(f[i - 1][1], f[i - 1][0] - prices[i]);
        }
        return f[n - 1][0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        int f[n][2];
        memset(f, 0, sizeof(f));
        f[0][1] = -prices[0];
        for (int i = 1; i < n; ++i) {
            f[i][0] = max(f[i - 1][0], f[i - 1][1] + prices[i] - fee);
            f[i][1] = max(f[i - 1][1], f[i - 1][0] - prices[i]);
        }
        return f[n - 1][0];
    }
};
```

#### Go

```go
func maxProfit(prices []int, fee int) int {
	n := len(prices)
	f := make([][2]int, n)
	f[0][1] = -prices[0]
	for i := 1; i < n; i++ {
		f[i][0] = max(f[i-1][0], f[i-1][1]+prices[i]-fee)
		f[i][1] = max(f[i-1][1], f[i-1][0]-prices[i])
	}
	return f[n-1][0]
}
```

#### TypeScript

```ts
function maxProfit(prices: number[], fee: number): number {
    const n = prices.length;
    const f: number[][] = Array.from({ length: n }, () => [0, 0]);
    f[0][1] = -prices[0];
    for (let i = 1; i < n; ++i) {
        f[i][0] = Math.max(f[i - 1][0], f[i - 1][1] + prices[i] - fee);
        f[i][1] = Math.max(f[i - 1][1], f[i - 1][0] - prices[i]);
    }
    return f[n - 1][0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động (Tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> Trạng thái ngày $i$ trong lời giải 2 chỉ phụ thuộc vào ngày $i-1$, nên không cần lưu toàn bộ bảng.
>
> Cập nhật luân phiên hai biến $f_0,f_1$. Phép gán đồng thời giữ nguyên cặp giá trị cũ trong lúc tính cả hai trạng thái, nên trạng thái đang giữ mới vẫn dùng lợi nhuận không giữ cũ. Độ phức tạp không gian là $O(1)$.

<!-- thinking:end -->

Bước chuyển chỉ cần trạng thái của ngày trước, nên chỉ cần hai biến và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        f0, f1 = 0, -prices[0]
        for x in prices[1:]:
            f0, f1 = max(f0, f1 + x - fee), max(f1, f0 - x)
        return f0
```

#### Java

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int f0 = 0, f1 = -prices[0];
        for (int i = 1; i < prices.length; ++i) {
            int g0 = Math.max(f0, f1 + prices[i] - fee);
            f1 = Math.max(f1, f0 - prices[i]);
            f0 = g0;
        }
        return f0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int f0 = 0, f1 = -prices[0];
        for (int i = 1; i < prices.size(); ++i) {
            int g0 = max(f0, f1 + prices[i] - fee);
            f1 = max(f1, f0 - prices[i]);
            f0 = g0;
        }
        return f0;
    }
};
```

#### Go

```go
func maxProfit(prices []int, fee int) int {
	f0, f1 := 0, -prices[0]
	for _, x := range prices[1:] {
		f0, f1 = max(f0, f1+x-fee), max(f1, f0-x)
	}
	return f0
}
```

#### TypeScript

```ts
function maxProfit(prices: number[], fee: number): number {
    const n = prices.length;
    let [f0, f1] = [0, -prices[0]];
    for (const x of prices.slice(1)) {
        [f0, f1] = [Math.max(f0, f1 + x - fee), Math.max(f1, f0 - x)];
    }
    return f0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
