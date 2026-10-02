---
comments: true
difficulty: Hard
rating: 2164
source: Weekly Contest 134 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Hash Table
    - Bidirectional Search
---

<!-- problem:start -->

# [1036. Escape a Large Maze](https://leetcode.com/problems/escape-a-large-maze)

[中文文档](/solution/1000-1099/1036.Escape%20a%20Large%20Maze/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lưới kích thước 1 triệu x 1 triệu trên mặt phẳng XY, tọa độ của mỗi ô là <code>(x, y)</code>.</p>

<p>Ta bắt đầu tại ô <code>source = [s<sub>x</sub>, s<sub>y</sub>]</code> và cần đến ô <code>target = [t<sub>x</sub>, t<sub>y</sub>]</code>. Ngoài ra còn có mảng các ô bị chặn <code>blocked</code>, trong đó mỗi <code>blocked[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu thị ô bị chặn có tọa độ <code>(x<sub>i</sub>, y<sub>i</sub>)</code>.</p>

<p>Mỗi lượt, ta có thể đi một ô về phía bắc, đông, nam hoặc tây nếu ô đó <strong>không</strong> nằm trong mảng <code>blocked</code>. Ta cũng không được đi ra ngoài lưới.</p>

<p>Trả về <code>true</code><em> khi và chỉ khi có thể đi từ ô </em><code>source</code><em> đến ô </em><code>target</code><em> bằng một chuỗi các nước đi hợp lệ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> blocked = [[0,1],[1,0]], source = [0,0], target = [0,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể đến ô đích từ ô xuất phát vì ta không thể di chuyển.
Ta không thể đi về phía bắc hoặc phía đông vì các ô đó bị chặn.
Ta không thể đi về phía nam hoặc phía tây vì sẽ ra ngoài lưới.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> blocked = [], source = [0,0], target = [999999,999999]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Vì không có ô nào bị chặn nên có thể đến ô đích.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= blocked.length &lt;= 200</code></li>
	<li><code>blocked[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt; 10<sup>6</sup></code></li>
	<li><code>source.length == target.length == 2</code></li>
	<li><code>0 &lt;= s<sub>x</sub>, s<sub>y</sub>, t<sub>x</sub>, t<sub>y</sub> &lt; 10<sup>6</sup></code></li>
	<li><code>source != target</code></li>
	<li>Đảm bảo <code>source</code> và <code>target</code> không bị chặn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Không thể tạo tường minh lưới $10^6\times 10^6$. Tối đa $200$ ô bị chặn chỉ có thể bao kín một vùng diện tích tối đa $|blocked|^2/2$; do đó, nếu từ một điểm ta thăm được nhiều ô hơn giới hạn này thì điểm đó không bị mắc kẹt.
>
> Chạy DFS từ source và từ target: nếu tìm thấy điểm còn lại thì thành công; nếu số ô đã thăm vượt quá giới hạn diện tích thì ta đã thoát khỏi vùng bị vây. Cả hai phía đều phải thoát được (hoặc gặp nhau).
>
> Lưu các ô bị chặn trong set để kiểm tra trong $O(1)$; quy mô tìm kiếm tăng theo bình phương số chướng ngại vật.

<!-- thinking:end -->

Có thể xem bài toán là xác định liệu có thể đi từ điểm xuất phát đến điểm đích trong lưới $10^6 \times 10^6$ khi có một số ít điểm bị chặn hay không.

Vì số điểm bị chặn nhỏ, diện tích tối đa có thể bị vây không vượt quá $|blocked|^2 / 2$. Do đó, ta có thể chạy depth-first search (DFS) bắt đầu từ cả source lẫn target. Quá trình tìm kiếm tiếp tục cho đến khi đến được điểm còn lại hoặc số điểm đã thăm vượt quá $|blocked|^2 / 2$. Nếu một trong hai điều kiện được thỏa mãn, trả về $\textit{true}$; nếu không, trả về $\textit{false}$.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ là kích thước vùng bị chặn. Ở bài này, $m \leq |blocked|^2 / 2 = 200^2 / 2 = 20000$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isEscapePossible(
        self, blocked: List[List[int]], source: List[int], target: List[int]
    ) -> bool:
        def dfs(source: List[int], target: List[int], vis: set) -> bool:
            vis.add(tuple(source))
            if len(vis) > m:
                return True
            for a, b in pairwise(dirs):
                x, y = source[0] + a, source[1] + b
                if 0 <= x < n and 0 <= y < n and (x, y) not in s and (x, y) not in vis:
                    if [x, y] == target or dfs([x, y], target, vis):
                        return True
            return False

        s = {(x, y) for x, y in blocked}
        dirs = (-1, 0, 1, 0, -1)
        n = 10**6
        m = len(blocked) ** 2 // 2
        return dfs(source, target, set()) and dfs(target, source, set())
```

#### Java

```java
class Solution {
    private final int n = (int) 1e6;
    private int m;
    private Set<Long> s = new HashSet<>();
    private final int[] dirs = {-1, 0, 1, 0, -1};

    public boolean isEscapePossible(int[][] blocked, int[] source, int[] target) {
        for (var b : blocked) {
            s.add(f(b[0], b[1]));
        }
        m = blocked.length * blocked.length / 2;
        int sx = source[0], sy = source[1];
        int tx = target[0], ty = target[1];
        return dfs(sx, sy, tx, ty, new HashSet<>()) && dfs(tx, ty, sx, sy, new HashSet<>());
    }

    private boolean dfs(int sx, int sy, int tx, int ty, Set<Long> vis) {
        if (vis.size() > m) {
            return true;
        }
        for (int k = 0; k < 4; ++k) {
            int x = sx + dirs[k], y = sy + dirs[k + 1];
            if (x >= 0 && x < n && y >= 0 && y < n) {
                if (x == tx && y == ty) {
                    return true;
                }
                long key = f(x, y);
                if (!s.contains(key) && vis.add(key) && dfs(x, y, tx, ty, vis)) {
                    return true;
                }
            }
        }
        return false;
    }

    private long f(int i, int j) {
        return (long) i * n + j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isEscapePossible(vector<vector<int>>& blocked, vector<int>& source, vector<int>& target) {
        const int n = 1e6;
        int m = blocked.size() * blocked.size() / 2;
        using ll = long long;
        unordered_set<ll> s;
        const int dirs[5] = {-1, 0, 1, 0, -1};
        auto f = [&](int i, int j) {
            return (ll) i * n + j;
        };
        for (const auto& b : blocked) {
            s.insert(f(b[0], b[1]));
        }
        int sx = source[0], sy = source[1];
        int tx = target[0], ty = target[1];
        unordered_set<ll> vis1, vis2;
        auto dfs = [&](this auto&& dfs, int sx, int sy, int tx, int ty, unordered_set<ll>& vis) -> bool {
            vis.insert(f(sx, sy));
            if (vis.size() > m) {
                return true;
            }
            for (int k = 0; k < 4; ++k) {
                int x = sx + dirs[k], y = sy + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n) {
                    if (x == tx && y == ty) {
                        return true;
                    }
                    auto key = f(x, y);
                    if (!s.contains(key) && !vis.contains(key) && dfs(x, y, tx, ty, vis)) {
                        return true;
                    }
                }
            }
            return false;
        };
        return dfs(sx, sy, tx, ty, vis1) && dfs(tx, ty, sx, sy, vis2);
    }
};
```

#### Go

```go
func isEscapePossible(blocked [][]int, source []int, target []int) bool {
	const n = 1_000_000
	m := len(blocked) * len(blocked) / 2
	dirs := [5]int{-1, 0, 1, 0, -1}

	f := func(i, j int) int64 {
		return int64(i*n + j)
	}

	s := make(map[int64]bool)
	for _, b := range blocked {
		s[f(b[0], b[1])] = true
	}

	var dfs func(sx, sy, tx, ty int, vis map[int64]bool) bool
	dfs = func(sx, sy, tx, ty int, vis map[int64]bool) bool {
		key := f(sx, sy)
		vis[key] = true
		if len(vis) > m {
			return true
		}
		for k := 0; k < 4; k++ {
			x, y := sx+dirs[k], sy+dirs[k+1]
			if x >= 0 && x < n && y >= 0 && y < n {
				if x == tx && y == ty {
					return true
				}
				key := f(x, y)
				if !s[key] && !vis[key] && dfs(x, y, tx, ty, vis) {
					return true
				}
			}
		}
		return false
	}

	sx, sy := source[0], source[1]
	tx, ty := target[0], target[1]
	return dfs(sx, sy, tx, ty, map[int64]bool{}) && dfs(tx, ty, sx, sy, map[int64]bool{})
}
```

#### TypeScript

```ts
function isEscapePossible(blocked: number[][], source: number[], target: number[]): boolean {
    const n = 10 ** 6;
    const m = (blocked.length ** 2) >> 1;
    const dirs = [-1, 0, 1, 0, -1];

    const s = new Set<number>();
    const f = (i: number, j: number): number => i * n + j;

    for (const [x, y] of blocked) {
        s.add(f(x, y));
    }

    const dfs = (sx: number, sy: number, tx: number, ty: number, vis: Set<number>): boolean => {
        vis.add(f(sx, sy));
        if (vis.size > m) {
            return true;
        }
        for (let k = 0; k < 4; k++) {
            const x = sx + dirs[k],
                y = sy + dirs[k + 1];
            if (x >= 0 && x < n && y >= 0 && y < n) {
                if (x === tx && y === ty) {
                    return true;
                }
                const key = f(x, y);
                if (!s.has(key) && !vis.has(key) && dfs(x, y, tx, ty, vis)) {
                    return true;
                }
            }
        }
        return false;
    };

    return (
        dfs(source[0], source[1], target[0], target[1], new Set()) &&
        dfs(target[0], target[1], source[0], source[1], new Set())
    );
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn is_escape_possible(blocked: Vec<Vec<i32>>, source: Vec<i32>, target: Vec<i32>) -> bool {
        const N: i64 = 1_000_000;
        let m = (blocked.len() * blocked.len()) as i64 / 2;

        let f = |i: i64, j: i64| -> i64 { i * N + j };

        let mut s: HashSet<i64> = HashSet::new();
        for b in &blocked {
            s.insert(f(b[0] as i64, b[1] as i64));
        }

        fn dfs(
            sx: i64,
            sy: i64,
            tx: i64,
            ty: i64,
            s: &HashSet<i64>,
            m: i64,
            vis: &mut HashSet<i64>,
        ) -> bool {
            static DIRS: [i64; 5] = [-1, 0, 1, 0, -1];
            let key = sx * 1_000_000 + sy;
            vis.insert(key);
            if vis.len() as i64 > m {
                return true;
            }
            for k in 0..4 {
                let x = sx + DIRS[k];
                let y = sy + DIRS[k + 1];
                let key = x * 1_000_000 + y;
                if x >= 0 && x < 1_000_000 && y >= 0 && y < 1_000_000 {
                    if x == tx && y == ty {
                        return true;
                    }
                    if !s.contains(&key) && vis.insert(key) && dfs(x, y, tx, ty, s, m, vis) {
                        return true;
                    }
                }
            }
            false
        }

        dfs(
            source[0] as i64,
            source[1] as i64,
            target[0] as i64,
            target[1] as i64,
            &s,
            m,
            &mut HashSet::new(),
        ) && dfs(
            target[0] as i64,
            target[1] as i64,
            source[0] as i64,
            source[1] as i64,
            &s,
            m,
            &mut HashSet::new(),
        )
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
