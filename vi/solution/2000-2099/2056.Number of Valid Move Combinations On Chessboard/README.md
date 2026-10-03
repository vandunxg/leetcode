---
comments: true
difficulty: Hard
rating: 2610
source: Biweekly Contest 64 Q4
tags:
    - Array
    - String
    - Backtracking
    - Simulation
---

<!-- problem:start -->

# [2056. Number of Valid Move Combinations On Chessboard](https://leetcode.com/problems/number-of-valid-move-combinations-on-chessboard)

[中文文档](/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/README.md)

## Mô tả

<!-- description:start -->

<p>Trên một bàn cờ <code>8 x 8</code> có <code>n</code> quân cờ (xe, hậu hoặc tượng). Cho một mảng chuỗi <code>pieces</code> có độ dài <code>n</code>, trong đó <code>pieces[i]</code> mô tả loại của quân cờ thứ <code>i<sup>th</sup></code> (xe, hậu hoặc tượng). Ngoài ra, cho một mảng số nguyên 2 chiều <code>positions</code> cũng có độ dài <code>n</code>, trong đó <code>positions[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> cho biết quân cờ thứ <code>i<sup>th</sup></code> hiện đang ở tọa độ <strong>1-based</strong> <code>(r<sub>i</sub>, c<sub>i</sub>)</code> trên bàn cờ.</p>

<p>Khi thực hiện một <strong>nước đi</strong> cho một quân cờ, bạn chọn một ô <strong>đích</strong> mà quân cờ sẽ di chuyển đến và dừng lại.</p>

<ul>
	<li>Xe chỉ có thể di chuyển <strong>theo chiều ngang hoặc chiều dọc</strong> từ <code>(r, c)</code> theo hướng của <code>(r+1, c)</code>, <code>(r-1, c)</code>, <code>(r, c+1)</code> hoặc <code>(r, c-1)</code>.</li>
	<li>Hậu chỉ có thể di chuyển <strong>theo chiều ngang, chiều dọc hoặc đường chéo</strong> từ <code>(r, c)</code> theo hướng của <code>(r+1, c)</code>, <code>(r-1, c)</code>, <code>(r, c+1)</code>, <code>(r, c-1)</code>, <code>(r+1, c+1)</code>, <code>(r+1, c-1)</code>, <code>(r-1, c+1)</code>, <code>(r-1, c-1)</code>.</li>
	<li>Tượng chỉ có thể di chuyển <strong>theo đường chéo</strong> từ <code>(r, c)</code> theo hướng của <code>(r+1, c+1)</code>, <code>(r+1, c-1)</code>, <code>(r-1, c+1)</code>, <code>(r-1, c-1)</code>.</li>
</ul>

<p>Bạn phải thực hiện một <strong>nước đi</strong> cho mọi quân cờ trên bàn cùng lúc. Một <strong>tổ hợp nước đi</strong> gồm tất cả các <strong>nước đi</strong> được thực hiện trên các quân cờ đã cho. Sau mỗi giây, mỗi quân cờ sẽ di chuyển tức thời <strong>một ô</strong> về phía đích nếu nó chưa ở đó. Tất cả quân cờ bắt đầu di chuyển ở giây thứ <code>0<sup>th</sup></code>. Một tổ hợp nước đi là <strong>không hợp lệ</strong> nếu tại một thời điểm nào đó, <strong>từ hai quân cờ trở lên</strong> cùng chiếm một ô.</p>

<p>Hãy trả về <em>số lượng tổ hợp nước đi <strong>hợp lệ</strong></em>​​​​​.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Không có hai quân cờ nào</strong> bắt đầu trên <strong>cùng một</strong> ô.</li>
	<li>Bạn có thể chọn ô mà quân cờ đang đứng làm <strong>đích</strong> của nó.</li>
	<li>Nếu hai quân cờ <strong>đứng ngay cạnh nhau</strong>, chúng có thể <strong>đi vượt qua nhau</strong> và đổi vị trí trong một giây mà vẫn hợp lệ.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/images/a1.png" style="width: 215px; height: 215px;" />
<pre>
<strong>Đầu vào:</strong> pieces = [&quot;rook&quot;], positions = [[1,1]]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Hình trên cho thấy các ô mà quân cờ có thể di chuyển đến.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/images/a2.png" style="width: 215px; height: 215px;" />
<pre>
<strong>Đầu vào:</strong> pieces = [&quot;queen&quot;], positions = [[1,1]]
<strong>Đầu ra:</strong> 22
<strong>Giải thích:</strong> Hình trên cho thấy các ô mà quân cờ có thể di chuyển đến.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2056.Number%20of%20Valid%20Move%20Combinations%20On%20Chessboard/images/a3.png" style="width: 214px; height: 215px;" />
<pre>
<strong>Đầu vào:</strong> pieces = [&quot;bishop&quot;], positions = [[4,3]]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Hình trên cho thấy các ô mà quân cờ có thể di chuyển đến.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == pieces.length </code></li>
	<li><code>n == positions.length</code></li>
	<li><code>1 &lt;= n &lt;= 4</code></li>
	<li><code>pieces</code> chỉ chứa các chuỗi <code>&quot;rook&quot;</code>, <code>&quot;queen&quot;</code> và <code>&quot;bishop&quot;</code>.</li>
	<li>Có nhiều nhất một hậu trên bàn cờ.</li>
	<li><code>1 &lt;= r<sub>i</sub>, c<sub>i</sub> &lt;= 8</code></li>
	<li>Mỗi <code>positions[i]</code> là khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất bốn quân cờ trên bàn $8 \times 8$, mỗi quân có tối đa $\le 8$ hướng di chuyển. Cây tìm kiếm khá lớn nhưng DFS vẫn đủ dùng: lần lượt chọn một tia di chuyển và thời điểm dừng cho từng quân, sau đó loại các va chạm với những quân đã xét.
>
> `dist[i][x][y]` là thời gian đi qua ô và `end` là điểm dừng. Một quân chỉ được dừng khi các quân trước đó đã rời khỏi ô; khi đi qua, không được trùng ô cùng thời điểm hoặc đi vào ô mà một quân đã dừng.
>
> Mỗi khi hoàn thành một phép gán, ta tăng đáp án lên một.

<!-- thinking:end -->

Bài toán có nhiều nhất $4$ quân cờ, mỗi quân có thể di chuyển theo tối đa $8$ hướng. Ta có thể dùng DFS để duyệt tất cả các tổ hợp nước đi có thể có.

Ta lần lượt xét từng quân cờ. Với mỗi quân, ta có thể chọn không di chuyển hoặc di chuyển theo đúng luật. Ta dùng một mảng $\textit{dist}[i]$ để ghi lại chuyển động của quân cờ thứ $i$, trong đó $\textit{dist}[i][x][y]$ biểu thị thời điểm quân cờ thứ $i$ đi qua tọa độ $(x, y)$. Ta dùng một mảng $\textit{end}[i]$ để ghi lại tọa độ và thời điểm kết thúc của quân cờ thứ $i$. Trong quá trình tìm kiếm, ta cần xác định liệu quân cờ hiện tại có thể dừng lại hay có thể tiếp tục di chuyển theo hướng hiện tại.

Ta định nghĩa phương thức $\text{checkStop}(i, x, y, t)$ để xác định liệu quân cờ thứ $i$ có thể dừng tại tọa độ $(x, y)$ ở thời điểm $t$ hay không. Nếu với mọi quân cờ trước đó $j$, $\textit{dist}[j][x][y] < t$, thì quân cờ thứ $i$ có thể dừng lại.

Ngoài ra, ta định nghĩa phương thức $\text{checkPass}(i, x, y, t)$ để xác định liệu quân cờ thứ $i$ có thể đi qua tọa độ $(x, y)$ ở thời điểm $t$ hay không. Nếu có bất kỳ quân cờ nào khác $j$ cũng đi qua tọa độ $(x, y)$ ở thời điểm $t$, hoặc quân cờ $j$ dừng tại $(x, y)$ và thời điểm dừng không lớn hơn $t$, thì quân cờ thứ $i$ không thể đi qua tọa độ $(x, y)$ ở thời điểm $t$.

Độ phức tạp thời gian là $O((n \times M)^n)$, độ phức tạp không gian là $O(n \times M)$. Trong đó, $n$ là số quân cờ và $M$ là phạm vi di chuyển của mỗi quân cờ.

<!-- tabs:start -->

#### Python3

```python
rook_dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]
bishop_dirs = [(1, 1), (1, -1), (-1, 1), (-1, -1)]
queue_dirs = rook_dirs + bishop_dirs


def get_dirs(piece: str) -> List[Tuple[int, int]]:
    match piece[0]:
        case "r":
            return rook_dirs
        case "b":
            return bishop_dirs
        case _:
            return queue_dirs


class Solution:
    def countCombinations(self, pieces: List[str], positions: List[List[int]]) -> int:
        def check_stop(i: int, x: int, y: int, t: int) -> bool:
            return all(dist[j][x][y] < t for j in range(i))

        def check_pass(i: int, x: int, y: int, t: int) -> bool:
            for j in range(i):
                if dist[j][x][y] == t:
                    return False
                if end[j][0] == x and end[j][1] == y and end[j][2] <= t:
                    return False
            return True

        def dfs(i: int) -> None:
            if i >= n:
                nonlocal ans
                ans += 1
                return
            x, y = positions[i]
            dist[i][:] = [[-1] * m for _ in range(m)]
            dist[i][x][y] = 0
            end[i] = (x, y, 0)
            if check_stop(i, x, y, 0):
                dfs(i + 1)
            dirs = get_dirs(pieces[i])
            for dx, dy in dirs:
                dist[i][:] = [[-1] * m for _ in range(m)]
                dist[i][x][y] = 0
                nx, ny, nt = x + dx, y + dy, 1
                while 1 <= nx < m and 1 <= ny < m and check_pass(i, nx, ny, nt):
                    dist[i][nx][ny] = nt
                    end[i] = (nx, ny, nt)
                    if check_stop(i, nx, ny, nt):
                        dfs(i + 1)
                    nx += dx
                    ny += dy
                    nt += 1

        n = len(pieces)
        m = 9
        dist = [[[-1] * m for _ in range(m)] for _ in range(n)]
        end = [(0, 0, 0) for _ in range(n)]
        ans = 0
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    int n, m = 9, ans;
    int[][][] dist;
    int[][] end;
    String[] pieces;
    int[][] positions;
    int[][] rookDirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int[][] bishopDirs = {{1, 1}, {1, -1}, {-1, 1}, {-1, -1}};
    int[][] queenDirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, 1}, {1, -1}, {-1, 1}, {-1, -1}};

    public int countCombinations(String[] pieces, int[][] positions) {
        n = pieces.length;
        dist = new int[n][m][m];
        end = new int[n][3];
        ans = 0;
        this.pieces = pieces;
        this.positions = positions;

        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i >= n) {
            ans++;
            return;
        }

        int x = positions[i][0], y = positions[i][1];
        resetDist(i);
        dist[i][x][y] = 0;
        end[i] = new int[] {x, y, 0};

        if (checkStop(i, x, y, 0)) {
            dfs(i + 1);
        }

        int[][] dirs = getDirs(pieces[i]);
        for (int[] dir : dirs) {
            resetDist(i);
            dist[i][x][y] = 0;
            int nx = x + dir[0], ny = y + dir[1], nt = 1;

            while (isValid(nx, ny) && checkPass(i, nx, ny, nt)) {
                dist[i][nx][ny] = nt;
                end[i] = new int[] {nx, ny, nt};
                if (checkStop(i, nx, ny, nt)) {
                    dfs(i + 1);
                }
                nx += dir[0];
                ny += dir[1];
                nt++;
            }
        }
    }

    private void resetDist(int i) {
        for (int j = 0; j < m; j++) {
            for (int k = 0; k < m; k++) {
                dist[i][j][k] = -1;
            }
        }
    }

    private boolean checkStop(int i, int x, int y, int t) {
        for (int j = 0; j < i; j++) {
            if (dist[j][x][y] >= t) {
                return false;
            }
        }
        return true;
    }

    private boolean checkPass(int i, int x, int y, int t) {
        for (int j = 0; j < i; j++) {
            if (dist[j][x][y] == t) {
                return false;
            }
            if (end[j][0] == x && end[j][1] == y && end[j][2] <= t) {
                return false;
            }
        }
        return true;
    }

    private boolean isValid(int x, int y) {
        return x >= 1 && x < m && y >= 1 && y < m;
    }

    private int[][] getDirs(String piece) {
        char c = piece.charAt(0);
        return switch (c) {
            case 'r' -> rookDirs;
            case 'b' -> bishopDirs;
            default -> queenDirs;
        };
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countCombinations(vector<string>& pieces, vector<vector<int>>& positions) {
        int n = pieces.size();
        const int m = 9;
        int ans = 0;

        vector<vector<vector<int>>> dist(n, vector<vector<int>>(m, vector<int>(m, -1)));
        vector<vector<int>> end(n, vector<int>(3));

        const int rookDirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        const int bishopDirs[4][2] = {{1, 1}, {1, -1}, {-1, 1}, {-1, -1}};
        const int queenDirs[8][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, 1}, {1, -1}, {-1, 1}, {-1, -1}};

        auto resetDist = [&](int i) {
            for (int j = 0; j < m; j++) {
                for (int k = 0; k < m; k++) {
                    dist[i][j][k] = -1;
                }
            }
        };

        auto checkStop = [&](int i, int x, int y, int t) -> bool {
            for (int j = 0; j < i; j++) {
                if (dist[j][x][y] >= t) {
                    return false;
                }
            }
            return true;
        };

        auto checkPass = [&](int i, int x, int y, int t) -> bool {
            for (int j = 0; j < i; j++) {
                if (dist[j][x][y] == t) {
                    return false;
                }
                if (end[j][0] == x && end[j][1] == y && end[j][2] <= t) {
                    return false;
                }
            }
            return true;
        };

        auto isValid = [&](int x, int y) -> bool {
            return x >= 1 && x < m && y >= 1 && y < m;
        };

        auto getDirs = [&](const string& piece) -> const int(*)[2] {
            char c = piece[0];
            if (c == 'r') {
                return rookDirs;
            }
            if (c == 'b') {
                return bishopDirs;
            }
            return queenDirs;
        };

        auto dfs = [&](this auto&& dfs, int i) -> void {
            if (i >= n) {
                ans++;
                return;
            }

            int x = positions[i][0], y = positions[i][1];
            resetDist(i);
            dist[i][x][y] = 0;
            end[i] = {x, y, 0};

            if (checkStop(i, x, y, 0)) {
                dfs(i + 1);
            }

            const int(*dirs)[2] = getDirs(pieces[i]);
            int dirsSize = (pieces[i][0] == 'q') ? 8 : 4;

            for (int d = 0; d < dirsSize; d++) {
                resetDist(i);
                dist[i][x][y] = 0;
                int nx = x + dirs[d][0], ny = y + dirs[d][1], nt = 1;

                while (isValid(nx, ny) && checkPass(i, nx, ny, nt)) {
                    dist[i][nx][ny] = nt;
                    end[i] = {nx, ny, nt};
                    if (checkStop(i, nx, ny, nt)) {
                        dfs(i + 1);
                    }
                    nx += dirs[d][0];
                    ny += dirs[d][1];
                    nt++;
                }
            }
        };

        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func countCombinations(pieces []string, positions [][]int) (ans int) {
	n := len(pieces)
	m := 9
	dist := make([][][]int, n)
	for i := range dist {
		dist[i] = make([][]int, m)
		for j := range dist[i] {
			dist[i][j] = make([]int, m)
		}
	}

	end := make([][3]int, n)

	rookDirs := [][2]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
	bishopDirs := [][2]int{{1, 1}, {1, -1}, {-1, 1}, {-1, -1}}
	queenDirs := [][2]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, 1}, {1, -1}, {-1, 1}, {-1, -1}}

	resetDist := func(i int) {
		for j := 0; j < m; j++ {
			for k := 0; k < m; k++ {
				dist[i][j][k] = -1
			}
		}
	}

	checkStop := func(i, x, y, t int) bool {
		for j := 0; j < i; j++ {
			if dist[j][x][y] >= t {
				return false
			}
		}
		return true
	}

	checkPass := func(i, x, y, t int) bool {
		for j := 0; j < i; j++ {
			if dist[j][x][y] == t {
				return false
			}
			if end[j][0] == x && end[j][1] == y && end[j][2] <= t {
				return false
			}
		}
		return true
	}

	isValid := func(x, y int) bool {
		return x >= 1 && x < m && y >= 1 && y < m
	}

	getDirs := func(piece string) [][2]int {
		switch piece[0] {
		case 'r':
			return rookDirs
		case 'b':
			return bishopDirs
		default:
			return queenDirs
		}
	}

	var dfs func(i int)
	dfs = func(i int) {
		if i >= n {
			ans++
			return
		}

		x, y := positions[i][0], positions[i][1]
		resetDist(i)
		dist[i][x][y] = 0
		end[i] = [3]int{x, y, 0}

		if checkStop(i, x, y, 0) {
			dfs(i + 1)
		}

		dirs := getDirs(pieces[i])
		for _, dir := range dirs {
			resetDist(i)
			dist[i][x][y] = 0
			nx, ny, nt := x+dir[0], y+dir[1], 1

			for isValid(nx, ny) && checkPass(i, nx, ny, nt) {
				dist[i][nx][ny] = nt
				end[i] = [3]int{nx, ny, nt}
				if checkStop(i, nx, ny, nt) {
					dfs(i + 1)
				}
				nx += dir[0]
				ny += dir[1]
				nt++
			}
		}
	}

	dfs(0)
	return
}
```

#### TypeScript

```ts
const rookDirs: [number, number][] = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
];
const bishopDirs: [number, number][] = [
    [1, 1],
    [1, -1],
    [-1, 1],
    [-1, -1],
];
const queenDirs = [...rookDirs, ...bishopDirs];

function countCombinations(pieces: string[], positions: number[][]): number {
    const n = pieces.length;
    const m = 9;
    let ans = 0;

    const dist = Array.from({ length: n }, () =>
        Array.from({ length: m }, () => Array(m).fill(-1)),
    );

    const end: [number, number, number][] = Array(n).fill([0, 0, 0]);

    const resetDist = (i: number) => {
        for (let j = 0; j < m; j++) {
            for (let k = 0; k < m; k++) {
                dist[i][j][k] = -1;
            }
        }
    };

    const checkStop = (i: number, x: number, y: number, t: number): boolean => {
        for (let j = 0; j < i; j++) {
            if (dist[j][x][y] >= t) {
                return false;
            }
        }
        return true;
    };

    const checkPass = (i: number, x: number, y: number, t: number): boolean => {
        for (let j = 0; j < i; j++) {
            if (dist[j][x][y] === t) {
                return false;
            }
            if (end[j][0] === x && end[j][1] === y && end[j][2] <= t) {
                return false;
            }
        }
        return true;
    };

    const isValid = (x: number, y: number): boolean => {
        return x >= 1 && x < m && y >= 1 && y < m;
    };

    const getDirs = (piece: string): [number, number][] => {
        switch (piece[0]) {
            case 'r':
                return rookDirs;
            case 'b':
                return bishopDirs;
            default:
                return queenDirs;
        }
    };

    const dfs = (i: number) => {
        if (i >= n) {
            ans++;
            return;
        }

        const [x, y] = positions[i];
        resetDist(i);
        dist[i][x][y] = 0;
        end[i] = [x, y, 0];

        if (checkStop(i, x, y, 0)) {
            dfs(i + 1);
        }

        const dirs = getDirs(pieces[i]);
        for (const [dx, dy] of dirs) {
            resetDist(i);
            dist[i][x][y] = 0;
            let nx = x + dx,
                ny = y + dy,
                nt = 1;

            while (isValid(nx, ny) && checkPass(i, nx, ny, nt)) {
                dist[i][nx][ny] = nt;
                end[i] = [nx, ny, nt];
                if (checkStop(i, nx, ny, nt)) {
                    dfs(i + 1);
                }
                nx += dx;
                ny += dy;
                nt++;
            }
        }
    };

    dfs(0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
