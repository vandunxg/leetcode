---
comments: true
difficulty: Easy
tags:
    - Recursion
    - Memoization
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number)

[中文文档](/solution/0500-0599/0509.Fibonacci%20Number/README.md)

## Mô tả

<!-- description:start -->

<p><b>Các số Fibonacci</b>, thường được ký hiệu là <code>F(n)</code>, tạo thành một dãy gọi là <b>dãy Fibonacci</b>. Mỗi số bằng tổng của hai số đứng trước, bắt đầu từ <code>0</code> và <code>1</code>. Cụ thể:</p>

<pre>
F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n &gt; 1.
</pre>

<p>Cho <code>n</code>, hãy tính <code>F(n)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> F(2) = F(1) + F(0) = 1 + 0 = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> F(3) = F(2) + F(1) = 1 + 1 = 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> F(4) = F(3) + F(2) = 2 + 1 = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Công thức truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Dãy Fibonacci tuân theo công thức $F_n = F_{n-1} + F_{n-2}$. Đệ quy đơn giản sẽ tính lại cùng một bài toán con nhiều lần. Dù $n \le 30$, chỉ cần áp dụng công thức truy hồi bằng cách lặp là đủ.
>
> Dùng hai biến lưu hai số liên tiếp trong dãy và lặp $n$ lần. Độ phức tạp thời gian tuyến tính, bộ nhớ phụ là hằng số.

<!-- thinking:end -->

Ta khai báo hai biến $a$ và $b$, ban đầu lần lượt có giá trị $a = 0$ và $b = 1$.

Tiếp theo, ta lặp $n$ lần. Ở mỗi lượt, cập nhật $a$ thành $b$ và $b$ thành $a + b$.

Cuối cùng, trả về $a$.

Độ phức tạp thời gian là $O(n)$, với $n$ là số nguyên đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fib(self, n: int) -> int:
        a, b = 0, 1
        for _ in range(n):
            a, b = b, a + b
        return a
```

#### Java

```java
class Solution {
    public int fib(int n) {
        int a = 0, b = 1;
        while (n-- > 0) {
            int c = a + b;
            a = b;
            b = c;
        }
        return a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int fib(int n) {
        int a = 0, b = 1;
        while (n--) {
            int c = a + b;
            a = b;
            b = c;
        }
        return a;
    }
};
```

#### Go

```go
func fib(n int) int {
	a, b := 0, 1
	for i := 0; i < n; i++ {
		a, b = b, a+b
	}
	return a
}
```

#### TypeScript

```ts
function fib(n: number): number {
    let [a, b] = [0, 1];
    while (n--) {
        [a, b] = [b, a + b];
    }
    return a;
}
```

#### Rust

```rust
impl Solution {
    pub fn fib(n: i32) -> i32 {
        let mut a = 0;
        let mut b = 1;
        for _ in 0..n {
            let t = b;
            b = a + b;
            a = t;
        }
        a
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
    let [a, b] = [0, 1];
    while (n--) {
        [a, b] = [b, a + b];
    }
    return a;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $n
     * @return Integer
     */
    function fib($n) {
        $a = 0;
        $b = 1;
        for ($i = 0; $i < $n; $i++) {
            $temp = $a;
            $a = $b;
            $b = $temp + $b;
        }
        return $a;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lũy thừa ma trận

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã có độ phức tạp $O(n)$ và đủ dùng ở đây, nhưng có thể tăng tốc cùng công thức truy hồi tuyến tính này khi $n$ lớn hơn.
>
> $F_n$ có thể được tính bằng lũy thừa của một ma trận chuyển trạng thái cố định kích thước $2 \times 2$. Lũy thừa nhanh giảm số phép nhân từ $n$ xuống còn $O(\log n)$. Hằng số ẩn lớn hơn, nên dùng cách này khi cần mở rộng cùng công thức truy hồi cho $n$ lớn.

<!-- thinking:end -->

Ta định nghĩa $\textit{Fib}(n)$ là ma trận $1 \times 2$ $\begin{bmatrix} F_n & F_{n - 1} \end{bmatrix}$, trong đó $F_n$ và $F_{n - 1}$ lần lượt là số Fibonacci thứ $n$ và thứ $(n - 1)$.

Ta muốn suy ra $\textit{Fib}(n)$ từ $\textit{Fib}(n - 1) = \begin{bmatrix} F_{n - 1} & F_{n - 2} \end{bmatrix}$. Nói cách khác, ta cần tìm ma trận $\textit{base}$ sao cho $\textit{Fib}(n - 1) \times \textit{base} = \textit{Fib}(n)$, tức là:

$$
\begin{bmatrix}
F_{n - 1} & F_{n - 2}
\end{bmatrix} \times \textit{base} = \begin{bmatrix} F_n & F_{n - 1} \end{bmatrix}
$$

Vì $F_n = F_{n - 1} + F_{n - 2}$, cột thứ nhất của ma trận $\textit{base}$ là:

$$
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$

Cột thứ hai là:

$$
\begin{bmatrix}
1 \\
0
\end{bmatrix}
$$

Vậy ta có:

$$
\begin{bmatrix} F_{n - 1} & F_{n - 2} \end{bmatrix} \times \begin{bmatrix}1 & 1 \\ 1 & 0\end{bmatrix} = \begin{bmatrix} F_n & F_{n - 1} \end{bmatrix}
$$

Ta định nghĩa ma trận ban đầu $res = \begin{bmatrix} 1 & 0 \end{bmatrix}$. Khi đó, $F_n$ bằng phần tử thứ hai ở hàng đầu tiên của ma trận kết quả khi nhân $res$ với $\textit{base}^{n}$. Ta có thể giải bài toán bằng phép lũy thừa ma trận.

Độ phức tạp thời gian là $O(\log n)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
import numpy as np


class Solution:
    def fib(self, n: int) -> int:
        factor = np.asmatrix([(1, 1), (1, 0)], np.dtype("O"))
        res = np.asmatrix([(1, 0)], np.dtype("O"))
        while n:
            if n & 1:
                res = res * factor
            factor = factor * factor
            n >>= 1
        return res[0, 1]
```

#### Java

```java
class Solution {
    public int fib(int n) {
        int[][] a = {{1, 1}, {1, 0}};
        return pow(a, n)[0][1];
    }

    private int[][] mul(int[][] a, int[][] b) {
        int m = a.length, n = b[0].length;
        int[][] c = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.length; ++k) {
                    c[i][j] = c[i][j] + a[i][k] * b[k][j];
                }
            }
        }
        return c;
    }

    private int[][] pow(int[][] a, int n) {
        int[][] res = {{1, 0}};
        while (n > 0) {
            if ((n & 1) == 1) {
                res = mul(res, a);
            }
            a = mul(a, a);
            n >>= 1;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int fib(int n) {
        vector<vector<int>> a = {{1, 1}, {1, 0}};
        return qpow(a, n)[0][1];
    }

    vector<vector<int>> mul(vector<vector<int>>& a, vector<vector<int>>& b) {
        int m = a.size(), n = b[0].size();
        vector<vector<int>> c(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.size(); ++k) {
                    c[i][j] = c[i][j] + a[i][k] * b[k][j];
                }
            }
        }
        return c;
    }

    vector<vector<int>> qpow(vector<vector<int>>& a, int n) {
        vector<vector<int>> res = {{1, 0}};
        while (n) {
            if (n & 1) {
                res = mul(res, a);
            }
            a = mul(a, a);
            n >>= 1;
        }
        return res;
    }
};
```

#### Go

```go
func fib(n int) int {
	a := [][]int{{1, 1}, {1, 0}}
	return pow(a, n)[0][1]
}

func mul(a, b [][]int) [][]int {
	m, n := len(a), len(b[0])
	c := make([][]int, m)
	for i := range c {
		c[i] = make([]int, n)
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			for k := 0; k < len(b); k++ {
				c[i][j] = c[i][j] + a[i][k]*b[k][j]
			}
		}
	}
	return c
}

func pow(a [][]int, n int) [][]int {
	res := [][]int{{1, 0}}
	for n > 0 {
		if n&1 == 1 {
			res = mul(res, a)
		}
		a = mul(a, a)
		n >>= 1
	}
	return res
}
```

#### TypeScript

```ts
function fib(n: number): number {
    const a: number[][] = [
        [1, 1],
        [1, 0],
    ];
    return pow(a, n)[0][1];
}

function mul(a: number[][], b: number[][]): number[][] {
    const [m, n] = [a.length, b[0].length];
    const c = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let k = 0; k < b.length; ++k) {
                c[i][j] += a[i][k] * b[k][j];
            }
        }
    }
    return c;
}

function pow(a: number[][], n: number): number[][] {
    let res = [[1, 0]];
    while (n) {
        if (n & 1) {
            res = mul(res, a);
        }
        a = mul(a, a);
        n >>= 1;
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn fib(n: i32) -> i32 {
        let a = vec![vec![1, 1], vec![1, 0]];
        pow(a, n as usize)[0][1]
    }
}

fn mul(a: Vec<Vec<i32>>, b: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
    let m = a.len();
    let n = b[0].len();
    let mut c = vec![vec![0; n]; m];

    for i in 0..m {
        for j in 0..n {
            for k in 0..b.len() {
                c[i][j] += a[i][k] * b[k][j];
            }
        }
    }
    c
}

fn pow(mut a: Vec<Vec<i32>>, mut n: usize) -> Vec<Vec<i32>> {
    let mut res = vec![vec![1, 0], vec![0, 1]];

    while n > 0 {
        if n & 1 == 1 {
            res = mul(res, a.clone());
        }
        a = mul(a.clone(), a);
        n >>= 1;
    }
    res
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
    const a = [
        [1, 1],
        [1, 0],
    ];
    return pow(a, n)[0][1];
};

function mul(a, b) {
    const m = a.length,
        n = b[0].length;
    const c = Array.from({ length: m }, () => Array(n).fill(0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let k = 0; k < b.length; ++k) {
                c[i][j] += a[i][k] * b[k][j];
            }
        }
    }
    return c;
}

function pow(a, n) {
    let res = [
        [1, 0],
        [0, 1],
    ];
    while (n > 0) {
        if (n & 1) {
            res = mul(res, a);
        }
        a = mul(a, a);
        n >>= 1;
    }
    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
