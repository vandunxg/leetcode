---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [08.10. Color Fill](https://leetcode.cn/problems/color-fill-lcci)

[中文文档](/lcci/08.10.Color%20Fill/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy triển khai hàm “paint fill”, một chức năng thường thấy trong nhiều phần mềm chỉnh sửa ảnh. Cụ thể, cho một màn hình (được biểu diễn bằng mảng hai chiều các màu), một điểm và một màu mới, hãy tô vùng xung quanh cho đến khi màu thay đổi so với màu ban đầu.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào</strong>:

image = [[1,1,1],[1,1,0],[1,0,1]]

sr = 1, sc = 1, newColor = 2

<strong>Đầu ra</strong>: [[2,2,2],[2,2,0],[2,0,1]]

<strong>Giải thích</strong>:

Từ trung tâm của ảnh (với vị trí (sr, sc) = (1, 1)), tất cả pixel được nối

bằng một đường đi có cùng màu với pixel ban đầu sẽ được tô bằng màu mới.

Lưu ý rằng góc dưới cùng không được tô thành 2 vì nó không được nối với

pixel ban đầu theo bốn hướng.</pre>

<p><b>Lưu ý:</b></p>

<ul>
	<li>Độ dài của <code>image</code> và <code>image[0]</code> sẽ nằm trong khoảng <code>[1, 50]</code>.</li>
	<li>Pixel bắt đầu đã cho sẽ thỏa mãn <code>0 &lt;= sr &lt; image.length</code> và <code>0 &lt;= sc &lt; image[0].length</code>.</li>
	<li>Giá trị của mỗi màu trong <code>image[i][j]</code> và <code>newColor</code> sẽ là một số nguyên trong khoảng <code>[0, 65535]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Thành phần 4-liên thông của $(sr,sc)$ phải được đổi màu. Quét toàn bộ ảnh sẽ đổi màu cả những ô cùng màu nhưng không liên thông.
>
> Chỉ các ô có màu $oc$ và có thể đi tới từ điểm bắt đầu mới cần quan tâm; DFS sẽ thăm chúng.
>
> Với vị trí nằm ngoài phạm vi, màu khác $oc$, hoặc màu mới đã được tô, ta trả về; nếu không, tô màu rồi đệ quy tới bốn ô lân cận. Việc kiểm tra màu mới cũng ngăn đệ quy vô hạn khi $oc$ bằng $newColor$.

<!-- thinking:end -->

Ta thiết kế một hàm $dfs(i, j)$ để bắt đầu tô màu từ $(i, j)$. Nếu $(i, j)$ không nằm trong phạm vi ảnh, hoặc màu của $(i, j)$ không phải màu ban đầu, hoặc màu của $(i, j)$ đã được tô bằng màu mới thì trả về. Nếu không, tô màu của $(i, j)$ bằng màu mới, sau đó đệ quy tìm kiếm theo bốn hướng: lên, xuống, trái và phải của $(i, j)$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ảnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def floodFill(
        self, image: List[List[int]], sr: int, sc: int, newColor: int
    ) -> List[List[int]]:
        def dfs(i, j):
            if (
                not 0 <= i < m
                or not 0 <= j < n
                or image[i][j] != oc
                or image[i][j] == newColor
            ):
                return
            image[i][j] = newColor
            for a, b in pairwise(dirs):
                dfs(i + a, j + b)

        dirs = (-1, 0, 1, 0, -1)
        m, n = len(image), len(image[0])
        oc = image[sr][sc]
        dfs(sr, sc)
        return image
```

#### Java

```java
class Solution {
    private int[] dirs = {-1, 0, 1, 0, -1};
    private int[][] image;
    private int nc;
    private int oc;

    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        nc = newColor;
        oc = image[sr][sc];
        this.image = image;
        dfs(sr, sc);
        return image;
    }

    private void dfs(int i, int j) {
        if (i < 0 || i >= image.length || j < 0 || j >= image[0].length || image[i][j] != oc
            || image[i][j] == nc) {
            return;
        }
        image[i][j] = nc;
        for (int k = 0; k < 4; ++k) {
            dfs(i + dirs[k], j + dirs[k + 1]);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int m = image.size(), n = image[0].size();
        int oc = image[sr][sc];
        int dirs[5] = {-1, 0, 1, 0, -1};
        function<void(int, int)> dfs = [&](int i, int j) {
            if (i < 0 || i >= m || j < 0 || j >= n || image[i][j] != oc || image[i][j] == newColor) {
                return;
            }
            image[i][j] = newColor;
            for (int k = 0; k < 4; ++k) {
                dfs(i + dirs[k], j + dirs[k + 1]);
            }
        };
        dfs(sr, sc);
        return image;
    }
};
```

#### Go

```go
func floodFill(image [][]int, sr int, sc int, newColor int) [][]int {
	oc := image[sr][sc]
	m, n := len(image), len(image[0])
	dirs := []int{-1, 0, 1, 0, -1}
	var dfs func(i, j int)
	dfs = func(i, j int) {
		if i < 0 || i >= m || j < 0 || j >= n || image[i][j] != oc || image[i][j] == newColor {
			return
		}
		image[i][j] = newColor
		for k := 0; k < 4; k++ {
			dfs(i+dirs[k], j+dirs[k+1])
		}
	}
	dfs(sr, sc)
	return image
}
```

#### TypeScript

```ts
function floodFill(image: number[][], sr: number, sc: number, newColor: number): number[][] {
    const dfs = (i: number, j: number): void => {
        if (i < 0 || i >= m) {
            return;
        }
        if (j < 0 || j >= n) {
            return;
        }
        if (image[i][j] !== oc || image[i][j] === nc) {
            return;
        }
        image[i][j] = nc;
        for (let k = 0; k < 4; ++k) {
            dfs(i + dirs[k], j + dirs[k + 1]);
        }
    };
    const dirs: number[] = [-1, 0, 1, 0, -1];
    const [m, n] = [image.length, image[0].length];
    const oc = image[sr][sc];
    const nc = newColor;
    dfs(sr, sc);
    return image;
}
```

#### Rust

```rust
impl Solution {
    fn dfs(i: usize, j: usize, target: i32, new_color: i32, image: &mut Vec<Vec<i32>>) {
        if image[i][j] != target {
            return;
        }
        image[i][j] = new_color;
        if i != 0 {
            Self::dfs(i - 1, j, target, new_color, image);
        }
        if j != 0 {
            Self::dfs(i, j - 1, target, new_color, image);
        }
        if i + 1 != image.len() {
            Self::dfs(i + 1, j, target, new_color, image);
        }
        if j + 1 != image[0].len() {
            Self::dfs(i, j + 1, target, new_color, image);
        }
    }

    pub fn flood_fill(mut image: Vec<Vec<i32>>, sr: i32, sc: i32, new_color: i32) -> Vec<Vec<i32>> {
        let (sr, sc) = (sr as usize, sc as usize);
        let target = image[sr][sc];
        if target == new_color {
            return image;
        }
        Self::dfs(sr, sc, target, new_color, &mut image);
        image
    }
}
```

#### Swift

```swift
class Solution {
    private var dirs = [-1, 0, 1, 0, -1]
    private var image: [[Int]] = []
    private var nc: Int = 0
    private var oc: Int = 0

    func floodFill(_ image: inout [[Int]], _ sr: Int, _ sc: Int, _ newColor: Int) -> [[Int]] {
        self.nc = newColor
        self.oc = image[sr][sc]
        self.image = image
        dfs(sr, sc)
        return self.image
    }

    private func dfs(_ i: Int, _ j: Int) {
        if i < 0 || i >= image.count || j < 0 || j >= image[0].count || image[i][j] != oc || image[i][j] == nc {
            return
        }
        image[i][j] = nc
        for k in 0..<4 {
            dfs(i + dirs[k], j + dirs[k + 1])
        }
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
> DFS sử dụng call stack, có thể sâu tới $mn$ trên một vùng liên thông dài và ngoằn ngoèo.
>
> BFS mở rộng cùng bốn ô lân cận từ một queue, cho kết quả tương đương mà không cần đệ quy sâu.

<!-- thinking:end -->

Ta có thể sử dụng phương pháp tìm kiếm theo chiều rộng. Bắt đầu từ điểm ban đầu, tô màu của điểm ban đầu bằng màu mới, sau đó thêm điểm ban đầu vào queue. Mỗi lần lấy một điểm ra khỏi queue, ta thêm các điểm ở bốn hướng: lên, xuống, trái và phải vào queue, cho đến khi queue rỗng.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ảnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def floodFill(
        self, image: List[List[int]], sr: int, sc: int, newColor: int
    ) -> List[List[int]]:
        if image[sr][sc] == newColor:
            return image
        q = deque([(sr, sc)])
        oc = image[sr][sc]
        image[sr][sc] = newColor
        dirs = (-1, 0, 1, 0, -1)
        while q:
            i, j = q.popleft()
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < len(image) and 0 <= y < len(image[0]) and image[x][y] == oc:
                    q.append((x, y))
                    image[x][y] = newColor
        return image
```

#### Java

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) {
            return image;
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {sr, sc});
        int oc = image[sr][sc];
        image[sr][sc] = newColor;
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < image.length && y >= 0 && y < image[0].length
                    && image[x][y] == oc) {
                    q.offer(new int[] {x, y});
                    image[x][y] = newColor;
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
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) return image;
        int oc = image[sr][sc];
        image[sr][sc] = newColor;
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
                    image[x][y] = newColor;
                }
            }
        }
        return image;
    }
};
```

#### Go

```go
func floodFill(image [][]int, sr int, sc int, newColor int) [][]int {
	if image[sr][sc] == newColor {
		return image
	}
	oc := image[sr][sc]
	q := [][]int{[]int{sr, sc}}
	image[sr][sc] = newColor
	dirs := []int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		for k := 0; k < 4; k++ {
			x, y := p[0]+dirs[k], p[1]+dirs[k+1]
			if x >= 0 && x < len(image) && y >= 0 && y < len(image[0]) && image[x][y] == oc {
				q = append(q, []int{x, y})
				image[x][y] = newColor
			}
		}
	}
	return image
}
```

#### TypeScript

```ts
function floodFill(image: number[][], sr: number, sc: number, newColor: number): number[][] {
    if (image[sr][sc] === newColor) {
        return image;
    }
    const q: number[][] = [[sr, sc]];
    const oc = image[sr][sc];
    image[sr][sc] = newColor;
    const dirs: number[] = [-1, 0, 1, 0, -1];
    while (q.length) {
        const [i, j] = q.pop()!;
        for (let k = 0; k < 4; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < image.length && y >= 0 && y < image[0].length && image[x][y] === oc) {
                q.push([x, y]);
                image[x][y] = newColor;
            }
        }
    }
    return image;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
