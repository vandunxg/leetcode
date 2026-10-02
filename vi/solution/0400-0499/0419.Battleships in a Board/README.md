---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [419. Battleships in a Board](https://leetcode.com/problems/battleships-in-a-board)

[中文文档](/solution/0400-0499/0419.Battleships%20in%20a%20Board/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>board</code>, trong đó mỗi ô là một phần của tàu chiến <code>&#39;X&#39;</code> hoặc ô trống <code>&#39;.&#39;</code>. Hãy trả về <em>số lượng <strong>tàu chiến</strong> trên</em> <code>board</code>.</p>

<p><strong>Tàu chiến</strong> chỉ có thể nằm ngang hoặc dọc trên <code>board</code>. Nói cách khác, tàu chỉ có thể có dạng <code>1 x k</code> (<code>1</code> hàng, <code>k</code> cột) hoặc <code>k x 1</code> (<code>k</code> hàng, <code>1</code> cột), với <code>k</code> có thể là bất kỳ giá trị nào. Giữa hai tàu chiến luôn có ít nhất một ô trống theo chiều ngang hoặc chiều dọc (tức là không có hai tàu nào nằm kề nhau).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img height="333" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0419.Battleships%20in%20a%20Board/images/image.png" width="333" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot;X&quot;,&quot;.&quot;,&quot;.&quot;,&quot;X&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;X&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;X&quot;]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> board = [[&quot;.&quot;]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>board[i][j]</code> chỉ có thể là <code>&#39;.&#39;</code> hoặc <code>&#39;X&#39;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này trong một lượt, chỉ dùng thêm <code>O(1)</code> bộ nhớ và không thay đổi các giá trị trong <code>board</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Các tàu nằm ngang hoặc dọc và không chạm nhau. Có thể dùng flood fill để đánh dấu toàn bộ một tàu, nhưng cách đó sẽ sửa board hoặc cần thêm cờ đánh dấu. Câu hỏi mở rộng yêu cầu duyệt một lượt và dùng bộ nhớ phụ hằng số.
>
> Mỗi tàu có đúng một ô $\texttt{X}$ ở góc trên bên trái: ô phía trên và ô bên trái đều không phải $\texttt{X}$. Đếm các ô góc này.
>
> Vì các tàu không chạm nhau nên mỗi góc như vậy là duy nhất; do đó không bị đếm thiếu hay đếm trùng.

<!-- thinking:end -->

Duyệt ma trận và tìm góc trên bên trái của mỗi tàu chiến, tức vị trí hiện tại là `X` còn ô phía trên và ô bên trái đều không phải `X`; mỗi khi tìm thấy thì tăng đáp án lên một.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBattleships(self, board: List[List[str]]) -> int:
        m, n = len(board), len(board[0])
        ans = 0
        for i in range(m):
            for j in range(n):
                if board[i][j] == '.':
                    continue
                if i > 0 and board[i - 1][j] == 'X':
                    continue
                if j > 0 and board[i][j - 1] == 'X':
                    continue
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countBattleships(char[][] board) {
        int m = board.length, n = board[0].length;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == '.') {
                    continue;
                }
                if (i > 0 && board[i - 1][j] == 'X') {
                    continue;
                }
                if (j > 0 && board[i][j - 1] == 'X') {
                    continue;
                }
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int m = board.size(), n = board[0].size();
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == '.') {
                    continue;
                }
                if (i > 0 && board[i - 1][j] == 'X') {
                    continue;
                }
                if (j > 0 && board[i][j - 1] == 'X') {
                    continue;
                }
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countBattleships(board [][]byte) (ans int) {
	for i, row := range board {
		for j, c := range row {
			if c == '.' {
				continue
			}
			if i > 0 && board[i-1][j] == 'X' {
				continue
			}
			if j > 0 && board[i][j-1] == 'X' {
				continue
			}
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countBattleships(board: string[][]): number {
    const m = board.length;
    const n = board[0].length;
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (board[i][j] === '.') {
                continue;
            }
            if (i && board[i - 1][j] === 'X') {
                continue;
            }
            if (j && board[i][j - 1] === 'X') {
                continue;
            }
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
