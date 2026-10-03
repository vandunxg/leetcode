---
comments: true
difficulty: Medium
rating: 1718
source: Weekly Contest 260 Q2
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [2017. Grid Game](https://leetcode.com/problems/grid-game)

[中文文档](/solution/2000-2099/2017.Grid%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng 2 chiều <strong>đánh chỉ số từ 0</strong> <code>grid</code> có kích thước <code>2 x n</code>, trong đó <code>grid[r][c]</code> biểu thị số điểm tại vị trí <code>(r, c)</code> trên ma trận. Có hai robot đang chơi một trò chơi trên ma trận này.</p>

<p>Ban đầu, cả hai robot đều ở vị trí <code>(0, 0)</code> và muốn đi đến <code>(1, n-1)</code>. Mỗi robot chỉ được phép di chuyển sang <strong>phải</strong> (<code>(r, c)</code> đến <code>(r, c + 1)</code>) hoặc đi <strong>xuống </strong>(<code>(r, c)</code> đến <code>(r + 1, c)</code>).</p>

<p>Khi trò chơi bắt đầu, robot <strong>thứ nhất</strong> di chuyển từ <code>(0, 0)</code> đến <code>(1, n-1)</code> và thu thập toàn bộ điểm từ các ô trên đường đi. Với mọi ô <code>(r, c)</code> nằm trên đường đi, <code>grid[r][c]</code> được gán bằng <code>0</code>. Sau đó, robot <strong>thứ hai</strong> di chuyển từ <code>(0, 0)</code> đến <code>(1, n-1)</code> và thu thập các điểm trên đường đi. Lưu ý rằng hai đường đi có thể giao nhau.</p>

<p>Robot <strong>thứ nhất</strong> muốn <strong>tối thiểu hóa</strong> số điểm mà robot <strong>thứ hai</strong> thu thập được. Ngược lại, robot <strong>thứ hai </strong>muốn <strong>tối đa hóa</strong> số điểm mà nó thu thập được. Nếu cả hai robot đều chơi <strong>tối ưu</strong>, hãy trả về <em><b>số điểm</b> mà robot <strong>thứ hai</strong> thu thập được.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2017.Grid%20Game/images/a1.png" style="width: 388px; height: 103px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,5,4],[1,5,1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Đường đi tối ưu của robot thứ nhất được biểu diễn bằng màu đỏ, còn đường đi tối ưu của robot thứ hai được biểu diễn bằng màu xanh.
Các ô được robot thứ nhất đi qua được gán bằng 0.
Robot thứ hai sẽ thu thập 0 + 0 + 4 + 0 = 4 điểm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2017.Grid%20Game/images/a2.png" style="width: 384px; height: 105px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,3,1],[8,5,2]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Đường đi tối ưu của robot thứ nhất được biểu diễn bằng màu đỏ, còn đường đi tối ưu của robot thứ hai được biểu diễn bằng màu xanh.
Các ô được robot thứ nhất đi qua được gán bằng 0.
Robot thứ hai sẽ thu thập 0 + 3 + 1 + 0 = 4 điểm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2017.Grid%20Game/images/a3.png" style="width: 493px; height: 103px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,3,1,15],[1,3,3,1]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích: </strong>Đường đi tối ưu của robot thứ nhất được biểu diễn bằng màu đỏ, còn đường đi tối ưu của robot thứ hai được biểu diễn bằng màu xanh.
Các ô được robot thứ nhất đi qua được gán bằng 0.
Robot thứ hai sẽ thu thập 0 + 1 + 3 + 3 + 0 = 7 điểm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>grid.length == 2</code></li>
	<li><code>n == grid[r].length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= grid[r][c] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Trên lưới $2 \times n$, robot thứ nhất chỉ có thể đi xuống tại một cột duy nhất $j$. Khi đó, robot thứ hai sẽ chọn giá trị lớn hơn giữa tổng hậu tố của hàng thứ nhất sau $j$ và tổng tiền tố của hàng thứ hai trước $j$.
>
> Với $n \le 5 \times 10^4$, ta cần tính hai tổng này trong $O(1)$. Duy trì tổng hậu tố còn lại của hàng thứ nhất $s_1$ và tổng tiền tố của hàng thứ hai $s_2$; số điểm của robot thứ hai là $\max(s_1,s_2)$.
>
> Robot thứ nhất sẽ tối thiểu hóa giá trị này theo $j$.

<!-- thinking:end -->

Ta nhận thấy rằng nếu xác định được vị trí $j$ nơi robot thứ nhất đi xuống, thì đường đi tối ưu của robot thứ hai cũng được xác định. Đường đi tối ưu của robot thứ hai là tổng các phần tử của hàng thứ nhất từ $j+1$ đến $n-1$, hoặc tổng các phần tử của hàng thứ hai từ $0$ đến $j-1$, lấy giá trị lớn hơn trong hai tổng này.

Trước hết, ta tính tổng hậu tố của các điểm trong hàng thứ nhất, ký hiệu là $s_1$, và tổng tiền tố của các điểm trong hàng thứ hai, ký hiệu là $s_2$. Ban đầu, $s_1 = \sum_{j=0}^{n-1} grid[0][j]$, $s_2 = 0$.

Sau đó, ta duyệt qua vị trí $j$ nơi robot thứ nhất đi xuống. Tại thời điểm này, ta cập nhật $s_1 = s_1 - grid[0][j]$. Khi đó, tổng điểm trên đường đi tối ưu của robot thứ hai là $max(s_1, s_2)$. Ta lấy giá trị nhỏ nhất của $max(s_1, s_2)$ với mọi $j$. Tiếp theo, ta cập nhật $s_2 = s_2 + grid[1][j]$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$. Ở đây, $n$ là số cột trong lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gridGame(self, grid: List[List[int]]) -> int:
        ans = inf
        s1, s2 = sum(grid[0]), 0
        for j, v in enumerate(grid[0]):
            s1 -= v
            ans = min(ans, max(s1, s2))
            s2 += grid[1][j]
        return ans
```

#### Java

```java
class Solution {
    public long gridGame(int[][] grid) {
        long ans = Long.MAX_VALUE;
        long s1 = 0, s2 = 0;
        for (int v : grid[0]) {
            s1 += v;
        }
        int n = grid[0].length;
        for (int j = 0; j < n; ++j) {
            s1 -= grid[0][j];
            ans = Math.min(ans, Math.max(s1, s2));
            s2 += grid[1][j];
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    long long gridGame(vector<vector<int>>& grid) {
        ll ans = LONG_MAX;
        int n = grid[0].size();
        ll s1 = 0, s2 = 0;
        for (int& v : grid[0]) s1 += v;
        for (int j = 0; j < n; ++j) {
            s1 -= grid[0][j];
            ans = min(ans, max(s1, s2));
            s2 += grid[1][j];
        }
        return ans;
    }
};
```

#### Go

```go
func gridGame(grid [][]int) int64 {
	ans := math.MaxInt64
	s1, s2 := 0, 0
	for _, v := range grid[0] {
		s1 += v
	}
	for j, v := range grid[0] {
		s1 -= v
		ans = min(ans, max(s1, s2))
		s2 += grid[1][j]
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function gridGame(grid: number[][]): number {
    let ans = Number.MAX_SAFE_INTEGER;
    let s1 = grid[0].reduce((a, b) => a + b, 0);
    let s2 = 0;
    for (let j = 0; j < grid[0].length; ++j) {
        s1 -= grid[0][j];
        ans = Math.min(ans, Math.max(s1, s2));
        s2 += grid[1][j];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
