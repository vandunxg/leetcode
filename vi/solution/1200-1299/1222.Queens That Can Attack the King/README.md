---
comments: true
difficulty: Medium
rating: 1391
source: Weekly Contest 158 Q2
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [1222. Queens That Can Attack the King](https://leetcode.com/problems/queens-that-can-attack-the-king)

[中文文档](/solution/1200-1299/1222.Queens%20That%20Can%20Attack%20the%20King/README.md)

## Mô tả

<!-- description:start -->

<p>Trên bàn cờ vua <code>8 x 8</code> <strong>đánh chỉ số từ 0</strong>, có thể có nhiều quân hậu đen và một quân vua trắng.</p>

<p>Cho mảng số nguyên hai chiều <code>queens</code>, trong đó <code>queens[i] = [xQueen<sub>i</sub>, yQueen<sub>i</sub>]</code> biểu thị vị trí của quân hậu đen thứ <code>i<sup>th</sup></code> trên bàn cờ. Ngoài ra, cho mảng số nguyên <code>king</code> có độ dài <code>2</code>, trong đó <code>king = [xKing, yKing]</code> biểu thị vị trí của quân vua trắng.</p>

<p>Trả về <em>tọa độ của các quân hậu đen có thể trực tiếp tấn công quân vua</em>. Bạn có thể trả kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1222.Queens%20That%20Can%20Attack%20the%20King/images/chess1.jpg" style="width: 400px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> queens = [[0,1],[1,0],[4,0],[0,4],[3,3],[2,4]], king = [0,0]
<strong>Đầu ra:</strong> [[0,1],[1,0],[3,3]]
<strong>Giải thích:</strong> Hình phía trên cho thấy ba quân hậu có thể trực tiếp tấn công quân vua và ba quân hậu không thể tấn công (được đánh dấu bằng các nét đỏ).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1222.Queens%20That%20Can%20Attack%20the%20King/images/chess2.jpg" style="width: 400px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> queens = [[0,0],[1,1],[2,2],[3,4],[3,5],[4,4],[4,5]], king = [3,3]
<strong>Đầu ra:</strong> [[2,2],[3,4],[4,4]]
<strong>Giải thích:</strong> Hình phía trên cho thấy ba quân hậu có thể trực tiếp tấn công quân vua và ba quân hậu không thể tấn công (được đánh dấu bằng các nét đỏ).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queens.length &lt; 64</code></li>
	<li><code>queens[i].length == king.length == 2</code></li>
	<li><code>0 &lt;= xQueen<sub>i</sub>, yQueen<sub>i</sub>, xKing, yKing &lt; 8</code></li>
	<li>Tất cả vị trí đã cho đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Bàn cờ có kích thước $8\times 8$ và ít hơn $64$ quân hậu. Quân hậu chỉ tấn công quân vua theo cùng hàng, cùng cột hoặc đường chéo, miễn là không có quân hậu nào chắn giữa. Khi đi từ quân vua ra ngoài theo tám hướng, quân hậu đầu tiên trên mỗi hướng là quân duy nhất có thể tấn công theo hướng đó.
>
> Ta lưu các ô có quân hậu trong một set để kiểm tra trong $O(1)$, sau đó đi từ quân vua theo tám vector đơn vị, ghi nhận quân hậu tìm thấy rồi dừng hướng đó. Cách duyệt này xử lý việc quân cờ chắn nhau; set giúp mỗi bước kiểm tra mất thời gian hằng số.

<!-- thinking:end -->

Trước tiên, ta lưu vị trí của tất cả quân hậu vào hash table hoặc mảng hai chiều $s$.

Tiếp theo, bắt đầu từ vị trí quân vua, ta tìm theo tám hướng: lên, xuống, trái, phải, chéo lên trái, chéo lên phải, chéo xuống trái và chéo xuống phải. Nếu gặp quân hậu theo một hướng nào đó, ta thêm vị trí của nó vào đáp án rồi dừng tìm theo hướng đó.

Sau khi tìm kiếm xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n^2)$. Trong bài này, $n = 8$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def queensAttacktheKing(
        self, queens: List[List[int]], king: List[int]
    ) -> List[List[int]]:
        n = 8
        s = {(i, j) for i, j in queens}
        ans = []
        for a in range(-1, 2):
            for b in range(-1, 2):
                if a or b:
                    x, y = king
                    while 0 <= x + a < n and 0 <= y + b < n:
                        x, y = x + a, y + b
                        if (x, y) in s:
                            ans.append([x, y])
                            break
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
        final int n = 8;
        var s = new boolean[n][n];
        for (var q : queens) {
            s[q[0]][q[1]] = true;
        }
        List<List<Integer>> ans = new ArrayList<>();
        for (int a = -1; a <= 1; ++a) {
            for (int b = -1; b <= 1; ++b) {
                if (a != 0 || b != 0) {
                    int x = king[0] + a, y = king[1] + b;
                    while (x >= 0 && x < n && y >= 0 && y < n) {
                        if (s[x][y]) {
                            ans.add(List.of(x, y));
                            break;
                        }
                        x += a;
                        y += b;
                    }
                }
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
    vector<vector<int>> queensAttacktheKing(vector<vector<int>>& queens, vector<int>& king) {
        int n = 8;
        bool s[8][8]{};
        for (auto& q : queens) {
            s[q[0]][q[1]] = true;
        }
        vector<vector<int>> ans;
        for (int a = -1; a <= 1; ++a) {
            for (int b = -1; b <= 1; ++b) {
                if (a || b) {
                    int x = king[0] + a, y = king[1] + b;
                    while (x >= 0 && x < n && y >= 0 && y < n) {
                        if (s[x][y]) {
                            ans.push_back({x, y});
                            break;
                        }
                        x += a;
                        y += b;
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func queensAttacktheKing(queens [][]int, king []int) (ans [][]int) {
	n := 8
	s := [8][8]bool{}
	for _, q := range queens {
		s[q[0]][q[1]] = true
	}
	for a := -1; a <= 1; a++ {
		for b := -1; b <= 1; b++ {
			if a != 0 || b != 0 {
				x, y := king[0]+a, king[1]+b
				for 0 <= x && x < n && 0 <= y && y < n {
					if s[x][y] {
						ans = append(ans, []int{x, y})
						break
					}
					x += a
					y += b
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function queensAttacktheKing(queens: number[][], king: number[]): number[][] {
    const n = 8;
    const s: boolean[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => false));
    queens.forEach(([x, y]) => (s[x][y] = true));
    const ans: number[][] = [];
    for (let a = -1; a <= 1; ++a) {
        for (let b = -1; b <= 1; ++b) {
            if (a || b) {
                let [x, y] = [king[0] + a, king[1] + b];
                while (x >= 0 && x < n && y >= 0 && y < n) {
                    if (s[x][y]) {
                        ans.push([x, y]);
                        break;
                    }
                    x += a;
                    y += b;
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
