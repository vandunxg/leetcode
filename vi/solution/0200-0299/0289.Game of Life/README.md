---
comments: true
difficulty: Medium
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [289. Game of Life](https://leetcode.com/problems/game-of-life)

[中文文档](/solution/0200-0299/0289.Game%20of%20Life/README.md)

## Mô tả

<!-- description:start -->

<p>Theo <a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life" target="_blank">bài viết trên Wikipedia</a>: &quot;<b>Game of Life</b>, còn được gọi đơn giản là <b>Life</b>, là một cellular automaton do nhà toán học người Anh John Horton Conway xây dựng vào năm 1970.&quot;</p>

<p>Bàn cờ gồm lưới ô kích thước <code>m x n</code>, mỗi ô có trạng thái ban đầu là <b>sống</b> (biểu diễn bằng <code>1</code>) hoặc <b>chết</b> (biểu diễn bằng <code>0</code>). Mỗi ô tương tác với <a href="https://en.wikipedia.org/wiki/Moore_neighborhood" target="_blank">tám ô lân cận</a> (theo chiều ngang, dọc và chéo) theo bốn quy tắc sau (trích từ bài viết Wikipedia ở trên):</p>

<ol>
	<li>Ô sống có ít hơn hai ô sống lân cận sẽ chết như thể do quần thể không đủ lớn.</li>
	<li>Ô sống có hai hoặc ba ô sống lân cận sẽ tiếp tục sống sang thế hệ tiếp theo.</li>
	<li>Ô sống có hơn ba ô sống lân cận sẽ chết như thể do quần thể quá đông.</li>
	<li>Ô chết có đúng ba ô sống lân cận sẽ trở thành ô sống như thể do sinh sản.</li>
</ol>

<p><span>Trạng thái tiếp theo của bàn cờ được xác định bằng cách áp dụng đồng thời các quy tắc trên cho mọi ô trong trạng thái hiện tại của lưới <code>m x n</code> <code>board</code>. Quá trình sinh và chết diễn ra <strong>đồng thời</strong>.</span></p>

<p><span>Cho trạng thái hiện tại của <code>board</code>, hãy <strong>cập nhật</strong> <code>board</code> để thể hiện trạng thái tiếp theo.</span></p>

<p><strong>Lưu ý</strong> rằng bạn không cần trả về giá trị nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0289.Game%20of%20Life/images/grid1.jpg" style="width: 562px; height: 322px;" />
<pre>
<strong>Đầu vào:</strong> board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
<strong>Đầu ra:</strong> [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0289.Game%20of%20Life/images/grid2.jpg" style="width: 402px; height: 162px;" />
<pre>
<strong>Đầu vào:</strong> board = [[1,1],[1,0]]
<strong>Đầu ra:</strong> [[1,1],[1,1]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 25</code></li>
	<li><code>board[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Bạn có thể giải bài này tại chỗ không? Hãy nhớ rằng cần cập nhật bàn cờ đồng thời: không thể cập nhật một số ô trước rồi dùng giá trị mới của chúng để cập nhật các ô khác.</li>
	<li>Trong bài này, bàn cờ được biểu diễn bằng mảng 2D. Về nguyên tắc, bàn cờ là vô hạn, nên sẽ phát sinh vấn đề khi vùng hoạt động chạm đến biên của mảng (tức là ô sống đi tới biên). Bạn sẽ xử lý vấn đề này như thế nào?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đánh dấu tại chỗ

<!-- thinking:start -->

> **Tư duy**
>
> Trạng thái tiếp theo phụ thuộc vào tám ô lân cận ở trạng thái hiện tại; nếu ghi đè ngay, ta sẽ làm sai dữ liệu của những ô chưa xử lý. Dùng giá trị đánh dấu: ô sống chuyển thành chết được ghi là $2$, ô chết chuyển thành sống được ghi là $-1$; khi đếm, các giá trị dương vẫn được xem là ô sống.
>
> Duyệt lần hai để đổi $2$ thành $0$ và $-1$ thành $1$, vẫn thao tác trực tiếp trên bàn cờ.

<!-- thinking:end -->

Ta dùng hai trạng thái đánh dấu mới. Trạng thái $2$ cho biết ô sống sẽ chết ở trạng thái tiếp theo, còn trạng thái $-1$ cho biết ô chết sẽ sống ở trạng thái tiếp theo. Vì vậy, khi duyệt lưới hiện tại, giá trị lớn hơn $0$ nghĩa là ô đang sống; ngược lại, ô đang chết.

Ta duyệt toàn bộ bàn cờ, đếm số ô sống lân cận của từng ô và lưu vào biến $live$. Nếu ô hiện tại đang sống, khi $live \lt 2$ hoặc $live \gt 3$, trạng thái tiếp theo của ô là chết, tức trạng thái $2$. Nếu ô hiện tại đang chết, khi $live = 3$, trạng thái tiếp theo của ô là sống, tức trạng thái $-1$.

Cuối cùng, ta duyệt bàn cờ lần nữa, đổi các ô ở trạng thái $2$ thành ô chết và các ô ở trạng thái $-1$ thành ô sống.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của bàn cờ, vì ta cần duyệt toàn bộ bàn cờ. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        m, n = len(board), len(board[0])
        for i in range(m):
            for j in range(n):
                live = -board[i][j]
                for x in range(i - 1, i + 2):
                    for y in range(j - 1, j + 2):
                        if 0 <= x < m and 0 <= y < n and board[x][y] > 0:
                            live += 1
                if board[i][j] and (live < 2 or live > 3):
                    board[i][j] = 2
                if board[i][j] == 0 and live == 3:
                    board[i][j] = -1
        for i in range(m):
            for j in range(n):
                if board[i][j] == 2:
                    board[i][j] = 0
                elif board[i][j] == -1:
                    board[i][j] = 1
```

#### Java

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int live = -board[i][j];
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] > 0) {
                            ++live;
                        }
                    }
                }
                if (board[i][j] == 1 && (live < 2 || live > 3)) {
                    board[i][j] = 2;
                }
                if (board[i][j] == 0 && live == 3) {
                    board[i][j] = -1;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 2) {
                    board[i][j] = 0;
                } else if (board[i][j] == -1) {
                    board[i][j] = 1;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size(), n = board[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int live = -board[i][j];
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] > 0) {
                            ++live;
                        }
                    }
                }
                if (board[i][j] == 1 && (live < 2 || live > 3)) {
                    board[i][j] = 2;
                }
                if (board[i][j] == 0 && live == 3) {
                    board[i][j] = -1;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 2) {
                    board[i][j] = 0;
                } else if (board[i][j] == -1) {
                    board[i][j] = 1;
                }
            }
        }
    }
};
```

#### Go

```go
func gameOfLife(board [][]int) {
	m, n := len(board), len(board[0])
	for i := 0; i < m; i++ {
		for j, v := range board[i] {
			live := -v
			for x := i - 1; x <= i+1; x++ {
				for y := j - 1; y <= j+1; y++ {
					if x >= 0 && x < m && y >= 0 && y < n && board[x][y] > 0 {
						live++
					}
				}
			}
			if v == 1 && (live < 2 || live > 3) {
				board[i][j] = 2
			}
			if v == 0 && live == 3 {
				board[i][j] = -1
			}
		}
	}
	for i := 0; i < m; i++ {
		for j, v := range board[i] {
			if v == 2 {
				board[i][j] = 0
			}
			if v == -1 {
				board[i][j] = 1
			}
		}
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify board in-place instead.
 */
function gameOfLife(board: number[][]): void {
    const m = board.length;
    const n = board[0].length;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            let live = -board[i][j];
            for (let x = i - 1; x <= i + 1; ++x) {
                for (let y = j - 1; y <= j + 1; ++y) {
                    if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] > 0) {
                        ++live;
                    }
                }
            }
            if (board[i][j] === 1 && (live < 2 || live > 3)) {
                board[i][j] = 2;
            }
            if (board[i][j] === 0 && live === 3) {
                board[i][j] = -1;
            }
        }
    }
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (board[i][j] === 2) {
                board[i][j] = 0;
            }
            if (board[i][j] === -1) {
                board[i][j] = 1;
            }
        }
    }
}
```

#### Rust

```rust
const DIR: [(i32, i32); 8] = [
    (-1, 0),
    (1, 0),
    (0, -1),
    (0, 1),
    (-1, -1),
    (-1, 1),
    (1, -1),
    (1, 1),
];

impl Solution {
    #[allow(dead_code)]
    pub fn game_of_life(board: &mut Vec<Vec<i32>>) {
        let n = board.len();
        let m = board[0].len();
        let mut weight_vec: Vec<Vec<i32>> = vec![vec![0; m]; n];

        // Initialize the weight vector
        for i in 0..n {
            for j in 0..m {
                if board[i][j] == 0 {
                    continue;
                }
                for (dx, dy) in DIR {
                    let x = (i as i32) + dx;
                    let y = (j as i32) + dy;
                    if Self::check_bounds(x, y, n as i32, m as i32) {
                        weight_vec[x as usize][y as usize] += 1;
                    }
                }
            }
        }

        // Update the board
        for i in 0..n {
            for j in 0..m {
                if weight_vec[i][j] < 2 {
                    board[i][j] = 0;
                } else if weight_vec[i][j] <= 3 {
                    if board[i][j] == 0 && weight_vec[i][j] == 3 {
                        board[i][j] = 1;
                    }
                } else {
                    board[i][j] = 0;
                }
            }
        }
    }

    #[allow(dead_code)]
    fn check_bounds(i: i32, j: i32, n: i32, m: i32) -> bool {
        i >= 0 && i < n && j >= 0 && j < m
    }
}
```

#### C#

```cs
public class Solution {
    public void GameOfLife(int[][] board) {
        int m = board.Length;
        int n = board[0].Length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int live = -board[i][j];
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] > 0) {
                            ++live;
                        }
                    }
                }
                if (board[i][j] == 1 && (live < 2 || live > 3)) {
                    board[i][j] = 2;
                }
                if (board[i][j] == 0 && live == 3) {
                    board[i][j] = -1;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 2) {
                    board[i][j] = 0;
                }
                if (board[i][j] == -1) {
                    board[i][j] = 1;
                }
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
