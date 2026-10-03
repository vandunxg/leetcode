---
comments: true
difficulty: Medium
rating: 1658
source: Biweekly Contest 58 Q2
tags:
    - Array
    - Enumeration
    - Matrix
---

<!-- problem:start -->

# [1958. Check if Move is Legal](https://leetcode.com/problems/check-if-move-is-legal)

[中文文档](/solution/1900-1999/1958.Check%20if%20Move%20is%20Legal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một bàn cờ <strong>được đánh chỉ số từ 0</strong> có kích thước <code>8 x 8</code>, tên là <code>board</code>, trong đó <code>board[r][c]</code> biểu diễn ô <code>(r, c)</code> trên bàn cờ. Trên bàn cờ, các ô trống được biểu diễn bằng <code>&#39;.&#39;</code>, các ô trắng được biểu diễn bằng <code>&#39;W&#39;</code>, và các ô đen được biểu diễn bằng <code>&#39;B&#39;</code>.</p>

<p>Mỗi nước đi trong trò chơi này gồm việc chọn một ô trống và đổi ô đó thành màu mà bạn đang chơi (trắng hoặc đen). Tuy nhiên, một nước đi chỉ <strong>hợp lệ</strong> nếu sau khi đổi, ô đó trở thành <strong>đầu mút của một đường tốt</strong> (ngang, dọc hoặc chéo).</p>

<p><strong>Đường tốt</strong> là một đường gồm <strong>từ ba ô trở lên (bao gồm cả hai đầu mút)</strong>, trong đó hai đầu mút có <strong>cùng một màu</strong>, còn các ô ở giữa có <strong>màu đối diện</strong> (không có ô trống nào trên đường). Hình dưới đây minh họa một số đường tốt:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1958.Check%20if%20Move%20is%20Legal/images/goodlines5.png" style="width: 500px; height: 312px;" />
<p>Cho hai số nguyên <code>rMove</code> và <code>cMove</code>, cùng một ký tự <code>color</code> biểu diễn màu bạn đang chơi (trắng hoặc đen), hãy trả về <code>true</code> <em>nếu đổi ô </em><code>(rMove, cMove)</code> <em>thành màu</em> <code>color</code> <em>là một nước đi <strong>hợp lệ</strong>, hoặc </em><code>false</code><em> nếu không hợp lệ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1958.Check%20if%20Move%20is%20Legal/images/grid11.png" style="width: 350px; height: 350px;" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;B&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;W&quot;,&quot;B&quot;,&quot;B&quot;,&quot;.&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;B&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;B&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;B&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;]], rMove = 4, cMove = 3, color = &quot;B&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> &#39;.&#39;, &#39;W&#39; và &#39;B&#39; lần lượt được biểu diễn bằng các màu xanh dương, trắng và đen, còn ô (rMove, cMove) được đánh dấu bằng &#39;X&#39;.
Hai đường tốt có ô được chọn làm một đầu mút được đánh dấu bằng các hình chữ nhật màu đỏ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1958.Check%20if%20Move%20is%20Legal/images/grid2.png" style="width: 350px; height: 351px;" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;B&quot;,&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;B&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;B&quot;,&quot;W&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;W&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;B&quot;]], rMove = 4, cMove = 4, color = &quot;W&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có các đường tốt mà ô được chọn nằm ở giữa, nhưng không có đường tốt nào mà ô được chọn là đầu mút.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>board.length == board[r].length == 8</code></li>
	<li><code>0 &lt;= rMove, cMove &lt; 8</code></li>
	<li><code>board[rMove][cMove] == &#39;.&#39;</code></li>
	<li><code>color</code> là <code>&#39;B&#39;</code> hoặc <code>&#39;W&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một nước đi hợp lệ cần một đoạn tốt: màu được chơi, một hoặc nhiều ô có màu đối diện, rồi đến ô cùng màu. Bàn cờ có kích thước $8\times 8$, nên chỉ cần kiểm tra tám tia.
>
> Di chuyển từ ô cần đi và đếm số bước. Nếu gặp ô trống hoặc ô cùng màu quá sớm thì dừng tia đó; nếu đi hơn một bước rồi gặp ô cùng màu thì tìm thấy kết quả.
>
> Ta không lật các quân cờ.

<!-- thinking:end -->

Ta liệt kê tất cả các hướng có thể. Với mỗi hướng $(a, b)$, ta bắt đầu từ $(\textit{rMove}, \textit{cMove})$ và dùng biến $\textit{cnt}$ để ghi nhận số ô đã đi qua. Nếu trong quá trình duyệt, ta gặp một ô có màu $\textit{color}$ và $\textit{cnt} > 1$, nghĩa là đã tìm thấy một đoạn đường tốt, khi đó trả về $\textit{true}$.

Nếu sau khi liệt kê không tìm thấy đoạn đường tốt nào, ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ là số hàng và $n$ là số cột của $\textit{board}$, với $m = n = 8$ trong bài toán này. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkMove(
        self, board: List[List[str]], rMove: int, cMove: int, color: str
    ) -> bool:
        for a in range(-1, 2):
            for b in range(-1, 2):
                if a == 0 and b == 0:
                    continue
                i, j = rMove, cMove
                cnt = 0
                while 0 <= i + a < 8 and 0 <= j + b < 8:
                    cnt += 1
                    i, j = i + a, j + b
                    if cnt > 1 and board[i][j] == color:
                        return True
                    if board[i][j] in (color, "."):
                        break
        return False
```

#### Java

```java
class Solution {
    public boolean checkMove(char[][] board, int rMove, int cMove, char color) {
        for (int a = -1; a <= 1; ++a) {
            for (int b = -1; b <= 1; ++b) {
                if (a == 0 && b == 0) {
                    continue;
                }
                int i = rMove, j = cMove;
                int cnt = 0;
                while (0 <= i + a && i + a < 8 && 0 <= j + b && j + b < 8) {
                    i += a;
                    j += b;
                    if (++cnt > 1 && board[i][j] == color) {
                        return true;
                    }
                    if (board[i][j] == color || board[i][j] == '.') {
                        break;
                    }
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkMove(vector<vector<char>>& board, int rMove, int cMove, char color) {
        for (int a = -1; a <= 1; ++a) {
            for (int b = -1; b <= 1; ++b) {
                if (a == 0 && b == 0) {
                    continue;
                }
                int i = rMove, j = cMove;
                int cnt = 0;
                while (0 <= i + a && i + a < 8 && 0 <= j + b && j + b < 8) {
                    i += a;
                    j += b;
                    if (++cnt > 1 && board[i][j] == color) {
                        return true;
                    }
                    if (board[i][j] == color || board[i][j] == '.') {
                        break;
                    }
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func checkMove(board [][]byte, rMove int, cMove int, color byte) bool {
	for a := -1; a <= 1; a++ {
		for b := -1; b <= 1; b++ {
			if a == 0 && b == 0 {
				continue
			}
			i, j := rMove, cMove
			cnt := 0
			for 0 <= i+a && i+a < 8 && 0 <= j+b && j+b < 8 {
				i += a
				j += b
				cnt++
				if cnt > 1 && board[i][j] == color {
					return true
				}
				if board[i][j] == color || board[i][j] == '.' {
					break
				}
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function checkMove(board: string[][], rMove: number, cMove: number, color: string): boolean {
    for (let a = -1; a <= 1; ++a) {
        for (let b = -1; b <= 1; ++b) {
            if (a === 0 && b === 0) {
                continue;
            }
            let [i, j] = [rMove, cMove];
            let cnt = 0;
            while (0 <= i + a && i + a < 8 && 0 <= j + b && j + b < 8) {
                i += a;
                j += b;
                if (++cnt > 1 && board[i][j] === color) {
                    return true;
                }
                if (board[i][j] === color || board[i][j] === '.') {
                    break;
                }
            }
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
