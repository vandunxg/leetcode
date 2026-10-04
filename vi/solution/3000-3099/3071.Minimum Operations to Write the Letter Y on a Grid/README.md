---
comments: true
difficulty: Medium
rating: 1689
source: Weekly Contest 387 Q3
tags:
    - Array
    - Hash Table
    - Counting
    - Matrix
---

<!-- problem:start -->

# [3071. Minimum Operations to Write the Letter Y on a Grid](https://leetcode.com/problems/minimum-operations-to-write-the-letter-y-on-a-grid)

[中文文档](/solution/3000-3099/3071.Minimum%20Operations%20to%20Write%20the%20Letter%20Y%20on%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một lưới <strong>được đánh chỉ số từ 0</strong> kích thước <code>n x n</code>, trong đó <code>n</code> là số lẻ và <code>grid[r][c]</code> là <code>0</code>, <code>1</code> hoặc <code>2</code>.</p>

<p>Một ô thuộc chữ <strong>Y</strong> nếu nó nằm trên một trong các đường sau:</p>

<ul>
	<li>Đường chéo bắt đầu từ ô trên cùng bên trái và kết thúc tại ô trung tâm của lưới.</li>
	<li>Đường chéo bắt đầu từ ô trên cùng bên phải và kết thúc tại ô trung tâm của lưới.</li>
	<li>Đường thẳng đứng bắt đầu từ ô trung tâm và kết thúc ở biên dưới của lưới.</li>
</ul>

<p>Chữ <strong>Y</strong> được viết trên lưới khi và chỉ khi:</p>

<ul>
	<li>Tất cả các giá trị tại những ô thuộc chữ Y đều bằng nhau.</li>
	<li>Tất cả các giá trị tại những ô không thuộc chữ Y đều bằng nhau.</li>
	<li>Giá trị tại những ô thuộc chữ Y khác với giá trị tại những ô không thuộc chữ Y.</li>
</ul>

<p>Trả về <em>số thao tác <strong>ít nhất</strong> cần thực hiện để viết chữ Y trên lưới, biết rằng trong một thao tác, bạn có thể thay đổi giá trị của bất kỳ ô nào thành</em> <code>0</code><em>,</em> <code>1</code><em>,</em> <em>hoặc</em> <code>2</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3071.Minimum%20Operations%20to%20Write%20the%20Letter%20Y%20on%20a%20Grid/images/y2.png" style="width: 461px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,2],[1,1,0],[0,1,0]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể viết chữ Y trên lưới bằng cách thực hiện các thay đổi được đánh dấu màu xanh trong hình trên. Sau các thao tác, tất cả các ô thuộc chữ Y, được in đậm, đều có cùng giá trị là 1, còn các ô không thuộc chữ Y đều có giá trị 0.
Có thể chứng minh rằng 3 là số thao tác ít nhất cần thực hiện để viết chữ Y trên lưới.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3071.Minimum%20Operations%20to%20Write%20the%20Letter%20Y%20on%20a%20Grid/images/y3.png" style="width: 701px; height: 201px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,0,1,0],[2,1,0,1,2],[2,2,2,0,1],[2,2,2,2,2],[2,1,2,2,2]]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Ta có thể viết chữ Y trên lưới bằng cách thực hiện các thay đổi được đánh dấu màu xanh trong hình trên. Sau các thao tác, tất cả các ô thuộc chữ Y, được in đậm, đều có cùng giá trị là 0, còn các ô không thuộc chữ Y đều có giá trị 2.
Có thể chứng minh rằng 12 là số thao tác ít nhất cần thực hiện để viết chữ Y trên lưới.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 49 </code></li>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 2</code></li>
	<li><code>n</code> là số lẻ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các ô trên chữ Y phải có cùng một giá trị $a$, còn các ô còn lại có giá trị $b \ne a$. Vì $n \le 49$ là số lẻ nên hình dạng của chữ Y là cố định.
>
> Khi biết số lần xuất hiện của mỗi giá trị trên và ngoài chữ Y, chỉ có $3 \times 2$ cặp $(a,b)$ cần xét, và số thao tác là $n^2$ trừ đi hai số lượng được giữ nguyên.
>
> Ta duyệt một lần để tách các ô thuộc chữ Y và các ô không thuộc chữ Y, sau đó tối thiểu hóa $n^2-\textit{cnt}_1[i]-\textit{cnt}_2[j]$ với $i \ne j$.

<!-- thinking:end -->

Ta dùng hai mảng có độ dài 3, `cnt1` và `cnt2`, để lưu số lần xuất hiện của các giá trị ô lần lượt thuộc `Y` và không thuộc `Y`. Sau đó, ta duyệt `i` và `j`, lần lượt biểu diễn giá trị của các ô thuộc `Y` và không thuộc `Y`, để tính số thao tác ít nhất.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là kích thước của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperationsToWriteY(self, grid: List[List[int]]) -> int:
        n = len(grid)
        cnt1 = Counter()
        cnt2 = Counter()
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                a = i == j and i <= n // 2
                b = i + j == n - 1 and i <= n // 2
                c = j == n // 2 and i >= n // 2
                if a or b or c:
                    cnt1[x] += 1
                else:
                    cnt2[x] += 1
        return min(
            n * n - cnt1[i] - cnt2[j] for i in range(3) for j in range(3) if i != j
        )
```

#### Java

```java
class Solution {
    public int minimumOperationsToWriteY(int[][] grid) {
        int n = grid.length;
        int[] cnt1 = new int[3];
        int[] cnt2 = new int[3];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                boolean a = i == j && i <= n / 2;
                boolean b = i + j == n - 1 && i <= n / 2;
                boolean c = j == n / 2 && i >= n / 2;
                if (a || b || c) {
                    ++cnt1[grid[i][j]];
                } else {
                    ++cnt2[grid[i][j]];
                }
            }
        }
        int ans = n * n;
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (i != j) {
                    ans = Math.min(ans, n * n - cnt1[i] - cnt2[j]);
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
    int minimumOperationsToWriteY(vector<vector<int>>& grid) {
        int n = grid.size();
        int cnt1[3]{};
        int cnt2[3]{};
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                bool a = i == j && i <= n / 2;
                bool b = i + j == n - 1 && i <= n / 2;
                bool c = j == n / 2 && i >= n / 2;
                if (a || b || c) {
                    ++cnt1[grid[i][j]];
                } else {
                    ++cnt2[grid[i][j]];
                }
            }
        }
        int ans = n * n;
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (i != j) {
                    ans = min(ans, n * n - cnt1[i] - cnt2[j]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumOperationsToWriteY(grid [][]int) int {
	n := len(grid)
	cnt1 := [3]int{}
	cnt2 := [3]int{}
	for i, row := range grid {
		for j, x := range row {
			a := i == j && i <= n/2
			b := i+j == n-1 && i <= n/2
			c := j == n/2 && i >= n/2
			if a || b || c {
				cnt1[x]++
			} else {
				cnt2[x]++
			}
		}
	}
	ans := n * n
	for i := 0; i < 3; i++ {
		for j := 0; j < 3; j++ {
			if i != j {
				ans = min(ans, n*n-cnt1[i]-cnt2[j])
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumOperationsToWriteY(grid: number[][]): number {
    const n = grid.length;
    const cnt1: number[] = Array(3).fill(0);
    const cnt2: number[] = Array(3).fill(0);
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            const a = i === j && i <= n >> 1;
            const b = i + j === n - 1 && i <= n >> 1;
            const c = j === n >> 1 && i >= n >> 1;
            if (a || b || c) {
                ++cnt1[grid[i][j]];
            } else {
                ++cnt2[grid[i][j]];
            }
        }
    }
    let ans = n * n;
    for (let i = 0; i < 3; ++i) {
        for (let j = 0; j < 3; ++j) {
            if (i !== j) {
                ans = Math.min(ans, n * n - cnt1[i] - cnt2[j]);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
