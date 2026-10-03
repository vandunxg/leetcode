---
comments: true
difficulty: Medium
rating: 1929
source: Weekly Contest 260 Q3
tags:
    - Array
    - Enumeration
    - Matrix
---

<!-- problem:start -->

# [2018. Check if Word Can Be Placed In Crossword](https://leetcode.com/problems/check-if-word-can-be-placed-in-crossword)

[中文文档](/solution/2000-2099/2018.Check%20if%20Word%20Can%20Be%20Placed%20In%20Crossword/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>m x n</code> <code>board</code>, biểu diễn trạng thái <strong>hiện tại</strong> của một trò chơi ô chữ. Trò chơi chứa các chữ cái tiếng Anh viết thường (từ những từ đã giải), <code>&#39; &#39;</code> biểu diễn các ô <strong>trống</strong>, và <code>&#39;#&#39;</code> biểu diễn các ô <strong>bị chặn</strong>.</p>

<p>Một từ có thể được đặt theo chiều <strong>ngang</strong> (từ trái sang phải <strong>hoặc</strong> từ phải sang trái) hoặc theo chiều <strong>dọc</strong> (từ trên xuống dưới <strong>hoặc</strong> từ dưới lên trên) trên bảng nếu:</p>

<ul>
	<li>Từ đó không chiếm một ô chứa ký tự <code>&#39;#&#39;</code>.</li>
	<li>Ô được đặt mỗi chữ cái phải là <code>&#39; &#39;</code> (trống) hoặc <strong>trùng khớp</strong> với chữ cái đã có trên <code>board</code>.</li>
	<li>Nếu từ được đặt theo chiều <strong>ngang</strong>, không được có ô trống <code>&#39; &#39;</code> hoặc chữ cái viết thường khác nào nằm <strong>ngay bên trái hoặc bên phải</strong><strong> </strong>của từ.</li>
	<li>Nếu từ được đặt theo chiều <strong>dọc</strong>, không được có ô trống <code>&#39; &#39;</code> hoặc chữ cái viết thường khác nào nằm <strong>ngay phía trên hoặc phía dưới</strong> của từ.</li>
</ul>

<p>Cho một chuỗi <code>word</code>, hãy trả về <code>true</code><em> nếu </em><code>word</code><em> có thể được đặt vào </em><code>board</code><em>, hoặc </em><code>false</code><em> <strong>nếu không</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2018.Check%20if%20Word%20Can%20Be%20Placed%20In%20Crossword/images/crossword-ex1-1.png" style="width: 478px; height: 180px;" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot;#&quot;, &quot; &quot;, &quot;#&quot;], [&quot; &quot;, &quot; &quot;, &quot;#&quot;], [&quot;#&quot;, &quot;c&quot;, &quot; &quot;]], word = &quot;abc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Từ &quot;abc&quot; có thể được đặt như hình minh họa ở trên (từ trên xuống dưới).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2018.Check%20if%20Word%20Can%20Be%20Placed%20In%20Crossword/images/crossword-ex2-1.png" style="width: 180px; height: 180px;" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot; &quot;, &quot;#&quot;, &quot;a&quot;], [&quot; &quot;, &quot;#&quot;, &quot;c&quot;], [&quot; &quot;, &quot;#&quot;, &quot;a&quot;]], word = &quot;ac&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể đặt từ này vì luôn có một ô trống/chữ cái ở phía trên hoặc phía dưới nó.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2018.Check%20if%20Word%20Can%20Be%20Placed%20In%20Crossword/images/crossword-ex3-1.png" style="width: 478px; height: 180px;" />
<pre>
<strong>Đầu vào:</strong> board = [[&quot;#&quot;, &quot; &quot;, &quot;#&quot;], [&quot; &quot;, &quot; &quot;, &quot;#&quot;], [&quot;#&quot;, &quot; &quot;, &quot;c&quot;]], word = &quot;ca&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Từ &quot;ca&quot; có thể được đặt như hình minh họa ở trên (từ phải sang trái).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>1 &lt;= m * n &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>board[i][j]</code> sẽ là <code>&#39; &#39;</code>, <code>&#39;#&#39;</code> hoặc một chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= word.length &lt;= max(m, n)</code></li>
	<li><code>word</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với $mn \le 2 \times 10^5$, từ cần nằm trong một đoạn được giới hạn bởi `#`, theo chiều thuận hoặc ngược. Thử bốn hướng từ mỗi ô có độ phức tạp tuyến tính theo kích thước lưới nhân với $|word|$.
>
> Điểm bắt đầu phải nằm ở biên hoặc cạnh `#`. `check` cũng yêu cầu ô ngay sau từ nằm ngoài bảng hoặc là `#`, đồng thời các chữ cái phải trùng khớp hoặc là ô trống.
>
> Chỉ cần một hướng hợp lệ là trả về true.

<!-- thinking:end -->

Ta có thể liệt kê từng vị trí $(i, j)$ trong ma trận, rồi kiểm tra xem có thể đặt từ `word` từ trái sang phải, từ phải sang trái, từ trên xuống dưới hoặc từ dưới lên trên bắt đầu tại vị trí này hay không.

Vị trí này phải thỏa mãn các điều kiện sau để được dùng làm điểm bắt đầu:

1. Nếu từ `word` được đặt từ trái sang phải, vị trí này phải nằm ở biên trái, hoặc ô `board[i][j - 1]` bên trái vị trí này phải là `'#'`.
2. Nếu từ `word` được đặt từ phải sang trái, vị trí này phải nằm ở biên phải, hoặc ô `board[i][j + 1]` bên phải vị trí này phải là `'#'`.
3. Nếu từ `word` được đặt từ trên xuống dưới, vị trí này phải nằm ở biên trên, hoặc ô `board[i - 1][j]` phía trên vị trí này phải là `'#'`.
4. Nếu từ `word` được đặt từ dưới lên trên, vị trí này phải nằm ở biên dưới, hoặc ô `board[i + 1][j]` phía dưới vị trí này phải là `'#'`.

Với các điều kiện trên, ta có thể bắt đầu từ vị trí này và kiểm tra xem có thể đặt từ `word` hay không. Ta xây dựng hàm $check(i, j, a, b)$, biểu diễn việc đặt từ `word` từ vị trí $(i, j)$ theo hướng $(a, b)$ có hợp lệ hay không. Nếu hợp lệ, trả về `true`, ngược lại trả về `false`.

Cách cài đặt hàm $check(i, j, a, b)$ như sau:

Trước tiên, ta tìm vị trí biên còn lại $(x, y)$ theo hướng hiện tại, tức là $(x, y) = (i + a \times k, j + b \times k)$, trong đó $k$ là độ dài của từ `word`. Nếu $(x, y)$ nằm trong ma trận và ô tại $(x, y)$ không phải là `'#'`, điều đó có nghĩa là vị trí biên còn lại theo hướng hiện tại không phải `'#'`, nên không thể đặt từ `word` và trả về `false`.

Sau đó, ta bắt đầu từ vị trí $(i, j)$ và duyệt từ `word` theo hướng $(a, b)$. Nếu gặp ô `board[i][j]` không phải là ô trống hoặc không trùng với ký tự hiện tại của từ `word`, điều đó có nghĩa là không thể đặt từ `word`, nên trả về `false`. Nếu duyệt hết từ `word`, nghĩa là có thể đặt từ `word`, khi đó trả về `true`.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(1)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def placeWordInCrossword(self, board: List[List[str]], word: str) -> bool:
        def check(i, j, a, b):
            x, y = i + a * k, j + b * k
            if 0 <= x < m and 0 <= y < n and board[x][y] != '#':
                return False
            for c in word:
                if (
                    i < 0
                    or i >= m
                    or j < 0
                    or j >= n
                    or (board[i][j] != ' ' and board[i][j] != c)
                ):
                    return False
                i, j = i + a, j + b
            return True

        m, n = len(board), len(board[0])
        k = len(word)
        for i in range(m):
            for j in range(n):
                left_to_right = (j == 0 or board[i][j - 1] == '#') and check(i, j, 0, 1)
                right_to_left = (j == n - 1 or board[i][j + 1] == '#') and check(
                    i, j, 0, -1
                )
                up_to_down = (i == 0 or board[i - 1][j] == '#') and check(i, j, 1, 0)
                down_to_up = (i == m - 1 or board[i + 1][j] == '#') and check(
                    i, j, -1, 0
                )
                if left_to_right or right_to_left or up_to_down or down_to_up:
                    return True
        return False
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private char[][] board;
    private String word;
    private int k;

    public boolean placeWordInCrossword(char[][] board, String word) {
        m = board.length;
        n = board[0].length;
        this.board = board;
        this.word = word;
        k = word.length();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                boolean leftToRight = (j == 0 || board[i][j - 1] == '#') && check(i, j, 0, 1);
                boolean rightToLeft = (j == n - 1 || board[i][j + 1] == '#') && check(i, j, 0, -1);
                boolean upToDown = (i == 0 || board[i - 1][j] == '#') && check(i, j, 1, 0);
                boolean downToUp = (i == m - 1 || board[i + 1][j] == '#') && check(i, j, -1, 0);
                if (leftToRight || rightToLeft || upToDown || downToUp) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean check(int i, int j, int a, int b) {
        int x = i + a * k, y = j + b * k;
        if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] != '#') {
            return false;
        }
        for (int p = 0; p < k; ++p) {
            if (i < 0 || i >= m || j < 0 || j >= n
                || (board[i][j] != ' ' && board[i][j] != word.charAt(p))) {
                return false;
            }
            i += a;
            j += b;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool placeWordInCrossword(vector<vector<char>>& board, string word) {
        int m = board.size(), n = board[0].size();
        int k = word.size();
        auto check = [&](int i, int j, int a, int b) {
            int x = i + a * k, y = j + b * k;
            if (x >= 0 && x < m && y >= 0 && y < n && board[x][y] != '#') {
                return false;
            }
            for (char& c : word) {
                if (i < 0 || i >= m || j < 0 || j >= n || (board[i][j] != ' ' && board[i][j] != c)) {
                    return false;
                }
                i += a;
                j += b;
            }
            return true;
        };
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                bool leftToRight = (j == 0 || board[i][j - 1] == '#') && check(i, j, 0, 1);
                bool rightToLeft = (j == n - 1 || board[i][j + 1] == '#') && check(i, j, 0, -1);
                bool upToDown = (i == 0 || board[i - 1][j] == '#') && check(i, j, 1, 0);
                bool downToUp = (i == m - 1 || board[i + 1][j] == '#') && check(i, j, -1, 0);
                if (leftToRight || rightToLeft || upToDown || downToUp) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func placeWordInCrossword(board [][]byte, word string) bool {
	m, n := len(board), len(board[0])
	k := len(word)
	check := func(i, j, a, b int) bool {
		x, y := i+a*k, j+b*k
		if x >= 0 && x < m && y >= 0 && y < n && board[x][y] != '#' {
			return false
		}
		for _, c := range word {
			if i < 0 || i >= m || j < 0 || j >= n || (board[i][j] != ' ' && board[i][j] != byte(c)) {
				return false
			}
			i, j = i+a, j+b
		}
		return true
	}
	for i := range board {
		for j := range board[i] {
			leftToRight := (j == 0 || board[i][j-1] == '#') && check(i, j, 0, 1)
			rightToLeft := (j == n-1 || board[i][j+1] == '#') && check(i, j, 0, -1)
			upToDown := (i == 0 || board[i-1][j] == '#') && check(i, j, 1, 0)
			downToUp := (i == m-1 || board[i+1][j] == '#') && check(i, j, -1, 0)
			if leftToRight || rightToLeft || upToDown || downToUp {
				return true
			}
		}
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
