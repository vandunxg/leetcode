---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Array
    - Dynamic Programming
    - Knapsack
    - Unbounded Knapsack
---

<!-- problem:start -->

# [322. Coin Change](https://leetcode.com/problems/coin-change)

[中文文档](/solution/0300-0399/0322.Coin%20Change/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>coins</code> biểu diễn các đồng xu có mệnh giá khác nhau và số nguyên <code>amount</code> biểu diễn tổng số tiền cần đổi.</p>

<p>Hãy trả về <em>số lượng đồng xu ít nhất cần dùng để tạo thành số tiền đó</em>. Nếu không thể tạo ra số tiền cần đổi bằng bất kỳ tổ hợp đồng xu nào, hãy trả về <code>-1</code>.</p>

<p>Có thể giả sử bạn có vô hạn đồng xu ở mỗi mệnh giá.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> coins = [1,2,5], amount = 11
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 11 = 5 + 5 + 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> coins = [2], amount = 3
<strong>Đầu ra:</strong> -1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> coins = [1], amount = 0
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= coins.length &lt;= 12</code></li>
	<li><code>1 &lt;= coins[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>0 &lt;= amount &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Knapsack không giới hạn)

<!-- thinking:start -->

> **Tư duy**
>
> Có thể dùng mỗi đồng xu nhiều lần tùy ý; ta cần ít đồng xu nhất để tổng mệnh giá bằng $amount$. Đây là bài toán unbounded knapsack. DFS trực tiếp sẽ tính lại các số tiền con nhiều lần.
>
> Gọi $f[i][j]$ là số đồng xu ít nhất cần dùng để tạo số tiền $j$ từ $i$ loại xu đầu tiên. Bỏ qua loại xu thứ $i$ thì dùng $f[i-1][j]$; nếu $j\ge x$, dùng thêm một đồng mệnh giá $x$ thì được $f[i][j-x]+1$. Khởi tạo $f[0][0]=0$, các giá trị còn lại bằng $\infty$; nếu không thể đổi được thì trả về $-1$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số đồng xu ít nhất cần dùng để tạo số tiền $j$ bằng $i$ loại xu đầu tiên. Ban đầu, $f[0][0] = 0$, còn các vị trí khác đều bằng dương vô cực.

Ta có thể xét số lượng $k$ đồng xu thuộc loại cuối cùng được dùng, khi đó:

$$
f[i][j] = \min(f[i - 1][j], f[i - 1][j - x] + 1, \cdots, f[i - 1][j - k \times x] + k)
$$

trong đó $x$ là mệnh giá của loại xu thứ $i$.

Đặt $j = j - x$, ta có:

$$
f[i][j - x] = \min(f[i - 1][j - x], f[i - 1][j - 2 \times x] + 1, \cdots, f[i - 1][j - k \times x] + k - 1)
$$

Thay phương trình thứ hai vào phương trình thứ nhất, ta thu được công thức chuyển trạng thái:

$$
f[i][j] = \min(f[i - 1][j], f[i][j - x] + 1)
$$

Đáp án cuối cùng là $f[m][n]$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ là số loại xu và $n$ là tổng số tiền cần đổi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        m, n = len(coins), amount
        f = [[inf] * (n + 1) for _ in range(m + 1)]
        f[0][0] = 0
        for i, x in enumerate(coins, 1):
            for j in range(n + 1):
                f[i][j] = f[i - 1][j]
                if j >= x:
                    f[i][j] = min(f[i][j], f[i][j - x] + 1)
        return -1 if f[m][n] >= inf else f[m][n]
```

#### Java

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        final int inf = 1 << 30;
        int m = coins.length;
        int n = amount;
        int[][] f = new int[m + 1][n + 1];
        for (var g : f) {
            Arrays.fill(g, inf);
        }
        f[0][0] = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= coins[i - 1]) {
                    f[i][j] = Math.min(f[i][j], f[i][j - coins[i - 1]] + 1);
                }
            }
        }
        return f[m][n] >= inf ? -1 : f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int m = coins.size(), n = amount;
        int f[m + 1][n + 1];
        memset(f, 0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= coins[i - 1]) {
                    f[i][j] = min(f[i][j], f[i][j - coins[i - 1]] + 1);
                }
            }
        }
        return f[m][n] > n ? -1 : f[m][n];
    }
};
```

#### Go

```go
func coinChange(coins []int, amount int) int {
	m, n := len(coins), amount
	f := make([][]int, m+1)
	const inf = 1 << 30
	for i := range f {
		f[i] = make([]int, n+1)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= m; i++ {
		for j := 0; j <= n; j++ {
			f[i][j] = f[i-1][j]
			if j >= coins[i-1] {
				f[i][j] = min(f[i][j], f[i][j-coins[i-1]]+1)
			}
		}
	}
	if f[m][n] > n {
		return -1
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function coinChange(coins: number[], amount: number): number {
    const m = coins.length;
    const n = amount;
    const f: number[][] = Array(m + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(1 << 30));
    f[0][0] = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 0; j <= n; ++j) {
            f[i][j] = f[i - 1][j];
            if (j >= coins[i - 1]) {
                f[i][j] = Math.min(f[i][j], f[i][j - coins[i - 1]] + 1);
            }
        }
    }
    return f[m][n] > n ? -1 : f[m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn coin_change(coins: Vec<i32>, amount: i32) -> i32 {
        let m = coins.len();
        let n = amount as usize;
        let inf = 1 << 30;
        let mut f = vec![vec![inf; n + 1]; m + 1];
        f[0][0] = 0;
        for i in 1..=m {
            let x = coins[i - 1] as usize;
            for j in 0..=n {
                f[i][j] = f[i - 1][j];
                if j >= x {
                    f[i][j] = f[i][j].min(f[i][j - x] + 1);
                }
            }
        }
        if f[m][n] > amount {
            -1
        } else {
            f[m][n]
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function (coins, amount) {
    const m = coins.length;
    const n = amount;
    const f = Array(m + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(1 << 30));
    f[0][0] = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 0; j <= n; ++j) {
            f[i][j] = f[i - 1][j];
            if (j >= coins[i - 1]) {
                f[i][j] = Math.min(f[i][j], f[i][j - coins[i - 1]] + 1);
            }
        }
    }
    return f[m][n] > n ? -1 : f[m][n];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 chỉ đọc $f[i-1][j]$ và $f[i][j-x]$. Ta có thể gộp thành một mảng và duyệt số tiền theo chiều tăng dần để $f[j-x]$ đã tính đến loại xu hiện tại. Công thức chuyển trạng thái giữ nguyên, còn không gian giảm xuống $O(amount)$.

<!-- thinking:end -->

Ta nhận thấy $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][j]$ và $f[i][j - x]$. Vì vậy, có thể tối ưu mảng hai chiều thành mảng một chiều, giảm độ phức tạp không gian xuống $O(n)$. Độ phức tạp thời gian vẫn là $O(m \times n)$.

Các bài tương tự:

- [279. Perfect Squares](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0279.Perfect%20Squares/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        n = amount
        f = [0] + [inf] * n
        for x in coins:
            for j in range(x, n + 1):
                f[j] = min(f[j], f[j - x] + 1)
        return -1 if f[n] >= inf else f[n]
```

#### Java

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        final int inf = 1 << 30;
        int n = amount;
        int[] f = new int[n + 1];
        Arrays.fill(f, inf);
        f[0] = 0;
        for (int x : coins) {
            for (int j = x; j <= n; ++j) {
                f[j] = Math.min(f[j], f[j - x] + 1);
            }
        }
        return f[n] >= inf ? -1 : f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int n = amount;
        int f[n + 1];
        memset(f, 0x3f, sizeof(f));
        f[0] = 0;
        for (int x : coins) {
            for (int j = x; j <= n; ++j) {
                f[j] = min(f[j], f[j - x] + 1);
            }
        }
        return f[n] > n ? -1 : f[n];
    }
};
```

#### Go

```go
func coinChange(coins []int, amount int) int {
	n := amount
	f := make([]int, n+1)
	for i := range f {
		f[i] = 1 << 30
	}
	f[0] = 0
	for _, x := range coins {
		for j := x; j <= n; j++ {
			f[j] = min(f[j], f[j-x]+1)
		}
	}
	if f[n] > n {
		return -1
	}
	return f[n]
}
```

#### TypeScript

```ts
function coinChange(coins: number[], amount: number): number {
    const n = amount;
    const f: number[] = Array(n + 1).fill(1 << 30);
    f[0] = 0;
    for (const x of coins) {
        for (let j = x; j <= n; ++j) {
            f[j] = Math.min(f[j], f[j - x] + 1);
        }
    }
    return f[n] > n ? -1 : f[n];
}
```

#### JavaScript

```js
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function (coins, amount) {
    const n = amount;
    const f = Array(n + 1).fill(1 << 30);
    f[0] = 0;
    for (const x of coins) {
        for (let j = x; j <= n; ++j) {
            f[j] = Math.min(f[j], f[j - x] + 1);
        }
    }
    return f[n] > n ? -1 : f[n];
};
```

#### Rust

```rust
impl Solution {
    pub fn coin_change(coins: Vec<i32>, amount: i32) -> i32 {
        let n = amount as usize;
        let mut f = vec![n + 1; n + 1];
        f[0] = 0;
        for &x in &coins {
            for j in x as usize..=n {
                f[j] = f[j].min(f[j - (x as usize)] + 1);
            }
        }
        if f[n] > n {
            -1
        } else {
            f[n] as i32
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
