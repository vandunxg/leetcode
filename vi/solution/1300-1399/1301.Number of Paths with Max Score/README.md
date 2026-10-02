---
comments: true
difficulty: Hard
rating: 1853
source: Biweekly Contest 16 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1301. Number of Paths with Max Score](https://leetcode.com/problems/number-of-paths-with-max-score)

[中文文档](/solution/1300-1399/1301.Number%20of%20Paths%20with%20Max%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>board</code> vuông gồm các ký tự. Bạn bắt đầu di chuyển từ ô dưới cùng bên phải, được đánh dấu bằng ký tự <code>&#39;S&#39;</code>.</p>

<p>Bạn cần đến ô trên cùng bên trái, được đánh dấu bằng ký tự <code>&#39;E&#39;</code>. Các ô còn lại được gán một chữ số <code>1, 2, ..., 9</code> hoặc là chướng ngại vật <code>&#39;X&#39;</code>. Mỗi bước, bạn chỉ có thể đi lên, sang trái hoặc chéo lên-trái nếu ô đó không có chướng ngại vật.</p>

<p>Trả về danh sách gồm hai số nguyên: số thứ nhất là tổng lớn nhất của các chữ số có thể thu thập, số thứ hai là số đường đi đạt được tổng lớn nhất đó, <strong>lấy modulo <code>10^9 + 7</code></strong>.</p>

<p>Nếu không có đường đi, trả về <code>[0, 0]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> board = ["E23","2X2","12S"]
<strong>Đầu ra:</strong> [7,1]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> board = ["E12","1X1","21S"]
<strong>Đầu ra:</strong> [4,2]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> board = ["E11","XXX","11S"]
<strong>Đầu ra:</strong> [0,0]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= board.length == board[i].length &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Từ góc dưới bên phải, ta có thể đi sang trái, lên trên hoặc chéo lên-trái. Với $n \le 100$, không thể liệt kê mọi đường đi để so sánh điểm số. Nhiều đường đi đi qua cùng một ô, và ta cần tính cả điểm tối đa lẫn số cách đạt được điểm đó.
>
> Vì vậy, ta tính từ dưới lên. Gọi $f[i][j]$ là điểm cao nhất để đến $(i,j)$ và $g[i][j]$ là số đường đi đạt điểm đó. Ta có thể đến ô $(i,j)$ từ $(i+1,j)$, $(i,j+1)$ hoặc $(i+1,j+1)$: nếu điểm từ ô trước lớn hơn thì thay thế số cách; nếu bằng nhau thì cộng số cách. Chướng ngại vật và ô xuất phát không đóng góp chữ số; số cách được lấy modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là điểm cao nhất có thể đạt được khi đi từ điểm xuất phát $(n - 1, n - 1)$ đến $(i, j)$, còn $g[i][j]$ là số cách đạt điểm cao nhất đó. Ban đầu, $f[n - 1][n - 1] = 0$ và $g[n - 1][n - 1] = 1$. Các vị trí khác của $f[i][j]$ được gán $-1$, còn $g[i][j]$ được gán $0$.

Từ ba vị trí $(i + 1, j)$, $(i, j + 1)$ và $(i + 1, j + 1)$, ta chuyển trạng thái để cập nhật $f[i][j]$ và $g[i][j]$. Nếu ô hiện tại $(i, j)$ là chướng ngại vật, là điểm xuất phát hoặc vị trí tiền nhiệm nằm ngoài board thì không cập nhật. Ngược lại, nếu một vị trí $(x, y)$ có $f[x][y] \gt f[i][j]$, ta cập nhật $f[i][j] = f[x][y]$ và $g[i][j] = g[x][y]$. Nếu $f[x][y] = f[i][j]$, ta cộng thêm số cách: $g[i][j] = g[i][j] + g[x][y]$. Cuối cùng, nếu ô hiện tại $(i, j)$ có thể đến được và chứa chữ số, ta cập nhật $f[i][j] = f[i][j] + board[i][j]$.

Cuối cùng, nếu $f[0][0] \lt 0$, nghĩa là không có đường đến đích, nên trả về $[0, 0]$. Nếu không, trả về $[f[0][0], g[0][0]]$. Lưu ý kết quả cần lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của board.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pathsWithMaxScore(self, board: List[str]) -> List[int]:
        def update(i, j, x, y):
            if x >= n or y >= n or f[x][y] == -1 or board[i][j] in "XS":
                return
            if f[x][y] > f[i][j]:
                f[i][j] = f[x][y]
                g[i][j] = g[x][y]
            elif f[x][y] == f[i][j]:
                g[i][j] += g[x][y]

        n = len(board)
        f = [[-1] * n for _ in range(n)]
        g = [[0] * n for _ in range(n)]
        f[-1][-1], g[-1][-1] = 0, 1
        for i in range(n - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                update(i, j, i + 1, j)
                update(i, j, i, j + 1)
                update(i, j, i + 1, j + 1)
                if f[i][j] != -1 and board[i][j].isdigit():
                    f[i][j] += int(board[i][j])
        mod = 10**9 + 7
        return [0, 0] if f[0][0] == -1 else [f[0][0], g[0][0] % mod]
```

#### Java

```java
class Solution {
    private List<String> board;
    private int n;
    private int[][] f;
    private int[][] g;
    private final int mod = (int) 1e9 + 7;

    public int[] pathsWithMaxScore(List<String> board) {
        n = board.size();
        this.board = board;
        f = new int[n][n];
        g = new int[n][n];
        for (var e : f) {
            Arrays.fill(e, -1);
        }
        f[n - 1][n - 1] = 0;
        g[n - 1][n - 1] = 1;
        for (int i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                update(i, j, i + 1, j);
                update(i, j, i, j + 1);
                update(i, j, i + 1, j + 1);
                if (f[i][j] != -1) {
                    char c = board.get(i).charAt(j);
                    if (c >= '0' && c <= '9') {
                        f[i][j] += (c - '0');
                    }
                }
            }
        }
        int[] ans = new int[2];
        if (f[0][0] != -1) {
            ans[0] = f[0][0];
            ans[1] = g[0][0];
        }
        return ans;
    }

    private void update(int i, int j, int x, int y) {
        if (x >= n || y >= n || f[x][y] == -1 || board.get(i).charAt(j) == 'X'
            || board.get(i).charAt(j) == 'S') {
            return;
        }
        if (f[x][y] > f[i][j]) {
            f[i][j] = f[x][y];
            g[i][j] = g[x][y];
        } else if (f[x][y] == f[i][j]) {
            g[i][j] = (g[i][j] + g[x][y]) % mod;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> pathsWithMaxScore(vector<string>& board) {
        int n = board.size();
        const int mod = 1e9 + 7;
        int f[n][n];
        int g[n][n];
        memset(f, -1, sizeof(f));
        memset(g, 0, sizeof(g));
        f[n - 1][n - 1] = 0;
        g[n - 1][n - 1] = 1;

        auto update = [&](int i, int j, int x, int y) {
            if (x >= n || y >= n || f[x][y] == -1 || board[i][j] == 'X' || board[i][j] == 'S') {
                return;
            }
            if (f[x][y] > f[i][j]) {
                f[i][j] = f[x][y];
                g[i][j] = g[x][y];
            } else if (f[x][y] == f[i][j]) {
                g[i][j] = (g[i][j] + g[x][y]) % mod;
            }
        };

        for (int i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                update(i, j, i + 1, j);
                update(i, j, i, j + 1);
                update(i, j, i + 1, j + 1);
                if (f[i][j] != -1) {
                    if (board[i][j] >= '0' && board[i][j] <= '9') {
                        f[i][j] += (board[i][j] - '0');
                    }
                }
            }
        }
        vector<int> ans(2);
        if (f[0][0] != -1) {
            ans[0] = f[0][0];
            ans[1] = g[0][0];
        }
        return ans;
    }
};
```

#### Go

```go
func pathsWithMaxScore(board []string) []int {
	n := len(board)
	f := make([][]int, n)
	g := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		g[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	f[n-1][n-1] = 0
	g[n-1][n-1] = 1
	const mod = 1e9 + 7

	update := func(i, j, x, y int) {
		if x >= n || y >= n || f[x][y] == -1 || board[i][j] == 'X' || board[i][j] == 'S' {
			return
		}
		if f[x][y] > f[i][j] {
			f[i][j] = f[x][y]
			g[i][j] = g[x][y]
		} else if f[x][y] == f[i][j] {
			g[i][j] = (g[i][j] + g[x][y]) % mod
		}
	}
	for i := n - 1; i >= 0; i-- {
		for j := n - 1; j >= 0; j-- {
			update(i, j, i+1, j)
			update(i, j, i, j+1)
			update(i, j, i+1, j+1)
			if f[i][j] != -1 && board[i][j] >= '0' && board[i][j] <= '9' {
				f[i][j] += int(board[i][j] - '0')
			}
		}
	}
	ans := make([]int, 2)
	if f[0][0] != -1 {
		ans[0], ans[1] = f[0][0], g[0][0]
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
