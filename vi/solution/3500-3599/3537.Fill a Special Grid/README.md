---
comments: true
difficulty: Medium
rating: 1541
source: Weekly Contest 448 Q2
tags:
    - Array
    - Divide and Conquer
    - Matrix
---

<!-- problem:start -->

# [3537. Fill a Special Grid](https://leetcode.com/problems/fill-a-special-grid)

[中文文档](/solution/3500-3599/3537.Fill%20a%20Special%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên không âm <code><font face="monospace">n</font></code> biểu diễn một lưới <code>2<sup>n</sup> x 2<sup>n</sup></code>. Bạn cần điền các số nguyên từ 0 đến <code>2<sup>2n</sup> - 1</code> vào lưới để tạo thành một lưới <strong>đặc biệt</strong>. Một lưới được gọi là <strong>đặc biệt</strong> nếu thỏa mãn <strong>tất cả</strong> các điều kiện sau:</p>

<ul>
    <li>Tất cả các số trong góc phần tư trên bên phải đều nhỏ hơn các số trong góc phần tư dưới bên phải.</li>
    <li>Tất cả các số trong góc phần tư dưới bên phải đều nhỏ hơn các số trong góc phần tư dưới bên trái.</li>
    <li>Tất cả các số trong góc phần tư dưới bên trái đều nhỏ hơn các số trong góc phần tư trên bên trái.</li>
    <li>Mỗi góc phần tư của lưới cũng là một lưới đặc biệt.</li>
</ul>

<p>Trả về lưới <strong>đặc biệt</strong> <code>2<sup>n</sup> x 2<sup>n</sup></code>.</p>

<p><strong>Lưu ý</strong>: Mọi lưới 1x1 đều là lưới đặc biệt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[0]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có thể đặt số 0, và trong lưới cũng chỉ có một vị trí khả dĩ.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[3,0],[2,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số trong mỗi góc phần tư là:</p>

<ul>
    <li>Góc phần tư trên bên phải: 0</li>
    <li>Góc phần tư dưới bên phải: 1</li>
    <li>Góc phần tư dưới bên trái: 2</li>
    <li>Góc phần tư trên bên trái: 3</li>
</ul>

<p>Vì <code>0 &lt; 1 &lt; 2 &lt; 3</code>, các ràng buộc đã cho được thỏa mãn.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[15,12,3,0],[14,13,2,1],[11,8,7,4],[10,9,6,5]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3537.Fill%20a%20Special%20Grid/images/4123example3p1drawio.png" style="width: 161px; height: 161px;" /></p>

<p>Các số trong mỗi góc phần tư là:</p>

<ul>
    <li>Góc phần tư trên bên phải: 3, 0, 2, 1</li>
    <li>Góc phần tư dưới bên phải: 7, 4, 6, 5</li>
    <li>Góc phần tư dưới bên trái: 11, 8, 10, 9</li>
    <li>Góc phần tư trên bên trái: 15, 12, 14, 13</li>
    <li><code>max(3, 0, 2, 1) &lt; min(7, 4, 6, 5)</code></li>
    <li><code>max(7, 4, 6, 5) &lt; min(11, 8, 10, 9)</code></li>
    <li><code>max(11, 8, 10, 9) &lt; min(15, 12, 14, 13)</code></li>
</ul>

<p>Điều này thỏa mãn ba yêu cầu đầu tiên. Ngoài ra, mỗi góc phần tư cũng là một lưới đặc biệt. Do đó, đây là một lưới đặc biệt.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>0 &lt;= n &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chia để trị đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Một lưới đặc biệt yêu cầu bốn góc phần tư thỏa mãn thứ tự góc phần tư trên bên phải $<$ góc phần tư dưới bên phải $<$ góc phần tư dưới bên trái $<$ góc phần tư trên bên trái, đồng thời mỗi góc phần tư cũng phải là một lưới đặc biệt. Việc điền lần lượt theo từng hàng không thể đảm bảo cả thứ tự cục bộ lẫn thứ tự toàn cục.
>
> Hãy đệ quy trên một khối có cạnh $k$ theo thứ tự góc phần tư trên bên phải, góc phần tư dưới bên phải, góc phần tư dưới bên trái, góc phần tư trên bên trái; ghi giá trị hiện tại khi $k=1$. Bắt đầu từ góc trên bên phải $(0, 2^n-1)$.

<!-- thinking:end -->

Một lưới đặc biệt yêu cầu các số trong mỗi góc phần tư thỏa mãn: góc phần tư trên bên phải < góc phần tư dưới bên phải < góc phần tư dưới bên trái < góc phần tư trên bên trái, và mỗi góc phần tư cũng là một lưới đặc biệt. Ta có thể xây dựng lưới đệ quy: với một lưới con có kích thước $k$, điền bốn góc phần tư theo thứ tự "góc phần tư trên bên phải → góc phần tư dưới bên phải → góc phần tư dưới bên trái → góc phần tư trên bên trái", đảm bảo các số nhỏ hơn được đặt vào góc phần tư trên bên phải trước, còn các số lớn hơn được đặt vào góc phần tư trên bên trái sau cùng.

Bắt đầu từ góc trên bên phải $(0, m - 1)$ của toàn bộ lưới, trong đó $m = 2^n$, với độ dài cạnh là $m$. Khi $k = 1$, ta điền ô bằng giá trị hiện tại rồi tăng giá trị này lên; nếu không, ta chia lưới thành bốn góc phần tư và thực hiện đệ quy.

Độ phức tạp thời gian là $O(4^n)$, và độ phức tạp không gian là $O(4^n)$, trong đó $n$ là tham số đầu vào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def specialGrid(self, n: int) -> List[List[int]]:
        def dfs(x: int, y: int, k: int):
            if k == 1:
                nonlocal val
                ans[x][y] = val
                val += 1
                return

            dfs(x, y, k // 2)
            dfs(x + k // 2, y, k // 2)
            dfs(x + k // 2, y - k // 2, k // 2)
            dfs(x, y - k // 2, k // 2)

        m = 1 << n
        ans = [[0] * m for _ in range(m)]
        val = 0
        dfs(0, m - 1, m)
        return ans
```

#### Java

```java
class Solution {
    private int[][] ans;
    private int val;

    public int[][] specialGrid(int n) {
        int m = 1 << n;
        ans = new int[m][m];
        dfs(0, m - 1, m);
        return ans;
    }

    private void dfs(int x, int y, int k) {
        if (k == 1) {
            ans[x][y] = val++;
            return;
        }

        int h = k / 2;
        dfs(x, y, h);
        dfs(x + h, y, h);
        dfs(x + h, y - h, h);
        dfs(x, y - h, h);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> specialGrid(int n) {
        int m = 1 << n;
        vector<vector<int>> ans(m, vector<int>(m));
        int val = 0;

        auto dfs = [&](this auto&& dfs, int x, int y, int k) -> void {
            if (k == 1) {
                ans[x][y] = val++;
                return;
            }

            int h = k / 2;
            dfs(x, y, h);
            dfs(x + h, y, h);
            dfs(x + h, y - h, h);
            dfs(x, y - h, h);
        };

        dfs(0, m - 1, m);
        return ans;
    }
};
```

#### Go

```go
func specialGrid(n int) [][]int {
    m := 1 << n
    ans := make([][]int, m)
    for i := range ans {
        ans[i] = make([]int, m)
    }
    val := 0

    var dfs func(int, int, int)
    dfs = func(x, y, k int) {
        if k == 1 {
            ans[x][y] = val
            val++
            return
        }

        h := k / 2
        dfs(x, y, h)
        dfs(x+h, y, h)
        dfs(x+h, y-h, h)
        dfs(x, y-h, h)
    }

    dfs(0, m-1, m)
    return ans
}
```

#### TypeScript

```ts
function specialGrid(n: number): number[][] {
    const m = 1 << n;
    const ans = Array.from({ length: m }, () => Array(m).fill(0));
    let val = 0;

    const dfs = (x: number, y: number, k: number): void => {
        if (k === 1) {
            ans[x][y] = val++;
            return;
        }

        const h = k >> 1;
        dfs(x, y, h);
        dfs(x + h, y, h);
        dfs(x + h, y - h, h);
        dfs(x, y - h, h);
    };

    dfs(0, m - 1, m);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn special_grid(n: i32) -> Vec<Vec<i32>> {
        fn dfs(x: usize, y: usize, k: usize, ans: &mut Vec<Vec<i32>>, val: &mut i32) {
            if k == 1 {
                ans[x][y] = *val;
                *val += 1;
                return;
            }

            let h = k / 2;
            dfs(x, y, h, ans, val);
            dfs(x + h, y, h, ans, val);
            dfs(x + h, y - h, h, ans, val);
            dfs(x, y - h, h, ans, val);
        }

        let m = 1usize << n;
        let mut ans = vec![vec![0; m]; m];
        let mut val = 0;
        dfs(0, m - 1, m, &mut ans, &mut val);
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
