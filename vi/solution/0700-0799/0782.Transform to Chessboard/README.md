---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Math
    - Matrix
---

<!-- problem:start -->

# [782. Transform to Chessboard](https://leetcode.com/problems/transform-to-chessboard)

[中文文档](/solution/0700-0799/0782.Transform%20to%20Chessboard/README.md)

## Mô tả

<!-- description:start -->

<p>Cho grid nhị phân <code>board</code> kích thước <code>n x n</code>. Trong mỗi lượt, bạn có thể hoán đổi hai hàng bất kỳ hoặc hai cột bất kỳ.</p>

<p>Hãy trả về <em>số lượt ít nhất để biến board thành một <strong>bàn cờ</strong></em>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p><strong>Bàn cờ</strong> là board mà không có hai ô <code>0</code> nào và cũng không có hai ô <code>1</code> nào kề nhau theo bốn hướng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0782.Transform%20to%20Chessboard/images/chessboard1-grid.jpg" style="width: 500px; height: 145px;" />
<pre>
<strong>Đầu vào:</strong> board = [[0,1,1,0],[0,1,1,0],[1,0,0,1],[1,0,0,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Một chuỗi thao tác có thể thực hiện được minh họa ở trên.
Lượt đầu tiên hoán đổi cột thứ nhất và cột thứ hai.
Lượt thứ hai hoán đổi hàng thứ hai và hàng thứ ba.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0782.Transform%20to%20Chessboard/images/chessboard2-grid.jpg" style="width: 164px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> board = [[0,1],[1,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Lưu ý rằng board có ô trên cùng bên trái là 0 cũng là một bàn cờ hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0782.Transform%20to%20Chessboard/images/chessboard3-grid.jpg" style="width: 164px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> board = [[1,0],[1,0]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Dù thực hiện chuỗi thao tác nào, bạn cũng không thể tạo được bàn cờ hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 30</code></li>
	<li><code>board[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận dạng quy luật + Nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ được hoán đổi nguyên hàng hoặc nguyên cột. $n\le 30$. Bàn cờ có hai mẫu hàng bổ sung cho nhau (tương tự với cột), đồng thời số lượng $0$ và $1$ phải cân bằng.
>
> Mask của hàng đầu tiên và cột đầu tiên xác định hai mẫu này; mọi hàng và cột còn lại phải khớp với một trong hai mẫu tương ứng.
>
> $f(\textit{mask},\textit{cnt})$ tính số lần hoán đổi cần thiết để đưa mask về mẫu $0101\ldots$ hoặc $1010\ldots$ tùy theo tính chẵn lẻ. Cộng kết quả của hàng và cột.

<!-- thinking:end -->

Trong bàn cờ hợp lệ, chỉ có đúng hai kiểu "hàng".

Ví dụ, nếu một hàng là "01010011" thì mọi hàng khác chỉ có thể là "01010011" hoặc "10101100". Các cột cũng có tính chất tương tự.

Ngoài ra, mỗi hàng và mỗi cột có số lượng $0$ và $1$ cân bằng. Giả sử bàn cờ có kích thước $n \times n$:

- Nếu $n = 2 \times k$, mỗi hàng và mỗi cột có $k$ số $1$ và $k$ số $0$.
- Nếu $n = 2 \times k + 1$, mỗi hàng có thể có $k$ số $1$ và $k + 1$ số $0$, hoặc $k + 1$ số $1$ và $k$ số $0$.

Dựa trên các nhận xét trên, ta có thể xác định board có thể biến thành bàn cờ hợp lệ hay không. Nếu có, ta tính số lượt ít nhất cần thực hiện.

Nếu $n$ chẵn, có hai bàn cờ hợp lệ có thể tạo thành: hàng đầu tiên là "010101..." hoặc "101010...". Ta tính số lần hoán đổi ít nhất cho từng trường hợp rồi lấy giá trị nhỏ hơn làm đáp án.

Nếu $n$ lẻ, chỉ có một bàn cờ hợp lệ có thể tạo thành. Nếu hàng đầu tiên có số $0$ nhiều hơn số $1$, thì hàng đầu tiên của bàn cờ cuối cùng phải là "01010..."; ngược lại, phải là "10101...". Ta tính số lần hoán đổi cần thiết và dùng kết quả đó làm đáp án.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là kích thước của bàn cờ. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def movesToChessboard(self, board: List[List[int]]) -> int:
        def f(mask, cnt):
            ones = mask.bit_count()
            if n & 1:
                if abs(n - 2 * ones) != 1 or abs(n - 2 * cnt) != 1:
                    return -1
                if ones == n // 2:
                    return n // 2 - (mask & 0xAAAAAAAA).bit_count()
                return (n + 1) // 2 - (mask & 0x55555555).bit_count()
            else:
                if ones != n // 2 or cnt != n // 2:
                    return -1
                cnt0 = n // 2 - (mask & 0xAAAAAAAA).bit_count()
                cnt1 = n // 2 - (mask & 0x55555555).bit_count()
                return min(cnt0, cnt1)

        n = len(board)
        mask = (1 << n) - 1
        rowMask = colMask = 0
        for i in range(n):
            rowMask |= board[0][i] << i
            colMask |= board[i][0] << i
        revRowMask = mask ^ rowMask
        revColMask = mask ^ colMask
        sameRow = sameCol = 0
        for i in range(n):
            curRowMask = curColMask = 0
            for j in range(n):
                curRowMask |= board[i][j] << j
                curColMask |= board[j][i] << j
            if curRowMask not in (rowMask, revRowMask) or curColMask not in (
                colMask,
                revColMask,
            ):
                return -1
            sameRow += curRowMask == rowMask
            sameCol += curColMask == colMask
        t1 = f(rowMask, sameRow)
        t2 = f(colMask, sameCol)
        return -1 if t1 == -1 or t2 == -1 else t1 + t2
```

#### Java

```java
class Solution {
    private int n;

    public int movesToChessboard(int[][] board) {
        n = board.length;
        int mask = (1 << n) - 1;
        int rowMask = 0, colMask = 0;
        for (int i = 0; i < n; ++i) {
            rowMask |= board[0][i] << i;
            colMask |= board[i][0] << i;
        }
        int revRowMask = mask ^ rowMask;
        int revColMask = mask ^ colMask;
        int sameRow = 0, sameCol = 0;
        for (int i = 0; i < n; ++i) {
            int curRowMask = 0, curColMask = 0;
            for (int j = 0; j < n; ++j) {
                curRowMask |= board[i][j] << j;
                curColMask |= board[j][i] << j;
            }
            if (curRowMask != rowMask && curRowMask != revRowMask) {
                return -1;
            }
            if (curColMask != colMask && curColMask != revColMask) {
                return -1;
            }
            sameRow += curRowMask == rowMask ? 1 : 0;
            sameCol += curColMask == colMask ? 1 : 0;
        }
        int t1 = f(rowMask, sameRow);
        int t2 = f(colMask, sameCol);
        return t1 == -1 || t2 == -1 ? -1 : t1 + t2;
    }

    private int f(int mask, int cnt) {
        int ones = Integer.bitCount(mask);
        if (n % 2 == 1) {
            if (Math.abs(n - ones * 2) != 1 || Math.abs(n - cnt * 2) != 1) {
                return -1;
            }
            if (ones == n / 2) {
                return n / 2 - Integer.bitCount(mask & 0xAAAAAAAA);
            }
            return (n / 2 + 1) - Integer.bitCount(mask & 0x55555555);
        } else {
            if (ones != n / 2 || cnt != n / 2) {
                return -1;
            }
            int cnt0 = n / 2 - Integer.bitCount(mask & 0xAAAAAAAA);
            int cnt1 = n / 2 - Integer.bitCount(mask & 0x55555555);
            return Math.min(cnt0, cnt1);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int n;
    int movesToChessboard(vector<vector<int>>& board) {
        n = board.size();
        int mask = (1 << n) - 1;
        int rowMask = 0, colMask = 0;
        for (int i = 0; i < n; ++i) {
            rowMask |= board[0][i] << i;
            colMask |= board[i][0] << i;
        }
        int revRowMask = mask ^ rowMask;
        int revColMask = mask ^ colMask;
        int sameRow = 0, sameCol = 0;
        for (int i = 0; i < n; ++i) {
            int curRowMask = 0, curColMask = 0;
            for (int j = 0; j < n; ++j) {
                curRowMask |= board[i][j] << j;
                curColMask |= board[j][i] << j;
            }
            if (curRowMask != rowMask && curRowMask != revRowMask) return -1;
            if (curColMask != colMask && curColMask != revColMask) return -1;
            sameRow += curRowMask == rowMask;
            sameCol += curColMask == colMask;
        }
        int t1 = f(rowMask, sameRow);
        int t2 = f(colMask, sameCol);
        return t1 == -1 || t2 == -1 ? -1 : t1 + t2;
    }

    int f(int mask, int cnt) {
        int ones = __builtin_popcount(mask);
        if (n & 1) {
            if (abs(n - ones * 2) != 1 || abs(n - cnt * 2) != 1) return -1;
            if (ones == n / 2) return n / 2 - __builtin_popcount(mask & 0xAAAAAAAA);
            return (n + 1) / 2 - __builtin_popcount(mask & 0x55555555);
        } else {
            if (ones != n / 2 || cnt != n / 2) return -1;
            int cnt0 = (n / 2 - __builtin_popcount(mask & 0xAAAAAAAA));
            int cnt1 = (n / 2 - __builtin_popcount(mask & 0x55555555));
            return min(cnt0, cnt1);
        }
    }
};
```

#### Go

```go
func movesToChessboard(board [][]int) int {
	n := len(board)
	mask := (1 << n) - 1
	rowMask, colMask := 0, 0
	for i := 0; i < n; i++ {
		rowMask |= board[0][i] << i
		colMask |= board[i][0] << i
	}
	revRowMask := mask ^ rowMask
	revColMask := mask ^ colMask
	sameRow, sameCol := 0, 0
	for i := 0; i < n; i++ {
		curRowMask, curColMask := 0, 0
		for j := 0; j < n; j++ {
			curRowMask |= board[i][j] << j
			curColMask |= board[j][i] << j
		}
		if curRowMask != rowMask && curRowMask != revRowMask {
			return -1
		}
		if curColMask != colMask && curColMask != revColMask {
			return -1
		}
		if curRowMask == rowMask {
			sameRow++
		}
		if curColMask == colMask {
			sameCol++
		}
	}
	f := func(mask, cnt int) int {
		ones := bits.OnesCount(uint(mask))
		if n%2 == 1 {
			if abs(n-ones*2) != 1 || abs(n-cnt*2) != 1 {
				return -1
			}
			if ones == n/2 {
				return n/2 - bits.OnesCount(uint(mask&0xAAAAAAAA))
			}
			return (n+1)/2 - bits.OnesCount(uint(mask&0x55555555))
		} else {
			if ones != n/2 || cnt != n/2 {
				return -1
			}
			cnt0 := n/2 - bits.OnesCount(uint(mask&0xAAAAAAAA))
			cnt1 := n/2 - bits.OnesCount(uint(mask&0x55555555))
			return min(cnt0, cnt1)
		}
	}
	t1 := f(rowMask, sameRow)
	t2 := f(colMask, sameCol)
	if t1 == -1 || t2 == -1 {
		return -1
	}
	return t1 + t2
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
