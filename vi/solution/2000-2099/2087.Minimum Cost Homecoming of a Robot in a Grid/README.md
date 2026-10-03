---
comments: true
difficulty: Medium
rating: 1743
source: Biweekly Contest 66 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2087. Minimum Cost Homecoming of a Robot in a Grid](https://leetcode.com/problems/minimum-cost-homecoming-of-a-robot-in-a-grid)

[中文文档](/solution/2000-2099/2087.Minimum%20Cost%20Homecoming%20of%20a%20Robot%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lưới <code>m x n</code>, trong đó <code>(0, 0)</code> là ô trên cùng bên trái và <code>(m - 1, n - 1)</code> là ô dưới cùng bên phải. Bạn được cho một mảng số nguyên <code>startPos</code>, trong đó <code>startPos = [start<sub>row</sub>, start<sub>col</sub>]</code> cho biết rằng <strong>ban đầu</strong>, một <strong>robot</strong> nằm ở ô <code>(start<sub>row</sub>, start<sub>col</sub>)</code>. Bạn cũng được cho một mảng số nguyên <code>homePos</code>, trong đó <code>homePos = [home<sub>row</sub>, home<sub>col</sub>]</code> cho biết rằng <strong>nhà</strong> của robot nằm ở ô <code>(home<sub>row</sub>, home<sub>col</sub>)</code>.</p>

<p>Robot cần đi về nhà. Nó có thể di chuyển một ô theo bốn hướng: <strong>trái</strong>, <strong>phải</strong>, <strong>lên</strong> hoặc <strong>xuống</strong>, và không thể đi ra ngoài biên. Mỗi bước di chuyển đều tốn một chi phí. Ngoài ra, bạn được cho hai mảng số nguyên <strong>đánh chỉ số từ 0</strong>: <code>rowCosts</code> có độ dài <code>m</code> và <code>colCosts</code> có độ dài <code>n</code>.</p>

<ul>
    <li>Nếu robot di chuyển <strong>lên</strong> hoặc <strong>xuống</strong> vào một ô có <strong>hàng</strong> là <code>r</code>, thì bước di chuyển này tốn <code>rowCosts[r]</code>.</li>
    <li>Nếu robot di chuyển <strong>trái</strong> hoặc <strong>phải</strong> vào một ô có <strong>cột</strong> là <code>c</code>, thì bước di chuyển này tốn <code>colCosts[c]</code>.</li>
</ul>

<p>Hãy trả về <em><strong>tổng chi phí nhỏ nhất</strong> để robot về nhà</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2087.Minimum%20Cost%20Homecoming%20of%20a%20Robot%20in%20a%20Grid/images/eg-1.png" style="width: 282px; height: 217px;" />
<pre>
<strong>Đầu vào:</strong> startPos = [1, 0], homePos = [2, 3], rowCosts = [5, 4, 3], colCosts = [8, 2, 6, 7]
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Một đường đi tối ưu là:
Bắt đầu từ (1, 0)
-&gt; Đi xuống đến (<u><strong>2</strong></u>, 0). Bước này tốn rowCosts[2] = 3.
-&gt; Đi sang phải đến (2, <u><strong>1</strong></u>). Bước này tốn colCosts[1] = 2.
-&gt; Đi sang phải đến (2, <u><strong>2</strong></u>). Bước này tốn colCosts[2] = 6.
-&gt; Đi sang phải đến (2, <u><strong>3</strong></u>). Bước này tốn colCosts[3] = 7.
Tổng chi phí là 3 + 2 + 6 + 7 = 18</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> startPos = [0, 0], homePos = [0, 0], rowCosts = [5], colCosts = [26]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Robot đã ở nhà. Vì không có bước di chuyển nào, tổng chi phí là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>m == rowCosts.length</code></li>
    <li><code>n == colCosts.length</code></li>
    <li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= rowCosts[r], colCosts[c] &lt;= 10<sup>4</sup></code></li>
    <li><code>startPos.length == 2</code></li>
    <li><code>homePos.length == 2</code></li>
    <li><code>0 &lt;= start<sub>row</sub>, home<sub>row</sub> &lt; m</code></li>
    <li><code>0 &lt;= start<sub>col</sub>, home<sub>col</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần đi vào một hàng hoặc một cột sẽ tốn chi phí của hàng hoặc cột đó. Mọi đường đi đơn điệu từ vị trí bắt đầu đến nhà đều đi qua cùng một tập hợp hàng và cột; những bước vòng chỉ làm tăng chi phí.
>
> Cộng chi phí của các hàng và cột đã đi qua. Hàng và cột ban đầu không bị tính phí vì robot không đi vào chúng.

<!-- thinking:end -->

Giả sử vị trí ban đầu của robot là $(x_0, y_0)$ và vị trí nhà là $(x_1, y_1)$.

Nếu $x_0 < x_1$, robot cần di chuyển xuống, đi qua các hàng $[x_0 + 1, x_1]$, với tổng chi phí là $\sum_{i = x_0 + 1}^{x_1} rowCosts[i]$. Nếu $x_0 > x_1$, robot cần di chuyển lên, đi qua các hàng $[x_1, x_0 - 1]$, với tổng chi phí là $\sum_{i = x_1}^{x_0 - 1} rowCosts[i]$. Nếu $x_0 = x_1$, robot không cần di chuyển theo chiều dọc, nên tổng chi phí là $0$.

Tương tự, nếu $y_0 < y_1$, robot cần di chuyển sang phải, đi qua các cột $[y_0 + 1, y_1]$, với tổng chi phí là $\sum_{j = y_0 + 1}^{y_1} colCosts[j]$. Nếu $y_0 > y_1$, robot cần di chuyển sang trái, đi qua các cột $[y_1, y_0 - 1]$, với tổng chi phí là $\sum_{j = y_1}^{y_0 - 1} colCosts[j]$. Nếu $y_0 = y_1$, robot không cần di chuyển theo chiều ngang, nên tổng chi phí là $0$.

Đáp án là tổng chi phí di chuyển theo chiều dọc và chiều ngang.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của $rowCosts$ và $colCosts$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(
        self,
        startPos: List[int],
        homePos: List[int],
        rowCosts: List[int],
        colCosts: List[int],
    ) -> int:
        x0, y0 = startPos
        x1, y1 = homePos
        dx = sum(rowCosts[x0 + 1 : x1 + 1]) if x0 < x1 else sum(rowCosts[x1:x0])
        dy = sum(colCosts[y0 + 1 : y1 + 1]) if y0 < y1 else sum(colCosts[y1:y0])
        return dx + dy
```

#### Java

```java
class Solution {
    public int minCost(int[] startPos, int[] homePos, int[] rowCosts, int[] colCosts) {
        int x0 = startPos[0], y0 = startPos[1];
        int x1 = homePos[0], y1 = homePos[1];
        int dx = x0 < x1 ? calc(rowCosts, x0 + 1, x1) : calc(rowCosts, x1, x0 - 1);
        int dy = y0 < y1 ? calc(colCosts, y0 + 1, y1) : calc(colCosts, y1, y0 - 1);
        return dx + dy;
    }

    private int calc(int[] nums, int i, int j) {
        int res = 0;
        for (int k = i; k <= j; ++k) {
            res += nums[k];
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(vector<int>& startPos, vector<int>& homePos, vector<int>& rowCosts, vector<int>& colCosts) {
        auto calc = [](vector<int>& nums, int i, int j) {
            int res = 0;
            for (int k = i; k <= j; ++k) {
                res += nums[k];
            }
            return res;
        };
        int x0 = startPos[0], y0 = startPos[1];
        int x1 = homePos[0], y1 = homePos[1];
        int dx = x0 < x1 ? calc(rowCosts, x0 + 1, x1) : calc(rowCosts, x1, x0 - 1);
        int dy = y0 < y1 ? calc(colCosts, y0 + 1, y1) : calc(colCosts, y1, y0 - 1);
        return dx + dy;
    }
};
```

#### Go

```go
func minCost(startPos []int, homePos []int, rowCosts []int, colCosts []int) int {
	x0, y0 := startPos[0], startPos[1]
	x1, y1 := homePos[0], homePos[1]
	calc := func(nums []int, i int, j int) int {
		res := 0
		for k := i; k <= j; k++ {
			res += nums[k]
		}
		return res
	}
	dx := 0
	if x0 < x1 {
		dx = calc(rowCosts, x0+1, x1)
	} else {
		dx = calc(rowCosts, x1, x0-1)
	}
	dy := 0
	if y0 < y1 {
		dy = calc(colCosts, y0+1, y1)
	} else {
		dy = calc(colCosts, y1, y0-1)
	}
	return dx + dy
}
```

#### TypeScript

```ts
function minCost(
    startPos: number[],
    homePos: number[],
    rowCosts: number[],
    colCosts: number[],
): number {
    const calc = (nums: number[], i: number, j: number): number => {
        let res = 0;
        for (let k = i; k <= j; ++k) {
            res += nums[k];
        }
        return res;
    };

    const [x0, y0] = startPos;
    const [x1, y1] = homePos;

    const dx = x0 < x1 ? calc(rowCosts, x0 + 1, x1) : calc(rowCosts, x1, x0 - 1);

    const dy = y0 < y1 ? calc(colCosts, y0 + 1, y1) : calc(colCosts, y1, y0 - 1);

    return dx + dy;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_cost(
        start_pos: Vec<i32>,
        home_pos: Vec<i32>,
        row_costs: Vec<i32>,
        col_costs: Vec<i32>,
    ) -> i32 {
        let calc = |nums: &Vec<i32>, i: i32, j: i32| -> i32 {
            let mut res = 0;
            for k in i..=j {
                res += nums[k as usize];
            }
            res
        };

        let x0 = start_pos[0];
        let y0 = start_pos[1];
        let x1 = home_pos[0];
        let y1 = home_pos[1];

        let dx = if x0 < x1 {
            calc(&row_costs, x0 + 1, x1)
        } else {
            calc(&row_costs, x1, x0 - 1)
        };

        let dy = if y0 < y1 {
            calc(&col_costs, y0 + 1, y1)
        } else {
            calc(&col_costs, y1, y0 - 1)
        };

        dx + dy
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
