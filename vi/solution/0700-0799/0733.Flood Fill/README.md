---
comments: true
difficulty: Easy
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [733. Flood Fill](https://leetcode.com/problems/flood-fill)

[中文文档](/solution/0700-0799/0733.Flood%20Fill/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ảnh được biểu diễn bằng lưới số nguyên <code>m x n</code> <code>image</code>, trong đó <code>image[i][j]</code> là giá trị pixel tại vị trí đó. Bạn cũng được cho ba số nguyên <code>sr</code>, <code>sc</code> và <code>color</code>. Hãy thực hiện thao tác <strong>flood fill</strong> trên ảnh, bắt đầu từ pixel <code>image[sr][sc]</code>.</p>

<p>Để thực hiện <strong>flood fill</strong>:</p>

<ol>
	<li>Bắt đầu từ pixel khởi điểm và đổi màu của nó thành <code>color</code>.</li>
	<li>Thực hiện tương tự với mỗi pixel <strong>kề trực tiếp</strong> (có chung cạnh với pixel ban đầu theo chiều ngang hoặc dọc) và có <strong>cùng màu</strong> với pixel khởi điểm.</li>
	<li>Tiếp tục <strong>lặp lại</strong> quá trình này bằng cách kiểm tra các pixel lân cận của những pixel <em>đã cập nhật</em> và đổi màu nếu chúng trùng với màu ban đầu của pixel khởi điểm.</li>
	<li>Quá trình <strong>dừng lại</strong> khi <strong>không còn</strong> pixel kề nào có màu ban đầu cần cập nhật.</li>
</ol>

<p>Trả về ảnh đã được <strong>chỉnh sửa</strong> sau khi thực hiện flood fill.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[2,2,2],[2,2,0],[2,0,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0733.Flood%20Fill/images/flood1-grid.jpg" style="width: 613px; height: 253px;" /></p>

<p>Bắt đầu từ pixel ở giữa ảnh tại vị trí <code>(sr, sc) = (1, 1)</code> (pixel màu đỏ), tất cả pixel có đường nối cùng màu với pixel khởi điểm (các pixel màu xanh) đều được đổi sang màu mới.</p>

<p>Lưu ý, pixel ở góc dưới <strong>không</strong> được đổi thành màu 2 vì nó không kết nối theo chiều ngang hoặc dọc với pixel khởi điểm.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[0,0,0],[0,0,0]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Pixel khởi điểm đã có màu 0, trùng với màu đích. Vì vậy, ảnh không thay đổi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == image.length</code></li>
	<li><code>n == image[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>0 &lt;= image[i][j], color &lt; 2<sup>16</sup></code></li>
	<li><code>0 &lt;= sr &lt; m</code></li>
	<li><code>0 &lt;= sc &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Đổi màu thành phần liên thông 4 hướng chứa pixel khởi điểm. Lưới có kích thước nhỏ. Nếu pixel khởi điểm đã có màu đích thì không được đệ quy, nếu không sẽ lặp vô hạn.
>
> Thành phần này tạo thành một grid graph. DFS đổi màu từng ô lân cận còn có màu ban đầu $\textit{oc}$; màu mới đóng vai trò đánh dấu đã thăm.
>
> Lưu màu ban đầu $\textit{oc}$ và chỉ bắt đầu đệ quy từ $(\textit{sr},\textit{sc})$ khi nó khác $\textit{color}$.

<!-- thinking:end -->

Gọi màu ban đầu của pixel là $\textit{oc}$. Nếu $\textit{oc}$ khác màu đích $\textit{color}$, ta bắt đầu depth-first search từ $(\textit{sr}, \textit{sc})$ để đổi màu tất cả pixel phù hợp thành màu đích.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của mảng 2 chiều $\textit{image}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def floodFill(
        self, image: List[List[int]], sr: int, sc: int, color: int
    ) -> List[List[int]]:
        def dfs(i: int, j: int):
            image[i][j] = color
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < len(image) and 0 <= y < len(image[0]) and image[x][y] == oc:
                    dfs(x, y)

        oc = image[sr][sc]
        if oc != color:
            dirs = (-1, 0, 1, 0, -1)
            dfs(sr, sc)
        return image
```

#### Java

```java
class Solution {
    private int[][] image;
    private int oc;
    private int color;
    private final int[] dirs = {-1, 0, 1, 0, -1};

    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        oc = image[sr][sc];
        if (oc == color) {
            return image;
        }
        this.image = image;
        this.color = color;
        dfs(sr, sc);
        return image;
    }

    private void dfs(int i, int j) {
        image[i][j] = color;
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < image.length && y >= 0 && y < image[0].length && image[x][y] == oc) {
                dfs(x, y);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int m = image.size(), n = image[0].size();
        int oc = image[sr][sc];
        if (oc == color) {
            return image;
        }
        const int dirs[5] = {-1, 0, 1, 0, -1};
        auto dfs = [&](this auto&& dfs, int i, int j) -> void {
            image[i][j] = color;
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && image[x][y] == oc) {
                    dfs(x, y);
                }
            }
        };
        dfs(sr, sc);
        return image;
    }
};
```

#### Go

```go
func floodFill(image [][]int, sr int, sc int, color int) [][]int {
	m, n := len(image), len(image[0])
	oc := image[sr][sc]
	if oc == color {
		return image
	}

	dirs := []int{-1, 0, 1, 0, -1}

	var dfs func(i, j int)
	dfs = func(i, j int) {
		image[i][j] = color
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && image[x][y] == oc {
				dfs(x, y)
			}
		}
	}

	dfs(sr, sc)
	return image
}
```

#### TypeScript

```ts
function floodFill(image: number[][], sr: number, sc: number, color: number): number[][] {
    const [m, n] = [image.length, image[0].length];
    const oc = image[sr][sc];
    if (oc === color) {
        return image;
    }

    const dirs = [-1, 0, 1, 0, -1];

    const dfs = (i: number, j: number): void => {
        image[i][j] = color;
        for (let k = 0; k < 4; k++) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < m && y >= 0 && y < n && image[x][y] === oc) {
                dfs(x, y);
            }
        }
    };

    dfs(sr, sc);
    return image;
}
```

#### Rust

```rust
impl Solution {
    pub fn flood_fill(mut image: Vec<Vec<i32>>, sr: i32, sc: i32, color: i32) -> Vec<Vec<i32>> {
        let m = image.len();
        let n = image[0].len();
        let sr = sr as usize;
        let sc = sc as usize;

        let oc = image[sr][sc];
        if oc == color {
            return image;
        }
        let dirs = [-1, 0, 1, 0, -1];
        fn dfs(
            image: &mut Vec<Vec<i32>>,
            i: usize,
            j: usize,
            oc: i32,
            color: i32,
            m: usize,
            n: usize,
            dirs: &[i32; 5],
        ) {
            image[i][j] = color;
            for k in 0..4 {
                let x = i as isize + dirs[k] as isize;
                let y = j as isize + dirs[k + 1] as isize;
                if x >= 0 && x < m as isize && y >= 0 && y < n as isize {
                    let x = x as usize;
                    let y = y as usize;
                    if image[x][y] == oc {
                        dfs(image, x, y, oc, color, m, n, dirs);
                    }
                }
            }
        }

        dfs(&mut image, sr, sc, oc, color, m, n, &dirs);
        image
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng đệ quy; một thành phần lớn có thể cần stack $O(mn)$. Ta có thể thực hiện flood tương tự theo từng lớp.
>
> BFS từ điểm bắt đầu: lấy một pixel khỏi queue, đổi màu các pixel lân cận còn có màu $\textit{oc}$ rồi đưa chúng vào queue. Độ phức tạp tiệm cận không đổi và không dùng call stack.

<!-- thinking:end -->

Trước tiên, kiểm tra màu pixel khởi điểm có bằng màu đích không. Nếu bằng, trả về ảnh ban đầu ngay. Nếu không, dùng breadth-first search bắt đầu từ $(\textit{sr}, \textit{sc})$ để đổi màu tất cả pixel phù hợp thành màu đích.

Cụ thể, tạo queue $\textit{q}$ và thêm pixel khởi điểm $(\textit{sr}, \textit{sc})$ vào queue. Sau đó, liên tục lấy pixel $(i, j)$ khỏi queue, đổi màu nó thành màu đích và thêm vào queue các pixel ở bốn hướng (trên, dưới, trái, phải) có cùng màu ban đầu với pixel khởi điểm. Khi queue rỗng, thao tác flood fill hoàn tất.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của mảng 2 chiều $\textit{image}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def floodFill(
        self, image: List[List[int]], sr: int, sc: int, color: int
    ) -> List[List[int]]:
        if image[sr][sc] == color:
            return image
        q = deque([(sr, sc)])
        oc = image[sr][sc]
        image[sr][sc] = color
        dirs = (-1, 0, 1, 0, -1)
        while q:
            i, j = q.popleft()
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < len(image) and 0 <= y < len(image[0]) and image[x][y] == oc:
                    q.append((x, y))
                    image[x][y] = color
        return image
```

#### Java

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        if (image[sr][sc] == color) {
            return image;
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {sr, sc});
        int oc = image[sr][sc];
        image[sr][sc] = color;
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < image.length && y >= 0 && y < image[0].length
                    && image[x][y] == oc) {
                    q.offer(new int[] {x, y});
                    image[x][y] = color;
                }
            }
        }
        return image;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        if (image[sr][sc] == color) return image;
        int oc = image[sr][sc];
        image[sr][sc] = color;
        queue<pair<int, int>> q;
        q.push({sr, sc});
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            auto [a, b] = q.front();
            q.pop();
            for (int k = 0; k < 4; ++k) {
                int x = a + dirs[k];
                int y = b + dirs[k + 1];
                if (x >= 0 && x < image.size() && y >= 0 && y < image[0].size() && image[x][y] == oc) {
                    q.push({x, y});
                    image[x][y] = color;
                }
            }
        }
        return image;
    }
};
```

#### Go

```go
func floodFill(image [][]int, sr int, sc int, color int) [][]int {
	if image[sr][sc] == color {
		return image
	}
	oc := image[sr][sc]
	q := [][]int{[]int{sr, sc}}
	image[sr][sc] = color
	dirs := []int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		for k := 0; k < 4; k++ {
			x, y := p[0]+dirs[k], p[1]+dirs[k+1]
			if x >= 0 && x < len(image) && y >= 0 && y < len(image[0]) && image[x][y] == oc {
				q = append(q, []int{x, y})
				image[x][y] = color
			}
		}
	}
	return image
}
```

#### TypeScript

```ts
function floodFill(image: number[][], sr: number, sc: number, color: number): number[][] {
    if (image[sr][sc] === color) {
        return image;
    }

    const oc = image[sr][sc];
    image[sr][sc] = color;

    const q: [number, number][] = [];
    q.push([sr, sc]);

    const dirs = [-1, 0, 1, 0, -1];
    const [m, n] = [image.length, image[0].length];

    while (q.length > 0) {
        const [a, b] = q.shift()!;
        for (let k = 0; k < 4; ++k) {
            const x = a + dirs[k];
            const y = b + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && image[x][y] === oc) {
                q.push([x, y]);
                image[x][y] = color;
            }
        }
    }

    return image;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn flood_fill(mut image: Vec<Vec<i32>>, sr: i32, sc: i32, color: i32) -> Vec<Vec<i32>> {
        let m = image.len();
        let n = image[0].len();
        let (sr, sc) = (sr as usize, sc as usize);

        if image[sr][sc] == color {
            return image;
        }

        let oc = image[sr][sc];
        image[sr][sc] = color;

        let mut q = VecDeque::new();
        q.push_back((sr, sc));

        let dirs = [-1, 0, 1, 0, -1];

        while let Some((i, j)) = q.pop_front() {
            for k in 0..4 {
                let x = i as isize + dirs[k] as isize;
                let y = j as isize + dirs[k + 1] as isize;

                if x >= 0 && x < m as isize && y >= 0 && y < n as isize {
                    let (x, y) = (x as usize, y as usize);
                    if image[x][y] == oc {
                        q.push_back((x, y));
                        image[x][y] = color;
                    }
                }
            }
        }

        image
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
