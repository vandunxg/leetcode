---
comments: true
difficulty: Easy
rating: 1142
source: Weekly Contest 147 Q1
tags:
    - Memoization
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [1137. N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number)

[中文文档](/solution/1100-1199/1137.N-th%20Tribonacci%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Dãy Tribonacci T<sub>n</sub> được định nghĩa như sau:&nbsp;</p>

<p>T<sub>0</sub> = 0, T<sub>1</sub> = 1, T<sub>2</sub> = 1, và T<sub>n+3</sub> = T<sub>n</sub> + T<sub>n+1</sub> + T<sub>n+2</sub> với n &gt;= 0.</p>

<p>Cho <code>n</code>, hãy trả về giá trị T<sub>n</sub>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 4
<strong>Output:</strong> 4
<strong>Giải thích:</strong>
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 25
<strong>Output:</strong> 1389537
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 37</code></li>
	<li>Đảm bảo đáp án vừa với số nguyên 32-bit, tức là <code>answer &lt;= 2^31 - 1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Dãy Tribonacci có công thức truy hồi dựa trên ba số hạng. Vì $n\le 37$, chỉ cần duyệt tuyến tính. Ba biến luân phiên lưu các số hạng gần nhất; sau $n$ lần cập nhật, biến đầu tiên chứa $T_n$, với bộ nhớ phụ hằng số.

<!-- thinking:end -->

Dựa trên công thức truy hồi của đề bài, ta có thể dùng quy hoạch động để giải.

Định nghĩa ba biến $a$, $b$, $c$ lần lượt biểu diễn $T_{n-3}$, $T_{n-2}$, $T_{n-1}$, với các giá trị ban đầu là $0$, $1$, $1$.

Sau đó, giảm $n$ dần về $0$, mỗi lần cập nhật các giá trị $a$, $b$, $c$. Khi $n$ bằng $0$, đáp án là $a$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, với $n$ là số nguyên đầu vào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tribonacci(self, n: int) -> int:
        a, b, c = 0, 1, 1
        for _ in range(n):
            a, b, c = b, c, a + b + c
        return a
```

#### Java

```java
class Solution {
    public int tribonacci(int n) {
        int a = 0, b = 1, c = 1;
        while (n-- > 0) {
            int d = a + b + c;
            a = b;
            b = c;
            c = d;
        }
        return a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int tribonacci(int n) {
        long long a = 0, b = 1, c = 1;
        while (n--) {
            long long d = a + b + c;
            a = b;
            b = c;
            c = d;
        }
        return (int) a;
    }
};
```

#### Go

```go
func tribonacci(n int) int {
	a, b, c := 0, 1, 1
	for i := 0; i < n; i++ {
		a, b, c = b, c, a+b+c
	}
	return a
}
```

#### TypeScript

```ts
function tribonacci(n: number): number {
    let [a, b, c] = [0, 1, 1];
    while (n--) {
        let d = a + b + c;
        a = b;
        b = c;
        c = d;
    }
    return a;
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var tribonacci = function (n) {
    let a = 0;
    let b = 1;
    let c = 1;
    while (n--) {
        let d = a + b + c;
        a = b;
        b = c;
        c = d;
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
    function tribonacci($n) {
        $a = 0;
        $b = 1;
        $c = 1;

        while ($n--) {
            $d = $a + $b + $c;
            $a = $b;
            $b = $c;
            $c = $d;
        }

        return $a;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lũy thừa ma trận để tăng tốc truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 có độ phức tạp $O(n)$. Viết công thức truy hồi dưới dạng state $1\times 3$ nhân với ma trận $3\times 3$ cho phép dùng lũy thừa nhị phân để tính $T_n$ với $O(\log n)$ phép nhân, hiệu quả hơn khi $n$ lớn.

<!-- thinking:end -->

Ta định nghĩa $Tib(n)$ là ma trận $1 \times 3$ có dạng $\begin{bmatrix} T_n & T_{n - 1} & T_{n - 2} \end{bmatrix}$, trong đó $T_n$, $T_{n - 1}$ và $T_{n - 2}$ lần lượt là số Tribonacci thứ $n$, $(n - 1)$ và $(n - 2)$.

Ta muốn suy ra $Tib(n)$ từ $Tib(n-1) = \begin{bmatrix} T_{n - 1} & T_{n - 2} & T_{n - 3} \end{bmatrix}$. Tức là, cần tìm ma trận $base$ sao cho $Tib(n - 1) \times base = Tib(n)$, cụ thể là

$$
\begin{bmatrix}
T_{n - 1} & T_{n - 2} & T_{n - 3}
\end{bmatrix} \times base = \begin{bmatrix} T_n & T_{n - 1} & T_{n - 2} \end{bmatrix}
$$

Vì $T_n = T_{n - 1} + T_{n - 2} + T_{n - 3}$, ma trận $base$ là:

$$
\begin{bmatrix}
 1 & 1 & 0 \\
 1 & 0 & 1 \\
 1 & 0 & 0
\end{bmatrix}
$$

Đặt ma trận khởi tạo $res = \begin{bmatrix} 1 & 1 & 0 \end{bmatrix}$. Khi đó, $T_n$ bằng tổng các phần tử của ma trận kết quả khi nhân $res$ với $base^{n - 3}$. Có thể tính bằng lũy thừa ma trận.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
import numpy as np


class Solution:
    def tribonacci(self, n: int) -> int:
        if n == 0:
            return 0
        if n < 3:
            return 1
        factor = np.asmatrix([(1, 1, 0), (1, 0, 1), (1, 0, 0)], np.dtype("O"))
        res = np.asmatrix([(1, 1, 0)], np.dtype("O"))
        n -= 3
        while n:
            if n & 1:
                res *= factor
            factor *= factor
            n >>= 1
        return res.sum()
```

#### Java

```java
class Solution {
    public int tribonacci(int n) {
        if (n == 0) {
            return 0;
        }
        if (n < 3) {
            return 1;
        }
        int[][] a = {{1, 1, 0}, {1, 0, 1}, {1, 0, 0}};
        int[][] res = pow(a, n - 3);
        int ans = 0;
        for (int x : res[0]) {
            ans += x;
        }
        return ans;
    }

    private int[][] mul(int[][] a, int[][] b) {
        int m = a.length, n = b[0].length;
        int[][] c = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.length; ++k) {
                    c[i][j] += a[i][k] * b[k][j];
                }
            }
        }
        return c;
    }

    private int[][] pow(int[][] a, int n) {
        int[][] res = {{1, 1, 0}};
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
    int tribonacci(int n) {
        if (n == 0) {
            return 0;
        }
        if (n < 3) {
            return 1;
        }
        vector<vector<ll>> a = {{1, 1, 0}, {1, 0, 1}, {1, 0, 0}};
        vector<vector<ll>> res = pow(a, n - 3);
        return accumulate(res[0].begin(), res[0].end(), 0);
    }

private:
    using ll = long long;
    vector<vector<ll>> mul(vector<vector<ll>>& a, vector<vector<ll>>& b) {
        int m = a.size(), n = b[0].size();
        vector<vector<ll>> c(m, vector<ll>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.size(); ++k) {
                    c[i][j] += a[i][k] * b[k][j];
                }
            }
        }
        return c;
    }

    vector<vector<ll>> pow(vector<vector<ll>>& a, int n) {
        vector<vector<ll>> res = {{1, 1, 0}};
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
func tribonacci(n int) (ans int) {
	if n == 0 {
		return 0
	}
	if n < 3 {
		return 1
	}
	a := [][]int{{1, 1, 0}, {1, 0, 1}, {1, 0, 0}}
	res := pow(a, n-3)
	for _, x := range res[0] {
		ans += x
	}
	return
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
				c[i][j] += a[i][k] * b[k][j]
			}
		}
	}
	return c
}

func pow(a [][]int, n int) [][]int {
	res := [][]int{{1, 1, 0}}
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
function tribonacci(n: number): number {
    if (n === 0) {
        return 0;
    }
    if (n < 3) {
        return 1;
    }
    const a = [
        [1, 1, 0],
        [1, 0, 1],
        [1, 0, 0],
    ];
    return pow(a, n - 3)[0].reduce((a, b) => a + b);
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
    let res = [[1, 1, 0]];
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

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var tribonacci = function (n) {
    if (n === 0) {
        return 0;
    }
    if (n < 3) {
        return 1;
    }
    const a = [
        [1, 1, 0],
        [1, 0, 1],
        [1, 0, 0],
    ];
    return pow(a, n - 3)[0].reduce((a, b) => a + b);
};

function mul(a, b) {
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

function pow(a, n) {
    let res = [[1, 1, 0]];
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

#### PHP

```php
class Solution {
    /**
     * @param Integer $n
     * @return Integer
     */
    function tribonacci($n) {
        if ($n === 0) {
            return 0;
        }
        if ($n < 3) {
            return 1;
        }

        $a = [[1, 1, 0], [1, 0, 1], [1, 0, 0]];

        $res = $this->pow($a, $n - 3);
        return array_sum($res[0]);
    }

    private function mul($a, $b) {
        $m = count($a);
        $n = count($b[0]);
        $p = count($b);

        $c = array_fill(0, $m, array_fill(0, $n, 0));

        for ($i = 0; $i < $m; ++$i) {
            for ($j = 0; $j < $n; ++$j) {
                for ($k = 0; $k < $p; ++$k) {
                    $c[$i][$j] += $a[$i][$k] * $b[$k][$j];
                }
            }
        }

        return $c;
    }

    private function pow($a, $n) {
        $res = [[1, 1, 0]];
        while ($n > 0) {
            if ($n & 1) {
                $res = $this->mul($res, $a);
            }
            $a = $this->mul($a, $a);
            $n >>= 1;
        }
        return $res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
