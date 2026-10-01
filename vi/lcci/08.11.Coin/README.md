---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [08.11. Coin](https://leetcode.cn/problems/coin-lcci)

[中文文档](/lcci/08.11.Coin/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số lượng không giới hạn các đồng 25 cent (quarter), 10 cent (dime), 5 cent (nickel) và 1 cent (penny), hãy viết code để tính số cách biểu diễn n cent.&nbsp;(Kết quả có thể rất lớn, vì vậy cần trả về phần dư khi chia cho 1000000007)</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>: n = 5

<strong> Đầu ra</strong>: 2

<strong> Giải thích</strong>: Có hai cách:

5=5

5=1+1+1+1+1

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>: n = 10

<strong> Đầu ra</strong>: 4

<strong> Giải thích</strong>: Có bốn cách:

10=10

10=5+5

10=5+1+1+1+1+1

10=1+1+1+1+1+1+1+1+1+1

</pre>
<p><strong>Ghi chú: </strong></p>
<p>Bạn có thể giả định rằng:</p>
<ul>
	<li>0 &lt;= n&nbsp;&lt;= 1000000</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Dùng các đồng $25,10,5,1$ để tạo ra $n$, không xét thứ tự. Duyệt lồng nhau theo số lượng từng loại đồng xu là khả thi và có cùng cấu trúc với công thức truy hồi của bài toán ba lô không giới hạn.
>
> $f[i][j]$ là số cách dùng $i$ loại đồng xu đầu tiên. Công thức chuyển là $f[i][j]=f[i-1][j]+f[i][j-c_i]$ khi $j\ge c_i$.
>
> Bốn loại đồng xu chỉ cần một bảng $5\times(n+1)$; $f[0][0]=1$ là cách tạo số tiền rỗng. Lấy phần dư theo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách tạo ra tổng tiền $j$ chỉ bằng $i$ loại đồng xu đầu tiên. Ban đầu, $f[0][0]=1$, các phần tử còn lại đều bằng $0$. Đáp án là $f[4][n]$.

Xét $f[i][j]$, ta có thể liệt kê số lượng $k$ đồng xu loại thứ $i$ được sử dụng, trong đó $0 \leq k \leq j / c_i$, khi đó $f[i][j]$ bằng tổng của tất cả $f[i−1][j−k \times c_i]$. Vì số lượng đồng xu là vô hạn, $k$ có thể bắt đầu từ $0$. Công thức chuyển trạng thái như sau:

$$
f[i][j] = f[i - 1][j] + f[i - 1][j - c_i] + \cdots + f[i - 1][j - k \times c_i]
$$

Đặt $j = j - c_i$, khi đó công thức chuyển trạng thái trên có thể viết thành:

$$
f[i][j - c_i] = f[i - 1][j - c_i] + f[i - 1][j - 2 \times c_i] + \cdots + f[i - 1][j - k \times c_i]
$$

Thay phương trình thứ hai vào phương trình thứ nhất, ta được:

$$
f[i][j]=
\begin{cases}
f[i - 1][j] + f[i][j - c_i], & j \geq c_i \\
f[i - 1][j], & j < c_i
\end{cases}
$$

Đáp án cuối cùng là $f[4][n]$.

Độ phức tạp thời gian là $O(C \times n)$, độ phức tạp không gian là $O(C \times n)$, trong đó $C$ là số loại đồng xu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToChange(self, n: int) -> int:
        mod = 10**9 + 7
        coins = [25, 10, 5, 1]
        f = [[0] * (n + 1) for _ in range(5)]
        f[0][0] = 1
        for i, c in enumerate(coins, 1):
            for j in range(n + 1):
                f[i][j] = f[i - 1][j]
                if j >= c:
                    f[i][j] = (f[i][j] + f[i][j - c]) % mod
        return f[-1][n]
```

#### Java

```java
class Solution {
    public int waysToChange(int n) {
        final int mod = (int) 1e9 + 7;
        int[] coins = {25, 10, 5, 1};
        int[][] f = new int[5][n + 1];
        f[0][0] = 1;
        for (int i = 1; i <= 4; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= coins[i - 1]) {
                    f[i][j] = (f[i][j] + f[i][j - coins[i - 1]]) % mod;
                }
            }
        }
        return f[4][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToChange(int n) {
        const int mod = 1e9 + 7;
        vector<int> coins = {25, 10, 5, 1};
        int f[5][n + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= 4; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= coins[i - 1]) {
                    f[i][j] = (f[i][j] + f[i][j - coins[i - 1]]) % mod;
                }
            }
        }
        return f[4][n];
    }
};
```

#### Go

```go
func waysToChange(n int) int {
	const mod int = 1e9 + 7
	coins := []int{25, 10, 5, 1}
	f := make([][]int, 5)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	f[0][0] = 1
	for i := 1; i <= 4; i++ {
		for j := 0; j <= n; j++ {
			f[i][j] = f[i-1][j]
			if j >= coins[i-1] {
				f[i][j] = (f[i][j] + f[i][j-coins[i-1]]) % mod
			}
		}
	}
	return f[4][n]
}
```

#### TypeScript

```ts
function waysToChange(n: number): number {
    const mod = 10 ** 9 + 7;
    const coins: number[] = [25, 10, 5, 1];
    const f: number[][] = Array.from({ length: 5 }, () => Array(n + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= 4; ++i) {
        for (let j = 0; j <= n; ++j) {
            f[i][j] = f[i - 1][j];
            if (j >= coins[i - 1]) {
                f[i][j] = (f[i][j] + f[i][j - coins[i - 1]]) % mod;
            }
        }
    }
    return f[4][n];
}
```

#### Swift

```swift
class Solution {
    func waysToChange(_ n: Int) -> Int {
        let mod = Int(1e9 + 7)
        let coins = [25, 10, 5, 1]
        var f = Array(repeating: Array(repeating: 0, count: n + 1), count: 5)
        f[0][0] = 1

        for i in 1...4 {
            for j in 0...n {
                f[i][j] = f[i - 1][j]
                if j >= coins[i - 1] {
                    f[i][j] = (f[i][j] + f[i][j - coins[i - 1]]) % mod
                }
            }
        }
        return f[4][n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $i$ chỉ đọc hàng $i-1$ và các giá trị $j$ nhỏ hơn trên cùng hàng, nên có thể loại bỏ chiều thứ nhất.
>
> Một mảng một chiều được cập nhật theo thứ tự $j$ tăng dần sẽ giữ nguyên công thức truy hồi với độ phức tạp không gian $O(n)$.

<!-- thinking:end -->

Ta nhận thấy phép tính $f[i][j]$ chỉ liên quan đến $f[i−1][..]$. Vì vậy, ta có thể bỏ chiều thứ nhất và tối ưu độ phức tạp không gian xuống còn $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToChange(self, n: int) -> int:
        mod = 10**9 + 7
        coins = [25, 10, 5, 1]
        f = [1] + [0] * n
        for c in coins:
            for j in range(c, n + 1):
                f[j] = (f[j] + f[j - c]) % mod
        return f[n]
```

#### Java

```java
class Solution {
    public int waysToChange(int n) {
        final int mod = (int) 1e9 + 7;
        int[] coins = {25, 10, 5, 1};
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int c : coins) {
            for (int j = c; j <= n; ++j) {
                f[j] = (f[j] + f[j - c]) % mod;
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
    int waysToChange(int n) {
        const int mod = 1e9 + 7;
        vector<int> coins = {25, 10, 5, 1};
        int f[n + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int c : coins) {
            for (int j = c; j <= n; ++j) {
                f[j] = (f[j] + f[j - c]) % mod;
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func waysToChange(n int) int {
	const mod int = 1e9 + 7
	coins := []int{25, 10, 5, 1}
	f := make([]int, n+1)
	f[0] = 1
	for _, c := range coins {
		for j := c; j <= n; j++ {
			f[j] = (f[j] + f[j-c]) % mod
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function waysToChange(n: number): number {
    const mod = 10 ** 9 + 7;
    const coins: number[] = [25, 10, 5, 1];
    const f: number[] = new Array(n + 1).fill(0);
    f[0] = 1;
    for (const c of coins) {
        for (let i = c; i <= n; ++i) {
            f[i] = (f[i] + f[i - c]) % mod;
        }
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
