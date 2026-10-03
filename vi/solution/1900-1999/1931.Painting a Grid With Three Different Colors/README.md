---
comments: true
difficulty: Hard
rating: 2170
source: Weekly Contest 249 Q3
tags:
    - Dynamic Programming
    - Graph Coloring
---

<!-- problem:start -->

# [1931. Painting a Grid With Three Different Colors](https://leetcode.com/problems/painting-a-grid-with-three-different-colors)

[中文文档](/solution/1900-1999/1931.Painting%20a%20Grid%20With%20Three%20Different%20Colors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>m</code> và <code>n</code>. Xét một lưới <code>m x n</code>, trong đó mỗi ô ban đầu có màu trắng. Bạn có thể tô mỗi ô màu <strong>đỏ</strong>, <strong>xanh lá</strong> hoặc <strong>xanh dương</strong>. <strong>Tất cả các ô đều phải được tô màu.</strong></p>

<p>Trả về<em> số cách tô màu lưới sao cho <strong>không có hai ô kề nhau nào cùng màu</strong></em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1931.Painting%20a%20Grid%20With%20Three%20Different%20Colors/images/colorthegrid.png" style="width: 200px; height: 50px;" />
<pre>
<strong>Đầu vào:</strong> m = 1, n = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ba cách tô màu có thể có được minh họa trong hình trên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1931.Painting%20a%20Grid%20With%20Three%20Different%20Colors/images/copy-of-colorthegrid.png" style="width: 321px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> m = 1, n = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Sáu cách tô màu có thể có được minh họa trong hình trên.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 5, n = 5
<strong>Đầu ra:</strong> 580986
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m &lt;= 5</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các ô kề nhau phải có màu khác nhau. Tô từng ô sẽ tạo ra quá nhiều trường hợp, nhưng vì $m\le 5$ nên một cột chỉ có $3^m$ cách tô màu.
>
> Ta giữ lại các mask mà hai ô kề nhau theo chiều dọc có màu khác nhau, sau đó tính trước các cặp mask cũng khác nhau theo chiều ngang. $f[j]$ là số cách mà cột trước đó có trạng thái $j$, rồi ta chuyển tiếp theo từng cột.
>
> Cột đầu tiên đếm các mask hợp lệ; tiếp theo thực hiện $n-1$ lần chuyển trạng thái, và đáp án là tổng theo modulo $10^9+7$.

<!-- thinking:end -->

Ta nhận thấy số hàng của lưới không vượt quá $5$, nên một cột có nhiều nhất $3^5=243$ cách tô màu khác nhau.

Do đó, ta định nghĩa $f[i][j]$ là số cách tô màu trong $i$ cột đầu tiên, trong đó trạng thái tô màu của cột thứ $i$ là $j$. Trạng thái $f[i][j]$ được chuyển từ $f[i - 1][k]$, trong đó $k$ là trạng thái tô màu của cột thứ $i - 1$, đồng thời $k$ và $j$ thỏa mãn điều kiện các màu kề nhau phải khác nhau. Cụ thể:

$$
f[i][j] = \sum_{k \in \textit{valid}(j)} f[i - 1][k]
$$

trong đó $\textit{valid}(j)$ biểu diễn tất cả các trạng thái đứng trước hợp lệ của trạng thái $j$.

Đáp án cuối cùng là tổng của $f[n][j]$, trong đó $j$ là một trạng thái hợp lệ bất kỳ.

Ta nhận thấy $f[i][j]$ chỉ liên quan đến $f[i - 1][k]$, nên có thể sử dụng mảng rolling để tối ưu độ phức tạp không gian.

Độ phức tạp thời gian là $O((m + n) \times 3^{2m})$, và độ phức tạp không gian là $O(3^m)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def colorTheGrid(self, m: int, n: int) -> int:
        def f1(x: int) -> bool:
            last = -1
            for _ in range(m):
                if x % 3 == last:
                    return False
                last = x % 3
                x //= 3
            return True

        def f2(x: int, y: int) -> bool:
            for _ in range(m):
                if x % 3 == y % 3:
                    return False
                x, y = x // 3, y // 3
            return True

        mod = 10**9 + 7
        mx = 3**m
        valid = {i for i in range(mx) if f1(i)}
        d = defaultdict(list)
        for x in valid:
            for y in valid:
                if f2(x, y):
                    d[x].append(y)
        f = [int(i in valid) for i in range(mx)]
        for _ in range(n - 1):
            g = [0] * mx
            for i in valid:
                for j in d[i]:
                    g[i] = (g[i] + f[j]) % mod
            f = g
        return sum(f) % mod
```

#### Java

```java
class Solution {
    private int m;

    public int colorTheGrid(int m, int n) {
        this.m = m;
        final int mod = (int) 1e9 + 7;
        int mx = (int) Math.pow(3, m);
        Set<Integer> valid = new HashSet<>();
        int[] f = new int[mx];
        for (int i = 0; i < mx; ++i) {
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
            int[] g = new int[mx];
            for (int i : valid) {
                for (int j : d.getOrDefault(i, List.of())) {
                    g[i] = (g[i] + f[j]) % mod;
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
        for (int i = 0; i < m; ++i) {
            if (x % 3 == last) {
                return false;
            }
            last = x % 3;
            x /= 3;
        }
        return true;
    }

    private boolean f2(int x, int y) {
        for (int i = 0; i < m; ++i) {
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
    int colorTheGrid(int m, int n) {
        auto f1 = [&](int x) {
            int last = -1;
            for (int i = 0; i < m; ++i) {
                if (x % 3 == last) {
                    return false;
                }
                last = x % 3;
                x /= 3;
            }
            return true;
        };
        auto f2 = [&](int x, int y) {
            for (int i = 0; i < m; ++i) {
                if (x % 3 == y % 3) {
                    return false;
                }
                x /= 3;
                y /= 3;
            }
            return true;
        };

        const int mod = 1e9 + 7;
        int mx = pow(3, m);
        unordered_set<int> valid;
        vector<int> f(mx);
        for (int i = 0; i < mx; ++i) {
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
            vector<int> g(mx);
            for (int i : valid) {
                for (int j : d[i]) {
                    g[i] = (g[i] + f[j]) % mod;
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
func colorTheGrid(m int, n int) (ans int) {
	f1 := func(x int) bool {
		last := -1
		for i := 0; i < m; i++ {
			if x%3 == last {
				return false
			}
			last = x % 3
			x /= 3
		}
		return true
	}
	f2 := func(x, y int) bool {
		for i := 0; i < m; i++ {
			if x%3 == y%3 {
				return false
			}
			x /= 3
			y /= 3
		}
		return true
	}
	mx := int(math.Pow(3, float64(m)))
	valid := map[int]bool{}
	f := make([]int, mx)
	for i := 0; i < mx; i++ {
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
		g := make([]int, mx)
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
function colorTheGrid(m: number, n: number): number {
    const f1 = (x: number): boolean => {
        let last = -1;
        for (let i = 0; i < m; ++i) {
            if (x % 3 === last) {
                return false;
            }
            last = x % 3;
            x = Math.floor(x / 3);
        }
        return true;
    };
    const f2 = (x: number, y: number): boolean => {
        for (let i = 0; i < m; ++i) {
            if (x % 3 === y % 3) {
                return false;
            }
            x = Math.floor(x / 3);
            y = Math.floor(y / 3);
        }
        return true;
    };
    const mx = 3 ** m;
    const valid = new Set<number>();
    const f: number[] = Array(mx).fill(0);
    for (let i = 0; i < mx; ++i) {
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
        const g: number[] = Array(mx).fill(0);
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

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
