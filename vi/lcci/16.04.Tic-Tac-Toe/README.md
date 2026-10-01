---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.04. Tic-Tac-Toe](https://leetcode.cn/problems/tic-tac-toe-lcci)

[中文文档](/lcci/16.04.Tic-Tac-Toe/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một thuật toán để xác định xem có người chơi nào thắng một ván tic-tac-toe hay không. Đầu vào là một mảng chuỗi có kích thước N x N, gồm các ký tự &quot; &quot;, &quot;X&quot; và &quot;O&quot;, trong đó &quot; &quot; biểu thị một ô trống.</p>
<p>Luật chơi tic-tac-toe như sau:</p>
<ul>
	<li>Người chơi lần lượt đặt ký tự vào ô trống (&quot; &quot;).</li>
	<li>Người chơi thứ nhất luôn đặt ký tự &quot;O&quot;, người chơi thứ hai đặt ký tự &quot;X&quot;.</li>
	<li>Người chơi chỉ được phép đặt ký tự vào ô trống. Không được thay thế ký tự đã có.</li>
	<li>Nếu có bất kỳ hàng, cột hoặc đường chéo nào được lấp đầy bởi N ký tự giống nhau, ván chơi kết thúc. Người chơi đặt ký tự cuối cùng là người thắng.</li>
	<li>Khi không còn ô trống, ván chơi kết thúc.</li>
	<li>Khi ván chơi kết thúc, người chơi không thể đặt thêm ký tự nào.</li>
</ul>
<p>Nếu có người thắng, trả về ký tự mà người thắng đã sử dụng. Nếu hòa, trả về &quot;Draw&quot;. Nếu ván chơi chưa kết thúc và chưa có người thắng, trả về &quot;Pending&quot;.</p>
<p><strong>Ví dụ 1: </strong></p>
<pre>

<strong>Đầu vào: </strong> board = [&quot;O X&quot;,&quot; XO&quot;,&quot;X O&quot;]

<strong>Đầu ra: </strong> &quot;X&quot;

</pre>
<p><strong>Ví dụ 2: </strong></p>
<pre>

<strong>Đầu vào: </strong> board = [&quot;OOX&quot;,&quot;XXO&quot;,&quot;OXO&quot;]

<strong>Đầu ra: </strong> &quot;Draw&quot;

<strong>Giải thích: </strong> không người chơi nào thắng và không còn ô trống

</pre>
<p><strong>Ví dụ 3: </strong></p>
<pre>

<strong>Đầu vào: </strong> board = [&quot;OOX&quot;,&quot;XXO&quot;,&quot;OX &quot;]

<strong>Đầu ra: </strong> &quot;Pending&quot;

<strong>Giải thích: </strong> không người chơi nào thắng nhưng vẫn còn một ô trống

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>1 &lt;= board.length == board[i].length &lt;= 100</code></li>
	<li>Đầu vào tuân theo các luật trên.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Xác định bàn cờ đã có người thắng, hòa hay chưa kết thúc. Việc đếm lại mọi đường sau khi ván chơi kết thúc là đúng nhưng dư thừa.
>
> Gán cho `X` giá trị $+1$ và `O` giá trị $-1$; tổng tuyệt đối bằng $n$ trên một hàng, cột hoặc đường chéo nghĩa là đó là một đường đầy.
>
> Cộng dồn $rows$, $cols$, $dg$ và $udg$ trong khi duyệt bàn cờ; trả về ký tự của ô khi một giá trị tuyệt đối nào đó đạt $n$. Nếu không còn ô trống thì là hòa, nếu không thì trả về `Pending`.

<!-- thinking:end -->

Với mỗi ô, nếu đó là `X`, ta cộng $1$ vào bộ đếm; nếu đó là `O`, ta trừ $1$ khỏi bộ đếm. Khi giá trị tuyệt đối của bộ đếm một hàng, cột hoặc đường chéo bằng $n$, điều đó có nghĩa là người chơi hiện tại đã đặt $n$ ký tự giống nhau trên hàng, cột hoặc đường chéo đó và ván chơi kết thúc. Ta có thể trả về ký tự tương ứng.

Cụ thể, ta sử dụng hai mảng một chiều $rows$ và $cols$ có độ dài $n$ để biểu diễn số lượng ký tự trên mỗi hàng và cột, đồng thời dùng $dg$ và $udg$ để biểu diễn số lượng ký tự trên hai đường chéo. Khi người chơi đặt một ký tự tại $(i, j)$, ta cập nhật các phần tử tương ứng trong các mảng $rows$, $cols$, $dg$ và $udg$ dựa trên việc ký tự đó là `X` hay `O`. Sau mỗi lần cập nhật, ta kiểm tra xem giá trị tuyệt đối của phần tử tương ứng có bằng $n$ hay không. Nếu có, điều đó có nghĩa là người chơi hiện tại đã đặt $n$ ký tự giống nhau trên hàng, cột hoặc đường chéo đó và ván chơi kết thúc. Ta có thể trả về ký tự tương ứng.

Cuối cùng, ta duyệt toàn bộ bàn cờ. Nếu có ký tự ` `, điều đó có nghĩa là ván chơi vẫn chưa kết thúc, và ta trả về `Pending`. Nếu không, ta trả về `Draw`.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài cạnh của bàn cờ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tictactoe(self, board: List[str]) -> str:
        n = len(board)
        rows = [0] * n
        cols = [0] * n
        dg = udg = 0
        has_empty_grid = False
        for i, row in enumerate(board):
            for j, c in enumerate(row):
                v = 1 if c == 'X' else -1
                if c == ' ':
                    has_empty_grid = True
                    v = 0
                rows[i] += v
                cols[j] += v
                if i == j:
                    dg += v
                if i + j + 1 == n:
                    udg += v
                if (
                    abs(rows[i]) == n
                    or abs(cols[j]) == n
                    or abs(dg) == n
                    or abs(udg) == n
                ):
                    return c
        return 'Pending' if has_empty_grid else 'Draw'
```

#### Java

```java
class Solution {
    public String tictactoe(String[] board) {
        int n = board.length;
        int[] rows = new int[n];
        int[] cols = new int[n];
        int dg = 0, udg = 0;
        boolean hasEmptyGrid = false;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                char c = board[i].charAt(j);
                if (c == ' ') {
                    hasEmptyGrid = true;
                    continue;
                }
                int v = c == 'X' ? 1 : -1;
                rows[i] += v;
                cols[j] += v;
                if (i == j) {
                    dg += v;
                }
                if (i + j + 1 == n) {
                    udg += v;
                }
                if (Math.abs(rows[i]) == n || Math.abs(cols[j]) == n || Math.abs(dg) == n
                    || Math.abs(udg) == n) {
                    return String.valueOf(c);
                }
            }
        }
        return hasEmptyGrid ? "Pending" : "Draw";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string tictactoe(vector<string>& board) {
        int n = board.size();
        vector<int> rows(n), cols(n);
        int dg = 0, udg = 0;
        bool hasEmptyGrid = false;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                char c = board[i][j];
                if (c == ' ') {
                    hasEmptyGrid = true;
                    continue;
                }
                int v = c == 'X' ? 1 : -1;
                rows[i] += v;
                cols[j] += v;
                if (i == j) {
                    dg += v;
                }
                if (i + j + 1 == n) {
                    udg += v;
                }
                if (abs(rows[i]) == n || abs(cols[j]) == n || abs(dg) == n || abs(udg) == n) {
                    return string(1, c);
                }
            }
        }
        return hasEmptyGrid ? "Pending" : "Draw";
    }
};
```

#### Go

```go
func tictactoe(board []string) string {
	n := len(board)
	rows := make([]int, n)
	cols := make([]int, n)
	dg, udg := 0, 0
	hasEmptyGrid := false
	for i, row := range board {
		for j, c := range row {
			if c == ' ' {
				hasEmptyGrid = true
				continue
			}
			v := 1
			if c == 'O' {
				v = -1
			}
			rows[i] += v
			cols[j] += v
			if i == j {
				dg += v
			}
			if i+j == n-1 {
				udg += v
			}
			if abs(rows[i]) == n || abs(cols[j]) == n || abs(dg) == n || abs(udg) == n {
				return string(c)
			}
		}
	}
	if hasEmptyGrid {
		return "Pending"
	}
	return "Draw"
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
function tictactoe(board: string[]): string {
    const n = board.length;
    const rows = Array(n).fill(0);
    const cols = Array(n).fill(0);
    let [dg, udg] = [0, 0];
    let hasEmptyGrid = false;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            const c = board[i][j];
            if (c === ' ') {
                hasEmptyGrid = true;
                continue;
            }
            const v = c === 'X' ? 1 : -1;
            rows[i] += v;
            cols[j] += v;
            if (i === j) {
                dg += v;
            }
            if (i + j === n - 1) {
                udg += v;
            }
            if (
                Math.abs(rows[i]) === n ||
                Math.abs(cols[j]) === n ||
                Math.abs(dg) === n ||
                Math.abs(udg) === n
            ) {
                return c;
            }
        }
    }
    return hasEmptyGrid ? 'Pending' : 'Draw';
}
```

#### Swift

```swift
class Solution {
    func tictactoe(_ board: [String]) -> String {
        let n = board.count
        var rows = Array(repeating: 0, count: n)
        var cols = Array(repeating: 0, count: n)
        var diagonal = 0, antiDiagonal = 0
        var hasEmptyGrid = false

        for i in 0..<n {
            for j in 0..<n {
                let c = Array(board[i])[j]
                if c == " " {
                    hasEmptyGrid = true
                    continue
                }
                let value = c == "X" ? 1 : -1
                rows[i] += value
                cols[j] += value
                if i == j {
                    diagonal += value
                }
                if i + j == n - 1 {
                    antiDiagonal += value
                }
                if abs(rows[i]) == n || abs(cols[j]) == n || abs(diagonal) == n || abs(antiDiagonal) == n {
                    return String(c)
                }
            }
        }

        return hasEmptyGrid ? "Pending" : "Draw"
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
