---
comments: true
difficulty: Hard
rating: 2387
source: Weekly Contest 347 Q4
tags:
    - Memoization
    - Array
    - Hash Table
    - Binary Search
    - Dynamic Programming
    - Matrix
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [2713. Maximum Strictly Increasing Cells in a Matrix](https://leetcode.com/problems/maximum-strictly-increasing-cells-in-a-matrix)

[Tài liệu tiếng Trung](/solution/2700-2799/2713.Maximum%20Strictly%20Increasing%20Cells%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>m x n</code> <code>mat</code> được đánh chỉ số bắt đầu từ <strong>1</strong>, bạn có thể chọn bất kỳ ô nào trong ma trận làm <strong>ô bắt đầu</strong>.</p>

<p>Từ ô bắt đầu, bạn có thể di chuyển đến bất kỳ ô nào khác <strong>trong</strong> <strong>cùng hàng hoặc cột</strong>, nhưng chỉ khi giá trị của ô đích <strong>lớn hơn nghiêm ngặt</strong> giá trị của ô hiện tại. Bạn có thể lặp lại quá trình này nhiều lần nhất có thể, di chuyển từ ô này sang ô khác cho đến khi không thể thực hiện thêm bước di chuyển nào.</p>

<p>Nhiệm vụ của bạn là tìm <strong>số ô lớn nhất</strong> mà bạn có thể đi qua trong ma trận khi bắt đầu từ một ô bất kỳ.</p>

<p>Trả về <em>một số nguyên biểu thị số ô lớn nhất có thể đi qua.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2713.Maximum%20Strictly%20Increasing%20Cells%20in%20a%20Matrix/images/diag1drawio.png" style="width: 200px; height: 176px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[3,1],[3,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình ảnh cho thấy ta có thể đi qua 2 ô khi bắt đầu từ hàng 1, cột 2. Có thể chứng minh rằng dù bắt đầu từ đâu, ta cũng không thể đi qua nhiều hơn 2 ô, nên đáp án là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2713.Maximum%20Strictly%20Increasing%20Cells%20in%20a%20Matrix/images/diag3drawio.png" style="width: 200px; height: 176px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[1,1],[1,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Vì các ô phải tăng nghiêm ngặt, trong ví dụ này ta chỉ có thể đi qua một ô.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2713.Maximum%20Strictly%20Increasing%20Cells%20in%20a%20Matrix/images/diag4drawio.png" style="width: 350px; height: 250px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[3,1,6],[-9,5,7]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Hình ảnh trên cho thấy ta có thể đi qua 4 ô khi bắt đầu từ hàng 2, cột 1. Có thể chứng minh rằng dù bắt đầu từ đâu, ta cũng không thể đi qua nhiều hơn 4 ô, nên đáp án là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length&nbsp;</code></li>
	<li><code>n == mat[i].length&nbsp;</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup>&nbsp;&lt;= mat[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Từ một ô, ta có thể di chuyển đến một ô có giá trị lớn hơn nghiêm ngặt trong cùng hàng hoặc cột, và cần tìm đường đi dài nhất. Có tối đa $10^5$ ô; việc xây dựng đồ thị tường minh rồi memoization sẽ phải xử lý số lượng cạnh đi ra lớn.
>
> Một ô có giá trị nhỏ hơn không thể được đi tới từ một ô có giá trị lớn hơn, vì vậy ta xử lý các giá trị theo thứ tự tăng dần. Các ô có cùng giá trị không bao giờ di chuyển đến nhau: tính mỗi ô dựa trên giá trị lớn nhất hiện tại của hàng/cột, rồi mới ghi các giá trị lớn nhất mới để các ô bằng nhau không ảnh hưởng lẫn nhau.

<!-- thinking:end -->

Dựa trên mô tả bài toán, giá trị của các ô mà ta đi qua theo thứ tự phải tăng nghiêm ngặt. Do đó, ta có thể sử dụng một hash table $g$ để lưu vị trí của tất cả các ô tương ứng với từng giá trị, rồi duyệt từ giá trị nhỏ nhất đến lớn nhất.

Trong quá trình này, ta duy trì hai mảng `rowMax` và `colMax`, lần lượt lưu độ dài tăng dần lớn nhất của từng hàng và từng cột. Ban đầu, mọi phần tử của hai mảng này đều bằng $0$.

Với tất cả vị trí ô tương ứng với mỗi giá trị, ta duyệt chúng theo thứ tự vị trí. Với mỗi vị trí $(i, j)$, ta có thể tính độ dài tăng dần lớn nhất kết thúc tại vị trí đó là $1 + \max(\textit{rowMax}[i], \textit{colMax}[j])$, cập nhật đáp án, sau đó cập nhật `rowMax[i]` và `colMax[j]`.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(m \times n \times \log(m \times n))$, và độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxIncreasingCells(self, mat: List[List[int]]) -> int:
        m, n = len(mat), len(mat[0])
        g = defaultdict(list)
        for i in range(m):
            for j in range(n):
                g[mat[i][j]].append((i, j))
        rowMax = [0] * m
        colMax = [0] * n
        ans = 0
        for _, pos in sorted(g.items()):
            mx = []
            for i, j in pos:
                mx.append(1 + max(rowMax[i], colMax[j]))
                ans = max(ans, mx[-1])
            for k, (i, j) in enumerate(pos):
                rowMax[i] = max(rowMax[i], mx[k])
                colMax[j] = max(colMax[j], mx[k])
        return ans
```

#### Java

```java
class Solution {
    public int maxIncreasingCells(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        TreeMap<Integer, List<int[]>> g = new TreeMap<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                g.computeIfAbsent(mat[i][j], k -> new ArrayList<>()).add(new int[] {i, j});
            }
        }
        int[] rowMax = new int[m];
        int[] colMax = new int[n];
        int ans = 0;
        for (var e : g.entrySet()) {
            var pos = e.getValue();
            int[] mx = new int[pos.size()];
            int k = 0;
            for (var p : pos) {
                int i = p[0], j = p[1];
                mx[k] = Math.max(rowMax[i], colMax[j]) + 1;
                ans = Math.max(ans, mx[k++]);
            }
            for (k = 0; k < mx.length; ++k) {
                int i = pos.get(k)[0], j = pos.get(k)[1];
                rowMax[i] = Math.max(rowMax[i], mx[k]);
                colMax[j] = Math.max(colMax[j], mx[k]);
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
    int maxIncreasingCells(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        map<int, vector<pair<int, int>>> g;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                g[mat[i][j]].emplace_back(i, j);
            }
        }
        vector<int> rowMax(m);
        vector<int> colMax(n);
        int ans = 0;
        for (auto& [_, pos] : g) {
            vector<int> mx;
            for (auto& [i, j] : pos) {
                mx.push_back(max(rowMax[i], colMax[j]) + 1);
                ans = max(ans, mx.back());
            }
            for (int k = 0; k < mx.size(); ++k) {
                auto& [i, j] = pos[k];
                rowMax[i] = max(rowMax[i], mx[k]);
                colMax[j] = max(colMax[j], mx[k]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxIncreasingCells(mat [][]int) (ans int) {
	m, n := len(mat), len(mat[0])
	g := map[int][][2]int{}
	for i, row := range mat {
		for j, v := range row {
			g[v] = append(g[v], [2]int{i, j})
		}
	}
	nums := make([]int, 0, len(g))
	for k := range g {
		nums = append(nums, k)
	}
	sort.Ints(nums)
	rowMax := make([]int, m)
	colMax := make([]int, n)
	for _, k := range nums {
		pos := g[k]
		mx := make([]int, len(pos))
		for i, p := range pos {
			mx[i] = max(rowMax[p[0]], colMax[p[1]]) + 1
			ans = max(ans, mx[i])
		}
		for i, p := range pos {
			rowMax[p[0]] = max(rowMax[p[0]], mx[i])
			colMax[p[1]] = max(colMax[p[1]], mx[i])
		}
	}
	return
}
```

#### TypeScript

```ts
function maxIncreasingCells(mat: number[][]): number {
    const m = mat.length;
    const n = mat[0].length;
    const g: { [key: number]: [number, number][] } = {};

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (!g[mat[i][j]]) {
                g[mat[i][j]] = [];
            }
            g[mat[i][j]].push([i, j]);
        }
    }

    const rowMax = Array(m).fill(0);
    const colMax = Array(n).fill(0);
    let ans = 0;

    const sortedKeys = Object.keys(g)
        .map(Number)
        .sort((a, b) => a - b);

    for (const key of sortedKeys) {
        const pos = g[key];
        const mx: number[] = [];

        for (const [i, j] of pos) {
            mx.push(1 + Math.max(rowMax[i], colMax[j]));
            ans = Math.max(ans, mx[mx.length - 1]);
        }

        for (let k = 0; k < pos.length; k++) {
            const [i, j] = pos[k];
            rowMax[i] = Math.max(rowMax[i], mx[k]);
            colMax[j] = Math.max(colMax[j], mx[k]);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
