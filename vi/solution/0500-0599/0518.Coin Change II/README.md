---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - Unbounded Knapsack
---

<!-- problem:start -->

# [518. Coin Change II](https://leetcode.com/problems/coin-change-ii)

[中文文档](/solution/0500-0599/0518.Coin%20Change%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>coins</code> biểu diễn các đồng xu có mệnh giá khác nhau và số nguyên <code>amount</code> biểu diễn tổng số tiền.</p>

<p>Hãy trả về <em>số cách kết hợp các đồng xu để tạo thành số tiền đó</em>. Nếu không có cách kết hợp nào tạo được số tiền này, hãy trả về <code>0</code>.</p>

<p>Có thể giả sử bạn có vô hạn đồng xu thuộc mỗi mệnh giá.</p>

<p>Đảm bảo đáp án <strong>cuối cùng</strong> nằm trong phạm vi của số nguyên có dấu <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> amount = 5, coins = [1,2,5]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> có bốn cách tạo thành số tiền này:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> amount = 3, coins = [2]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> không thể tạo số tiền 3 chỉ bằng các đồng xu mệnh giá 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> amount = 10, coins = [10]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= coins.length &lt;= 300</code></li>
	<li><code>1 &lt;= coins[i] &lt;= 5000</code></li>
	<li>Mọi giá trị trong <code>coins</code> đều <strong>khác nhau</strong>.</li>
	<li><code>0 &lt;= amount &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Knapsack không giới hạn)

<!-- thinking:start -->

> **Tư duy**
>
> Ta đếm tổ hợp, không phải hoán vị. Nếu liệt kê các chuỗi chọn xu, cùng một tập xu sẽ bị đếm nhiều lần.
>
> Đây là bài toán đếm số cách trong unbounded knapsack: duyệt từng mệnh giá ở vòng lặp ngoài để mỗi loại xu được xét theo một thứ tự cố định. $f[i][j]$ là số cách tạo số tiền $j$ bằng $i$ loại xu đầu tiên: không dùng xu hiện tại thì kế thừa $f[i-1][j]$, còn dùng xu thì cộng $f[i][j-x]$. $f[0][0]=1$ tương ứng với tổ hợp rỗng.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách kết hợp các đồng xu để tạo số tiền $j$ bằng $i$ mệnh giá đầu tiên. Ban đầu, $f[0][0] = 1$, các vị trí còn lại đều bằng $0$.

Gọi $k$ là số lượng đồng xu loại cuối cùng được dùng, ta có phương trình thứ nhất:

$$
f[i][j] = f[i - 1][j] + f[i - 1][j - x] + f[i - 1][j - 2 \times x] + \cdots + f[i - 1][j - k \times x]
$$

trong đó $x$ là mệnh giá của loại xu thứ $i$.

Đặt $j = j - x$, ta có phương trình thứ hai:

$$
f[i][j - x] = f[i - 1][j - x] + f[i - 1][j - 2 \times x] + \cdots + f[i - 1][j - k \times x]
$$

Thay phương trình thứ hai vào phương trình thứ nhất, ta thu được công thức chuyển trạng thái:

$$
f[i][j] = f[i - 1][j] + f[i][j - x]
$$

Đáp án cuối cùng là $f[m][n]$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ là số mệnh giá xu và $n$ là tổng số tiền.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        m, n = len(coins), amount
        f = [[0] * (n + 1) for _ in range(m + 1)]
        f[0][0] = 1
        for i, x in enumerate(coins, 1):
            for j in range(n + 1):
                f[i][j] = f[i - 1][j]
                if j >= x:
                    f[i][j] += f[i][j - x]
        return f[m][n]
```

#### Java

```java
class Solution {
    public int change(int amount, int[] coins) {
        int m = coins.length, n = amount;
        int[][] f = new int[m + 1][n + 1];
        f[0][0] = 1;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= coins[i - 1]) {
                    f[i][j] += f[i][j - coins[i - 1]];
                }
            }
        }
        return f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int m = coins.size(), n = amount;
        unsigned f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= coins[i - 1]) {
                    f[i][j] += f[i][j - coins[i - 1]];
                }
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func change(amount int, coins []int) int {
	m, n := len(coins), amount
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	f[0][0] = 1
	for i := 1; i <= m; i++ {
		for j := 0; j <= n; j++ {
			f[i][j] = f[i-1][j]
			if j >= coins[i-1] {
				f[i][j] += f[i][j-coins[i-1]]
			}
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function change(amount: number, coins: number[]): number {
    const [m, n] = [coins.length, amount];
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= m; ++i) {
        for (let j = 0; j <= n; ++j) {
            f[i][j] = f[i - 1][j];
            if (j >= coins[i - 1]) {
                f[i][j] += f[i][j - coins[i - 1]];
            }
        }
    }
    return f[m][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> $f[i][j]$ chỉ phụ thuộc vào hàng trước tại $j$ và hàng hiện tại tại $j-x$. Với mỗi mệnh giá, cập nhật mảng một chiều theo chiều tăng từ $x$ đến `amount` để tái sử dụng các trạng thái mới của cùng loại xu.
>
> Độ phức tạp không gian giảm xuống còn $O(\textit{amount})$; số lần chuyển trạng thái không đổi.

<!-- thinking:end -->

Ta nhận thấy $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][j]$ và $f[i][j - x]$. Vì vậy, có thể tối ưu mảng hai chiều thành mảng một chiều, giảm độ phức tạp không gian xuống $O(n)$. Độ phức tạp thời gian vẫn là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        n = amount
        f = [1] + [0] * n
        for x in coins:
            for j in range(x, n + 1):
                f[j] += f[j - x]
        return f[n]
```

#### Java

```java
class Solution {
    public int change(int amount, int[] coins) {
        int n = amount;
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int x : coins) {
            for (int j = x; j <= n; ++j) {
                f[j] += f[j - x];
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int n = amount;
        unsigned f[n + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int x : coins) {
            for (int j = x; j <= n; ++j) {
                f[j] += f[j - x];
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func change(amount int, coins []int) int {
	n := amount
	f := make([]int, n+1)
	f[0] = 1
	for _, x := range coins {
		for j := x; j <= n; j++ {
			f[j] += f[j-x]
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function change(amount: number, coins: number[]): number {
    const n = amount;
    const f: number[] = Array(n + 1).fill(0);
    f[0] = 1;
    for (const x of coins) {
        for (let j = x; j <= n; ++j) {
            f[j] += f[j - x];
        }
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
