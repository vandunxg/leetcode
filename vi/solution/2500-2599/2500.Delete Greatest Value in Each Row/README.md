---
comments: true
difficulty: Easy
rating: 1309
source: Weekly Contest 323 Q1
tags:
    - Array
    - Matrix
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2500. Delete Greatest Value in Each Row](https://leetcode.com/problems/delete-greatest-value-in-each-row)

[中文文档](/solution/2500-2599/2500.Delete%20Greatest%20Value%20in%20Each%20Row/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <code>m x n</code> <code>grid</code> gồm các số nguyên dương.</p>

<p>Thực hiện thao tác sau cho đến khi <code>grid</code> trở thành rỗng:</p>

<ul>
	<li>Xóa phần tử có giá trị lớn nhất khỏi mỗi hàng. Nếu có nhiều phần tử như vậy, có thể xóa bất kỳ phần tử nào.</li>
	<li>Cộng giá trị lớn nhất trong các phần tử đã xóa vào đáp án.</li>
</ul>

<p><strong>Lưu ý</strong> rằng số cột giảm đi một sau mỗi thao tác.</p>

<p>Trả về <em>đáp án sau khi thực hiện các thao tác được mô tả ở trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2500.Delete%20Greatest%20Value%20in%20Each%20Row/images/q1ex1.jpg" style="width: 600px; height: 135px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,4],[3,3,1]]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Biểu đồ phía trên thể hiện các giá trị bị xóa trong từng bước.
- Trong thao tác đầu tiên, ta xóa 4 khỏi hàng đầu tiên và 3 khỏi hàng thứ hai (lưu ý rằng có hai ô có giá trị 3 và có thể xóa bất kỳ ô nào trong số đó). Ta cộng 4 vào đáp án.
- Trong thao tác thứ hai, ta xóa 2 khỏi hàng đầu tiên và 3 khỏi hàng thứ hai. Ta cộng 3 vào đáp án.
- Trong thao tác thứ ba, ta xóa 1 khỏi hàng đầu tiên và 1 khỏi hàng thứ hai. Ta cộng 1 vào đáp án.
Đáp án cuối cùng = 4 + 3 + 1 = 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2500.Delete%20Greatest%20Value%20in%20Each%20Row/images/q1ex2.jpg" style="width: 83px; height: 83px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[10]]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Biểu đồ phía trên thể hiện các giá trị bị xóa trong từng bước.
- Trong thao tác đầu tiên, ta xóa 10 khỏi hàng đầu tiên. Ta cộng 10 vào đáp án.
Đáp án cuối cùng = 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa giá trị lớn nhất hiện tại của từng hàng rồi cộng giá trị lớn nhất trong các giá trị vừa xóa. Quét để tìm các giá trị lớn nhất trong mỗi lượt tốn $O(mn)$, thực hiện $n$ lượt; với $m,n\le 50$ thì vẫn đủ nhanh, nhưng các giá trị bị xóa trong một hàng chính là các phần tử của hàng đó theo thứ tự giảm dần.
>
> Sắp xếp từng hàng theo thứ tự tăng dần. Khi đó, thao tác thứ $j$ tương ứng với phần tử thứ $j$ của mỗi hàng (tính từ bên phải), nên đáp án là tổng các giá trị lớn nhất theo từng cột. Sau khi sắp xếp, dùng $\textit{zip}$ để gom các cột và không cần xóa thêm phần tử nào.

<!-- thinking:end -->

Vì mỗi thao tác gồm việc xóa giá trị lớn nhất của từng hàng rồi cộng giá trị lớn nhất vào đáp án, ta có thể sắp xếp từng hàng trước.

Tiếp theo, ta duyệt qua từng cột, lấy giá trị lớn nhất trong mỗi cột và cộng vào đáp án.

Độ phức tạp thời gian là $O(m \times n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def deleteGreatestValue(self, grid: List[List[int]]) -> int:
        for row in grid:
            row.sort()
        return sum(max(col) for col in zip(*grid))
```

#### Java

```java
class Solution {
    public int deleteGreatestValue(int[][] grid) {
        for (var row : grid) {
            Arrays.sort(row);
        }
        int ans = 0;
        for (int j = 0; j < grid[0].length; ++j) {
            int t = 0;
            for (int i = 0; i < grid.length; ++i) {
                t = Math.max(t, grid[i][j]);
            }
            ans += t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int deleteGreatestValue(vector<vector<int>>& grid) {
        for (auto& row : grid) {
            ranges::sort(row);
        }
        int ans = 0;
        for (int j = 0; j < grid[0].size(); ++j) {
            int t = 0;
            for (int i = 0; i < grid.size(); ++i) {
                t = max(t, grid[i][j]);
            }
            ans += t;
        }
        return ans;
    }
};
```

#### Go

```go
func deleteGreatestValue(grid [][]int) (ans int) {
	for _, row := range grid {
		sort.Ints(row)
	}
	for j := range grid[0] {
		t := 0
		for i := range grid {
			if t < grid[i][j] {
				t = grid[i][j]
			}
		}
		ans += t
	}
	return
}
```

#### TypeScript

```ts
function deleteGreatestValue(grid: number[][]): number {
    for (const row of grid) {
        row.sort((a, b) => a - b);
    }

    let ans = 0;
    for (let j = 0; j < grid[0].length; ++j) {
        let t = 0;
        for (let i = 0; i < grid.length; ++i) {
            t = Math.max(t, grid[i][j]);
        }
        ans += t;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn delete_greatest_value(grid: Vec<Vec<i32>>) -> i32 {
        let mut grid = grid;
        for i in 0..grid.len() {
            grid[i].sort();
        }

        let mut ans = 0;
        for j in 0..grid[0].len() {
            let mut mx = 0;

            for i in 0..grid.len() {
                if grid[i][j] > mx {
                    mx = grid[i][j];
                }
            }

            ans += mx;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
