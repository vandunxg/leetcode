---
comments: true
difficulty: Easy
rating: 1337
source: Biweekly Contest 129 Q1
tags:
    - Array
    - Enumeration
    - Matrix
---

<!-- problem:start -->

# [3127. Make a Square with the Same Color](https://leetcode.com/problems/make-a-square-with-the-same-color)

[中文文档](/solution/3100-3199/3127.Make%20a%20Square%20with%20the%20Same%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận 2D <code>grid</code> có kích thước <code>3 x 3</code>, chỉ gồm các ký tự <code>&#39;B&#39;</code> và <code>&#39;W&#39;</code>. Ký tự <code>&#39;W&#39;</code> biểu thị màu trắng<!-- notionvc: 06a49cc0-a296-4bd2-9bfe-c8818edeb53a -->, còn ký tự <code>&#39;B&#39;</code> biểu thị màu đen<!-- notionvc: 06a49cc0-a296-4bd2-9bfe-c8818edeb53a -->.</p>

<p>Nhiệm vụ của bạn là thay đổi màu của <strong>nhiều nhất một</strong> ô<!-- notionvc: c04cb478-8dd5-49b1-80bb-727c6b1e0232 --> sao cho ma trận có một hình vuông <code>2 x 2</code> mà tất cả các ô đều cùng màu.<!-- notionvc: adf957e1-fa0f-40e5-9a2e-933b95e276a7 --></p>

<p>Trả về <code>true</code> nếu có thể tạo một hình vuông <code>2 x 2</code> gồm các ô cùng màu, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<style type="text/css">.grid-container {
  display: grid;
  grid-template-columns: 30px 30px 30px;
  padding: 10px;
}
.grid-item {
  background-color: black;
  border: 1px solid gray;
  height: 30px;
  font-size: 30px;
  text-align: center;
}
.grid-item-white {
  background-color: white;
}
</style>
<style class="darkreader darkreader--sync" media="screen" type="text/css">
</style>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="grid-container">
<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>
</div>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;B&quot;,&quot;W&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;W&quot;,&quot;W&quot;],[&quot;B&quot;,&quot;W&quot;,&quot;B&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể thực hiện bằng cách thay đổi màu của <code>grid[0][2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="grid-container">
<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>
</div>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;B&quot;,&quot;W&quot;,&quot;B&quot;],[&quot;W&quot;,&quot;B&quot;,&quot;W&quot;],[&quot;B&quot;,&quot;W&quot;,&quot;B&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể thực hiện bằng cách thay đổi nhiều nhất một ô.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="grid-container">
<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>

<div class="grid-item grid-item-white">&nbsp;</div>
</div>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;B&quot;,&quot;W&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;W&quot;,&quot;W&quot;],[&quot;B&quot;,&quot;W&quot;,&quot;W&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>grid</code> đã chứa một hình vuông <code>2 x 2</code> gồm các ô cùng màu.<!-- notionvc: 9a8b2d3d-1e73-457a-abe0-c16af51ad5c2 --></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>grid.length == 3</code></li>
	<li><code>grid[i].length == 3</code></li>
	<li><code>grid[i][j]</code> là <code>&#39;W&#39;</code> hoặc <code>&#39;B&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Bàn cờ có kích thước $3\times 3$, và việc đổi màu một ô cần tạo ra một hình vuông $2\times 2$ đơn sắc. Có thể thử đổi màu từng ô, nhưng kiểm tra trực tiếp từng khung sẽ tự nhiên hơn.
>
> Một hình vuông $2\times 2$ đang không cân bằng (có ba ô cùng màu) sẽ trở nên đồng nhất sau một lần thay đổi; hình vuông vốn đã đồng nhất thì không cần thay đổi. Cả hai trường hợp đều tương đương với điều kiện “số ô đen $\neq$ số ô trắng”.
>
> Liệt kê bốn khung, đếm số ô `W` và `B`, rồi trả về true ngay khi phát hiện sự chênh lệch. Kích thước bàn cờ là hằng số.

<!-- thinking:end -->

Ta có thể liệt kê từng hình vuông $2 \times 2$, đếm số ô màu đen và màu trắng. Nếu hai số lượng không bằng nhau, ta có thể tạo ra một hình vuông gồm các ô cùng màu, nên trả về `true`.

Nếu không, sau khi duyệt xong, trả về `false`.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMakeSquare(self, grid: List[List[str]]) -> bool:
        for i in range(0, 2):
            for j in range(0, 2):
                cnt1 = cnt2 = 0
                for a, b in pairwise((0, 0, 1, 1, 0)):
                    x, y = i + a, j + b
                    cnt1 += grid[x][y] == "W"
                    cnt2 += grid[x][y] == "B"
                if cnt1 != cnt2:
                    return True
        return False
```

#### Java

```java
class Solution {
    public boolean canMakeSquare(char[][] grid) {
        final int[] dirs = {0, 0, 1, 1, 0};
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 2; ++j) {
                int cnt1 = 0, cnt2 = 0;
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    cnt1 += grid[x][y] == 'W' ? 1 : 0;
                    cnt2 += grid[x][y] == 'B' ? 1 : 0;
                }
                if (cnt1 != cnt2) {
                    return true;
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
    bool canMakeSquare(vector<vector<char>>& grid) {
        int dirs[5] = {0, 0, 1, 1, 0};
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 2; ++j) {
                int cnt1 = 0, cnt2 = 0;
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    cnt1 += grid[x][y] == 'W';
                    cnt2 += grid[x][y] == 'B';
                }
                if (cnt1 != cnt2) {
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
func canMakeSquare(grid [][]byte) bool {
	dirs := [5]int{0, 0, 1, 1, 0}
	for i := 0; i < 2; i++ {
		for j := 0; j < 2; j++ {
			cnt1, cnt2 := 0, 0
			for k := 0; k < 4; k++ {
				x, y := i+dirs[k], j+dirs[k+1]
				if grid[x][y] == 'W' {
					cnt1++
				} else {
					cnt2++
				}
			}
			if cnt1 != cnt2 {
				return true
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function canMakeSquare(grid: string[][]): boolean {
    const dirs: number[] = [0, 0, 1, 1, 0];
    for (let i = 0; i < 2; ++i) {
        for (let j = 0; j < 2; ++j) {
            let [cnt1, cnt2] = [0, 0];
            for (let k = 0; k < 4; ++k) {
                const [x, y] = [i + dirs[k], j + dirs[k + 1]];
                if (grid[x][y] === 'W') {
                    ++cnt1;
                } else {
                    ++cnt2;
                }
            }
            if (cnt1 !== cnt2) {
                return true;
            }
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
