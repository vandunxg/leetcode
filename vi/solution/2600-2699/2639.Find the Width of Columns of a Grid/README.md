---
comments: true
difficulty: Easy
rating: 1282
source: Biweekly Contest 102 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [2639. Find the Width of Columns of a Grid](https://leetcode.com/problems/find-the-width-of-columns-of-a-grid)

[中文文档](/solution/2600-2699/2639.Find%20the%20Width%20of%20Columns%20of%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên <strong>được đánh chỉ số từ 0</strong> có kích thước <code>m x n</code> là <code>grid</code>. Độ rộng của một cột là <strong>độ dài </strong>lớn nhất của các số nguyên trong cột đó.</p>

<ul>
	<li>Ví dụ, nếu <code>grid = [[-10], [3], [12]]</code>, độ rộng của cột duy nhất là <code>3</code> vì <code>-10</code> có độ dài <code>3</code>.</li>
</ul>

<p>Hãy trả về <em>một mảng số nguyên</em> <code>ans</code> <em>có kích thước</em> <code>n</code>, <em>trong đó</em> <code>ans[i]</code> <em>là độ rộng của</em> <code>i<sup>th</sup></code> <em>cột</em>.</p>

<p><strong>Độ dài</strong> của một số nguyên <code>x</code> có <code>len</code> chữ số bằng <code>len</code> nếu <code>x</code> không âm, và bằng <code>len + 1</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1],[22],[333]]
<strong>Đầu ra:</strong> [3]
<strong>Giải thích:</strong> Trong cột thứ 0<sup>th</sup>, 333 có độ dài 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[-15,1,3],[15,7,12],[5,6,-2]]
<strong>Đầu ra:</strong> [3,1,2]
<strong>Giải thích:</strong>
Trong cột thứ 0<sup>th</sup>, chỉ có -15 có độ dài 3.
Trong cột thứ 1<sup>st</sup>, tất cả các số nguyên đều có độ dài 1.
Trong cột thứ 2<sup>nd</sup>, cả 12 và -2 đều có độ dài 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100 </code></li>
	<li><code>-10<sup>9</sup> &lt;= grid[r][c] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Độ rộng của một cột là độ dài của biểu diễn thập phân dài nhất trong cột đó, bao gồm cả dấu trừ. Ma trận có kích thước tối đa là $100 \times 100$, nên chỉ cần lấy độ dài `str` lớn nhất trong mỗi cột.

<!-- thinking:end -->

Gọi số cột của ma trận là $n$, ta tạo một mảng $ans$ có độ dài $n$, trong đó $ans[i]$ biểu diễn độ rộng của cột thứ $i$. Ban đầu, $ans[i] = 0$.

Ta duyệt qua từng hàng trong ma trận. Với mỗi phần tử trong từng hàng, ta tính độ dài chuỗi $w$, rồi cập nhật giá trị của $ans[j]$ thành $\max(ans[j], w)$.

Sau khi duyệt qua tất cả các hàng, mỗi phần tử trong mảng $ans$ chính là độ rộng của cột tương ứng.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(\log M)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận, còn $M$ là giá trị tuyệt đối của phần tử lớn nhất trong ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findColumnWidth(self, grid: List[List[int]]) -> List[int]:
        return [max(len(str(x)) for x in col) for col in zip(*grid)]
```

#### Java

```java
class Solution {
    public int[] findColumnWidth(int[][] grid) {
        int n = grid[0].length;
        int[] ans = new int[n];
        for (var row : grid) {
            for (int j = 0; j < n; ++j) {
                int w = String.valueOf(row[j]).length();
                ans[j] = Math.max(ans[j], w);
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
    vector<int> findColumnWidth(vector<vector<int>>& grid) {
        int n = grid[0].size();
        vector<int> ans(n);
        for (auto& row : grid) {
            for (int j = 0; j < n; ++j) {
                int w = to_string(row[j]).size();
                ans[j] = max(ans[j], w);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findColumnWidth(grid [][]int) []int {
	ans := make([]int, len(grid[0]))
	for _, row := range grid {
		for j, x := range row {
			w := len(strconv.Itoa(x))
			ans[j] = max(ans[j], w)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findColumnWidth(grid: number[][]): number[] {
    const n = grid[0].length;
    const ans: number[] = new Array(n).fill(0);
    for (const row of grid) {
        for (let j = 0; j < n; ++j) {
            const w: number = String(row[j]).length;
            ans[j] = Math.max(ans[j], w);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_column_width(grid: Vec<Vec<i32>>) -> Vec<i32> {
        let mut ans = vec![0; grid[0].len()];

        for row in grid.iter() {
            for (j, num) in row.iter().enumerate() {
                let width = num.to_string().len() as i32;
                ans[j] = std::cmp::max(ans[j], width);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
