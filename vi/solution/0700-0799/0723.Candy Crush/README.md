---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [723. Candy Crush 🔒](https://leetcode.com/problems/candy-crush)

[中文文档](/solution/0700-0799/0723.Candy%20Crush/README.md)

## Mô tả

<!-- description:start -->

<p>Bài toán yêu cầu triển khai thuật toán loại bỏ cơ bản cho trò Candy Crush.</p>

<p>Cho mảng số nguyên <code>m x n</code> <code>board</code> biểu diễn lưới kẹo, trong đó <code>board[i][j]</code> biểu diễn loại kẹo. Giá trị <code>board[i][j] == 0</code> nghĩa là ô đó đang trống.</p>

<p>Board được cho là trạng thái trò chơi sau lượt đi của người chơi. Hãy đưa board về trạng thái ổn định bằng cách nghiền kẹo theo các quy tắc sau:</p>

<ul>
	<li>Nếu có ít nhất ba viên kẹo cùng loại nằm liền nhau theo chiều dọc hoặc ngang, hãy nghiền chúng đồng thời; các vị trí đó trở thành ô trống.</li>
	<li>Sau khi nghiền đồng thời tất cả kẹo, nếu phía trên một ô trống còn có kẹo thì những viên kẹo đó sẽ đồng thời rơi xuống cho đến khi chạm kẹo khác hoặc chạm đáy. Không có kẹo mới nào rơi từ ngoài biên trên xuống.</li>
	<li>Sau các bước trên, có thể vẫn còn những viên kẹo có thể bị nghiền. Nếu vậy, hãy lặp lại các bước trên.</li>
	<li>Nếu không còn kẹo nào có thể bị nghiền (tức board đã ổn định), hãy trả về board hiện tại.</li>
</ul>

<p>Hãy áp dụng các quy tắc trên cho đến khi board ổn định, sau đó trả về <em>board ổn định</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0723.Candy%20Crush/images/candy_crush_example_2.png" style="width: 600px; height: 411px;" />
<pre>
<strong>Đầu vào:</strong> board = [[110,5,112,113,114],[210,211,5,213,214],[310,311,3,313,314],[410,411,412,5,414],[5,1,512,3,3],[610,4,1,613,614],[710,1,2,713,714],[810,1,2,1,1],[1,1,2,2,2],[4,1,4,4,1014]]
<strong>Đầu ra:</strong> [[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0],[110,0,0,0,114],[210,0,0,0,214],[310,0,0,113,314],[410,0,0,213,414],[610,211,112,313,614],[710,311,412,613,714],[810,411,512,713,1014]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> board = [[1,3,5,5,2],[3,4,3,3,1],[3,2,4,5,2],[2,4,4,5,5],[1,4,4,1,1]]
<strong>Đầu ra:</strong> [[1,3,0,0,0],[3,4,0,5,2],[3,2,0,3,1],[2,4,0,5,2],[1,4,3,1,1]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>3 &lt;= m, n &lt;= 50</code></li>
	<li><code>1 &lt;= board[i][j] &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Nghiền mọi dãy có ít nhất ba viên kẹo giống nhau, cho kẹo rơi xuống rồi lặp lại đến khi board ổn định. Kích thước board đủ nhỏ để mô phỏng trực tiếp.
>
> Nếu nghiền ngay, ta sẽ làm ảnh hưởng đến những dãy khác cũng cần bị loại trong cùng lượt. Vì vậy, hãy đánh dấu tất cả dãy hiện tại trước rồi mới cho kẹo rơi một lần.
>
> Dùng các cửa sổ độ dài $3$ trên từng hàng và cột để đánh dấu ô cần nghiền bằng giá trị âm (so sánh trị tuyệt đối). Nếu có ô được đánh dấu, dồn các giá trị dương xuống đáy từng cột, điền số 0 vào các ô còn lại rồi lặp lại.

<!-- thinking:end -->

Ta có thể duyệt ma trận theo từng hàng và từng cột để tìm ba phần tử liên tiếp giống nhau rồi đánh dấu chúng bằng số âm. Nếu có phần tử được đánh dấu, ta cần dồn các phần tử trong ma trận xuống dưới cho đến khi không còn phần tử nào có thể rơi thêm.

Độ phức tạp thời gian là $O(m^2 \times n^2)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def candyCrush(self, board: List[List[int]]) -> List[List[int]]:
        m, n = len(board), len(board[0])
        run = True
        while run:
            run = False
            for i in range(m):
                for j in range(2, n):
                    if board[i][j] and abs(board[i][j]) == abs(board[i][j - 1]) == abs(
                        board[i][j - 2]
                    ):
                        run = True
                        board[i][j] = board[i][j - 1] = board[i][j - 2] = -abs(
                            board[i][j]
                        )
            for j in range(n):
                for i in range(2, m):
                    if board[i][j] and abs(board[i][j]) == abs(board[i - 1][j]) == abs(
                        board[i - 2][j]
                    ):
                        run = True
                        board[i][j] = board[i - 1][j] = board[i - 2][j] = -abs(
                            board[i][j]
                        )
            if run:
                for j in range(n):
                    k = m - 1
                    for i in range(m - 1, -1, -1):
                        if board[i][j] > 0:
                            board[k][j] = board[i][j]
                            k -= 1
                    while k >= 0:
                        board[k][j] = 0
                        k -= 1
        return board
```

#### Java

```java
class Solution {
    public int[][] candyCrush(int[][] board) {
        int m = board.length, n = board[0].length;
        boolean run = true;

        while (run) {
            run = false;
            for (int i = 0; i < m; i++) {
                for (int j = 2; j < n; j++) {
                    if (board[i][j] != 0 && Math.abs(board[i][j]) == Math.abs(board[i][j - 1])
                        && Math.abs(board[i][j]) == Math.abs(board[i][j - 2])) {
                        run = true;
                        int val = Math.abs(board[i][j]);
                        board[i][j] = board[i][j - 1] = board[i][j - 2] = -val;
                    }
                }
            }
            for (int j = 0; j < n; j++) {
                for (int i = 2; i < m; i++) {
                    if (board[i][j] != 0 && Math.abs(board[i][j]) == Math.abs(board[i - 1][j])
                        && Math.abs(board[i][j]) == Math.abs(board[i - 2][j])) {
                        run = true;
                        int val = Math.abs(board[i][j]);
                        board[i][j] = board[i - 1][j] = board[i - 2][j] = -val;
                    }
                }
            }
            if (run) {
                for (int j = 0; j < n; j++) {
                    int k = m - 1;
                    for (int i = m - 1; i >= 0; i--) {
                        if (board[i][j] > 0) {
                            board[k][j] = board[i][j];
                            k--;
                        }
                    }
                    while (k >= 0) {
                        board[k][j] = 0;
                        k--;
                    }
                }
            }
        }

        return board;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> candyCrush(vector<vector<int>>& board) {
        int m = board.size(), n = board[0].size();
        bool run = true;
        while (run) {
            run = false;
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n - 2; ++j) {
                    if (board[i][j] != 0 && abs(board[i][j]) == abs(board[i][j + 1]) && abs(board[i][j]) == abs(board[i][j + 2])) {
                        run = true;
                        board[i][j] = board[i][j + 1] = board[i][j + 2] = -abs(board[i][j]);
                    }
                }
            }
            for (int j = 0; j < n; ++j) {
                for (int i = 0; i < m - 2; ++i) {
                    if (board[i][j] != 0 && abs(board[i][j]) == abs(board[i + 1][j]) && abs(board[i][j]) == abs(board[i + 2][j])) {
                        run = true;
                        board[i][j] = board[i + 1][j] = board[i + 2][j] = -abs(board[i][j]);
                    }
                }
            }
            if (run) {
                for (int j = 0; j < n; ++j) {
                    int curr = m - 1;
                    for (int i = m - 1; i >= 0; --i) {
                        if (board[i][j] > 0) {
                            board[curr][j] = board[i][j];
                            --curr;
                        }
                    }
                    while (curr > -1) {
                        board[curr][j] = 0;
                        --curr;
                    }
                }
            }
        }
        return board;
    }
};
```

#### Go

```go
func candyCrush(board [][]int) [][]int {
	m := len(board)
	n := len(board[0])
	run := true

	for run {
		run = false
		for i := 0; i < m; i++ {
			for j := 2; j < n; j++ {
				if board[i][j] != 0 && abs(board[i][j]) == abs(board[i][j-1]) && abs(board[i][j]) == abs(board[i][j-2]) {
					run = true
					val := abs(board[i][j])
					board[i][j] = -val
					board[i][j-1] = -val
					board[i][j-2] = -val
				}
			}
		}
		for j := 0; j < n; j++ {
			for i := 2; i < m; i++ {
				if board[i][j] != 0 && abs(board[i][j]) == abs(board[i-1][j]) && abs(board[i][j]) == abs(board[i-2][j]) {
					run = true
					val := abs(board[i][j])
					board[i][j] = -val
					board[i-1][j] = -val
					board[i-2][j] = -val
				}
			}
		}
		if run {
			for j := 0; j < n; j++ {
				k := m - 1
				for i := m - 1; i >= 0; i-- {
					if board[i][j] > 0 {
						board[k][j] = board[i][j]
						k--
					}
				}
				for k >= 0 {
					board[k][j] = 0
					k--
				}
			}
		}
	}

	return board
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
function candyCrush(board: number[][]): number[][] {
    const m = board.length;
    const n = board[0].length;
    let run = true;
    while (run) {
        run = false;
        for (let i = 0; i < m; i++) {
            for (let j = 2; j < n; j++) {
                if (
                    board[i][j] !== 0 &&
                    Math.abs(board[i][j]) === Math.abs(board[i][j - 1]) &&
                    Math.abs(board[i][j]) === Math.abs(board[i][j - 2])
                ) {
                    run = true;
                    const val = Math.abs(board[i][j]);
                    board[i][j] = board[i][j - 1] = board[i][j - 2] = -val;
                }
            }
        }
        for (let j = 0; j < n; j++) {
            for (let i = 2; i < m; i++) {
                if (
                    board[i][j] !== 0 &&
                    Math.abs(board[i][j]) === Math.abs(board[i - 1][j]) &&
                    Math.abs(board[i][j]) === Math.abs(board[i - 2][j])
                ) {
                    run = true;
                    const val = Math.abs(board[i][j]);
                    board[i][j] = board[i - 1][j] = board[i - 2][j] = -val;
                }
            }
        }
        if (run) {
            for (let j = 0; j < n; j++) {
                let k = m - 1;
                for (let i = m - 1; i >= 0; i--) {
                    if (board[i][j] > 0) {
                        board[k][j] = board[i][j];
                        k--;
                    }
                }
                while (k >= 0) {
                    board[k][j] = 0;
                    k--;
                }
            }
        }
    }
    return board;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
