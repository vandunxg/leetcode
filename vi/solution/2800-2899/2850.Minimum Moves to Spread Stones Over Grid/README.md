---
comments: true
difficulty: Medium
rating: 2001
source: Weekly Contest 362 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
    - Matrix
---

<!-- problem:start -->

# [2850. Minimum Moves to Spread Stones Over Grid](https://leetcode.com/problems/minimum-moves-to-spread-stones-over-grid)

[中文文档](/solution/2800-2899/2850.Minimum%20Moves%20to%20Spread%20Stones%20Over%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên 2 chiều <code>grid</code> có chỉ số bắt đầu từ <strong>0</strong>, kích thước <code>3 * 3</code>, biểu diễn số viên đá trong mỗi ô. Ma trận chứa chính xác <code>9</code> viên đá và một ô có thể chứa <strong>nhiều</strong> viên đá.</p>

<p>Trong một bước, bạn có thể di chuyển một viên đá từ ô hiện tại đến bất kỳ ô nào khác nếu hai ô có chung một cạnh.</p>

<p>Trả về <em><strong>số bước di chuyển nhỏ nhất</strong> cần thiết để đặt mỗi ô đúng một viên đá</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2850.Minimum%20Moves%20to%20Spread%20Stones%20Over%20Grid/images/example1-3.svg" style="width: 401px; height: 281px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0],[1,1,1],[1,2,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Một chuỗi di chuyển có thể đặt mỗi ô đúng một viên đá là:
1- Di chuyển một viên đá từ ô (2,1) đến ô (2,2).
2- Di chuyển một viên đá từ ô (2,2) đến ô (1,2).
3- Di chuyển một viên đá từ ô (1,2) đến ô (0,2).
Tổng cộng cần 3 bước di chuyển để mỗi ô trong ma trận có đúng một viên đá.
Có thể chứng minh rằng 3 là số bước di chuyển nhỏ nhất cần thiết để mỗi ô trong ma trận có đúng một viên đá.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2850.Minimum%20Moves%20to%20Spread%20Stones%20Over%20Grid/images/example2-2.svg" style="width: 401px; height: 281px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,3,0],[1,0,0],[1,0,3]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một chuỗi di chuyển có thể đặt mỗi ô đúng một viên đá là:
1- Di chuyển một viên đá từ ô (0,1) đến ô (0,2).
2- Di chuyển một viên đá từ ô (0,1) đến ô (1,1).
3- Di chuyển một viên đá từ ô (2,2) đến ô (1,2).
4- Di chuyển một viên đá từ ô (2,2) đến ô (2,1).
Tổng cộng cần 4 bước di chuyển để mỗi ô trong ma trận có đúng một viên đá.
Có thể chứng minh rằng 4 là số bước di chuyển nhỏ nhất cần thiết để mỗi ô trong ma trận có đúng một viên đá.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>grid.length == grid[i].length == 3</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 9</code></li>
	<li>Tổng của <code>grid</code> bằng <code>9</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS ngây thơ

<!-- thinking:start -->

> **Tư duy**
>
> Bàn cờ có kích thước $3\times 3$, nên không gian trạng thái rất nhỏ. Việc di chuyển một viên đá thừa đến một ô trống kề bên là bài toán đường đi ngắn nhất; chỉ cần BFS từ ma trận ban đầu đến ma trận toàn số 1.

<!-- thinking:end -->

Về bản chất, bài toán là tìm đường đi ngắn nhất từ trạng thái ban đầu đến trạng thái đích trong một đồ thị trạng thái, vì vậy ta có thể dùng BFS. Trạng thái ban đầu là `grid`, còn trạng thái đích là `[[1, 1, 1], [1, 1, 1], [1, 1, 1]]`. Trong mỗi thao tác, ta có thể di chuyển một viên đá từ ô có nhiều hơn $1$ viên đến một ô kề bên không có quá $1$ viên. Khi tìm thấy trạng thái đích, ta có thể trả về số lớp hiện tại, đây chính là số bước di chuyển nhỏ nhất.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumMoves(self, grid: List[List[int]]) -> int:
        q = deque([tuple(tuple(row) for row in grid)])
        vis = set(q)
        ans = 0
        dirs = (-1, 0, 1, 0, -1)
        while 1:
            for _ in range(len(q)):
                cur = q.popleft()
                if all(x for row in cur for x in row):
                    return ans
                for i in range(3):
                    for j in range(3):
                        if cur[i][j] > 1:
                            for a, b in pairwise(dirs):
                                x, y = i + a, j + b
                                if 0 <= x < 3 and 0 <= y < 3 and cur[x][y] < 2:
                                    nxt = [list(row) for row in cur]
                                    nxt[i][j] -= 1
                                    nxt[x][y] += 1
                                    nxt = tuple(tuple(row) for row in nxt)
                                    if nxt not in vis:
                                        vis.add(nxt)
                                        q.append(nxt)
            ans += 1
```

#### Java

```java
class Solution {
    public int minimumMoves(int[][] grid) {
        Deque<String> q = new ArrayDeque<>();
        q.add(f(grid));
        Set<String> vis = new HashSet<>();
        vis.add(f(grid));
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int ans = 0;; ++ans) {
            for (int k = q.size(); k > 0; --k) {
                String p = q.poll();
                if ("111111111".equals(p)) {
                    return ans;
                }
                int[][] cur = g(p);
                for (int i = 0; i < 3; ++i) {
                    for (int j = 0; j < 3; ++j) {
                        if (cur[i][j] > 1) {
                            for (int d = 0; d < 4; ++d) {
                                int x = i + dirs[d];
                                int y = j + dirs[d + 1];
                                if (x >= 0 && x < 3 && y >= 0 && y < 3 && cur[x][y] < 2) {
                                    int[][] nxt = new int[3][3];
                                    for (int r = 0; r < 3; ++r) {
                                        for (int c = 0; c < 3; ++c) {
                                            nxt[r][c] = cur[r][c];
                                        }
                                    }
                                    nxt[i][j]--;
                                    nxt[x][y]++;
                                    String s = f(nxt);
                                    if (!vis.contains(s)) {
                                        vis.add(s);
                                        q.add(s);
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }

    private String f(int[][] grid) {
        StringBuilder sb = new StringBuilder();
        for (int[] row : grid) {
            for (int x : row) {
                sb.append(x);
            }
        }
        return sb.toString();
    }

    private int[][] g(String s) {
        int[][] grid = new int[3][3];
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                grid[i][j] = s.charAt(i * 3 + j) - '0';
            }
        }
        return grid;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& grid) {
        queue<string> q;
        q.push(f(grid));
        unordered_set<string> vis;
        vis.insert(f(grid));
        vector<int> dirs = {-1, 0, 1, 0, -1};

        for (int ans = 0;; ++ans) {
            int sz = q.size();
            while (sz--) {
                string p = q.front();
                q.pop();
                if (p == "111111111") {
                    return ans;
                }
                vector<vector<int>> cur = g(p);

                for (int i = 0; i < 3; ++i) {
                    for (int j = 0; j < 3; ++j) {
                        if (cur[i][j] > 1) {
                            for (int d = 0; d < 4; ++d) {
                                int x = i + dirs[d];
                                int y = j + dirs[d + 1];
                                if (x >= 0 && x < 3 && y >= 0 && y < 3 && cur[x][y] < 2) {
                                    vector<vector<int>> nxt = cur;
                                    nxt[i][j]--;
                                    nxt[x][y]++;
                                    string s = f(nxt);
                                    if (!vis.count(s)) {
                                        vis.insert(s);
                                        q.push(s);
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }

private:
    string f(const vector<vector<int>>& grid) {
        string s;
        for (const auto& row : grid) {
            for (int x : row) {
                s += to_string(x);
            }
        }
        return s;
    }

    vector<vector<int>> g(const string& s) {
        vector<vector<int>> grid(3, vector<int>(3));
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                grid[i][j] = s[i * 3 + j] - '0';
            }
        }
        return grid;
    }
};
```

#### Go

```go
type Queue []string

func (q *Queue) Push(s string) {
	*q = append(*q, s)
}

func (q *Queue) Pop() string {
	s := (*q)[0]
	*q = (*q)[1:]
	return s
}

func (q *Queue) Empty() bool {
	return len(*q) == 0
}

func minimumMoves(grid [][]int) int {
	q := Queue{f(grid)}
	vis := map[string]bool{f(grid): true}
	dirs := []int{-1, 0, 1, 0, -1}

	for ans := 0; ; ans++ {
		sz := len(q)
		for ; sz > 0; sz-- {
			p := q.Pop()
			if p == "111111111" {
				return ans
			}
			cur := g(p)

			for i := 0; i < 3; i++ {
				for j := 0; j < 3; j++ {
					if cur[i][j] > 1 {
						for d := 0; d < 4; d++ {
							x, y := i+dirs[d], j+dirs[d+1]
							if x >= 0 && x < 3 && y >= 0 && y < 3 && cur[x][y] < 2 {
								nxt := make([][]int, 3)
								for r := range nxt {
									nxt[r] = append([]int(nil), cur[r]...)
								}
								nxt[i][j]--
								nxt[x][y]++
								s := f(nxt)
								if !vis[s] {
									vis[s] = true
									q.Push(s)
								}
							}
						}
					}
				}
			}
		}
	}
}

func f(grid [][]int) string {
	var sb strings.Builder
	for _, row := range grid {
		for _, x := range row {
			sb.WriteByte(byte(x) + '0')
		}
	}
	return sb.String()
}

func g(s string) [][]int {
	grid := make([][]int, 3)
	for i := range grid {
		grid[i] = make([]int, 3)
		for j := 0; j < 3; j++ {
			grid[i][j] = int(s[i*3+j] - '0')
		}
	}
	return grid
}
```

#### TypeScript

```ts
function minimumMoves(grid: number[][]): number {
    const q: string[] = [f(grid)];
    const vis: Set<string> = new Set([f(grid)]);
    const dirs: number[] = [-1, 0, 1, 0, -1];

    for (let ans = 0; ; ans++) {
        let sz = q.length;
        while (sz-- > 0) {
            const p = q.shift()!;
            if (p === '111111111') {
                return ans;
            }
            const cur = g(p);

            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 3; j++) {
                    if (cur[i][j] > 1) {
                        for (let d = 0; d < 4; d++) {
                            const x = i + dirs[d],
                                y = j + dirs[d + 1];
                            if (x >= 0 && x < 3 && y >= 0 && y < 3 && cur[x][y] < 2) {
                                const nxt = cur.map(row => [...row]);
                                nxt[i][j]--;
                                nxt[x][y]++;
                                const s = f(nxt);
                                if (!vis.has(s)) {
                                    vis.add(s);
                                    q.push(s);
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}

function f(grid: number[][]): string {
    return grid.flat().join('');
}

function g(s: string): number[][] {
    return Array.from({ length: 3 }, (_, i) =>
        Array.from({ length: 3 }, (_, j) => Number(s[i * 3 + j])),
    );
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> BFS vẫn phải mở rộng từng bước di chuyển giữa các ô kề nhau. Các ô trống và các viên đá thừa tạo thành một bài toán ghép cặp có chi phí nhỏ nhất. Với nhiều nhất tám ô trống, ta có thể dùng bit mask của các viên đá thừa đã được ghép và chi phí Manhattan để xây dựng DP, qua đó bỏ qua quá trình di chuyển từng bước.

<!-- thinking:end -->

Ta có thể đưa toàn bộ tọa độ $(i, j)$ của các ô có giá trị bằng $0$ vào một mảng $left$. Nếu giá trị $v$ của một ô lớn hơn $1$, ta đưa $v-1$ bản sao tọa độ $(i, j)$ vào một mảng $right$. Khi đó, bài toán trở thành: mỗi tọa độ $(i, j)$ trong $right$ cần được di chuyển đến một tọa độ $(x, y)$ trong $left$, và ta cần tìm tổng số bước di chuyển nhỏ nhất.

Gọi độ dài của $left$ là $n$. Ta dùng một số nhị phân gồm $n$ bit để biểu diễn việc mỗi tọa độ trong $left$ đã được một tọa độ trong $right$ lấp đầy hay chưa, trong đó $1$ biểu thị đã được lấp đầy và $0$ biểu thị chưa được lấp đầy. Ban đầu, $f[i] = \infty$, còn $f[0]=0$.

Xét $f[i]$, giả sử số bit $1$ trong biểu diễn nhị phân của $i$ là $k$. Ta duyệt $j$ trong khoảng $[0..n)$; nếu bit thứ $j$ của $i$ bằng $1$, thì $f[i]$ có thể được chuyển từ $f[i \oplus (1 << j)]$, với chi phí chuyển là $cal(left[k-1], right[j])$, trong đó $cal$ biểu thị khoảng cách Manhattan giữa hai tọa độ. Đáp án cuối cùng là $f[(1 << n) - 1]$.

Độ phức tạp thời gian là $O(n \times 2^n)$, còn độ phức tạp không gian là $O(2^n)$. Ở đây, $n$ là độ dài của $left$ và trong bài toán này $n \le 9$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumMoves(self, grid: List[List[int]]) -> int:
        def cal(a: tuple, b: tuple) -> int:
            return abs(a[0] - b[0]) + abs(a[1] - b[1])

        left, right = [], []
        for i in range(3):
            for j in range(3):
                if grid[i][j] == 0:
                    left.append((i, j))
                else:
                    for _ in range(grid[i][j] - 1):
                        right.append((i, j))

        n = len(left)
        f = [inf] * (1 << n)
        f[0] = 0
        for i in range(1, 1 << n):
            k = i.bit_count()
            for j in range(n):
                if i >> j & 1:
                    f[i] = min(f[i], f[i ^ (1 << j)] + cal(left[k - 1], right[j]))
        return f[-1]
```

#### Java

```java
class Solution {
    public int minimumMoves(int[][] grid) {
        List<int[]> left = new ArrayList<>();
        List<int[]> right = new ArrayList<>();
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (grid[i][j] == 0) {
                    left.add(new int[] {i, j});
                } else {
                    for (int k = 1; k < grid[i][j]; ++k) {
                        right.add(new int[] {i, j});
                    }
                }
            }
        }
        int n = left.size();
        int[] f = new int[1 << n];
        Arrays.fill(f, 1 << 30);
        f[0] = 0;
        for (int i = 1; i < 1 << n; ++i) {
            int k = Integer.bitCount(i);
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    f[i] = Math.min(f[i], f[i ^ (1 << j)] + cal(left.get(k - 1), right.get(j)));
                }
            }
        }
        return f[(1 << n) - 1];
    }

    private int cal(int[] a, int[] b) {
        return Math.abs(a[0] - b[0]) + Math.abs(a[1] - b[1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& grid) {
        using pii = pair<int, int>;
        vector<pii> left, right;
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (grid[i][j] == 0) {
                    left.emplace_back(i, j);
                } else {
                    for (int k = 1; k < grid[i][j]; ++k) {
                        right.emplace_back(i, j);
                    }
                }
            }
        }
        auto cal = [](pii a, pii b) {
            return abs(a.first - b.first) + abs(a.second - b.second);
        };
        int n = left.size();
        int f[1 << n];
        memset(f, 0x3f, sizeof(f));
        f[0] = 0;
        for (int i = 1; i < 1 << n; ++i) {
            int k = __builtin_popcount(i);
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    f[i] = min(f[i], f[i ^ (1 << j)] + cal(left[k - 1], right[j]));
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```

#### Go

```go
func minimumMoves(grid [][]int) int {
	left := [][2]int{}
	right := [][2]int{}
	for i := 0; i < 3; i++ {
		for j := 0; j < 3; j++ {
			if grid[i][j] == 0 {
				left = append(left, [2]int{i, j})
			} else {
				for k := 1; k < grid[i][j]; k++ {
					right = append(right, [2]int{i, j})
				}
			}
		}
	}
	cal := func(a, b [2]int) int {
		return abs(a[0]-b[0]) + abs(a[1]-b[1])
	}
	n := len(left)
	f := make([]int, 1<<n)
	f[0] = 0
	for i := 1; i < 1<<n; i++ {
		f[i] = 1 << 30
		k := bits.OnesCount(uint(i))
		for j := 0; j < n; j++ {
			if i>>j&1 == 1 {
				f[i] = min(f[i], f[i^(1<<j)]+cal(left[k-1], right[j]))
			}
		}
	}
	return f[(1<<n)-1]
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minimumMoves(grid: number[][]): number {
    const left: number[][] = [];
    const right: number[][] = [];
    for (let i = 0; i < 3; ++i) {
        for (let j = 0; j < 3; ++j) {
            if (grid[i][j] === 0) {
                left.push([i, j]);
            } else {
                for (let k = 1; k < grid[i][j]; ++k) {
                    right.push([i, j]);
                }
            }
        }
    }
    const cal = (a: number[], b: number[]) => {
        return Math.abs(a[0] - b[0]) + Math.abs(a[1] - b[1]);
    };
    const n = left.length;
    const f: number[] = Array(1 << n).fill(1 << 30);
    f[0] = 0;
    for (let i = 0; i < 1 << n; ++i) {
        let k = 0;
        for (let j = 0; j < n; ++j) {
            if ((i >> j) & 1) {
                ++k;
            }
        }
        for (let j = 0; j < n; ++j) {
            if ((i >> j) & 1) {
                f[i] = Math.min(f[i], f[i ^ (1 << j)] + cal(left[k - 1], right[j]));
            }
        }
    }
    return f[(1 << n) - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
