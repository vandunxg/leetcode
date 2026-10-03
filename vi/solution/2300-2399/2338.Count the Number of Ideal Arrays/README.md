---
comments: true
difficulty: Hard
rating: 2615
source: Weekly Contest 301 Q4
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
    - Number Theory
    - Prime Factorization
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [2338. Count the Number of Ideal Arrays](https://leetcode.com/problems/count-the-number-of-ideal-arrays)

[中文文档](/solution/2300-2399/2338.Count%20the%20Number%20of%20Ideal%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>n</code> và <code>maxValue</code>, dùng để mô tả một mảng <strong>ideal</strong>.</p>

<p>Một mảng số nguyên <code>arr</code> có độ dài <code>n</code>, được đánh chỉ số <strong>0-indexed</strong>, được xem là <strong>ideal</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Mọi <code>arr[i]</code> đều là một giá trị từ <code>1</code> đến <code>maxValue</code>, với <code>0 &lt;= i &lt; n</code>.</li>
	<li>Mọi <code>arr[i]</code> đều chia hết cho <code>arr[i - 1]</code>, với <code>0 &lt; i &lt; n</code>.</li>
</ul>

<p>Hãy trả về <em>số lượng mảng ideal <strong>khác nhau</strong> có độ dài </em><code>n</code>. Vì đáp án có thể rất lớn, hãy trả về kết quả theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, maxValue = 5
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Các mảng ideal có thể là:
- Các mảng bắt đầu bằng giá trị 1 (5 mảng): [1,1], [1,2], [1,3], [1,4], [1,5]
- Các mảng bắt đầu bằng giá trị 2 (2 mảng): [2,2], [2,4]
- Các mảng bắt đầu bằng giá trị 3 (1 mảng): [3,3]
- Các mảng bắt đầu bằng giá trị 4 (1 mảng): [4,4]
- Các mảng bắt đầu bằng giá trị 5 (1 mảng): [5,5]
Tổng cộng có 5 + 2 + 1 + 1 + 1 = 10 mảng ideal khác nhau.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, maxValue = 3
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Các mảng ideal có thể là:
- Các mảng bắt đầu bằng giá trị 1 (9 mảng):
   - Không có giá trị khác biệt nào khác (1 mảng): [1,1,1,1,1]
   - Giá trị khác biệt thứ 2<sup>nd</sup> là 2 (4 mảng): [1,1,1,1,2], [1,1,1,2,2], [1,1,2,2,2], [1,2,2,2,2]
   - Giá trị khác biệt thứ 2<sup>nd</sup> là 3 (4 mảng): [1,1,1,1,3], [1,1,1,3,3], [1,1,3,3,3], [1,3,3,3,3]
- Các mảng bắt đầu bằng giá trị 2 (1 mảng): [2,2,2,2,2]
- Các mảng bắt đầu bằng giá trị 3 (1 mảng): [3,3,3,3,3]
Tổng cộng có 9 + 1 + 1 = 11 mảng ideal khác nhau.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= maxValue &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị tiếp theo phải là bội số của giá trị trước đó. Vì cả $n$ và $\textit{maxValue}$ đều có thể đạt $10^4$, không thể liệt kê các mảng. Một chuỗi nhân nghiêm ngặt có độ dài tối đa $\log \textit{maxValue}$.
>
> Ta đếm các chuỗi $f[i][j]$ kết thúc tại $i$ và có $j$ giá trị khác nhau, sau đó mở rộng một chuỗi gồm $j$ giá trị thành $n$ vị trí bằng phương pháp chia sao và vạch, nhân với $c_{n-1}^{j-1}$. Các tổ hợp được xây dựng lần lượt theo từng hàng; độ dài chuỗi được giới hạn ở $16$.

<!-- thinking:end -->

Gọi $f[i][j]$ là số lượng dãy kết thúc bằng $i$ và gồm $j$ phần tử khác nhau. Giá trị khởi tạo là $f[i][1] = 1$.

Xét $n$ quả bóng, cuối cùng được chia thành $j$ phần. Theo "phương pháp vạch ngăn", ta có thể đặt $j-1$ vạch ngăn vào $n-1$ vị trí, nên số tổ hợp là $c_{n-1}^{j-1}$.

Ta có thể tiền xử lý các số tổ hợp $c[i][j]$ bằng công thức truy hồi $c[i][j] = c[i-1][j] + c[i-1][j-1]$. Cụ thể, khi $j=0$, $c[i][j] = 1$.

Đáp án cuối cùng là:

$$
\sum\limits_{i=1}^{k}\sum\limits_{j=1}^{\log_2 k + 1} f[i][j] \times c_{n-1}^{j-1}
$$

trong đó $k$ là giá trị lớn nhất của mảng, tức là $\textit{maxValue}$.

- **Độ phức tạp thời gian**: $O(m \times \log^2 m)$
- **Độ phức tạp không gian**: $O(m \times \log m)$

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def idealArrays(self, n: int, maxValue: int) -> int:
        @cache
        def dfs(i, cnt):
            res = c[-1][cnt - 1]
            if cnt < n:
                k = 2
                while k * i <= maxValue:
                    res = (res + dfs(k * i, cnt + 1)) % mod
                    k += 1
            return res

        c = [[0] * 16 for _ in range(n)]
        mod = 10**9 + 7
        for i in range(n):
            for j in range(min(16, i + 1)):
                c[i][j] = 1 if j == 0 else (c[i - 1][j] + c[i - 1][j - 1]) % mod
        ans = 0
        for i in range(1, maxValue + 1):
            ans = (ans + dfs(i, 1)) % mod
        return ans
```

#### Java

```java
class Solution {
    private int[][] f;
    private int[][] c;
    private int n;
    private int m;
    private static final int MOD = (int) 1e9 + 7;

    public int idealArrays(int n, int maxValue) {
        this.n = n;
        this.m = maxValue;
        this.f = new int[maxValue + 1][16];
        for (int[] row : f) {
            Arrays.fill(row, -1);
        }
        c = new int[n][16];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j <= i && j < 16; ++j) {
                c[i][j] = j == 0 ? 1 : (c[i - 1][j] + c[i - 1][j - 1]) % MOD;
            }
        }
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            ans = (ans + dfs(i, 1)) % MOD;
        }
        return ans;
    }

    private int dfs(int i, int cnt) {
        if (f[i][cnt] != -1) {
            return f[i][cnt];
        }
        int res = c[n - 1][cnt - 1];
        if (cnt < n) {
            for (int k = 2; k * i <= m; ++k) {
                res = (res + dfs(k * i, cnt + 1)) % MOD;
            }
        }
        f[i][cnt] = res;
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int m, n;
    const int mod = 1e9 + 7;
    vector<vector<int>> f;
    vector<vector<int>> c;

    int idealArrays(int n, int maxValue) {
        this->m = maxValue;
        this->n = n;
        f.assign(maxValue + 1, vector<int>(16, -1));
        c.assign(n, vector<int>(16, 0));
        for (int i = 0; i < n; ++i)
            for (int j = 0; j <= i && j < 16; ++j)
                c[i][j] = !j ? 1 : (c[i - 1][j] + c[i - 1][j - 1]) % mod;
        int ans = 0;
        for (int i = 1; i <= m; ++i) ans = (ans + dfs(i, 1)) % mod;
        return ans;
    }

    int dfs(int i, int cnt) {
        if (f[i][cnt] != -1) return f[i][cnt];
        int res = c[n - 1][cnt - 1];
        if (cnt < n)
            for (int k = 2; k * i <= m; ++k)
                res = (res + dfs(k * i, cnt + 1)) % mod;
        f[i][cnt] = res;
        return res;
    }
};
```

#### Go

```go
func idealArrays(n int, maxValue int) int {
	mod := int(1e9) + 7
	m := maxValue
	c := make([][]int, n)
	f := make([][]int, m+1)
	for i := range c {
		c[i] = make([]int, 16)
	}
	for i := range f {
		f[i] = make([]int, 16)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(int, int) int
	dfs = func(i, cnt int) int {
		if f[i][cnt] != -1 {
			return f[i][cnt]
		}
		res := c[n-1][cnt-1]
		if cnt < n {
			for k := 2; k*i <= m; k++ {
				res = (res + dfs(k*i, cnt+1)) % mod
			}
		}
		f[i][cnt] = res
		return res
	}
	for i := 0; i < n; i++ {
		for j := 0; j <= i && j < 16; j++ {
			if j == 0 {
				c[i][j] = 1
			} else {
				c[i][j] = (c[i-1][j] + c[i-1][j-1]) % mod
			}
		}
	}
	ans := 0
	for i := 1; i <= m; i++ {
		ans = (ans + dfs(i, 1)) % mod
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phần trình bày trước đã đưa ra dynamic programming trên các chuỗi và hệ số tổ hợp. Tab này lặp lại cùng công thức truy hồi mà không có trạng thái mới.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def idealArrays(self, n: int, maxValue: int) -> int:
        c = [[0] * 16 for _ in range(n)]
        mod = 10**9 + 7
        for i in range(n):
            for j in range(min(16, i + 1)):
                c[i][j] = 1 if j == 0 else (c[i - 1][j] + c[i - 1][j - 1]) % mod
        f = [[0] * 16 for _ in range(maxValue + 1)]
        for i in range(1, maxValue + 1):
            f[i][1] = 1
        for j in range(1, 15):
            for i in range(1, maxValue + 1):
                k = 2
                while k * i <= maxValue:
                    f[k * i][j + 1] = (f[k * i][j + 1] + f[i][j]) % mod
                    k += 1
        ans = 0
        for i in range(1, maxValue + 1):
            for j in range(1, 16):
                ans = (ans + f[i][j] * c[-1][j - 1]) % mod
        return ans
```

#### Java

```java
class Solution {
    public int idealArrays(int n, int maxValue) {
        final int mod = (int) 1e9 + 7;
        int[][] c = new int[n][16];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j <= i && j < 16; ++j) {
                c[i][j] = j == 0 ? 1 : (c[i - 1][j] + c[i - 1][j - 1]) % mod;
            }
        }
        long[][] f = new long[maxValue + 1][16];
        for (int i = 1; i <= maxValue; ++i) {
            f[i][1] = 1;
        }
        for (int j = 1; j < 15; ++j) {
            for (int i = 1; i <= maxValue; ++i) {
                int k = 2;
                for (; k * i <= maxValue; ++k) {
                    f[k * i][j + 1] = (f[k * i][j + 1] + f[i][j]) % mod;
                }
            }
        }
        long ans = 0;
        for (int i = 1; i <= maxValue; ++i) {
            for (int j = 1; j < 16; ++j) {
                ans = (ans + f[i][j] * c[n - 1][j - 1]) % mod;
            }
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int idealArrays(int n, int maxValue) {
        const int mod = 1e9 + 7;
        vector<vector<int>> c(n, vector<int>(16));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j <= i && j < 16; ++j) {
                if (j == 0) {
                    c[i][j] = 1;
                } else {
                    c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
                }
            }
        }

        vector<vector<long long>> f(maxValue + 1, vector<long long>(16));
        for (int i = 1; i <= maxValue; ++i) {
            f[i][1] = 1;
        }

        for (int j = 1; j < 15; ++j) {
            for (int i = 1; i <= maxValue; ++i) {
                for (int k = 2; k * i <= maxValue; ++k) {
                    f[k * i][j + 1] = (f[k * i][j + 1] + f[i][j]) % mod;
                }
            }
        }

        long long ans = 0;
        for (int i = 1; i <= maxValue; ++i) {
            for (int j = 1; j < 16; ++j) {
                ans = (ans + f[i][j] * c[n - 1][j - 1]) % mod;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func idealArrays(n int, maxValue int) (ans int) {
	const mod = int(1e9 + 7)
	c := make([][]int, n)
	for i := 0; i < n; i++ {
		c[i] = make([]int, 16)
		for j := 0; j <= i && j < 16; j++ {
			if j == 0 {
				c[i][j] = 1
			} else {
				c[i][j] = (c[i-1][j] + c[i-1][j-1]) % mod
			}
		}
	}

	f := make([][16]int, maxValue+1)
	for i := 1; i <= maxValue; i++ {
		f[i][1] = 1
	}
	for j := 1; j < 15; j++ {
		for i := 1; i <= maxValue; i++ {
			for k := 2; k*i <= maxValue; k++ {
				f[k*i][j+1] = (f[k*i][j+1] + f[i][j]) % mod
			}
		}
	}

	for i := 1; i <= maxValue; i++ {
		for j := 1; j < 16; j++ {
			ans = (ans + f[i][j]*c[n-1][j-1]) % mod
		}
	}
	return
}
```

#### TypeScript

```ts
function idealArrays(n: number, maxValue: number): number {
    const mod = 1e9 + 7;

    const c: number[][] = Array.from({ length: n }, () => Array(16).fill(0));
    for (let i = 0; i < n; i++) {
        for (let j = 0; j <= i && j < 16; j++) {
            if (j === 0) {
                c[i][j] = 1;
            } else {
                c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
            }
        }
    }

    const f: number[][] = Array.from({ length: maxValue + 1 }, () => Array(16).fill(0));
    for (let i = 1; i <= maxValue; i++) {
        f[i][1] = 1;
    }

    for (let j = 1; j < 15; j++) {
        for (let i = 1; i <= maxValue; i++) {
            for (let k = 2; k * i <= maxValue; k++) {
                f[k * i][j + 1] = (f[k * i][j + 1] + f[i][j]) % mod;
            }
        }
    }

    let ans = 0;
    for (let i = 1; i <= maxValue; i++) {
        for (let j = 1; j < 16; j++) {
            ans = (ans + f[i][j] * c[n - 1][j - 1]) % mod;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
