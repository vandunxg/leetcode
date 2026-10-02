---
comments: true
difficulty: Hard
rating: 1844
source: Weekly Contest 184 Q4
tags:
    - Dynamic Programming
    - Graph Coloring
---

<!-- problem:start -->

# [1411. Number of Ways to Paint N × 3 Grid](https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid)

[中文文档](/solution/1400-1499/1411.Number%20of%20Ways%20to%20Paint%20N%20%C3%97%203%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một <code>grid</code> kích thước <code>n x 3</code> và muốn tô mỗi ô của grid bằng đúng một trong ba màu: <strong>Red</strong>, <strong>Yellow,</strong> hoặc <strong>Green</strong>, đồng thời bảo đảm không có hai ô kề nhau nào cùng màu (tức là không có hai ô chung cạnh dọc hoặc ngang nào cùng màu).</p>

<p>Cho <code>n</code> là số hàng của grid, hãy trả về <em>số cách</em> có thể tô grid này. Vì đáp án có thể rất lớn, <strong>bắt buộc phải</strong> tính đáp án modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1411.Number%20of%20Ways%20to%20Paint%20N%20%C3%97%203%20Grid/images/e1.png" style="width: 400px; height: 257px;" />
<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> 12
<strong>Explanation:</strong> Có 12 cách có thể tô grid như hình minh họa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 5000
<strong>Output:</strong> 30228214
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Với ba màu, các ô kề nhau khác màu và $n\le 5000$, việc tô màu toàn bộ grid là bất khả thi. Một hàng gồm ba ô thuộc một trong hai nhóm đối xứng: loại $010$ (hai màu) và loại $012$ (ba màu).
>
> Đếm các trạng thái kế tiếp ta được $g_0=3f_0+2f_1$ và $g_1=2f_0+2f_1$. Cả hai nhóm đều bắt đầu từ $6$; sau $n-1$ lần chuyển trạng thái, tổng của chúng là đáp án.

<!-- thinking:end -->

Ta phân loại tất cả trạng thái có thể có của mỗi hàng. Theo nguyên lý đối xứng, khi một hàng chỉ có $3$ phần tử, mọi trạng thái hợp lệ được phân loại thành: loại $010$, loại $012$.

- Khi trạng thái là loại $010$: Các trạng thái có thể có ở hàng tiếp theo là: $101$, $102$, $121$, $201$, $202$. Có thể tóm tắt $5$ trạng thái này thành $3$ loại $010$ và $2$ loại $012$.
- Khi trạng thái là loại $012$: Các trạng thái có thể có ở hàng tiếp theo là: $101$, $120$, $121$, $201$. Có thể tóm tắt $4$ trạng thái này thành $2$ loại $010$ và $2$ loại $012$.

Tóm lại, ta có: $newf0 = 3 \times f0 + 2 \times f1$, $newf1 = 2 \times f0 + 2 \times f1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số hàng của grid. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfWays(self, n: int) -> int:
        mod = 10**9 + 7
        f0 = f1 = 6
        for _ in range(n - 1):
            g0 = (3 * f0 + 2 * f1) % mod
            g1 = (2 * f0 + 2 * f1) % mod
            f0, f1 = g0, g1
        return (f0 + f1) % mod
```

#### Java

```java
class Solution {
    public int numOfWays(int n) {
        int mod = (int) 1e9 + 7;
        long f0 = 6, f1 = 6;
        for (int i = 0; i < n - 1; ++i) {
            long g0 = (3 * f0 + 2 * f1) % mod;
            long g1 = (2 * f0 + 2 * f1) % mod;
            f0 = g0;
            f1 = g1;
        }
        return (int) (f0 + f1) % mod;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    int numOfWays(int n) {
        int mod = 1e9 + 7;
        ll f0 = 6, f1 = 6;
        while (--n) {
            ll g0 = (f0 * 3 + f1 * 2) % mod;
            ll g1 = (f0 * 2 + f1 * 2) % mod;
            f0 = g0;
            f1 = g1;
        }
        return (int) (f0 + f1) % mod;
    }
};
```

#### Go

```go
func numOfWays(n int) int {
	mod := int(1e9) + 7
	f0, f1 := 6, 6
	for n > 1 {
		n--
		g0 := (f0*3 + f1*2) % mod
		g1 := (f0*2 + f1*2) % mod
		f0, f1 = g0, g1
	}
	return (f0 + f1) % mod
}
```

#### TypeScript

```ts
function numOfWays(n: number): number {
    const mod: number = 10 ** 9 + 7;
    let f0: number = 6;
    let f1: number = 6;

    for (let i = 1; i < n; i++) {
        const g0: number = (3 * f0 + 2 * f1) % mod;
        const g1: number = (2 * f0 + 2 * f1) % mod;
        f0 = g0;
        f1 = g1;
    }

    return (f0 + f1) % mod;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_of_ways(n: i32) -> i32 {
        const MOD: i64 = 1_000_000_007;
        let mut f0: i64 = 6;
        let mut f1: i64 = 6;

        for _ in 0..n - 1 {
            let g0 = (3 * f0 + 2 * f1) % MOD;
            let g1 = (2 * f0 + 2 * f1) % MOD;
            f0 = g0;
            f1 = g1;
        }

        ((f0 + f1) % MOD) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Nén trạng thái + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 cần tự suy ra các hệ số chuyển trạng thái. Vì chỉ có ba cột nên mỗi hàng có nhiều nhất $27$ cách tô màu, do đó ta có thể liệt kê các mask hợp lệ và các cặp tương thích.
>
> $f[i][j]$ là số cách tô hàng $i$ bằng mask $j$, được tính bằng cách cộng các mask trước đó tương thích. Dùng mảng rolling để chỉ giữ lại một layer, không cần các hệ số đóng.

<!-- thinking:end -->

Ta nhận thấy grid chỉ có $3$ cột, nên một hàng có nhiều nhất $3^3=27$ cách tô màu khác nhau.

Vì vậy, ta định nghĩa $f[i][j]$ là số cách trong $i$ hàng đầu tiên, trong đó trạng thái tô màu của hàng thứ $i$ là $j$. Trạng thái $f[i][j]$ được chuyển từ $f[i - 1][k]$, trong đó $k$ là trạng thái tô màu của hàng thứ $i - 1$, đồng thời $k$ và $j$ thỏa mãn yêu cầu các màu kề nhau phải khác nhau. Cụ thể:

$$
f[i][j] = \sum_{k \in \textit{valid}(j)} f[i - 1][k]
$$

trong đó $\textit{valid}(j)$ biểu diễn tất cả các trạng thái trước đó hợp lệ của trạng thái $j$.

Đáp án cuối cùng là tổng của $f[n][j]$, trong đó $j$ là bất kỳ trạng thái hợp lệ nào.

Ta nhận thấy $f[i][j]$ chỉ liên quan đến $f[i - 1][k]$, nên có thể dùng mảng rolling để tối ưu độ phức tạp không gian.

Độ phức tạp thời gian là $O((m + n) \times 3^{2m})$, và độ phức tạp không gian là $O(3^m)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfWays(self, n: int) -> int:
        def f1(x: int) -> bool:
            last = -1
            for _ in range(3):
                if x % 3 == last:
                    return False
                last = x % 3
                x //= 3
            return True

        def f2(x: int, y: int) -> bool:
            for _ in range(3):
                if x % 3 == y % 3:
                    return False
                x //= 3
                y //= 3
            return True

        mod = 10**9 + 7
        m = 27
        valid = {i for i in range(m) if f1(i)}
        d = defaultdict(list)
        for i in valid:
            for j in valid:
                if f2(i, j):
                    d[i].append(j)
        f = [int(i in valid) for i in range(m)]
        for _ in range(n - 1):
            g = [0] * m
            for i in valid:
                for j in d[i]:
                    g[j] = (g[j] + f[i]) % mod
            f = g
        return sum(f) % mod
```

#### Java

```java
class Solution {
    public int numOfWays(int n) {
        final int mod = (int) 1e9 + 7;
        int m = 27;
        Set<Integer> valid = new HashSet<>();
        int[] f = new int[m];
        for (int i = 0; i < m; ++i) {
            if (f1(i)) {
                valid.add(i);
                f[i] = 1;
            }
        }
        Map<Integer, List<Integer>> d = new HashMap<>();
        for (int i : valid) {
            for (int j : valid) {
                if (f2(i, j)) {
                    d.computeIfAbsent(i, k -> new ArrayList<>()).add(j);
                }
            }
        }
        for (int k = 1; k < n; ++k) {
            int[] g = new int[m];
            for (int i : valid) {
                for (int j : d.getOrDefault(i, List.of())) {
                    g[j] = (g[j] + f[i]) % mod;
                }
            }
            f = g;
        }
        int ans = 0;
        for (int x : f) {
            ans = (ans + x) % mod;
        }
        return ans;
    }

    private boolean f1(int x) {
        int last = -1;
        for (int i = 0; i < 3; ++i) {
            if (x % 3 == last) {
                return false;
            }
            last = x % 3;
            x /= 3;
        }
        return true;
    }

    private boolean f2(int x, int y) {
        for (int i = 0; i < 3; ++i) {
            if (x % 3 == y % 3) {
                return false;
            }
            x /= 3;
            y /= 3;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numOfWays(int n) {
        int m = 27;

        auto f1 = [&](int x) {
            int last = -1;
            for (int i = 0; i < 3; ++i) {
                if (x % 3 == last) {
                    return false;
                }
                last = x % 3;
                x /= 3;
            }
            return true;
        };
        auto f2 = [&](int x, int y) {
            for (int i = 0; i < 3; ++i) {
                if (x % 3 == y % 3) {
                    return false;
                }
                x /= 3;
                y /= 3;
            }
            return true;
        };

        const int mod = 1e9 + 7;
        unordered_set<int> valid;
        vector<int> f(m);
        for (int i = 0; i < m; ++i) {
            if (f1(i)) {
                valid.insert(i);
                f[i] = 1;
            }
        }
        unordered_map<int, vector<int>> d;
        for (int i : valid) {
            for (int j : valid) {
                if (f2(i, j)) {
                    d[i].push_back(j);
                }
            }
        }
        for (int k = 1; k < n; ++k) {
            vector<int> g(m);
            for (int i : valid) {
                for (int j : d[i]) {
                    g[j] = (g[j] + f[i]) % mod;
                }
            }
            f = move(g);
        }
        int ans = 0;
        for (int x : f) {
            ans = (ans + x) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numOfWays(n int) (ans int) {
	f1 := func(x int) bool {
		last := -1
		for i := 0; i < 3; i++ {
			if x%3 == last {
				return false
			}
			last = x % 3
			x /= 3
		}
		return true
	}
	f2 := func(x, y int) bool {
		for i := 0; i < 3; i++ {
			if x%3 == y%3 {
				return false
			}
			x /= 3
			y /= 3
		}
		return true
	}
	m := 27
	valid := map[int]bool{}
	f := make([]int, m)
	for i := 0; i < m; i++ {
		if f1(i) {
			valid[i] = true
			f[i] = 1
		}
	}
	d := map[int][]int{}
	for i := range valid {
		for j := range valid {
			if f2(i, j) {
				d[i] = append(d[i], j)
			}
		}
	}
	const mod int = 1e9 + 7
	for k := 1; k < n; k++ {
		g := make([]int, m)
		for i := range valid {
			for _, j := range d[i] {
				g[i] = (g[i] + f[j]) % mod
			}
		}
		f = g
	}
	for _, x := range f {
		ans = (ans + x) % mod
	}
	return
}
```

#### TypeScript

```ts
function numOfWays(n: number): number {
    const f1 = (x: number): boolean => {
        let last = -1;
        for (let i = 0; i < 3; ++i) {
            if (x % 3 === last) {
                return false;
            }
            last = x % 3;
            x = Math.floor(x / 3);
        }
        return true;
    };
    const f2 = (x: number, y: number): boolean => {
        for (let i = 0; i < 3; ++i) {
            if (x % 3 === y % 3) {
                return false;
            }
            x = Math.floor(x / 3);
            y = Math.floor(y / 3);
        }
        return true;
    };
    const m = 27;
    const valid = new Set<number>();
    const f: number[] = Array(m).fill(0);
    for (let i = 0; i < m; ++i) {
        if (f1(i)) {
            valid.add(i);
            f[i] = 1;
        }
    }
    const d: Map<number, number[]> = new Map();
    for (const i of valid) {
        for (const j of valid) {
            if (f2(i, j)) {
                d.set(i, (d.get(i) || []).concat(j));
            }
        }
    }
    const mod = 10 ** 9 + 7;
    for (let k = 1; k < n; ++k) {
        const g: number[] = Array(m).fill(0);
        for (const i of valid) {
            for (const j of d.get(i) || []) {
                g[i] = (g[i] + f[j]) % mod;
            }
        }
        f.splice(0, f.length, ...g);
    }
    let ans = 0;
    for (const x of f) {
        ans = (ans + x) % mod;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::{HashSet, HashMap};

impl Solution {
    pub fn num_of_ways(n: i32) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let m = 27;
        let mut valid = HashSet::new();
        let mut f = vec![0; m];

        for i in 0..m {
            if Self::f1(i as i32) {
                valid.insert(i as i32);
                f[i] = 1;
            }
        }

        let mut d: HashMap<i32, Vec<i32>> = HashMap::new();
        for &i in &valid {
            for &j in &valid {
                if Self::f2(i, j) {
                    d.entry(i).or_insert_with(Vec::new).push(j);
                }
            }
        }

        for _ in 1..n {
            let mut g = vec![0; m];
            for &i in &valid {
                if let Some(neighbors) = d.get(&i) {
                    for &j in neighbors {
                        g[j as usize] = (g[j as usize] + f[i as usize]) % MOD;
                    }
                }
            }
            f = g;
        }

        let mut ans = 0;
        for x in f {
            ans = (ans + x) % MOD;
        }
        ans
    }

    fn f1(mut x: i32) -> bool {
        let mut last = -1;
        for _ in 0..3 {
            if x % 3 == last {
                return false;
            }
            last = x % 3;
            x /= 3;
        }
        true
    }

    fn f2(mut x: i32, mut y: i32) -> bool {
        for _ in 0..3 {
            if x % 3 == y % 3 {
                return false;
            }
            x /= 3;
            y /= 3;
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
