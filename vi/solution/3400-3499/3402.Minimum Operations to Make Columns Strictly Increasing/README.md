---
comments: true
difficulty: Easy
rating: 1245
source: Weekly Contest 430 Q1
tags:
    - Greedy
    - Array
    - Matrix
---

<!-- problem:start -->

# [3402. Minimum Operations to Make Columns Strictly Increasing](https://leetcode.com/problems/minimum-operations-to-make-columns-strictly-increasing)

[中文文档](/solution/3400-3499/3402.Minimum%20Operations%20to%20Make%20Columns%20Strictly%20Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <code>m x n</code> <code>grid</code> gồm các số nguyên <b>không âm</b>.</p>

<p>Trong một thao tác, bạn có thể tăng giá trị của bất kỳ <code>grid[i][j]</code> nào lên 1.</p>

<p>Hãy trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để tất cả các cột của <code>grid</code> <strong>tăng nghiêm ngặt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[3,2],[1,3],[3,4],[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Để làm cho cột <code>0<sup>th</sup></code> tăng nghiêm ngặt, ta có thể thực hiện 3 thao tác trên <code>grid[1][0]</code>, 2 thao tác trên <code>grid[2][0]</code> và 6 thao tác trên <code>grid[3][0]</code>.</li>
    <li>Để làm cho cột <code>1<sup>st</sup></code> tăng nghiêm ngặt, ta có thể thực hiện 4 thao tác trên <code>grid[3][1]</code>.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3402.Minimum%20Operations%20to%20Make%20Columns%20Strictly%20Increasing/images/firstexample.png" style="width: 200px; height: 347px;" /></div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[3,2,1],[2,1,0],[1,2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Để làm cho cột <code>0<sup>th</sup></code> tăng nghiêm ngặt, ta có thể thực hiện 2 thao tác trên <code>grid[1][0]</code> và 4 thao tác trên <code>grid[2][0]</code>.</li>
    <li>Để làm cho cột <code>1<sup>st</sup></code> tăng nghiêm ngặt, ta có thể thực hiện 2 thao tác trên <code>grid[1][1]</code> và 2 thao tác trên <code>grid[2][1]</code>.</li>
    <li>Để làm cho cột <code>2<sup>nd</sup></code> tăng nghiêm ngặt, ta có thể thực hiện 2 thao tác trên <code>grid[1][2]</code>.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3402.Minimum%20Operations%20to%20Make%20Columns%20Strictly%20Increasing/images/secondexample.png" style="width: 300px; height: 257px;" /></div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>m == grid.length</code></li>
    <li><code>n == grid[i].length</code></li>
    <li><code>1 &lt;= m, n &lt;= 50</code></li>
    <li><code>0 &lt;= grid[i][j] &lt; 2500</code></li>
</ul>

<p>&nbsp;</p>
<div class="spoiler">
<div>
<pre>

&nbsp;</pre>
</div>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính theo từng cột

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ có thể tăng các ô, và các cột độc lập với nhau. Việc thử mọi giá trị cuối cùng khả dĩ trong một cột là không cần thiết: chỉ cần duyệt một lần qua ma trận là đủ với giới hạn kích thước đã cho.
>
> Điều kiện tăng nghiêm ngặt làm cận dưới của các ô phía sau tăng lên. Khi một ô bị buộc phải tăng, mọi ô tiếp theo trong cùng cột sẽ thừa hưởng một cận dưới lớn hơn.
>
> Vì vậy, ta duyệt từng cột từ trên xuống dưới và duy trì giá trị cuối cùng trước đó $\textit{pre}$. Nếu giá trị hiện tại đã lớn hơn $\textit{pre}$, ta giữ nguyên; nếu không, ta tăng nó lên $\textit{pre}+1$ và cộng phần chênh lệch vào đáp án. Cách này tạo ra số lần tăng nhỏ nhất cho từng cột một cách độc lập.

<!-- thinking:end -->

Ta có thể duyệt ma trận theo từng cột. Với mỗi cột, ta tính số thao tác nhỏ nhất cần thiết để cột đó tăng nghiêm ngặt. Cụ thể, với mỗi cột, ta duy trì biến $\textit{pre}$ biểu diễn giá trị của phần tử trước đó trong cột hiện tại. Sau đó, ta duyệt cột hiện tại từ trên xuống dưới. Với phần tử hiện tại $\textit{cur}$, nếu $\textit{pre} < \textit{cur}$, điều đó có nghĩa là phần tử hiện tại đã lớn hơn phần tử trước đó, nên ta chỉ cần cập nhật $\textit{pre} = \textit{cur}$. Ngược lại, ta cần tăng phần tử hiện tại lên $\textit{pre} + 1$ và cộng số lần tăng vào đáp án.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận $\textit{grid}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, grid: List[List[int]]) -> int:
        ans = 0
        for col in zip(*grid):
            pre = -1
            for cur in col:
                if pre < cur:
                    pre = cur
                else:
                    pre += 1
                    ans += pre - cur
        return ans
```

#### Java

```java
class Solution {
    public int minimumOperations(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            int pre = -1;
            for (int i = 0; i < m; ++i) {
                int cur = grid[i][j];
                if (pre < cur) {
                    pre = cur;
                } else {
                    ++pre;
                    ans += pre - cur;
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
    int minimumOperations(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            int pre = -1;
            for (int i = 0; i < m; ++i) {
                int cur = grid[i][j];
                if (pre < cur) {
                    pre = cur;
                } else {
                    ++pre;
                    ans += pre - cur;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumOperations(grid [][]int) (ans int) {
    m, n := len(grid), len(grid[0])
    for j := 0; j < n; j++ {
        pre := -1
        for i := 0; i < m; i++ {
            cur := grid[i][j]
            if pre < cur {
                pre = cur
            } else {
                pre++
                ans += pre - cur
            }
        }
    }
    return
}
```

#### TypeScript

```ts
function minimumOperations(grid: number[][]): number {
    const [m, n] = [grid.length, grid[0].length];
    let ans: number = 0;
    for (let j = 0; j < n; ++j) {
        let pre: number = -1;
        for (let i = 0; i < m; ++i) {
            const cur = grid[i][j];
            if (pre < cur) {
                pre = cur;
            } else {
                ++pre;
                ans += pre - cur;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
