---
comments: true
difficulty: Medium
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [885. Spiral Matrix III](https://leetcode.com/problems/spiral-matrix-iii)

[中文文档](/solution/0800-0899/0885.Spiral%20Matrix%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn bắt đầu tại ô <code>(rStart, cStart)</code> của lưới <code>rows x cols</code>, hướng mặt về phía đông. Góc tây bắc là hàng đầu tiên và cột đầu tiên; góc đông nam là hàng cuối cùng và cột cuối cùng của lưới.</p>

<p>Bạn sẽ đi theo đường xoắn ốc theo chiều kim đồng hồ để ghé thăm mọi vị trí trong lưới. Khi đi ra ngoài ranh giới lưới, bạn vẫn tiếp tục bước bên ngoài (sau đó có thể quay lại lưới). Cuối cùng, bạn sẽ đi qua đủ <code>rows * cols</code> ô.</p>

<p>Trả về <em>mảng tọa độ biểu diễn các vị trí trong lưới theo thứ tự bạn đã ghé thăm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0885.Spiral%20Matrix%20III/images/example_1.png" style="width: 174px; height: 99px;" />
<pre>
<strong>Input:</strong> rows = 1, cols = 4, rStart = 0, cStart = 0
<strong>Output:</strong> [[0,0],[0,1],[0,2],[0,3]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0885.Spiral%20Matrix%20III/images/example_2.png" style="width: 202px; height: 142px;" />
<pre>
<strong>Input:</strong> rows = 5, cols = 6, rStart = 1, cStart = 4
<strong>Output:</strong> [[1,4],[1,5],[2,5],[2,4],[2,3],[1,3],[0,3],[0,4],[0,5],[3,5],[3,4],[3,3],[3,2],[2,2],[1,2],[0,2],[4,5],[4,4],[4,3],[4,2],[4,1],[3,1],[2,1],[1,1],[0,1],[4,0],[3,0],[2,0],[1,0],[0,0]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rows, cols &lt;= 100</code></li>
	<li><code>0 &lt;= rStart &lt; rows</code></li>
	<li><code>0 &lt;= cStart &lt; cols</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bắt đầu từ vị trí cho trước và đi theo vòng xoắn ốc lần lượt hướng đông, nam, tây, bắc. Độ dài các đoạn đi lần lượt là $1,1,2,2,3,3,\ldots$. Lưới có kích thước tối đa $100\times 100$, nên mô phỏng là đủ.
>
> Di chuyển theo hướng và độ dài hiện tại, ghi lại các ô nằm trong lưới. Sau khi đi hướng đông và nam, tăng độ dài thêm một; rồi tiếp tục tăng sau hướng tây và bắc. Dừng khi đã thu thập đủ $rows\cdot cols$ ô.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def spiralMatrixIII(
        self, rows: int, cols: int, rStart: int, cStart: int
    ) -> List[List[int]]:
        ans = [[rStart, cStart]]
        if rows * cols == 1:
            return ans
        k = 1
        while True:
            for dr, dc, dk in [[0, 1, k], [1, 0, k], [0, -1, k + 1], [-1, 0, k + 1]]:
                for _ in range(dk):
                    rStart += dr
                    cStart += dc
                    if 0 <= rStart < rows and 0 <= cStart < cols:
                        ans.append([rStart, cStart])
                        if len(ans) == rows * cols:
                            return ans
            k += 2
```

#### Java

```java
class Solution {
    public int[][] spiralMatrixIII(int rows, int cols, int rStart, int cStart) {
        int cnt = rows * cols;
        int[][] ans = new int[cnt][2];
        ans[0] = new int[] {rStart, cStart};
        if (cnt == 1) {
            return ans;
        }
        for (int k = 1, idx = 1;; k += 2) {
            int[][] dirs = new int[][] {{0, 1, k}, {1, 0, k}, {0, -1, k + 1}, {-1, 0, k + 1}};
            for (int[] dir : dirs) {
                int r = dir[0], c = dir[1], dk = dir[2];
                while (dk-- > 0) {
                    rStart += r;
                    cStart += c;
                    if (rStart >= 0 && rStart < rows && cStart >= 0 && cStart < cols) {
                        ans[idx++] = new int[] {rStart, cStart};
                        if (idx == cnt) {
                            return ans;
                        }
                    }
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> spiralMatrixIII(int rows, int cols, int rStart, int cStart) {
        int cnt = rows * cols;
        vector<vector<int>> ans;
        ans.push_back({rStart, cStart});
        if (cnt == 1) return ans;
        for (int k = 1;; k += 2) {
            vector<vector<int>> dirs = {{0, 1, k}, {1, 0, k}, {0, -1, k + 1}, {-1, 0, k + 1}};
            for (auto& dir : dirs) {
                int r = dir[0], c = dir[1], dk = dir[2];
                while (dk-- > 0) {
                    rStart += r;
                    cStart += c;
                    if (rStart >= 0 && rStart < rows && cStart >= 0 && cStart < cols) {
                        ans.push_back({rStart, cStart});
                        if (ans.size() == cnt) return ans;
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func spiralMatrixIII(rows int, cols int, rStart int, cStart int) [][]int {
	cnt := rows * cols
	ans := [][]int{[]int{rStart, cStart}}
	if cnt == 1 {
		return ans
	}
	for k := 1; ; k += 2 {
		dirs := [][]int{{0, 1, k}, {1, 0, k}, {0, -1, k + 1}, {-1, 0, k + 1}}
		for _, dir := range dirs {
			r, c, dk := dir[0], dir[1], dir[2]
			for dk > 0 {
				rStart += r
				cStart += c
				if rStart >= 0 && rStart < rows && cStart >= 0 && cStart < cols {
					ans = append(ans, []int{rStart, cStart})
					if len(ans) == cnt {
						return ans
					}
				}
				dk--
			}
		}
	}
}
```

#### TypeScript

```ts
function spiralMatrixIII(rows: number, cols: number, rStart: number, cStart: number): number[][] {
    // prettier-ignore
    const dir = [[1,0],[0,1],[-1,0],[0,-1]];
    let [x, y, i, size] = [cStart, rStart, 0, 0];
    const ans: number[][] = [[y, x]];
    const total = rows * cols;

    while (ans.length < total) {
        if (i % 2 === 0) size++;

        for (let j = 0; ans.length < total && j < size; j++) {
            x += dir[i][0];
            y += dir[i][1];

            if (0 <= x && x < cols && 0 <= y && y < rows) {
                ans.push([y, x]);
            }
        }

        i = (i + 1) % 4;
    }

    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number} rows
 * @param {number} cols
 * @param {number} rStart
 * @param {number} cStart
 * @return {number[][]}
 */
var spiralMatrixIII = function (rows, cols, rStart, cStart) {
    // prettier-ignore
    const dir = [[1,0],[0,1],[-1,0],[0,-1]];
    let [x, y, i, size] = [cStart, rStart, 0, 0];
    const ans = [[y, x]];
    const total = rows * cols;

    while (ans.length < total) {
        if (i % 2 === 0) size++;

        for (let j = 0; ans.length < total && j < size; j++) {
            x += dir[i][0];
            y += dir[i][1];

            if (0 <= x && x < cols && 0 <= y && y < rows) {
                ans.push([y, x]);
            }
        }

        i = (i + 1) % 4;
    }

    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
