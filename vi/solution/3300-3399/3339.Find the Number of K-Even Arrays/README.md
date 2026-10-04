---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [3339. Find the Number of K-Even Arrays 🔒](https://leetcode.com/problems/find-the-number-of-k-even-arrays)

[中文文档](/solution/3300-3399/3339.Find%20the%20Number%20of%20K-Even%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>n</code>, <code>m</code> và <code>k</code>.</p>

<p>Một mảng <code>arr</code> được gọi là <strong>k-even</strong> nếu có <strong>đúng</strong> <code>k</code> chỉ số sao cho với mỗi chỉ số <code>i</code> trong các chỉ số đó (<code>0 &lt;= i &lt; n - 1</code>):</p>

<ul>
    <li><code>(arr[i] * arr[i + 1]) - arr[i] - arr[i + 1]</code> là <em>chẵn</em>.</li>
</ul>

<p>Trả về số lượng mảng <strong>k-even</strong> có kích thước <code>n</code> mà mọi phần tử đều nằm trong khoảng <code>[1, m]</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, m = 4, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>8 mảng 2-even có thể có là:</p>

<ul>
    <li><code>[2, 2, 2]</code></li>
    <li><code>[2, 2, 4]</code></li>
    <li><code>[2, 4, 2]</code></li>
    <li><code>[2, 4, 4]</code></li>
    <li><code>[4, 2, 2]</code></li>
    <li><code>[4, 2, 4]</code></li>
    <li><code>[4, 4, 2]</code></li>
    <li><code>[4, 4, 4]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, m = 1, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng 0-even duy nhất là <code>[1, 1, 1, 1, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, m = 7, k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5832</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 750</code></li>
    <li><code>0 &lt;= k &lt;= n - 1</code></li>
    <li><code>1 &lt;= m &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm với memoization

<!-- thinking:start -->

> **Tư duy**
>
> Ta đếm các mảng độ dài $n$ trên $[1,m]$ có đúng $k$ cặp kề nhau thỏa điều kiện chẵn. Chỉ cần quan tâm đến tính chẵn lẻ: có $\lfloor m/2 \rfloor$ số chẵn và $m-\lfloor m/2 \rfloor$ số lẻ.
>
> Trạng thái $(i,j,\textit{last})$ là số cách sau $i$ vị trí, với $j$ cặp chẵn còn thiếu và tính chẵn lẻ của phần tử trước đó. Một cặp chẵn tiêu tốn một cặp chỉ khi giá trị trước đó là số chẵn.
>
> $j$ âm cho kết quả bằng 0; mảng hoàn chỉnh cho kết quả bằng 1 khi và chỉ khi $j=0$. Tính chẵn lẻ trước đó giả lập là lẻ để ô đầu tiên không tạo thành một cặp.

<!-- thinking:end -->

Với các số $[1, m]$, có $\textit{cnt0} = \lfloor \frac{m}{2} \rfloor$ số chẵn và $\textit{cnt1} = m - \textit{cnt0}$ số lẻ.

Ta thiết kế hàm $\textit{dfs}(i, j, k)$, biểu thị số cách điền đến vị trí thứ $i$, với $j$ vị trí còn lại cần thỏa điều kiện và tính chẵn lẻ của vị trí cuối cùng là $k$, trong đó $k = 0$ biểu thị vị trí cuối cùng là số chẵn, còn $k = 1$ biểu thị vị trí cuối cùng là số lẻ. Đáp án là $\textit{dfs}(0, k, 1)$.

Logic thực thi của hàm $\textit{dfs}(i, j, k)$ như sau:

- Nếu $j < 0$, nghĩa là số vị trí còn lại nhỏ hơn $0$, trả về $0$;
- Nếu $i \ge n$, nghĩa là đã điền xong mọi vị trí. Nếu $j = 0$, nghĩa là điều kiện được thỏa mãn, trả về $1$, ngược lại trả về $0$;
- Nếu không, ta có thể chọn điền một số lẻ hoặc một số chẵn, tính số cách cho cả hai trường hợp rồi trả về tổng của chúng.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n \times k)$. Ở đây, $n$ và $k$ là các tham số được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOfArrays(self, n: int, m: int, k: int) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if j < 0:
                return 0
            if i >= n:
                return int(j == 0)
            return (
                cnt1 * dfs(i + 1, j, 1) + cnt0 * dfs(i + 1, j - (k & 1 ^ 1), 0)
            ) % mod

        cnt0 = m // 2
        cnt1 = m - cnt0
        mod = 10**9 + 7
        ans = dfs(0, k, 1)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private Integer[][][] f;
    private long cnt0, cnt1;
    private final int mod = (int) 1e9 + 7;

    public int countOfArrays(int n, int m, int k) {
        f = new Integer[n][k + 1][2];
        cnt0 = m / 2;
        cnt1 = m - cnt0;
        return dfs(0, k, 1);
    }

    private int dfs(int i, int j, int k) {
        if (j < 0) {
            return 0;
        }
        if (i >= f.length) {
            return j == 0 ? 1 : 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int a = (int) (cnt1 * dfs(i + 1, j, 1) % mod);
        int b = (int) (cnt0 * dfs(i + 1, j - (k & 1 ^ 1), 0) % mod);
        return f[i][j][k] = (a + b) % mod;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOfArrays(int n, int m, int k) {
        int f[n][k + 1][2];
        memset(f, -1, sizeof(f));
        const int mod = 1e9 + 7;
        int cnt0 = m / 2;
        int cnt1 = m - cnt0;
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> int {
            if (j < 0) {
                return 0;
            }
            if (i >= n) {
                return j == 0 ? 1 : 0;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            int a = 1LL * cnt1 * dfs(i + 1, j, 1) % mod;
            int b = 1LL * cnt0 * dfs(i + 1, j - (k & 1 ^ 1), 0) % mod;
            return f[i][j][k] = (a + b) % mod;
        };
        return dfs(0, k, 1);
    }
};
```

#### Go

```go
func countOfArrays(n int, m int, k int) int {
    f := make([][][2]int, n)
    for i := range f {
        f[i] = make([][2]int, k+1)
        for j := range f[i] {
            f[i][j] = [2]int{-1, -1}
        }
    }
    const mod int = 1e9 + 7
    cnt0 := m / 2
    cnt1 := m - cnt0
    var dfs func(int, int, int) int
    dfs = func(i, j, k int) int {
        if j < 0 {
            return 0
        }
        if i >= n {
            if j == 0 {
                return 1
            }
            return 0
        }
        if f[i][j][k] != -1 {
            return f[i][j][k]
        }
        a := cnt1 * dfs(i+1, j, 1) % mod
        b := cnt0 * dfs(i+1, j-(k&1^1), 0) % mod
        f[i][j][k] = (a + b) % mod
        return f[i][j][k]
    }
    return dfs(0, k, 1)
}
```

#### TypeScript

```ts
function countOfArrays(n: number, m: number, k: number): number {
    const f = Array.from({ length: n }, () =>
        Array.from({ length: k + 1 }, () => Array(2).fill(-1)),
    );
    const mod = 1e9 + 7;
    const cnt0 = Math.floor(m / 2);
    const cnt1 = m - cnt0;
    const dfs = (i: number, j: number, k: number): number => {
        if (j < 0) {
            return 0;
        }
        if (i >= n) {
            return j === 0 ? 1 : 0;
        }
        if (f[i][j][k] !== -1) {
            return f[i][j][k];
        }
        const a = (cnt1 * dfs(i + 1, j, 1)) % mod;
        const b = (cnt0 * dfs(i + 1, j - ((k & 1) ^ 1), 0)) % mod;
        return (f[i][j][k] = (a + b) % mod);
    };
    return dfs(0, k, 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm có memoization có thể chậm hơn trong một số ngôn ngữ. Ta có thể chuyển cùng công thức chuyển trạng thái thành DP theo từng lớp: $f[i][j][0/1]$ sau $i$ vị trí, với $j$ cặp chẵn và tính chẵn lẻ của phần tử cuối cùng.
>
> Một số chẵn được tạo ra từ phần tử cuối là số lẻ, hoặc từ phần tử cuối là số chẵn với $j-1$ cặp; một số lẻ không làm tăng số cặp. $f[0][0][1]=1$ tương ứng với trạng thái cơ sở của phép tìm kiếm.
>
> Đáp án là $f[n][k][0]+f[n][k][1]$.

<!-- thinking:end -->

Ta có thể chuyển phép tìm kiếm có memoization ở Lời giải 1 thành quy hoạch động.

Định nghĩa $f[i][j][k]$ là số cách điền vị trí thứ $i$, với $j$ vị trí thỏa điều kiện và tính chẵn lẻ của vị trí trước đó là $k$. Đáp án sẽ là $\sum_{k = 0}^{1} f[n][k]$.

Ban đầu, ta đặt $f[0][0][1] = 1$, biểu thị rằng sau khi điền vị trí thứ $0$, có $0$ vị trí thỏa điều kiện và tính chẵn lẻ của vị trí trước đó là lẻ. Mọi $f[i][j][k]$ khác được khởi tạo bằng $0$.

Các phương trình chuyển trạng thái như sau:

$$
\begin{aligned}
f[i][j][0] &= \left( f[i - 1][j][1] + \left( f[i - 1][j - 1][0] \text{ if } j > 0 \right) \right) \times \textit{cnt0} \bmod \textit{mod}, \\
f[i][j][1] &= \left( f[i - 1][j][0] + f[i - 1][j][1] \right) \times \textit{cnt1} \bmod \textit{mod}.
\end{aligned}
$$

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ và $k$ là các tham số được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOfArrays(self, n: int, m: int, k: int) -> int:
        f = [[[0] * 2 for _ in range(k + 1)] for _ in range(n + 1)]
        cnt0 = m // 2
        cnt1 = m - cnt0
        mod = 10**9 + 7
        f[0][0][1] = 1
        for i in range(1, n + 1):
            for j in range(k + 1):
                f[i][j][0] = (
                    (f[i - 1][j][1] + (f[i - 1][j - 1][0] if j else 0)) * cnt0 % mod
                )
                f[i][j][1] = (f[i - 1][j][0] + f[i - 1][j][1]) * cnt1 % mod
        return sum(f[n][k]) % mod
```

#### Java

```java
class Solution {
    public int countOfArrays(int n, int m, int k) {
        int[][][] f = new int[n + 1][k + 1][2];
        int cnt0 = m / 2;
        int cnt1 = m - cnt0;
        final int mod = (int) 1e9 + 7;
        f[0][0][1] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= k; ++j) {
                f[i][j][0]
                    = (int) (1L * cnt0 * (f[i - 1][j][1] + (j > 0 ? f[i - 1][j - 1][0] : 0)) % mod);
                f[i][j][1] = (int) (1L * cnt1 * (f[i - 1][j][0] + f[i - 1][j][1]) % mod);
            }
        }
        return (f[n][k][0] + f[n][k][1]) % mod;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOfArrays(int n, int m, int k) {
        int f[n + 1][k + 1][2];
        memset(f, 0, sizeof(f));
        f[0][0][1] = 1;
        const int mod = 1e9 + 7;
        int cnt0 = m / 2;
        int cnt1 = m - cnt0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= k; ++j) {
                f[i][j][0] = 1LL * (f[i - 1][j][1] + (j ? f[i - 1][j - 1][0] : 0)) * cnt0 % mod;
                f[i][j][1] = 1LL * (f[i - 1][j][0] + f[i - 1][j][1]) * cnt1 % mod;
            }
        }
        return (f[n][k][0] + f[n][k][1]) % mod;
    }
};
```

#### Go

```go
func countOfArrays(n int, m int, k int) int {
    f := make([][][2]int, n+1)
    for i := range f {
        f[i] = make([][2]int, k+1)
    }
    f[0][0][1] = 1
    cnt0 := m / 2
    cnt1 := m - cnt0
    const mod int = 1e9 + 7
    for i := 1; i <= n; i++ {
        for j := 0; j <= k; j++ {
            f[i][j][0] = cnt0 * f[i-1][j][1] % mod
            if j > 0 {
                f[i][j][0] = (f[i][j][0] + cnt0*f[i-1][j-1][0]%mod) % mod
            }
            f[i][j][1] = cnt1 * (f[i-1][j][0] + f[i-1][j][1]) % mod
        }
    }
    return (f[n][k][0] + f[n][k][1]) % mod
}
```

#### TypeScript

```ts
function countOfArrays(n: number, m: number, k: number): number {
    const f: number[][][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: k + 1 }, () => Array(2).fill(0)),
    );
    f[0][0][1] = 1;
    const mod = 1e9 + 7;
    const cnt0 = Math.floor(m / 2);
    const cnt1 = m - cnt0;
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j <= k; ++j) {
            f[i][j][0] = (cnt0 * (f[i - 1][j][1] + (j ? f[i - 1][j - 1][0] : 0))) % mod;
            f[i][j][1] = (cnt1 * (f[i - 1][j][0] + f[i - 1][j][1])) % mod;
        }
    }
    return (f[n][k][0] + f[n][k][1]) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động (Tối ưu hóa không gian)

<!-- thinking:start -->

> **Tư duy**
>
> $f[i]$ chỉ phụ thuộc vào $f[i-1]$, nên có thể loại bỏ chỉ số đầu tiên của bảng.
>
> Một bộ đệm $g$ nhận lớp mới rồi thay thế $f$, giảm không gian xuống $O(k)$ mà không làm thay đổi bậc thời gian.
>
> Đáp án vẫn là tổng của hai tính chẵn lẻ tại $j=k$ sau $n$ lớp.

<!-- thinking:end -->

Ta nhận thấy việc tính $f[i]$ chỉ phụ thuộc vào $f[i - 1]$, cho phép tối ưu mức sử dụng không gian bằng một mảng cuộn.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(k)$, trong đó $n$ và $k$ là các tham số được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOfArrays(self, n: int, m: int, k: int) -> int:
        f = [[0] * 2 for _ in range(k + 1)]
        cnt0 = m // 2
        cnt1 = m - cnt0
        mod = 10**9 + 7
        f[0][1] = 1
        for _ in range(n):
            g = [[0] * 2 for _ in range(k + 1)]
            for j in range(k + 1):
                g[j][0] = (f[j][1] + (f[j - 1][0] if j else 0)) * cnt0 % mod
                g[j][1] = (f[j][0] + f[j][1]) * cnt1 % mod
            f = g
        return sum(f[k]) % mod
```

#### Java

```java
class Solution {
    public int countOfArrays(int n, int m, int k) {
        int[][] f = new int[k + 1][2];
        int cnt0 = m / 2;
        int cnt1 = m - cnt0;
        final int mod = (int) 1e9 + 7;
        f[0][1] = 1;
        for (int i = 0; i < n; ++i) {
            int[][] g = new int[k + 1][2];
            for (int j = 0; j <= k; ++j) {
                g[j][0] = (int) (1L * cnt0 * (f[j][1] + (j > 0 ? f[j - 1][0] : 0)) % mod);
                g[j][1] = (int) (1L * cnt1 * (f[j][0] + f[j][1]) % mod);
            }
            f = g;
        }
        return (f[k][0] + f[k][1]) % mod;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOfArrays(int n, int m, int k) {
        vector<vector<int>> f(k + 1, vector<int>(2));
        int cnt0 = m / 2;
        int cnt1 = m - cnt0;
        const int mod = 1e9 + 7;
        f[0][1] = 1;

        for (int i = 0; i < n; ++i) {
            vector<vector<int>> g(k + 1, vector<int>(2));
            for (int j = 0; j <= k; ++j) {
                g[j][0] = (1LL * cnt0 * (f[j][1] + (j > 0 ? f[j - 1][0] : 0)) % mod) % mod;
                g[j][1] = (1LL * cnt1 * (f[j][0] + f[j][1]) % mod) % mod;
            }
            f = g;
        }
        return (f[k][0] + f[k][1]) % mod;
    }
};
```

#### Go

```go
func countOfArrays(n int, m int, k int) int {
    const mod = 1e9 + 7
    cnt0 := m / 2
    cnt1 := m - cnt0
    f := make([][2]int, k+1)
    f[0][1] = 1

    for i := 0; i < n; i++ {
        g := make([][2]int, k+1)
        for j := 0; j <= k; j++ {
            g[j][0] = (cnt0 * (f[j][1] + func() int {
                if j > 0 {
                    return f[j-1][0]
                }
                return 0
            }()) % mod) % mod
            g[j][1] = (cnt1 * (f[j][0] + f[j][1]) % mod) % mod
        }
        f = g
    }

    return (f[k][0] + f[k][1]) % mod
}
```

#### TypeScript

```ts
function countOfArrays(n: number, m: number, k: number): number {
    const mod = 1e9 + 7;
    const cnt0 = Math.floor(m / 2);
    const cnt1 = m - cnt0;
    const f: number[][] = Array.from({ length: k + 1 }, () => [0, 0]);
    f[0][1] = 1;
    for (let i = 0; i < n; i++) {
        const g: number[][] = Array.from({ length: k + 1 }, () => [0, 0]);
        for (let j = 0; j <= k; j++) {
            g[j][0] = ((cnt0 * (f[j][1] + (j > 0 ? f[j - 1][0] : 0))) % mod) % mod;
            g[j][1] = ((cnt1 * (f[j][0] + f[j][1])) % mod) % mod;
        }
        f.splice(0, f.length, ...g);
    }
    return (f[k][0] + f[k][1]) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
