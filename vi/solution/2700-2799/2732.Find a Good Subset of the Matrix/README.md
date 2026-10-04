---
comments: true
difficulty: Hard
rating: 2239
source: Biweekly Contest 106 Q4
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [2732. Find a Good Subset of the Matrix](https://leetcode.com/problems/find-a-good-subset-of-the-matrix)

[中文文档](/solution/2700-2799/2732.Find%20a%20Good%20Subset%20of%20the%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận nhị phân <code>m x n</code> được <strong>đánh chỉ số từ 0</strong> là <code>grid</code>.</p>

<p>Ta gọi một tập con <strong>không rỗng</strong> của các hàng là <strong>tốt</strong> nếu tổng các phần tử ở mỗi cột của tập con không vượt quá một nửa độ dài của tập con.</p>

<p>Cụ thể hơn, nếu độ dài của tập con các hàng được chọn là <code>k</code>, thì tổng các phần tử ở mỗi cột phải nhỏ hơn hoặc bằng <code>floor(k / 2)</code>.</p>

<p>Trả về <em>một mảng số nguyên chứa các chỉ số hàng của một tập con tốt theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Nếu có nhiều tập con tốt, bạn có thể trả về bất kỳ tập nào trong số đó. Nếu không có tập con tốt nào, trả về một mảng rỗng.</p>

<p>Một <strong>tập con</strong> của các hàng trong ma trận <code>grid</code> là bất kỳ ma trận nào có thể thu được bằng cách xóa một số hàng (có thể không xóa hoặc xóa tất cả các hàng) khỏi <code>grid</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,1,1,0],[0,0,0,1],[1,1,1,1]]
<strong>Đầu ra:</strong> [0,1]
<strong>Giải thích:</strong> Ta có thể chọn hàng thứ 0<sup>th</sup> và hàng thứ 1<sup>st</sup> để tạo thành một tập con tốt của các hàng.
Độ dài của tập con được chọn là 2.
- Tổng của cột thứ 0<sup>th</sup> là 0 + 0 = 0, không vượt quá một nửa độ dài của tập con.
- Tổng của cột thứ 1<sup>st</sup> là 1 + 0 = 1, không vượt quá một nửa độ dài của tập con.
- Tổng của cột thứ 2<sup>nd</sup> là 1 + 0 = 1, không vượt quá một nửa độ dài của tập con.
- Tổng của cột thứ 3<sup>rd</sup> là 0 + 1 = 1, không vượt quá một nửa độ dài của tập con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0]]
<strong>Đầu ra:</strong> [0]
<strong>Giải thích:</strong> Ta có thể chọn hàng thứ 0<sup>th</sup> để tạo thành một tập con tốt của các hàng.
Độ dài của tập con được chọn là 1.
- Tổng của cột thứ 0<sup>th</sup> là 0, không vượt quá một nửa độ dài của tập con.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,1,1]]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không thể chọn tập con nào của các hàng để tạo thành một tập con tốt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= n &lt;= 5</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Chọn các hàng sao cho tổng ở mỗi cột không vượt quá một nửa số hàng được chọn. Có thể có $10^4$ hàng và nhiều nhất $5$ cột, nên không thể liệt kê các tập con, nhưng chỉ có $2^n$ mask của các cột.
>
> Chỉ có thể xét các kích thước $1$ hoặc $2$: một hàng toàn số 0 tự nó đã thỏa mãn; hai hàng có phép AND bit bằng $0$ thì mỗi cột có nhiều nhất một số $1$. Với các trường hợp này, ta mã hóa mỗi hàng thành một mask rồi kiểm tra.

<!-- thinking:end -->

Ta có thể xét số hàng $k$ được chọn cho đáp án từ nhỏ đến lớn.

- Nếu $k = 1$, tổng lớn nhất của mỗi cột là $0$. Vì vậy, phải tồn tại một hàng mà tất cả phần tử đều là $0$, nếu không điều kiện sẽ không thể thỏa mãn.
- Nếu $k = 2$, tổng lớn nhất của mỗi cột là $1$. Phải tồn tại hai hàng sao cho kết quả AND bit của các phần tử tương ứng trong hai hàng bằng $0$, nếu không điều kiện sẽ không thể thỏa mãn.
- Nếu $k = 3$, tổng lớn nhất của mỗi cột cũng là $1$. Nếu điều kiện với $k = 2$ không được thỏa mãn, thì điều kiện với $k = 3$ chắc chắn cũng không được thỏa mãn. Vì vậy, không cần xét các trường hợp $k > 2$ và $k$ là số lẻ.
- Nếu $k = 4$, tổng lớn nhất của mỗi cột là $2$. Trường hợp này chắc chắn xảy ra khi điều kiện với $k = 2$ không được thỏa mãn, nghĩa là với mọi cặp gồm 2 hàng được chọn, tồn tại ít nhất một cột có tổng bằng $2$. Khi chọn 2 hàng bất kỳ trong 4 hàng, có tổng cộng $C_4^2 = 6$ cách chọn, nên có ít nhất $6$ cột có tổng bằng $2$. Vì số cột $n \le 5$, phải có ít nhất một cột có tổng lớn hơn $2$, nên điều kiện với $k = 4$ cũng không được thỏa mãn.
- Với $k > 4$ và $k$ là số chẵn, ta có thể rút ra kết luận tương tự: chắc chắn $k$ không thỏa mãn điều kiện.

Tóm lại, ta chỉ cần xét các trường hợp $k = 1$ và $k = 2$. Nghĩa là kiểm tra xem có hàng nào chỉ gồm các số $0$ hay không, hoặc có tồn tại hai hàng mà kết quả AND bit của chúng bằng $0$ hay không.

Độ phức tạp thời gian là $O(m \times n + 4^n)$, độ phức tạp không gian là $O(2^n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def goodSubsetofBinaryMatrix(self, grid: List[List[int]]) -> List[int]:
        g = {}
        for i, row in enumerate(grid):
            mask = 0
            for j, x in enumerate(row):
                mask |= x << j
            if mask == 0:
                return [i]
            g[mask] = i
        for a, i in g.items():
            for b, j in g.items():
                if (a & b) == 0:
                    return sorted([i, j])
        return []
```

#### Java

```java
class Solution {
    public List<Integer> goodSubsetofBinaryMatrix(int[][] grid) {
        Map<Integer, Integer> g = new HashMap<>();
        for (int i = 0; i < grid.length; ++i) {
            int mask = 0;
            for (int j = 0; j < grid[0].length; ++j) {
                mask |= grid[i][j] << j;
            }
            if (mask == 0) {
                return List.of(i);
            }
            g.put(mask, i);
        }
        for (var e1 : g.entrySet()) {
            for (var e2 : g.entrySet()) {
                if ((e1.getKey() & e2.getKey()) == 0) {
                    int i = e1.getValue(), j = e2.getValue();
                    return List.of(Math.min(i, j), Math.max(i, j));
                }
            }
        }
        return List.of();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> goodSubsetofBinaryMatrix(vector<vector<int>>& grid) {
        unordered_map<int, int> g;
        for (int i = 0; i < grid.size(); ++i) {
            int mask = 0;
            for (int j = 0; j < grid[0].size(); ++j) {
                mask |= grid[i][j] << j;
            }
            if (mask == 0) {
                return {i};
            }
            g[mask] = i;
        }
        for (auto& [a, i] : g) {
            for (auto& [b, j] : g) {
                if ((a & b) == 0) {
                    return {min(i, j), max(i, j)};
                }
            }
        }
        return {};
    }
};
```

#### Go

```go
func goodSubsetofBinaryMatrix(grid [][]int) []int {
	g := map[int]int{}
	for i, row := range grid {
		mask := 0
		for j, x := range row {
			mask |= x << j
		}
		if mask == 0 {
			return []int{i}
		}
		g[mask] = i
	}
	for a, i := range g {
		for b, j := range g {
			if a&b == 0 {
				return []int{min(i, j), max(i, j)}
			}
		}
	}
	return []int{}
}
```

#### TypeScript

```ts
function goodSubsetofBinaryMatrix(grid: number[][]): number[] {
    const g: Map<number, number> = new Map();
    const m = grid.length;
    const n = grid[0].length;
    for (let i = 0; i < m; ++i) {
        let mask = 0;
        for (let j = 0; j < n; ++j) {
            mask |= grid[i][j] << j;
        }
        if (!mask) {
            return [i];
        }
        g.set(mask, i);
    }
    for (const [a, i] of g.entries()) {
        for (const [b, j] of g.entries()) {
            if ((a & b) === 0) {
                return [Math.min(i, j), Math.max(i, j)];
            }
        }
    }
    return [];
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn good_subsetof_binary_matrix(grid: Vec<Vec<i32>>) -> Vec<i32> {
        let mut g: HashMap<i32, i32> = HashMap::new();
        for (i, row) in grid.iter().enumerate() {
            let mut mask = 0;
            for (j, &x) in row.iter().enumerate() {
                mask |= x << j;
            }
            if mask == 0 {
                return vec![i as i32];
            }
            g.insert(mask, i as i32);
        }

        for (&a, &i) in g.iter() {
            for (&b, &j) in g.iter() {
                if (a & b) == 0 {
                    return vec![i.min(j), i.max(j)];
                }
            }
        }

        vec![]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
