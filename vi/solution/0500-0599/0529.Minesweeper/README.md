---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [529. Minesweeper](https://leetcode.com/problems/minesweeper)

[中文文档](/solution/0500-0599/0529.Minesweeper/README.md)

## Mô tả

<!-- description:start -->

<p>Cùng chơi Minesweeper (<a href="https://en.wikipedia.org/wiki/Minesweeper_(video_game)" target="_blank">Wikipedia</a>, <a href="http://minesweeperonline.com" target="_blank">trò chơi trực tuyến</a>) nhé!</p>

<p>Cho ma trận ký tự <code>m x n</code> <code>board</code> biểu diễn bàn chơi, trong đó:</p>

<ul>
	<li><code>&#39;M&#39;</code> biểu thị một quả mìn chưa được mở,</li>
	<li><code>&#39;E&#39;</code> biểu thị một ô trống chưa được mở,</li>
	<li><code>&#39;B&#39;</code> biểu thị một ô trống đã mở và không có mìn nào ở các ô kề (phía trên, dưới, trái, phải và cả 4 đường chéo),</li>
	<li>chữ số (<code>&#39;1&#39;</code> đến <code>&#39;8&#39;</code>) biểu thị số mìn nằm kề ô đã mở này, và</li>
	<li><code>&#39;X&#39;</code> biểu thị một quả mìn đã được mở.</li>
</ul>

<p>Bạn còn được cho mảng số nguyên <code>click</code>, trong đó <code>click = [click<sub>r</sub>, click<sub>c</sub>]</code> biểu thị vị trí được nhấp tiếp theo trong các ô chưa mở (<code>&#39;M&#39;</code> hoặc <code>&#39;E&#39;</code>).</p>

<p>Hãy trả về <em>bàn chơi sau khi mở ô này theo các quy tắc sau</em>:</p>

<ol>
	<li>Nếu mở phải mìn <code>&#39;M&#39;</code>, trò chơi kết thúc. Hãy đổi ô đó thành <code>&#39;X&#39;</code>.</li>
	<li>Nếu mở một ô trống <code>&#39;E&#39;</code> không có mìn kề, hãy đổi ô đó thành ô trống đã mở <code>&#39;B&#39;</code> và mở đệ quy tất cả các ô chưa mở kề nó.</li>
	<li>Nếu mở một ô trống <code>&#39;E&#39;</code> có ít nhất một mìn kề, hãy đổi ô đó thành chữ số (<code>&#39;1&#39;</code> đến <code>&#39;8&#39;</code>) biểu thị số mìn kề.</li>
	<li>Trả về bàn chơi khi không còn ô nào được mở thêm.</li>
</ol>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0529.Minesweeper/images/untitled.jpeg" style="width: 500px; max-width: 400px; height: 269px;" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot;E&quot;,&quot;E&quot;,&quot;E&quot;,&quot;E&quot;,&quot;E&quot;],[&quot;E&quot;,&quot;E&quot;,&quot;M&quot;,&quot;E&quot;,&quot;E&quot;],[&quot;E&quot;,&quot;E&quot;,&quot;E&quot;,&quot;E&quot;,&quot;E&quot;],[&quot;E&quot;,&quot;E&quot;,&quot;E&quot;,&quot;E&quot;,&quot;E&quot;]], click = [3,0]
<strong>Đầu ra:</strong> [[&quot;B&quot;,&quot;1&quot;,&quot;E&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;1&quot;,&quot;M&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;1&quot;,&quot;1&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;B&quot;,&quot;B&quot;,&quot;B&quot;,&quot;B&quot;]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0529.Minesweeper/images/untitled-2.jpeg" style="width: 489px; max-width: 400px; height: 269px;" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot;B&quot;,&quot;1&quot;,&quot;E&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;1&quot;,&quot;M&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;1&quot;,&quot;1&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;B&quot;,&quot;B&quot;,&quot;B&quot;,&quot;B&quot;]], click = [1,2]
<strong>Đầu ra:</strong> [[&quot;B&quot;,&quot;1&quot;,&quot;E&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;1&quot;,&quot;X&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;1&quot;,&quot;1&quot;,&quot;1&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;B&quot;,&quot;B&quot;,&quot;B&quot;,&quot;B&quot;]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>board[i][j]</code> is either <code>&#39;M&#39;</code>, <code>&#39;E&#39;</code>, <code>&#39;B&#39;</code>, or a digit from <code>&#39;1&#39;</code> to <code>&#39;8&#39;</code>.</li>
	<li><code>click.length == 2</code></li>
	<li><code>0 &lt;= click<sub>r</sub> &lt; m</code></li>
	<li><code>0 &lt;= click<sub>c</sub> &lt; n</code></li>
	<li><code>board[click<sub>r</sub>][click<sub>c</sub>]</code> is either <code>&#39;M&#39;</code> or <code>&#39;E&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nếu nhấp vào mìn thì đổi thành `X`. Nếu không, đếm số mìn ở các ô kề: ghi số lượng đó, hoặc ghi `B` rồi mở rộng sang các ô trống lân cận. Chỉ cần mở vùng bắt đầu từ vị trí được nhấp.
>
> Nếu ô hiện tại là `M`, đánh dấu thành `X`. Nếu không, dùng DFS để đếm các ô kề: nếu có mìn thì ghi chữ số tương ứng và dừng; nếu không thì ghi `B` rồi đệ quy trên các ô `E` kề nó. Mỗi ô được mở rộng tối đa một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def updateBoard(self, board: List[List[str]], click: List[int]) -> List[List[str]]:
        def dfs(i: int, j: int):
            cnt = 0
            for x in range(i - 1, i + 2):
                for y in range(j - 1, j + 2):
                    if 0 <= x < m and 0 <= y < n and board[x][y] == "M":
                        cnt += 1
            if cnt:
                board[i][j] = str(cnt)
            else:
                board[i][j] = "B"
                for x in range(i - 1, i + 2):
                    for y in range(j - 1, j + 2):
                        if 0 <= x < m and 0 <= y < n and board[x][y] == "E":
                            dfs(x, y)

        m, n = len(board), len(board[0])
        i, j = click
        if board[i][j] == "M":
            board[i][j] = "X"
        else:
            dfs(i, j)
        return board
```

#### Java

```java
class Solution {
    private char[][] board;
    private int m;
    private int n;

    public char[][] updateBoard(char[][] board, int[] click) {
        m = board.length;
        n = board[0].length;
        this.board = board;
        int i = click[0], j = click[1];
        if (board[i][j] == 'M') {
            board[i][j] = 'X';
        } else {
            dfs(i, j);
        }
        return board;
    }

    private void dfs(int i, int j) {
        int cnt = 0;
        for (int x = i - 1; x <= i + 1; ++x) {
            for (int y = j - 1; y <= j + 1; ++y) {
                if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] == 'M') {
                    ++cnt;
                }
            }
        }
        if (cnt > 0) {
            board[i][j] = (char) (cnt + '0');
        } else {
            board[i][j] = 'B';
            for (int x = i - 1; x <= i + 1; ++x) {
                for (int y = j - 1; y <= j + 1; ++y) {
                    if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] == 'E') {
                        dfs(x, y);
                    }
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
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        int m = board.size(), n = board[0].size();
        int i = click[0], j = click[1];

        function<void(int, int)> dfs = [&](int i, int j) {
            int cnt = 0;
            for (int x = i - 1; x <= i + 1; ++x) {
                for (int y = j - 1; y <= j + 1; ++y) {
                    if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] == 'M') {
                        ++cnt;
                    }
                }
            }
            if (cnt) {
                board[i][j] = cnt + '0';
            } else {
                board[i][j] = 'B';
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] == 'E') {
                            dfs(x, y);
                        }
                    }
                }
            }
        };

        if (board[i][j] == 'M') {
            board[i][j] = 'X';
        } else {
            dfs(i, j);
        }
        return board;
    }
};
```

#### Go

```go
func updateBoard(board [][]byte, click []int) [][]byte {
	m, n := len(board), len(board[0])
	i, j := click[0], click[1]

	var dfs func(i, j int)
	dfs = func(i, j int) {
		cnt := 0
		for x := i - 1; x <= i+1; x++ {
			for y := j - 1; y <= j+1; y++ {
				if x >= 0 && x < m && y >= 0 && y < n && board[x][y] == 'M' {
					cnt++
				}
			}
		}
		if cnt > 0 {
			board[i][j] = byte(cnt + '0')
			return
		}
		board[i][j] = 'B'
		for x := i - 1; x <= i+1; x++ {
			for y := j - 1; y <= j+1; y++ {
				if x >= 0 && x < m && y >= 0 && y < n && board[x][y] == 'E' {
					dfs(x, y)
				}
			}
		}
	}

	if board[i][j] == 'M' {
		board[i][j] = 'X'
	} else {
		dfs(i, j)
	}
	return board
}
```

#### TypeScript

```ts
function updateBoard(board: string[][], click: number[]): string[][] {
    const m = board.length;
    const n = board[0].length;
    const [i, j] = click;

    const dfs = (i: number, j: number) => {
        let cnt = 0;
        for (let x = i - 1; x <= i + 1; ++x) {
            for (let y = j - 1; y <= j + 1; ++y) {
                if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] === 'M') {
                    ++cnt;
                }
            }
        }
        if (cnt > 0) {
            board[i][j] = cnt.toString();
            return;
        }
        board[i][j] = 'B';
        for (let x = i - 1; x <= i + 1; ++x) {
            for (let y = j - 1; y <= j + 1; ++y) {
                if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] === 'E') {
                    dfs(x, y);
                }
            }
        }
    };

    if (board[i][j] === 'M') {
        board[i][j] = 'X';
    } else {
        dfs(i, j);
    }
    return board;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
