---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown)

[中文文档](/solution/0300-0399/0309.Best%20Time%20to%20Buy%20and%20Sell%20Stock%20with%20Cooldown/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>prices</code>, trong đó <code>prices[i]</code> là giá cổ phiếu vào ngày thứ <code>i<sup>th</sup></code>.</p>

<p>Hãy tìm lợi nhuận tối đa có thể đạt được. Bạn có thể thực hiện bao nhiêu giao dịch tùy ý (tức là nhiều lần mua và bán một cổ phiếu) với hạn chế sau:</p>

<ul>
	<li>Sau khi bán cổ phiếu, bạn không thể mua cổ phiếu vào ngày hôm sau (tức là cần cooldown một ngày).</li>
</ul>

<p><strong>Lưu ý:</strong> Bạn không thể thực hiện nhiều giao dịch cùng lúc (tức là phải bán cổ phiếu trước khi mua lại).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,2,3,0,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các thao tác lần lượt là mua, bán, cooldown, mua, bán.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 5000</code></li>
	<li><code>0 &lt;= prices[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể giao dịch nhiều lần, nhưng sau mỗi lần bán phải nghỉ một ngày. Cây quyết định trực tiếp theo ngày, trạng thái đang nắm giữ và cooldown sẽ tính lại cùng trạng thái nhiều lần.
>
> Rút gọn trạng thái thành $(i,j)$: đang ở ngày $i$ và có nắm giữ cổ phiếu hay không. Ta có thể bỏ qua ngày đó; nếu đang nắm giữ thì bán và chuyển sang ngày $i+2$, còn nếu chưa nắm giữ thì mua và chuyển sang trạng thái đang nắm giữ. Memoization đảm bảo mỗi trạng thái chỉ được tính một lần; việc nhảy qua một ngày sau khi bán chính là cooldown.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(i, j)$ là lợi nhuận tối đa có thể đạt được khi bắt đầu từ ngày thứ $i$ với trạng thái $j$. Giá trị $j$ bằng $0$ hoặc $1$, lần lượt biểu thị hiện không nắm giữ hoặc đang nắm giữ cổ phiếu. Đáp án là $dfs(0, 0)$.

Hàm $dfs(i, j)$ hoạt động như sau:

Nếu $i \geq n$, nghĩa là không còn ngày nào để giao dịch, nên trả về $0$.

Ngược lại, ta có thể không giao dịch, khi đó $dfs(i, j) = dfs(i + 1, j)$. Ta cũng có thể giao dịch: nếu $j > 0$, nghĩa là đang nắm giữ cổ phiếu và có thể bán, khi đó $dfs(i, j) = prices[i] + dfs(i + 2, 0)$. Nếu $j = 0$, nghĩa là hiện không nắm giữ cổ phiếu và có thể mua, khi đó $dfs(i, j) = -prices[i] + dfs(i + 1, 1)$. Giá trị lớn nhất trong các lựa chọn là kết quả trả về của $dfs(i, j)$.

Đáp án là $dfs(0, 0)$.

Để tránh tính toán lặp lại, ta dùng memoization và mảng $f$ để lưu giá trị trả về của $dfs(i, j)$. Nếu $f[i][j]$ khác $-1$, trạng thái này đã được tính nên ta có thể trả về ngay $f[i][j]$.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài của mảng $prices$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= len(prices):
                return 0
            ans = dfs(i + 1, j)
            if j:
                ans = max(ans, prices[i] + dfs(i + 2, 0))
            else:
                ans = max(ans, -prices[i] + dfs(i + 1, 1))
            return ans

        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private int[] prices;
    private Integer[][] f;

    public int maxProfit(int[] prices) {
        this.prices = prices;
        f = new Integer[prices.length][2];
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
            ans = Math.max(ans, prices[i] + dfs(i + 2, 0));
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
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int f[n][2];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i >= n) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int ans = dfs(i + 1, j);
            if (j) {
                ans = max(ans, prices[i] + dfs(i + 2, 0));
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
func maxProfit(prices []int) int {
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
			ans = max(ans, prices[i]+dfs(i+2, 0))
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
function maxProfit(prices: number[]): number {
    const n = prices.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: 2 }, () => -1));
    const dfs = (i: number, j: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        let ans = dfs(i + 1, j);
        if (j) {
            ans = Math.max(ans, prices[i] + dfs(i + 2, 0));
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
> Đệ quy có memoization sử dụng cùng công thức truy hồi, nhưng ta tính lần lượt từ ngày đầu. Gọi $f[i][0/1]$ là lợi nhuận tốt nhất sau ngày $i$ khi không nắm giữ hoặc đang nắm giữ cổ phiếu. Trạng thái không nắm giữ có thể đến từ việc tiếp tục không nắm giữ hoặc bán hôm nay; trạng thái đang nắm giữ có thể đến từ việc tiếp tục nắm giữ hoặc mua sau cooldown, tức từ $f[i-2][0]$.
>
> Ta tính từ trái sang phải; đáp án là trạng thái không nắm giữ vào ngày cuối. Độ phức tạp thời gian vẫn là $O(n)$ và không cần đệ quy.

<!-- thinking:end -->

Ta cũng có thể dùng quy hoạch động để giải bài toán này.

Ta định nghĩa $f[i][j]$ là lợi nhuận tối đa có thể đạt được vào ngày thứ $i$ với trạng thái $j$. Giá trị $j$ bằng $0$ hoặc $1$, lần lượt biểu thị hiện không nắm giữ hoặc đang nắm giữ cổ phiếu. Ban đầu, $f[0][0] = 0$, $f[0][1] = -prices[0]$.

Khi $i \geq 1$, nếu hiện không nắm giữ cổ phiếu thì $f[i][0]$ được chuyển từ $f[i - 1][0]$ hoặc $f[i - 1][1] + prices[i]$, tức $f[i][0] = \max(f[i - 1][0], f[i - 1][1] + prices[i])$. Nếu đang nắm giữ cổ phiếu thì $f[i][1]$ được chuyển từ $f[i - 1][1]$ hoặc $f[i - 2][0] - prices[i]$, tức $f[i][1] = \max(f[i - 1][1], f[i - 2][0] - prices[i])$. Đáp án cuối cùng là $f[n - 1][0]$.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài của mảng $prices$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        f = [[0] * 2 for _ in range(n)]
        f[0][1] = -prices[0]
        for i in range(1, n):
            f[i][0] = max(f[i - 1][0], f[i - 1][1] + prices[i])
            f[i][1] = max(f[i - 1][1], f[i - 2][0] - prices[i])
        return f[n - 1][0]
```

#### Java

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] f = new int[n][2];
        f[0][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            f[i][0] = Math.max(f[i - 1][0], f[i - 1][1] + prices[i]);
            f[i][1] = Math.max(f[i - 1][1], (i > 1 ? f[i - 2][0] : 0) - prices[i]);
        }
        return f[n - 1][0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int f[n][2];
        memset(f, 0, sizeof(f));
        f[0][1] = -prices[0];
        for (int i = 1; i < n; ++i) {
            f[i][0] = max(f[i - 1][0], f[i - 1][1] + prices[i]);
            f[i][1] = max(f[i - 1][1], (i > 1 ? f[i - 2][0] : 0) - prices[i]);
        }
        return f[n - 1][0];
    }
};
```

#### Go

```go
func maxProfit(prices []int) int {
	n := len(prices)
	f := make([][2]int, n)
	f[0][1] = -prices[0]
	for i := 1; i < n; i++ {
		f[i][0] = max(f[i-1][0], f[i-1][1]+prices[i])
		if i > 1 {
			f[i][1] = max(f[i-1][1], f[i-2][0]-prices[i])
		} else {
			f[i][1] = max(f[i-1][1], -prices[i])
		}
	}
	return f[n-1][0]
}
```

#### TypeScript

```ts
function maxProfit(prices: number[]): number {
    const n = prices.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: 2 }, () => 0));
    f[0][1] = -prices[0];
    for (let i = 1; i < n; ++i) {
        f[i][0] = Math.max(f[i - 1][0], f[i - 1][1] + prices[i]);
        f[i][1] = Math.max(f[i - 1][1], (i > 1 ? f[i - 2][0] : 0) - prices[i]);
    }
    return f[n - 1][0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 chỉ cần các trạng thái ở ngày $i-1$ và $i-2$, nên không cần giữ toàn bộ bảng. Ba biến luân phiên (không nắm giữ từ hai ngày trước, không nắm giữ hôm qua, đang nắm giữ hôm qua) biểu diễn cùng các chuyển trạng thái với độ phức tạp không gian $O(1)$.

<!-- thinking:end -->

Mỗi chuyển trạng thái chỉ cần dữ liệu của hai ngày trước đó, nên chỉ cần ba biến và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        f, f0, f1 = 0, 0, -prices[0]
        for x in prices[1:]:
            f, f0, f1 = f0, max(f0, f1 + x), max(f1, f - x)
        return f0
```

#### Java

```java
class Solution {
    public int maxProfit(int[] prices) {
        int f = 0, f0 = 0, f1 = -prices[0];
        for (int i = 1; i < prices.length; ++i) {
            int g0 = Math.max(f0, f1 + prices[i]);
            f1 = Math.max(f1, f - prices[i]);
            f = f0;
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
    int maxProfit(vector<int>& prices) {
        int f = 0, f0 = 0, f1 = -prices[0];
        for (int i = 1; i < prices.size(); ++i) {
            int g0 = max(f0, f1 + prices[i]);
            f1 = max(f1, f - prices[i]);
            f = f0;
            f0 = g0;
        }
        return f0;
    }
};
```

#### Go

```go
func maxProfit(prices []int) int {
	f, f0, f1 := 0, 0, -prices[0]
	for _, x := range prices[1:] {
		f, f0, f1 = f0, max(f0, f1+x), max(f1, f-x)
	}
	return f0
}
```

#### TypeScript

```ts
function maxProfit(prices: number[]): number {
    let [f, f0, f1] = [0, 0, -prices[0]];
    for (const x of prices.slice(1)) {
        [f, f0, f1] = [f0, Math.max(f0, f1 + x), Math.max(f1, f - x)];
    }
    return f0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
