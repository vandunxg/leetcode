---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [573. Squirrel Simulation 🔒](https://leetcode.com/problems/squirrel-simulation)

[中文文档](/solution/0500-0599/0573.Squirrel%20Simulation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>height</code> và <code>width</code> biểu thị khu vườn có kích thước <code>height x width</code>. Ngoài ra, bạn được cho:</p>

<ul>
	<li>mảng <code>tree</code>, trong đó <code>tree = [tree<sub>r</sub>, tree<sub>c</sub>]</code> là vị trí của cây trong khu vườn;</li>
	<li>mảng <code>squirrel</code>, trong đó <code>squirrel = [squirrel<sub>r</sub>, squirrel<sub>c</sub>]</code> là vị trí của sóc trong khu vườn;</li>
	<li>và mảng <code>nuts</code>, trong đó <code>nuts[i] = [nut<sub>i<sub>r</sub></sub>, nut<sub>i<sub>c</sub></sub>]</code> là vị trí của hạt thứ <code>i<sup>th</sup></code> trong khu vườn.</li>
</ul>

<p>Mỗi lần sóc chỉ có thể mang tối đa một hạt và có thể di chuyển sang ô kề theo bốn hướng: lên, xuống, trái và phải.</p>

<p>Hãy trả về <em><strong>quãng đường ngắn nhất</strong> để sóc nhặt tất cả các hạt rồi lần lượt đặt chúng dưới gốc cây</em>.</p>

<p><strong>Khoảng cách</strong> là số bước di chuyển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0573.Squirrel%20Simulation/images/squirrel1-grid.jpg" style="width: 573px; height: 413px;" />
<pre>
<strong>Đầu vào:</strong> height = 5, width = 7, tree = [2,2], squirrel = [4,4], nuts = [[3,0], [2,5]]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Để đi được quãng đường ngắn nhất, sóc nên đến hạt ở vị trí [2, 5] trước.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0573.Squirrel%20Simulation/images/squirrel2-grid.jpg" style="width: 253px; height: 93px;" />
<pre>
<strong>Đầu vào:</strong> height = 1, width = 3, tree = [0,1], squirrel = [0,0], nuts = [[0,2]]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= height, width &lt;= 100</code></li>
	<li><code>tree.length == 2</code></li>
	<li><code>squirrel.length == 2</code></li>
	<li><code>1 &lt;= nuts.length &lt;= 5000</code></li>
	<li><code>nuts[i].length == 2</code></li>
	<li><code>0 &lt;= tree<sub>r</sub>, squirrel<sub>r</sub>, nut<sub>i<sub>r</sub></sub> &lt;= height</code></li>
	<li><code>0 &lt;= tree<sub>c</sub>, squirrel<sub>c</sub>, nut<sub>i<sub>c</sub></sub> &lt;= width</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Đầu tiên, sóc mang một hạt về cây; với mỗi hạt còn lại, sóc đi khứ hồi từ cây đến hạt đó. Mô phỏng toàn bộ hành trình cho từng hạt đầu tiên sẽ lặp lại các lượt đi giống nhau.
>
> Gọi $s$ là hai lần tổng khoảng cách từ các hạt đến cây. Nếu nhặt hạt $(r,c)$ trước, ta thay quãng đường từ hạt đó đến cây $a$ bằng quãng đường từ sóc đến hạt $b$, nên tổng là $s-a+b$. Chọn hạt đầu tiên sao cho tổng này nhỏ nhất.

<!-- thinking:end -->

Quan sát hành trình của sóc, ta thấy sóc sẽ đi đến một hạt trước rồi mang hạt đó đến cây. Sau đó, với mỗi hạt còn lại, tổng quãng đường của sóc bằng "tổng khoảng cách từ các hạt còn lại đến cây" nhân với $2$.

Vì vậy, để có hành trình ngắn nhất, ta chỉ cần chọn hạt mà sóc sẽ nhặt đầu tiên sao cho tổng quãng đường là nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số lượng hạt. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDistance(
        self,
        height: int,
        width: int,
        tree: List[int],
        squirrel: List[int],
        nuts: List[List[int]],
    ) -> int:
        tr, tc = tree
        sr, sc = squirrel
        s = sum(abs(r - tr) + abs(c - tc) for r, c in nuts) * 2
        ans = inf
        for r, c in nuts:
            a = abs(r - tr) + abs(c - tc)
            b = abs(r - sr) + abs(c - sc)
            ans = min(ans, s - a + b)
        return ans
```

#### Java

```java
import static java.lang.Math.*;

class Solution {
    public int minDistance(int height, int width, int[] tree, int[] squirrel, int[][] nuts) {
        int tr = tree[0], tc = tree[1];
        int sr = squirrel[0], sc = squirrel[1];
        int s = 0;
        for (var e : nuts) {
            s += abs(e[0] - tr) + abs(e[1] - tc);
        }
        s <<= 1;
        int ans = Integer.MAX_VALUE;
        for (var e : nuts) {
            int a = abs(e[0] - tr) + abs(e[1] - tc);
            int b = abs(e[0] - sr) + abs(e[1] - sc);
            ans = min(ans, s - a + b);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDistance(int height, int width, vector<int>& tree, vector<int>& squirrel, vector<vector<int>>& nuts) {
        int tr = tree[0], tc = tree[1];
        int sr = squirrel[0], sc = squirrel[1];
        int s = 0;
        for (const auto& e : nuts) {
            s += abs(e[0] - tr) + abs(e[1] - tc);
        }
        s <<= 1;
        int ans = INT_MAX;
        for (const auto& e : nuts) {
            int a = abs(e[0] - tr) + abs(e[1] - tc);
            int b = abs(e[0] - sr) + abs(e[1] - sc);
            ans = min(ans, s - a + b);
        }
        return ans;
    }
};
```

#### Go

```go
func minDistance(height int, width int, tree []int, squirrel []int, nuts [][]int) int {
	tr, tc := tree[0], tree[1]
	sr, sc := squirrel[0], squirrel[1]
	s := 0
	for _, e := range nuts {
		s += abs(e[0]-tr) + abs(e[1]-tc)
	}
	s <<= 1
	ans := math.MaxInt32
	for _, e := range nuts {
		a := abs(e[0]-tr) + abs(e[1]-tc)
		b := abs(e[0]-sr) + abs(e[1]-sc)
		ans = min(ans, s-a+b)
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minDistance(
    height: number,
    width: number,
    tree: number[],
    squirrel: number[],
    nuts: number[][],
): number {
    const [tr, tc] = tree;
    const [sr, sc] = squirrel;
    const s = nuts.reduce((acc, [r, c]) => acc + (Math.abs(tr - r) + Math.abs(tc - c)) * 2, 0);
    let ans = Infinity;
    for (const [r, c] of nuts) {
        const a = Math.abs(tr - r) + Math.abs(tc - c);
        const b = Math.abs(sr - r) + Math.abs(sc - c);
        ans = Math.min(ans, s - a + b);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_distance(
        height: i32,
        width: i32,
        tree: Vec<i32>,
        squirrel: Vec<i32>,
        nuts: Vec<Vec<i32>>,
    ) -> i32 {
        let (tr, tc) = (tree[0], tree[1]);
        let (sr, sc) = (squirrel[0], squirrel[1]);
        let s: i32 = nuts
            .iter()
            .map(|nut| (nut[0] - tr).abs() + (nut[1] - tc).abs())
            .sum::<i32>()
            * 2;

        let mut ans = i32::MAX;
        for nut in &nuts {
            let a = (nut[0] - tr).abs() + (nut[1] - tc).abs();
            let b = (nut[0] - sr).abs() + (nut[1] - sc).abs();
            ans = ans.min(s - a + b);
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MinDistance(int height, int width, int[] tree, int[] squirrel, int[][] nuts) {
        int tr = tree[0], tc = tree[1];
        int sr = squirrel[0], sc = squirrel[1];
        int s = 0;

        foreach (var e in nuts) {
            s += Math.Abs(e[0] - tr) + Math.Abs(e[1] - tc);
        }
        s <<= 1;

        int ans = int.MaxValue;
        foreach (var e in nuts) {
            int a = Math.Abs(e[0] - tr) + Math.Abs(e[1] - tc);
            int b = Math.Abs(e[0] - sr) + Math.Abs(e[1] - sc);
            ans = Math.Min(ans, s - a + b);
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
