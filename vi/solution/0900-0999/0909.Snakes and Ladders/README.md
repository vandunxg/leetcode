---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [909. Snakes and Ladders](https://leetcode.com/problems/snakes-and-ladders)

[中文文档](/solution/0900-0999/0909.Snakes%20and%20Ladders/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>n x n</code> <code>board</code>, trong đó các ô được đánh số từ <code>1</code> đến <code>n<sup>2</sup></code> theo <a href="https://en.wikipedia.org/wiki/Boustrophedon" target="_blank"><strong>kiểu Boustrophedon</strong></a>, bắt đầu từ góc dưới bên trái (tức <code>board[n - 1][0]</code>) và đổi hướng sau mỗi hàng.</p>

<p>Bạn bắt đầu ở ô <code>1</code>. Trong mỗi lượt, từ ô <code>curr</code>, thực hiện như sau:</p>

<ul>
	<li>Chọn ô đích <code>next</code> có nhãn trong khoảng <code>[curr + 1, min(curr + 6, n<sup>2</sup>)]</code>.

    <ul>
    	<li>Lựa chọn này mô phỏng kết quả của một lần <strong>gieo xúc xắc 6 mặt</strong>: luôn có tối đa 6 ô đích, bất kể kích thước bàn cờ.</li>
    </ul>
    </li>
    <li>Nếu <code>next</code> có rắn hoặc thang, bạn <strong>phải</strong> di chuyển đến ô đích của rắn hoặc thang đó. Nếu không, bạn đi đến <code>next</code>.</li>
    <li>Trò chơi kết thúc khi bạn đến ô <code>n<sup>2</sup></code>.</li>

</ul>

<p>Ô ở hàng <code>r</code>, cột <code>c</code> có rắn hoặc thang nếu <code>board[r][c] != -1</code>. Ô đích của rắn hoặc thang đó là <code>board[r][c]</code>. Các ô <code>1</code> và <code>n<sup>2</sup></code> không phải điểm bắt đầu của rắn hay thang nào.</p>

<p>Lưu ý, trong mỗi lần gieo xúc xắc, bạn chỉ đi theo rắn hoặc thang tối đa một lần. Nếu ô đích của rắn hoặc thang là điểm bắt đầu của một rắn hoặc thang khác, bạn <strong>không</strong> đi tiếp theo rắn hoặc thang kế tiếp đó.</p>

<ul>
	<li>Ví dụ, giả sử bàn cờ là <code>[[-1,4],[-1,3]]</code> và trong lượt đầu tiên, bạn chọn ô đích <code>2</code>. Bạn đi theo thang đến ô <code>3</code>, nhưng <strong>không</strong> đi tiếp theo thang đến ô <code>4</code>.</li>
</ul>

<p>Hãy trả về <em>số lần gieo xúc xắc ít nhất để đến ô </em><code>n<sup>2</sup></code><em>. Nếu không thể đến ô đó, trả về </em><code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0909.Snakes%20and%20Ladders/images/snakes.png" style="width: 500px; height: 394px;" />
<pre>
<strong>Đầu vào:</strong> board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 
Ban đầu, bạn ở ô 1 (hàng 5, cột 0).
Bạn chọn đi đến ô 2 và phải đi theo thang đến ô 15.
Sau đó, bạn chọn đi đến ô 17 và phải đi theo rắn đến ô 13.
Tiếp theo, bạn chọn đi đến ô 14 và phải đi theo thang đến ô 35.
Cuối cùng, bạn đi đến ô 36 và kết thúc trò chơi.
Đây là số lượt ít nhất có thể để đến ô cuối, nên trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> board = [[-1,-1],[-1,3]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == board.length == board[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 20</code></li>
	<li><code>board[i][j]</code> bằng <code>-1</code> hoặc nằm trong khoảng <code>[1, n<sup>2</sup>]</code>.</li>
	<li>Các ô mang nhãn <code>1</code> và <code>n<sup>2</sup></code> không phải điểm bắt đầu của rắn hay thang nào.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Bàn cờ có kích thước tối đa $20\times 20$. Tìm số lần gieo xúc xắc ít nhất tương đương tìm đường đi ngắn nhất trên đồ thị không trọng số. Các nhãn đi ziczac trên lưới; rắn hoặc thang đưa ta đến ô đích tương ứng.
>
> Chạy BFS theo từng lớp từ ô $1$, mỗi ô chỉ thăm một lần. Lớp đầu tiên đến được $n^2$ cho biết đáp án; nếu queue rỗng, trả về $-1$.

<!-- thinking:end -->

Ta có thể dùng Breadth-First Search (BFS): bắt đầu từ ô xuất phát, mỗi lần tiến từ 1 đến 6 ô rồi kiểm tra có rắn hoặc thang hay không. Nếu có, đi đến ô đích của rắn hoặc thang; nếu không, ở ô vừa chọn.

Cụ thể, ta dùng queue $\textit{q}$ để lưu các số ô hiện có thể đến được; ban đầu đưa ô $1$ vào queue. Đồng thời, dùng set $\textit{vis}$ để ghi lại các ô đã đến nhằm tránh thăm lại; ban đầu thêm ô $1$ vào set $\textit{vis}$.

Mỗi lượt, lấy số ô $x$ ở đầu queue. Nếu $x$ là ô đích, trả về số bước hiện tại. Nếu không, thử đi từ $x$ thêm 1 đến 6 ô và gọi ô mới là $y$. Nếu $y$ vượt khỏi bàn cờ, bỏ qua. Nếu không, ta cần tìm hàng và cột tương ứng với $y$. Vì số hàng giảm dần từ dưới lên, còn số cột phụ thuộc vào tính chẵn lẻ của hàng, ta cần tính toán để xác định hàng và cột của $y$.

Nếu ô ứng với $y$ có rắn hoặc thang, ta đi đến ô đích của nó, ký hiệu là $z$. Nếu chưa thăm $z$, thêm $z$ vào queue và set để tiếp tục BFS.

Nếu cuối cùng không thể đến ô đích, trả về $-1$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của bàn cờ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def snakesAndLadders(self, board: List[List[int]]) -> int:
        n = len(board)
        q = deque([1])
        vis = {1}
        ans = 0
        m = n * n
        while q:
            for _ in range(len(q)):
                x = q.popleft()
                if x == m:
                    return ans
                for y in range(x + 1, min(x + 6, m) + 1):
                    i, j = divmod(y - 1, n)
                    if i & 1:
                        j = n - j - 1
                    i = n - i - 1
                    z = y if board[i][j] == -1 else board[i][j]
                    if z not in vis:
                        vis.add(z)
                        q.append(z)
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        int n = board.length;
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(1);
        int m = n * n;
        boolean[] vis = new boolean[m + 1];
        vis[1] = true;
        for (int ans = 0; !q.isEmpty(); ++ans) {
            for (int k = q.size(); k > 0; --k) {
                int x = q.poll();
                if (x == m) {
                    return ans;
                }
                for (int y = x + 1; y <= Math.min(x + 6, m); ++y) {
                    int i = (y - 1) / n, j = (y - 1) % n;
                    if (i % 2 == 1) {
                        j = n - j - 1;
                    }
                    i = n - i - 1;
                    int z = board[i][j] == -1 ? y : board[i][j];
                    if (!vis[z]) {
                        vis[z] = true;
                        q.offer(z);
                    }
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int snakesAndLadders(vector<vector<int>>& board) {
        int n = board.size();
        queue<int> q{{1}};
        int m = n * n;
        vector<bool> vis(m + 1);
        vis[1] = true;

        for (int ans = 0; !q.empty(); ++ans) {
            for (int k = q.size(); k > 0; --k) {
                int x = q.front();
                q.pop();
                if (x == m) {
                    return ans;
                }
                for (int y = x + 1; y <= min(x + 6, m); ++y) {
                    int i = (y - 1) / n, j = (y - 1) % n;
                    if (i % 2 == 1) {
                        j = n - j - 1;
                    }
                    i = n - i - 1;
                    int z = board[i][j] == -1 ? y : board[i][j];
                    if (!vis[z]) {
                        vis[z] = true;
                        q.push(z);
                    }
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func snakesAndLadders(board [][]int) int {
	n := len(board)
	q := []int{1}
	m := n * n
	vis := make([]bool, m+1)
	vis[1] = true

	for ans := 0; len(q) > 0; ans++ {
		for k := len(q); k > 0; k-- {
			x := q[0]
			q = q[1:]
			if x == m {
				return ans
			}
			for y := x + 1; y <= min(x+6, m); y++ {
				i, j := (y-1)/n, (y-1)%n
				if i%2 == 1 {
					j = n - j - 1
				}
				i = n - i - 1
				z := y
				if board[i][j] != -1 {
					z = board[i][j]
				}
				if !vis[z] {
					vis[z] = true
					q = append(q, z)
				}
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function snakesAndLadders(board: number[][]): number {
    const n = board.length;
    const q: number[] = [1];
    const m = n * n;
    const vis: boolean[] = Array(m + 1).fill(false);
    vis[1] = true;

    for (let ans = 0; q.length > 0; ans++) {
        const nq: number[] = [];
        for (const x of q) {
            if (x === m) {
                return ans;
            }
            for (let y = x + 1; y <= Math.min(x + 6, m); y++) {
                let i = Math.floor((y - 1) / n);
                let j = (y - 1) % n;
                if (i % 2 === 1) {
                    j = n - j - 1;
                }
                i = n - i - 1;
                const z = board[i][j] === -1 ? y : board[i][j];
                if (!vis[z]) {
                    vis[z] = true;
                    nq.push(z);
                }
            }
        }
        q.length = 0;
        for (const x of nq) {
            q.push(x);
        }
    }
    return -1;
}
```

#### Rust

```rust
use std::collections::{HashSet, VecDeque};

impl Solution {
    pub fn snakes_and_ladders(board: Vec<Vec<i32>>) -> i32 {
        let n = board.len();
        let m = (n * n) as i32;
        let mut q = VecDeque::new();
        q.push_back(1);
        let mut vis = HashSet::new();
        vis.insert(1);
        let mut ans = 0;

        while !q.is_empty() {
            for _ in 0..q.len() {
                let x = q.pop_front().unwrap();
                if x == m {
                    return ans;
                }
                for y in x + 1..=i32::min(x + 6, m) {
                    let (mut i, mut j) = ((y - 1) / n as i32, (y - 1) % n as i32);
                    if i % 2 == 1 {
                        j = (n as i32 - 1) - j;
                    }
                    i = (n as i32 - 1) - i;
                    let z = if board[i as usize][j as usize] == -1 {
                        y
                    } else {
                        board[i as usize][j as usize]
                    };
                    if !vis.contains(&z) {
                        vis.insert(z);
                        q.push_back(z);
                    }
                }
            }
            ans += 1;
        }

        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
