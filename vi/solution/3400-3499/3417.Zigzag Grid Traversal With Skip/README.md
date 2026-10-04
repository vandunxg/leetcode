---
comments: true
difficulty: Easy
rating: 1290
source: Weekly Contest 432 Q1
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [3417. Zigzag Grid Traversal With Skip](https://leetcode.com/problems/zigzag-grid-traversal-with-skip)

[中文文档](/solution/3400-3499/3417.Zigzag%20Grid%20Traversal%20With%20Skip/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2D <code>m x n</code> là <strong>các số nguyên dương</strong> <code>grid</code>.</p>

<p>Nhiệm vụ của bạn là duyệt <code>grid</code> theo dạng <strong>zigzag</strong>, đồng thời bỏ qua các ô <strong>xen kẽ</strong>.</p>

<p>Việc duyệt theo dạng zigzag được định nghĩa bởi các bước sau:</p>

<ul>
	<li>Bắt đầu tại ô trên cùng bên trái <code>(0, 0)</code>.</li>
	<li>Di chuyển <em>sang phải</em> trong một hàng cho đến khi đi đến cuối hàng.</li>
	<li>Đi xuống hàng tiếp theo, sau đó duyệt <em>sang trái</em> cho đến khi đi đến đầu hàng.</li>
	<li>Tiếp tục <strong>luân phiên</strong> giữa việc duyệt sang phải và sang trái cho đến khi đã duyệt qua mọi hàng.</li>
</ul>

<p><strong>Lưu ý</strong> rằng bạn <strong>phải bỏ qua</strong> mọi ô <em>xen kẽ</em> trong quá trình duyệt.</p>

<p>Trả về một mảng số nguyên <code>result</code> chứa các giá trị của những ô đã đi qua trong quá trình duyệt zigzag có bỏ qua, theo đúng <strong>thứ tự</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3417.Zigzag%20Grid%20Traversal%20With%20Skip/images/4012_example0.png" style="width: 200px; height: 200px;" /></strong></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[2,1],[2,1],[2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3417.Zigzag%20Grid%20Traversal%20With%20Skip/images/4012_example1.png" style="width: 200px; height: 240px;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2,3],[4,5,6],[7,8,9]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,3,5,7,9]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3417.Zigzag%20Grid%20Traversal%20With%20Skip/images/4012_example2.png" style="width: 260px; height: 250px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == grid.length &lt;= 50</code></li>
	<li><code>2 &lt;= m == grid[i].length &lt;= 50</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 2500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì grid có kích thước tối đa là $50\times 50$, ta có thể thực hiện đúng đường đi đã nêu. Các hàng chẵn được duyệt từ trái sang phải, các hàng lẻ từ phải sang trái, và ta giữ lại một ô sau mỗi hai ô.
>
> Việc xây dựng toàn bộ đường đi zigzag rồi lấy các vị trí chẵn cần thêm một mảng. Ta có thể dùng một cờ bật tắt để quyết định có thêm ô hiện tại hay không trong lúc duyệt.
>
> Ta duyệt lần lượt từng hàng, đảo ngược tại chỗ các hàng lẻ, và chỉ thêm một ô khi $\textit{ok}$ là true, sau đó đảo trạng thái của $\textit{ok}$ sau mỗi ô. Cờ này được duy trì xuyên suốt các hàng, nên không cần một chỉ số toàn cục.

<!-- thinking:end -->

Ta duyệt từng hàng. Nếu chỉ số của hàng hiện tại là số lẻ, ta đảo ngược các phần tử trong hàng đó. Sau đó, ta duyệt các phần tử của hàng và thêm chúng vào mảng kết quả theo các quy tắc được nêu trong đề bài.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của mảng 2D $\textit{grid}$. Không tính phần bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def zigzagTraversal(self, grid: List[List[int]]) -> List[int]:
        ok = True
        ans = []
        for i, row in enumerate(grid):
            if i % 2:
                row.reverse()
            for x in row:
                if ok:
                    ans.append(x)
                ok = not ok
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> zigzagTraversal(int[][] grid) {
        boolean ok = true;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < grid.length; ++i) {
            if (i % 2 == 1) {
                reverse(grid[i]);
            }
            for (int x : grid[i]) {
                if (ok) {
                    ans.add(x);
                }
                ok = !ok;
            }
        }
        return ans;
    }

    private void reverse(int[] nums) {
        for (int i = 0, j = nums.length - 1; i < j; ++i, --j) {
            int t = nums[i];
            nums[i] = nums[j];
            nums[j] = t;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> zigzagTraversal(vector<vector<int>>& grid) {
        vector<int> ans;
        bool ok = true;
        for (int i = 0; i < grid.size(); ++i) {
            if (i % 2 != 0) {
                ranges::reverse(grid[i]);
            }
            for (int x : grid[i]) {
                if (ok) {
                    ans.push_back(x);
                }
                ok = !ok;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func zigzagTraversal(grid [][]int) (ans []int) {
	ok := true
	for i, row := range grid {
		if i%2 != 0 {
			slices.Reverse(row)
		}
		for _, x := range row {
			if ok {
				ans = append(ans, x)
			}
			ok = !ok
		}
	}
	return
}
```

#### TypeScript

```ts
function zigzagTraversal(grid: number[][]): number[] {
    const ans: number[] = [];
    let ok: boolean = true;
    for (let i = 0; i < grid.length; ++i) {
        if (i % 2) {
            grid[i].reverse();
        }
        for (const x of grid[i]) {
            if (ok) {
                ans.push(x);
            }
            ok = !ok;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
