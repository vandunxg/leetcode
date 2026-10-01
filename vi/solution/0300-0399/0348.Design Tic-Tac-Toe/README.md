---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [348. Design Tic-Tac-Toe 🔒](https://leetcode.com/problems/design-tic-tac-toe)

[中文文档](/solution/0300-0399/0348.Design%20Tic-Tac-Toe/README.md)

## Mô tả

<!-- description:start -->

<p>Giả sử trò chơi tic-tac-toe giữa hai người chơi trên bàn cờ <code>n x n</code> tuân theo các quy tắc sau:</p>

<ol>
	<li>Mỗi nước đi luôn hợp lệ và được đánh vào một ô trống.</li>
	<li>Khi đã có người thắng, không được thực hiện thêm nước đi nào.</li>
	<li>Người chơi thắng khi đánh được <code>n</code> dấu của mình liên tiếp theo hàng ngang, hàng dọc hoặc đường chéo.</li>
</ol>

<p>Hãy triển khai class <code>TicTacToe</code>:</p>

<ul>
	<li><code>TicTacToe(int n)</code> Khởi tạo object với kích thước bàn cờ là <code>n</code>.</li>
	<li><code>int move(int row, int col, int player)</code> Cho biết người chơi có id <code>player</code> đánh vào ô <code>(row, col)</code> trên bàn cờ. Nước đi luôn hợp lệ và hai người chơi lần lượt thực hiện. Trả về
	<ul>
		<li><code>0</code> nếu sau nước đi chưa có <strong>người thắng</strong>,</li>
		<li><code>1</code> nếu sau nước đi <strong>player 1</strong> thắng, hoặc</li>
		<li><code>2</code> nếu sau nước đi <strong>player 2</strong> thắng.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;TicTacToe&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;]
[[3], [0, 0, 1], [0, 2, 2], [2, 2, 1], [1, 1, 2], [2, 0, 1], [1, 0, 2], [2, 1, 1]]
<strong>Đầu ra</strong>
[null, 0, 0, 0, 0, 0, 0, 1]

<strong>Giải thích</strong>
TicTacToe ticTacToe = new TicTacToe(3);
Giả sử player 1 là &quot;X&quot; và player 2 là &quot;O&quot; trên bàn cờ.
ticTacToe.move(0, 0, 1); // return 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

ticTacToe.move(0, 2, 2); // return 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

ticTacToe.move(2, 2, 1); // return 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

ticTacToe.move(1, 1, 2); // return 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

ticTacToe.move(2, 0, 1); // return 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

ticTacToe.move(1, 0, 2); // return 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

ticTacToe.move(2, 1, 1); // return 1&nbsp;(player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li>player bằng <code>1</code> hoặc <code>2</code>.</li>
	<li><code>0 &lt;= row, col &lt; n</code></li>
	<li>Mỗi lần gọi <code>move</code> phải có cặp <code>(row, col)</code> <strong>khác nhau</strong>.</li>
	<li>Sẽ có tối đa <code>n<sup>2</sup></code> lần gọi <code>move</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giảm độ phức tạp xuống thấp hơn <code>O(n<sup>2</sup>)</code> cho mỗi thao tác <code>move()</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Sau mỗi nước đi, cần kiểm tra xem có ai thắng chưa. Duyệt cả bàn cờ tốn $O(n^2)$. Điều kiện thắng là có $n$ dấu trên cùng một hàng, cột hoặc đường chéo, nên chỉ cần lưu các bộ đếm tương ứng.
>
> Mỗi người chơi lưu số dấu trên từng hàng, từng cột và hai đường chéo. Tăng các bộ đếm tương ứng sau mỗi nước đi; nếu bộ đếm nào đạt $n$ thì người chơi đó thắng. Mỗi nước đi tốn $O(1)$.

<!-- thinking:end -->

Ta có thể dùng mảng độ dài $n \times 2 + 2$ để ghi số quân của người chơi trên từng hàng, từng cột và hai đường chéo. Cần hai mảng như vậy, mỗi mảng lưu số quân của một người chơi.

Người chơi thắng khi có $n$ quân trên cùng một hàng, cột hoặc đường chéo.

Độ phức tạp thời gian của mỗi nước đi là $O(1)$. Độ phức tạp không gian là $O(n)$, với $n$ là độ dài cạnh của bàn cờ.

<!-- tabs:start -->

#### Python3

```python
class TicTacToe:

    def __init__(self, n: int):
        self.n = n
        self.cnt = [defaultdict(int), defaultdict(int)]

    def move(self, row: int, col: int, player: int) -> int:
        cur = self.cnt[player - 1]
        n = self.n
        cur[row] += 1
        cur[n + col] += 1
        if row == col:
            cur[n << 1] += 1
        if row + col == n - 1:
            cur[n << 1 | 1] += 1
        if any(cur[i] == n for i in (row, n + col, n << 1, n << 1 | 1)):
            return player
        return 0


# Your TicTacToe object will be instantiated and called as such:
# obj = TicTacToe(n)
# param_1 = obj.move(row,col,player)
```

#### Java

```java
class TicTacToe {
    private int n;
    private int[][] cnt;

    public TicTacToe(int n) {
        this.n = n;
        cnt = new int[2][(n << 1) + 2];
    }

    public int move(int row, int col, int player) {
        int[] cur = cnt[player - 1];
        ++cur[row];
        ++cur[n + col];
        if (row == col) {
            ++cur[n << 1];
        }
        if (row + col == n - 1) {
            ++cur[n << 1 | 1];
        }
        if (cur[row] == n || cur[n + col] == n || cur[n << 1] == n || cur[n << 1 | 1] == n) {
            return player;
        }
        return 0;
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```

#### C++

```cpp
class TicTacToe {
private:
    int n;
    vector<vector<int>> cnt;

public:
    TicTacToe(int n)
        : n(n)
        , cnt(2, vector<int>((n << 1) + 2, 0)) {
    }

    int move(int row, int col, int player) {
        vector<int>& cur = cnt[player - 1];
        ++cur[row];
        ++cur[n + col];
        if (row == col) {
            ++cur[n << 1];
        }
        if (row + col == n - 1) {
            ++cur[n << 1 | 1];
        }
        if (cur[row] == n || cur[n + col] == n || cur[n << 1] == n || cur[n << 1 | 1] == n) {
            return player;
        }
        return 0;
    }
};

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe* obj = new TicTacToe(n);
 * int param_1 = obj->move(row,col,player);
 */
```

#### Go

```go
type TicTacToe struct {
	n   int
	cnt [][]int
}

func Constructor(n int) TicTacToe {
	cnt := make([][]int, 2)
	for i := range cnt {
		cnt[i] = make([]int, (n<<1)+2)
	}
	return TicTacToe{n, cnt}
}

func (this *TicTacToe) Move(row int, col int, player int) int {
	cur := this.cnt[player-1]
	cur[row]++
	cur[this.n+col]++
	if row == col {
		cur[this.n<<1]++
	}
	if row+col == this.n-1 {
		cur[this.n<<1|1]++
	}
	if cur[row] == this.n || cur[this.n+col] == this.n || cur[this.n<<1] == this.n || cur[this.n<<1|1] == this.n {
		return player
	}
	return 0
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * obj := Constructor(n);
 * param_1 := obj.Move(row,col,player);
 */
```

#### TypeScript

```ts
class TicTacToe {
    private n: number;
    private cnt: number[][];

    constructor(n: number) {
        this.n = n;
        this.cnt = [Array((n << 1) + 2).fill(0), Array((n << 1) + 2).fill(0)];
    }

    move(row: number, col: number, player: number): number {
        const cur = this.cnt[player - 1];
        cur[row]++;
        cur[this.n + col]++;
        if (row === col) {
            cur[this.n << 1]++;
        }
        if (row + col === this.n - 1) {
            cur[(this.n << 1) | 1]++;
        }
        if (
            cur[row] === this.n ||
            cur[this.n + col] === this.n ||
            cur[this.n << 1] === this.n ||
            cur[(this.n << 1) | 1] === this.n
        ) {
            return player;
        }
        return 0;
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * var obj = new TicTacToe(n)
 * var param_1 = obj.move(row,col,player)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
