---
comments: true
difficulty: Medium
rating: 1536
source: Biweekly Contest 52 Q3
tags:
    - Array
    - Two Pointers
    - Matrix
---

<!-- problem:start -->

# [1861. Rotating the Box](https://leetcode.com/problems/rotating-the-box)

[中文文档](/solution/1800-1899/1861.Rotating%20the%20Box/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận ký tự <code>m x n</code> <code>boxGrid</code> biểu diễn hình chiếu bên của một chiếc hộp. Mỗi ô trong hộp thuộc một trong các loại sau:</p>

<ul>
	<li>Một viên đá <code>&#39;#&#39;</code></li>
	<li>Một chướng ngại vật cố định <code>&#39;*&#39;</code></li>
	<li>Ô trống <code>&#39;.&#39;</code></li>
</ul>

<p>Hộp được xoay <strong>90 độ theo chiều kim đồng hồ</strong>, khiến một số viên đá rơi xuống do trọng lực. Mỗi viên đá rơi xuống cho đến khi chạm vào chướng ngại vật, một viên đá khác hoặc đáy hộp. Trọng lực <strong>không</strong> ảnh hưởng đến vị trí của chướng ngại vật, và quán tính do hộp xoay <strong>không </strong>ảnh hưởng đến vị trí ngang của các viên đá.</p>

<p><strong>Đảm bảo</strong> rằng mỗi viên đá trong <code>boxGrid</code> đều đang tựa trên một chướng ngại vật, một viên đá khác hoặc đáy hộp.</p>

<p>Trả về <em>ma trận </em><code>n x m</code><em> biểu diễn chiếc hộp sau phép xoay được mô tả ở trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1861.Rotating%20the%20Box/images/rotatingtheboxleetcodewithstones.png" style="width: 300px; height: 150px;" /></p>

<pre>
<strong>Đầu vào:</strong> boxGrid = [[&quot;#&quot;,&quot;.&quot;,&quot;#&quot;]]
<strong>Đầu ra:</strong> [[&quot;.&quot;],
&nbsp;        [&quot;#&quot;],
&nbsp;        [&quot;#&quot;]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1861.Rotating%20the%20Box/images/rotatingtheboxleetcode2withstones.png" style="width: 375px; height: 195px;" /></p>

<pre>
<strong>Đầu vào:</strong> boxGrid = [[&quot;#&quot;,&quot;.&quot;,&quot;*&quot;,&quot;.&quot;],
&nbsp;             [&quot;#&quot;,&quot;#&quot;,&quot;*&quot;,&quot;.&quot;]]
<strong>Đầu ra:</strong> [[&quot;#&quot;,&quot;.&quot;],
&nbsp;        [&quot;#&quot;,&quot;#&quot;],
&nbsp;        [&quot;*&quot;,&quot;*&quot;],
&nbsp;        [&quot;.&quot;,&quot;.&quot;]]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1861.Rotating%20the%20Box/images/rotatingtheboxleetcode3withstone.png" style="width: 400px; height: 218px;" /></p>

<pre>
<strong>Đầu vào:</strong> boxGrid = [[&quot;#&quot;,&quot;#&quot;,&quot;*&quot;,&quot;.&quot;,&quot;*&quot;,&quot;.&quot;],
&nbsp;             [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;*&quot;,&quot;.&quot;,&quot;.&quot;],
&nbsp;             [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;.&quot;,&quot;#&quot;,&quot;.&quot;]]
<strong>Đầu ra:</strong> [[&quot;.&quot;,&quot;#&quot;,&quot;#&quot;],
&nbsp;        [&quot;.&quot;,&quot;#&quot;,&quot;#&quot;],
&nbsp;        [&quot;#&quot;,&quot;#&quot;,&quot;*&quot;],
&nbsp;        [&quot;#&quot;,&quot;*&quot;,&quot;.&quot;],
&nbsp;        [&quot;#&quot;,&quot;.&quot;,&quot;*&quot;],
&nbsp;        [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == boxGrid.length</code></li>
	<li><code>n == boxGrid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>boxGrid[i][j]</code> là một trong các ký tự <code>&#39;#&#39;</code>, <code>&#39;*&#39;</code> hoặc <code>&#39;.&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng Queue

<!-- thinking:start -->

> **Tư duy**
>
> Xoay $90^\circ$ theo chiều kim đồng hồ, sau đó để các viên đá rơi xuống cho đến khi chạm chướng ngại vật hoặc đáy hộp. Nếu di chuyển từng viên đá từng bước, ta có thể phải xét lại cùng một ô nhiều lần.
>
> Sau khi xoay, duyệt từng cột từ dưới lên: đưa các hàng trống vào queue, đổi chỗ viên đá với vị trí trống đầu tiên, và xóa queue khi gặp chướng ngại vật. Mỗi ô chỉ được xử lý một lần, và các viên đá sẽ nằm ở những ô trống thấp nhất mà chúng có thể tới.

<!-- thinking:end -->

Trước hết, ta xoay ma trận 90 độ theo chiều kim đồng hồ, sau đó mô phỏng quá trình các viên đá rơi xuống trong từng cột.

Cụ thể, ta dùng queue $q$ để lưu chỉ số hàng của các vị trí trống trong cột hiện tại. Khi duyệt từng cột, ta quét từ dưới lên trên. Nếu gặp một viên đá, ta thả nó xuống vị trí trống đầu tiên trong $q$, xóa vị trí trống đó khỏi $q$, rồi thêm chỉ số hàng của vị trí hiện tại vào $q$ vì vị trí này đã trở thành ô trống. Nếu gặp chướng ngại vật, ta xóa $q$ vì các viên đá không thể đi xuyên qua chướng ngại vật. Nếu gặp ô trống, ta thêm chỉ số hàng của ô đó vào $q$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rotateTheBox(self, boxGrid: List[List[str]]) -> List[List[str]]:
        m, n = len(boxGrid), len(boxGrid[0])
        ans = [[None] * m for _ in range(n)]
        for i in range(m):
            for j in range(n):
                ans[j][m - i - 1] = boxGrid[i][j]
        for j in range(m):
            q = deque()
            for i in range(n - 1, -1, -1):
                if ans[i][j] == "*":
                    q.clear()
                elif ans[i][j] == ".":
                    q.append(i)
                elif q:
                    ans[q.popleft()][j] = "#"
                    ans[i][j] = "."
                    q.append(i)
        return ans
```

#### Java

```java
class Solution {
    public char[][] rotateTheBox(char[][] boxGrid) {
        int m = boxGrid.length, n = boxGrid[0].length;
        char[][] ans = new char[n][m];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[j][m - i - 1] = boxGrid[i][j];
            }
        }
        for (int j = 0; j < m; ++j) {
            Deque<Integer> q = new ArrayDeque<>();
            for (int i = n - 1; i >= 0; --i) {
                if (ans[i][j] == '*') {
                    q.clear();
                } else if (ans[i][j] == '.') {
                    q.offer(i);
                } else if (!q.isEmpty()) {
                    ans[q.pollFirst()][j] = '#';
                    ans[i][j] = '.';
                    q.offer(i);
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
    vector<vector<char>> rotateTheBox(vector<vector<char>>& boxGrid) {
        int m = boxGrid.size(), n = boxGrid[0].size();
        vector<vector<char>> ans(n, vector<char>(m));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[j][m - i - 1] = boxGrid[i][j];
            }
        }
        for (int j = 0; j < m; ++j) {
            queue<int> q;
            for (int i = n - 1; ~i; --i) {
                if (ans[i][j] == '*') {
                    queue<int> t;
                    swap(t, q);
                } else if (ans[i][j] == '.') {
                    q.push(i);
                } else if (!q.empty()) {
                    ans[q.front()][j] = '#';
                    q.pop();
                    ans[i][j] = '.';
                    q.push(i);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func rotateTheBox(boxGrid [][]byte) [][]byte {
	m, n := len(boxGrid), len(boxGrid[0])
	ans := make([][]byte, n)
	for i := range ans {
		ans[i] = make([]byte, m)
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			ans[j][m-i-1] = boxGrid[i][j]
		}
	}
	for j := 0; j < m; j++ {
		q := []int{}
		for i := n - 1; i >= 0; i-- {
			if ans[i][j] == '*' {
				q = []int{}
			} else if ans[i][j] == '.' {
				q = append(q, i)
			} else if len(q) > 0 {
				ans[q[0]][j] = '#'
				q = q[1:]
				ans[i][j] = '.'
				q = append(q, i)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function rotateTheBox(boxGrid: string[][]): string[][] {
    const m = boxGrid.length;
    const n = boxGrid[0].length;
    const ans: string[][] = Array.from({ length: n }, () => Array(m));

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            ans[j][m - i - 1] = boxGrid[i][j];
        }
    }

    for (let j = 0; j < m; j++) {
        const q: number[] = [];
        for (let i = n - 1; i >= 0; i--) {
            if (ans[i][j] === '*') {
                q.length = 0;
            } else if (ans[i][j] === '.') {
                q.push(i);
            } else if (q.length > 0) {
                const t = q.shift()!;
                ans[t][j] = '#';
                ans[i][j] = '.';
                q.push(i);
            }
        }
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn rotate_the_box(box_grid: Vec<Vec<char>>) -> Vec<Vec<char>> {
        let m: usize = box_grid.len();
        let n: usize = box_grid[0].len();
        let mut ans: Vec<Vec<char>> = vec![vec![' '; m]; n];

        for i in 0..m {
            for j in 0..n {
                ans[j][m - i - 1] = box_grid[i][j];
            }
        }

        for j in 0..m {
            let mut q: VecDeque<usize> = VecDeque::new();
            for i in (0..n).rev() {
                if ans[i][j] == '*' {
                    q.clear();
                } else if ans[i][j] == '.' {
                    q.push_back(i);
                } else if !q.is_empty() {
                    let t = q.pop_front().unwrap();
                    ans[t][j] = '#';
                    ans[i][j] = '.';
                    q.push_back(i);
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
