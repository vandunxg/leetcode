---
comments: true
difficulty: Medium
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [794. Valid Tic-Tac-Toe State](https://leetcode.com/problems/valid-tic-tac-toe-state)

[中文文档](/solution/0700-0799/0794.Valid%20Tic-Tac-Toe%20State/README.md)

## Mô tả

<!-- description:start -->

<p>Cho bàn cờ Tic-Tac-Toe được biểu diễn bằng mảng chuỗi <code>board</code>. Trả về <code>true</code> khi và chỉ khi trạng thái bàn cờ này có thể xuất hiện trong một ván tic-tac-toe hợp lệ.</p>

<p>Bàn cờ là mảng <code>3 x 3</code> gồm các ký tự <code>&#39; &#39;</code>, <code>&#39;X&#39;</code> và <code>&#39;O&#39;</code>. Ký tự <code>&#39; &#39;</code> biểu thị một ô trống.</p>

<p>Luật chơi Tic-Tac-Toe như sau:</p>

<ul>
	<li>Hai người chơi lần lượt đặt ký tự vào các ô trống <code>&#39; &#39;</code>.</li>
	<li>Người chơi thứ nhất luôn đặt ký tự <code>&#39;X&#39;</code>, còn người chơi thứ hai luôn đặt ký tự <code>&#39;O&#39;</code>.</li>
	<li>Ký tự <code>&#39;X&#39;</code> và <code>&#39;O&#39;</code> chỉ được đặt vào ô trống, không được đặt vào ô đã có ký tự.</li>
	<li>Ván đấu kết thúc khi có ba ký tự giống nhau (khác ký tự trống) nằm trên cùng một hàng, cột hoặc đường chéo.</li>
	<li>Ván đấu cũng kết thúc khi tất cả ô đều đã được đánh dấu.</li>
	<li>Khi ván đấu kết thúc thì không được thực hiện thêm lượt nào.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0794.Valid%20Tic-Tac-Toe%20State/images/tictactoe1-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> board = [&quot;O  &quot;,&quot;   &quot;,&quot;   &quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Người chơi thứ nhất luôn đánh &quot;X&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0794.Valid%20Tic-Tac-Toe%20State/images/tictactoe2-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> board = [&quot;XOX&quot;,&quot; X &quot;,&quot;   &quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Hai người chơi phải lần lượt thực hiện nước đi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0794.Valid%20Tic-Tac-Toe%20State/images/tictactoe4-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> board = [&quot;XOX&quot;,&quot;O O&quot;,&quot;XOX&quot;]
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>board.length == 3</code></li>
	<li><code>board[i].length == 3</code></li>
	<li><code>board[i][j]</code> là <code>&#39;X&#39;</code>, <code>&#39;O&#39;</code> hoặc <code>&#39; &#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xác định trạng thái bàn cờ $3\times 3$ có thể xuất hiện trong ván đấu bắt đầu bằng X hay không. Bàn có chín ô, nên chỉ cần đếm số ký hiệu và kiểm tra bên nào đã thắng.
>
> Số quân X phải bằng $o$ hoặc $o+1$. Nếu X thắng thì cần $x=o+1$; nếu O thắng thì cần $x=o$. Các điều kiện về số quân này cũng loại trừ trường hợp cả hai cùng thắng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validTicTacToe(self, board: List[str]) -> bool:
        def win(x):
            for i in range(3):
                if all(board[i][j] == x for j in range(3)):
                    return True
                if all(board[j][i] == x for j in range(3)):
                    return True
            if all(board[i][i] == x for i in range(3)):
                return True
            return all(board[i][2 - i] == x for i in range(3))

        x = sum(board[i][j] == 'X' for i in range(3) for j in range(3))
        o = sum(board[i][j] == 'O' for i in range(3) for j in range(3))
        if x != o and x - 1 != o:
            return False
        if win('X') and x - 1 != o:
            return False
        return not (win('O') and x != o)
```

#### Java

```java
class Solution {
    private String[] board;

    public boolean validTicTacToe(String[] board) {
        this.board = board;
        int x = count('X'), o = count('O');
        if (x != o && x - 1 != o) {
            return false;
        }
        if (win('X') && x - 1 != o) {
            return false;
        }
        return !(win('O') && x != o);
    }

    private boolean win(char x) {
        for (int i = 0; i < 3; ++i) {
            if (board[i].charAt(0) == x && board[i].charAt(1) == x && board[i].charAt(2) == x) {
                return true;
            }
            if (board[0].charAt(i) == x && board[1].charAt(i) == x && board[2].charAt(i) == x) {
                return true;
            }
        }
        if (board[0].charAt(0) == x && board[1].charAt(1) == x && board[2].charAt(2) == x) {
            return true;
        }
        return board[0].charAt(2) == x && board[1].charAt(1) == x && board[2].charAt(0) == x;
    }

    private int count(char x) {
        int cnt = 0;
        for (var row : board) {
            for (var c : row.toCharArray()) {
                if (c == x) {
                    ++cnt;
                }
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validTicTacToe(vector<string>& board) {
        auto count = [&](char x) {
            int ans = 0;
            for (auto& row : board)
                for (auto& c : row) ans += c == x;
            return ans;
        };
        auto win = [&](char x) {
            for (int i = 0; i < 3; ++i) {
                if (board[i][0] == x && board[i][1] == x && board[i][2] == x) return true;
                if (board[0][i] == x && board[1][i] == x && board[2][i] == x) return true;
            }
            if (board[0][0] == x && board[1][1] == x && board[2][2] == x) return true;
            return board[0][2] == x && board[1][1] == x && board[2][0] == x;
        };
        int x = count('X'), o = count('O');
        if (x != o && x - 1 != o) return false;
        if (win('X') && x - 1 != o) return false;
        return !(win('O') && x != o);
    }
};
```

#### Go

```go
func validTicTacToe(board []string) bool {
	var x, o int
	for _, row := range board {
		for _, c := range row {
			if c == 'X' {
				x++
			} else if c == 'O' {
				o++
			}
		}
	}
	win := func(x byte) bool {
		for i := 0; i < 3; i++ {
			if board[i][0] == x && board[i][1] == x && board[i][2] == x {
				return true
			}
			if board[0][i] == x && board[1][i] == x && board[2][i] == x {
				return true
			}
		}
		if board[0][0] == x && board[1][1] == x && board[2][2] == x {
			return true
		}
		return board[0][2] == x && board[1][1] == x && board[2][0] == x
	}
	if x != o && x-1 != o {
		return false
	}
	if win('X') && x-1 != o {
		return false
	}
	return !(win('O') && x != o)
}
```

#### JavaScript

```js
/**
 * @param {string[]} board
 * @return {boolean}
 */
var validTicTacToe = function (board) {
    function count(x) {
        let cnt = 0;
        for (const row of board) {
            for (const c of row) {
                cnt += c == x;
            }
        }
        return cnt;
    }
    function win(x) {
        for (let i = 0; i < 3; ++i) {
            if (board[i][0] == x && board[i][1] == x && board[i][2] == x) {
                return true;
            }
            if (board[0][i] == x && board[1][i] == x && board[2][i] == x) {
                return true;
            }
        }
        if (board[0][0] == x && board[1][1] == x && board[2][2] == x) {
            return true;
        }
        return board[0][2] == x && board[1][1] == x && board[2][0] == x;
    }
    const [x, o] = [count('X'), count('O')];
    if (x != o && x - 1 != o) {
        return false;
    }
    if (win('X') && x - 1 != o) {
        return false;
    }
    return !(win('O') && x != o);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
