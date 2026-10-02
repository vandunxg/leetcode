---
comments: true
difficulty: Hard
rating: 2126
source: Weekly Contest 188 Q4
tags:
    - Memoization
    - Array
    - Dynamic Programming
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [1444. Number of Ways of Cutting a Pizza](https://leetcode.com/problems/number-of-ways-of-cutting-a-pizza)

[中文文档](/solution/1400-1499/1444.Number%20of%20Ways%20of%20Cutting%20a%20Pizza/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chiếc pizza hình chữ nhật được biểu diễn bằng ma trận <code>rows x cols</code>&nbsp;chứa các ký tự sau: <code>&#39;A&#39;</code> (một quả táo) và <code>&#39;.</code>&#39; (ô trống), cùng với số nguyên <code>k</code>. Bạn cần cắt pizza thành <code>k</code> phần bằng <code>k-1</code> lần cắt.&nbsp;</p>

<p>Với mỗi lần cắt, bạn chọn hướng cắt: dọc hoặc ngang, sau đó chọn vị trí cắt tại ranh giới giữa các ô và cắt pizza thành hai phần. Nếu cắt pizza theo chiều dọc, đưa phần bên trái cho một người. Nếu cắt pizza theo chiều ngang, đưa phần bên trên cho một người. Đưa phần pizza cuối cùng cho người cuối cùng.</p>

<p><em>Trả về số cách cắt pizza sao cho mỗi phần chứa <strong>ít nhất</strong> một quả táo.&nbsp;</em>Vì đáp án có thể là một số rất lớn, hãy trả về kết quả theo modulo 10^9 + 7.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1444.Number%20of%20Ways%20of%20Cutting%20a%20Pizza/images/ways_to_cut_apple_1.png" style="width: 500px; height: 378px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> pizza = [&quot;A..&quot;,&quot;AAA&quot;,&quot;...&quot;], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hình trên minh họa ba cách cắt pizza. Lưu ý rằng mỗi phần phải chứa ít nhất một quả táo.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pizza = [&quot;A..&quot;,&quot;AA.&quot;,&quot;...&quot;], k = 3
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> pizza = [&quot;A..&quot;,&quot;A..&quot;,&quot;...&quot;], k = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rows, cols &lt;= 50</code></li>
	<li><code>rows ==&nbsp;pizza.length</code></li>
	<li><code>cols ==&nbsp;pizza[i].length</code></li>
	<li><code>1 &lt;= k &lt;= 10</code></li>
	<li><code>pizza</code> chỉ gồm các ký tự <code>&#39;A&#39;</code>&nbsp;và <code>&#39;.</code>&#39;.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: 2D Prefix Sum + Memoized Search

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần cắt có thể theo chiều ngang hoặc dọc, và phần được đưa đi phải chứa một quả táo. Vì $rows,cols\le 50$, $k\le 10$, cây cắt sẽ tính lại cùng một hình chữ nhật còn lại và số lần cắt còn lại nhiều lần.
>
> 2D prefix sum cho phép kiểm tra số táo trong $O(1)$. $dfs(i,j,k)$ là số cách cắt pizza có góc trên bên trái tại $(i,j)$ thêm $k$ lần: thử các cách cắt hợp lệ rồi đệ quy trên phần còn lại. Khi $k=0$, đếm là $1$ nếu phần còn lại có ít nhất một quả táo.

<!-- thinking:end -->

Ta có thể dùng 2D prefix sum để nhanh chóng tính số quả táo trong mỗi hình chữ nhật con. Đặt $s[i][j]$ là số quả táo trong hình chữ nhật con gồm $i$ hàng đầu tiên và $j$ cột đầu tiên. Khi đó, $s[i][j]$ có thể được suy ra từ số quả táo trong ba hình chữ nhật con $s[i-1][j]$, $s[i][j-1]$ và $s[i-1][j-1]$. Cách tính cụ thể như sau:

$$
s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + (pizza[i-1][j-1] == 'A')
$$

Ở đây, $pizza[i-1][j-1]$ biểu diễn ký tự tại hàng thứ $i$ và cột thứ $j$ trong hình chữ nhật. Nếu đó là một quả táo thì giá trị là $1$, ngược lại là $0$.

Tiếp theo, ta xây dựng hàm $dfs(i, j, k)$, biểu diễn số cách cắt hình chữ nhật $(i, j, m-1, n-1)$ với $k$ lần cắt để thu được $k+1$ phần pizza. Trong đó, $(i, j)$ và $(m-1, n-1)$ lần lượt là tọa độ góc trên bên trái và góc dưới bên phải của hình chữ nhật. Cách tính hàm $dfs(i, j, k)$ như sau:

- Nếu $k = 0$, nghĩa là không thể thực hiện thêm lần cắt nào. Ta cần kiểm tra xem hình chữ nhật còn quả táo nào không. Nếu có táo, trả về $1$; ngược lại trả về $0$.
- Nếu $k \gt 0$, ta cần liệt kê vị trí của lần cắt cuối cùng. Nếu lần cắt cuối cùng là cắt ngang, ta liệt kê vị trí cắt $x$, với $i \lt x \lt m$. Nếu $s[x][n] - s[i][n] - s[x][j] + s[i][j] \gt 0$, nghĩa là phần pizza phía trên có táo, ta cộng giá trị của $dfs(x, j, k-1)$ vào đáp án. Nếu lần cắt cuối cùng là cắt dọc, ta liệt kê vị trí cắt $y$, với $j \lt y \lt n$. Nếu $s[m][y] - s[i][y] - s[m][j] + s[i][j] \gt 0$, nghĩa là phần pizza bên trái có táo, ta cộng giá trị của $dfs(i, y, k-1)$ vào đáp án.

Đáp án cuối cùng là giá trị của $dfs(0, 0, k-1)$.

Để tránh tính toán lặp lại, ta có thể dùng memoized search. Ta dùng mảng 3 chiều $f$ để lưu giá trị của $dfs(i, j, k)$. Khi cần tính giá trị của $dfs(i, j, k)$, nếu $f[i][j][k]$ khác $-1$, nghĩa là giá trị này đã được tính trước đó, và ta có thể trả về trực tiếp $f[i][j][k]$. Nếu không, ta tính giá trị của $dfs(i, j, k)$ theo cách trên rồi lưu kết quả vào $f[i][j][k]$.

Độ phức tạp thời gian là $O(m \times n \times k \times (m + n))$, và độ phức tạp không gian là $O(m \times n \times k)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của hình chữ nhật.

Các bài toán tương tự:

- [2312. Selling Pieces of Wood](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2312.Selling%20Pieces%20of%20Wood/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def ways(self, pizza: List[str], k: int) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if k == 0:
                return int(s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0)
            ans = 0
            for x in range(i + 1, m):
                if s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0:
                    ans += dfs(x, j, k - 1)
            for y in range(j + 1, n):
                if s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0:
                    ans += dfs(i, y, k - 1)
            return ans % mod

        mod = 10**9 + 7
        m, n = len(pizza), len(pizza[0])
        s = [[0] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(pizza, 1):
            for j, c in enumerate(row, 1):
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + int(c == 'A')
        return dfs(0, 0, k - 1)
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] s;
    private Integer[][][] f;
    private final int mod = (int) 1e9 + 7;

    public int ways(String[] pizza, int k) {
        m = pizza.length;
        n = pizza[0].length();
        s = new int[m + 1][n + 1];
        f = new Integer[m][n][k];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = pizza[i - 1].charAt(j - 1) == 'A' ? 1 : 0;
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x;
            }
        }
        return dfs(0, 0, k - 1);
    }

    private int dfs(int i, int j, int k) {
        if (k == 0) {
            return s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0 ? 1 : 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int ans = 0;
        for (int x = i + 1; x < m; ++x) {
            if (s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0) {
                ans = (ans + dfs(x, j, k - 1)) % mod;
            }
        }
        for (int y = j + 1; y < n; ++y) {
            if (s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0) {
                ans = (ans + dfs(i, y, k - 1)) % mod;
            }
        }
        return f[i][j][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int ways(vector<string>& pizza, int k) {
        const int mod = 1e9 + 7;
        int m = pizza.size(), n = pizza[0].size();
        vector<vector<vector<int>>> f(m, vector<vector<int>>(n, vector<int>(k, -1)));
        vector<vector<int>> s(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = pizza[i - 1][j - 1] == 'A' ? 1 : 0;
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x;
            }
        }
        function<int(int, int, int)> dfs = [&](int i, int j, int k) -> int {
            if (k == 0) {
                return s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0 ? 1 : 0;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            int ans = 0;
            for (int x = i + 1; x < m; ++x) {
                if (s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0) {
                    ans = (ans + dfs(x, j, k - 1)) % mod;
                }
            }
            for (int y = j + 1; y < n; ++y) {
                if (s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0) {
                    ans = (ans + dfs(i, y, k - 1)) % mod;
                }
            }
            return f[i][j][k] = ans;
        };
        return dfs(0, 0, k - 1);
    }
};
```

#### Go

```go
func ways(pizza []string, k int) int {
	const mod = 1e9 + 7
	m, n := len(pizza), len(pizza[0])
	f := make([][][]int, m)
	s := make([][]int, m+1)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, k)
			for h := range f[i][j] {
				f[i][j][h] = -1
			}
		}
	}
	for i := range s {
		s[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1]
			if pizza[i-1][j-1] == 'A' {
				s[i][j]++
			}
		}
	}
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if f[i][j][k] != -1 {
			return f[i][j][k]
		}
		if k == 0 {
			if s[m][n]-s[m][j]-s[i][n]+s[i][j] > 0 {
				return 1
			}
			return 0
		}
		ans := 0
		for x := i + 1; x < m; x++ {
			if s[x][n]-s[x][j]-s[i][n]+s[i][j] > 0 {
				ans = (ans + dfs(x, j, k-1)) % mod
			}
		}
		for y := j + 1; y < n; y++ {
			if s[m][y]-s[m][j]-s[i][y]+s[i][j] > 0 {
				ans = (ans + dfs(i, y, k-1)) % mod
			}
		}
		f[i][j][k] = ans
		return ans
	}
	return dfs(0, 0, k-1)
}
```

#### TypeScript

```ts
function ways(pizza: string[], k: number): number {
    const mod = 1e9 + 7;
    const m = pizza.length;
    const n = pizza[0].length;
    const f = new Array(m).fill(0).map(() => new Array(n).fill(0).map(() => new Array(k).fill(-1)));
    const s = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            const x = pizza[i - 1][j - 1] === 'A' ? 1 : 0;
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x;
        }
    }
    const dfs = (i: number, j: number, k: number): number => {
        if (f[i][j][k] !== -1) {
            return f[i][j][k];
        }
        if (k === 0) {
            return s[m][n] - s[i][n] - s[m][j] + s[i][j] > 0 ? 1 : 0;
        }
        let ans = 0;
        for (let x = i + 1; x < m; ++x) {
            if (s[x][n] - s[i][n] - s[x][j] + s[i][j] > 0) {
                ans = (ans + dfs(x, j, k - 1)) % mod;
            }
        }
        for (let y = j + 1; y < n; ++y) {
            if (s[m][y] - s[i][y] - s[m][j] + s[i][j] > 0) {
                ans = (ans + dfs(i, y, k - 1)) % mod;
            }
        }
        return (f[i][j][k] = ans);
    };
    return dfs(0, 0, k - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
