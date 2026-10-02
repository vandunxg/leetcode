---
comments: true
difficulty: Easy
rating: 1336
source: Weekly Contest 165 Q1
tags:
    - Array
    - Hash Table
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [1275. Find Winner on a Tic Tac Toe Game](https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game)

[中文文档](/solution/1200-1299/1275.Find%20Winner%20on%20a%20Tic%20Tac%20Toe%20Game/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Tic-tac-toe</strong> là trò chơi dành cho hai người chơi <code>A</code> và <code>B</code> trên lưới <code>3 x 3</code>. Luật chơi như sau:</p>

<ul>
	<li>Hai người chơi lần lượt đặt ký tự vào các ô trống <code>&#39; &#39;</code>.</li>
	<li>Người chơi thứ nhất <code>A</code> luôn đặt ký tự <code>&#39;X&#39;</code>, còn người chơi thứ hai <code>B</code> luôn đặt ký tự <code>&#39;O&#39;</code>.</li>
	<li>Ký tự <code>&#39;X&#39;</code> và <code>&#39;O&#39;</code> luôn được đặt vào ô trống, không bao giờ đặt lên ô đã có ký tự.</li>
	<li>Trò chơi kết thúc khi có <strong>ba</strong> ký tự giống nhau (khác ký tự trống) nằm trên cùng một hàng, cột hoặc đường chéo.</li>
	<li>Trò chơi cũng kết thúc khi tất cả các ô đều đã được điền.</li>
	<li>Khi trò chơi đã kết thúc thì không thực hiện thêm nước đi nào.</li>
</ul>

<p>Cho mảng số nguyên 2 chiều <code>moves</code>, trong đó <code>moves[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> cho biết nước đi thứ <code>i</code> được thực hiện tại ô <code>grid[row<sub>i</sub>][col<sub>i</sub>]</code>. Hãy trả về <em>người chiến thắng nếu có</em> (<code>A</code> hoặc <code>B</code>). Nếu ván đấu kết thúc hòa, trả về <code>&quot;Draw&quot;</code>. Nếu vẫn còn nước đi có thể thực hiện, trả về <code>&quot;Pending&quot;</code>.</p>

<p>Có thể giả sử <code>moves</code> hợp lệ (tức là tuân theo luật <strong>Tic-Tac-Toe</strong>), lưới ban đầu trống và <code>A</code> đi trước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1275.Find%20Winner%20on%20a%20Tic%20Tac%20Toe%20Game/images/xo1-grid.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> moves = [[0,0],[2,0],[1,1],[2,1],[2,2]]
<strong>Đầu ra:</strong> &quot;A&quot;
<strong>Giải thích:</strong> A thắng vì người này luôn đi trước.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1275.Find%20Winner%20on%20a%20Tic%20Tac%20Toe%20Game/images/xo2-grid.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> moves = [[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]
<strong>Đầu ra:</strong> &quot;B&quot;
<strong>Giải thích:</strong> B thắng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1275.Find%20Winner%20on%20a%20Tic%20Tac%20Toe%20Game/images/xo3-grid.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> moves = [[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]
<strong>Đầu ra:</strong> &quot;Draw&quot;
<strong>Giải thích:</strong> Ván đấu kết thúc hòa vì không còn nước đi nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= moves.length &lt;= 9</code></li>
	<li><code>moves[i].length == 2</code></li>
	<li><code>0 &lt;= row<sub>i</sub>, col<sub>i</sub> &lt;= 2</code></li>
	<li>Không có phần tử nào bị lặp trong <code>moves</code>.</li>
	<li><code>moves</code> tuân theo luật tic tac toe.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Kiểm tra người đi cuối có thắng hay không

<!-- thinking:start -->

> **Tư duy**
>
> Các nước đi đều hợp lệ và trò chơi dừng ngay khi có người thắng, nên ta chỉ cần kiểm tra người đi cuối đã tạo được ba ký tự liên tiếp hay chưa. Duyệt ngược các nước đi cách nhau hai lượt, ta đếm số ô của người đó trên từng hàng, cột và đường chéo; nếu có số đếm bằng $3$ thì người đó thắng. Nếu không, đủ chín nước đi thì hòa, còn ít hơn thì ván đấu vẫn đang chờ.

<!-- thinking:end -->

Vì `moves` hợp lệ, không có trường hợp người chơi tiếp tục đi sau khi đã có người thắng. Do đó, ta chỉ cần xác định người đi cuối có thắng hay không.

Ta dùng mảng `cnt` độ dài $8$ để đếm số nước đi trên các hàng, cột và đường chéo. $cnt[0, 1, 2]$ lần lượt lưu số nước đi trên các hàng $0, 1, 2$; $cnt[3, 4, 5]$ lần lượt lưu số nước đi trên các cột $0, 1, 2$. Ngoài ra, $cnt[6]$ và $cnt[7]$ lần lượt đếm nước đi trên hai đường chéo. Trong trò chơi, nếu một người có $3$ nước đi trên cùng hàng, cột hoặc đường chéo thì người đó thắng.

Nếu người đi cuối không thắng, ta kiểm tra bàn cờ đã đầy chưa. Nếu đầy thì hòa; nếu chưa thì ván đấu vẫn chưa kết thúc.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài của `moves`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tictactoe(self, moves: List[List[int]]) -> str:
        n = len(moves)
        cnt = [0] * 8
        for k in range(n - 1, -1, -2):
            i, j = moves[k]
            cnt[i] += 1
            cnt[j + 3] += 1
            if i == j:
                cnt[6] += 1
            if i + j == 2:
                cnt[7] += 1
            if any(v == 3 for v in cnt):
                return "B" if k & 1 else "A"
        return "Draw" if n == 9 else "Pending"
```

#### Java

```java
class Solution {
    public String tictactoe(int[][] moves) {
        int n = moves.length;
        int[] cnt = new int[8];
        for (int k = n - 1; k >= 0; k -= 2) {
            int i = moves[k][0], j = moves[k][1];
            cnt[i]++;
            cnt[j + 3]++;
            if (i == j) {
                cnt[6]++;
            }
            if (i + j == 2) {
                cnt[7]++;
            }
            if (cnt[i] == 3 || cnt[j + 3] == 3 || cnt[6] == 3 || cnt[7] == 3) {
                return k % 2 == 0 ? "A" : "B";
            }
        }
        return n == 9 ? "Draw" : "Pending";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string tictactoe(vector<vector<int>>& moves) {
        int n = moves.size();
        int cnt[8]{};
        for (int k = n - 1; k >= 0; k -= 2) {
            int i = moves[k][0], j = moves[k][1];
            cnt[i]++;
            cnt[j + 3]++;
            if (i == j) {
                cnt[6]++;
            }
            if (i + j == 2) {
                cnt[7]++;
            }
            if (cnt[i] == 3 || cnt[j + 3] == 3 || cnt[6] == 3 || cnt[7] == 3) {
                return k % 2 == 0 ? "A" : "B";
            }
        }
        return n == 9 ? "Draw" : "Pending";
    }
};
```

#### Go

```go
func tictactoe(moves [][]int) string {
	n := len(moves)
	cnt := [8]int{}
	for k := n - 1; k >= 0; k -= 2 {
		i, j := moves[k][0], moves[k][1]
		cnt[i]++
		cnt[j+3]++
		if i == j {
			cnt[6]++
		}
		if i+j == 2 {
			cnt[7]++
		}
		if cnt[i] == 3 || cnt[j+3] == 3 || cnt[6] == 3 || cnt[7] == 3 {
			if k%2 == 0 {
				return "A"
			}
			return "B"
		}
	}
	if n == 9 {
		return "Draw"
	}
	return "Pending"
}
```

#### TypeScript

```ts
function tictactoe(moves: number[][]): string {
    const n = moves.length;
    const cnt = new Array(8).fill(0);
    for (let k = n - 1; k >= 0; k -= 2) {
        const [i, j] = moves[k];
        cnt[i]++;
        cnt[j + 3]++;
        if (i == j) {
            cnt[6]++;
        }
        if (i + j == 2) {
            cnt[7]++;
        }
        if (cnt[i] == 3 || cnt[j + 3] == 3 || cnt[6] == 3 || cnt[7] == 3) {
            return k % 2 == 0 ? 'A' : 'B';
        }
    }
    return n == 9 ? 'Draw' : 'Pending';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
