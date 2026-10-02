---
comments: true
difficulty: Medium
rating: 1712
source: Weekly Contest 136 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Graph Coloring
---

<!-- problem:start -->

# [1042. Flower Planting With No Adjacent](https://leetcode.com/problems/flower-planting-with-no-adjacent)

[中文文档](/solution/1000-1099/1042.Flower%20Planting%20With%20No%20Adjacent/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> khu vườn được đánh số từ <code>1</code> đến <code>n</code>, cùng mảng <code>paths</code>, trong đó <code>paths[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> mô tả một con đường hai chiều nối khu vườn <code>x<sub>i</sub></code> với khu vườn <code>y<sub>i</sub></code>. Bạn muốn trồng một trong 4 loại hoa ở mỗi khu vườn.</p>

<p>Mỗi khu vườn có <strong>nhiều nhất 3</strong> con đường đi vào hoặc đi ra.</p>

<p>Nhiệm vụ của bạn là chọn loại hoa cho từng khu vườn sao cho hai khu vườn nối với nhau bằng một con đường luôn trồng hai loại hoa khác nhau.</p>

<p>Hãy trả về <em>bất kỳ cách chọn nào như vậy dưới dạng mảng </em><code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là loại hoa được trồng ở khu vườn thứ </em><code>(i+1)<sup>th</sup></code><em>. Các loại hoa được ký hiệu bằng </em><code>1</code><em>, </em><code>2</code><em>, </em><code>3</code><em> hoặc </em><code>4</code><em>. Đảm bảo luôn tồn tại đáp án.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, paths = [[1,2],[2,3],[3,1]]
<strong>Đầu ra:</strong> [1,2,3]
<strong>Giải thích:</strong>
Khu vườn 1 và 2 trồng hai loại hoa khác nhau.
Khu vườn 2 và 3 trồng hai loại hoa khác nhau.
Khu vườn 3 và 1 trồng hai loại hoa khác nhau.
Vì vậy, [1,2,3] là một đáp án hợp lệ. Các đáp án hợp lệ khác gồm [1,2,4], [1,4,2] và [3,2,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, paths = [[1,2],[3,4]]
<strong>Đầu ra:</strong> [1,2,1,2]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
<strong>Đầu ra:</strong> [1,2,3,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= paths.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>paths[i].length == 2</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= n</code></li>
	<li><code>x<sub>i</sub> != y<sub>i</sub></code></li>
	<li>Mỗi khu vườn có <strong>nhiều nhất 3</strong> con đường đi vào hoặc đi ra.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt thử

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đỉnh có bậc không quá 3 và có 4 màu, nên có thể tô màu tham lam mà không cần quay lui.
>
> Tạo danh sách kề. Với khu vườn $x$, lấy các màu mà những khu vườn kề với nó đã dùng, rồi gán màu đầu tiên còn trống trong khoảng $1..4$.
>
> Đỉnh có bậc $\le 3$ luôn còn ít nhất một màu chưa dùng; chỉ cần duyệt một lượt là xong.

<!-- thinking:end -->

Trước tiên, ta dựng đồ thị $g$ từ mảng $\textit{paths}$, trong đó $g[x]$ là danh sách các khu vườn kề với khu vườn $x$.

Tiếp theo, với mỗi khu vườn $x$, ta tìm các khu vườn $y$ kề với $x$ và đánh dấu những loại hoa đã trồng ở $y$ là đã dùng. Sau đó, lần lượt xét các loại hoa từ $1$ cho đến khi tìm được loại $c$ chưa được dùng. Gán loại $c$ cho khu vườn $x$ rồi chuyển sang khu vườn tiếp theo.

Sau khi duyệt xong, ta trả về kết quả.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ là số khu vườn và $m$ là số con đường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gardenNoAdj(self, n: int, paths: List[List[int]]) -> List[int]:
        g = defaultdict(list)
        for x, y in paths:
            x, y = x - 1, y - 1
            g[x].append(y)
            g[y].append(x)
        ans = [0] * n
        for x in range(n):
            used = {ans[y] for y in g[x]}
            for c in range(1, 5):
                if c not in used:
                    ans[x] = c
                    break
        return ans
```

#### Java

```java
class Solution {
    public int[] gardenNoAdj(int n, int[][] paths) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var p : paths) {
            int x = p[0] - 1, y = p[1] - 1;
            g[x].add(y);
            g[y].add(x);
        }
        int[] ans = new int[n];
        boolean[] used = new boolean[5];
        for (int x = 0; x < n; ++x) {
            Arrays.fill(used, false);
            for (int y : g[x]) {
                used[ans[y]] = true;
            }
            for (int c = 1; c < 5; ++c) {
                if (!used[c]) {
                    ans[x] = c;
                    break;
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
    vector<int> gardenNoAdj(int n, vector<vector<int>>& paths) {
        vector<vector<int>> g(n);
        for (auto& p : paths) {
            int x = p[0] - 1, y = p[1] - 1;
            g[x].push_back(y);
            g[y].push_back(x);
        }
        vector<int> ans(n);
        bool used[5];
        for (int x = 0; x < n; ++x) {
            memset(used, false, sizeof(used));
            for (int y : g[x]) {
                used[ans[y]] = true;
            }
            for (int c = 1; c < 5; ++c) {
                if (!used[c]) {
                    ans[x] = c;
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func gardenNoAdj(n int, paths [][]int) []int {
	g := make([][]int, n)
	for _, p := range paths {
		x, y := p[0]-1, p[1]-1
		g[x] = append(g[x], y)
		g[y] = append(g[y], x)
	}
	ans := make([]int, n)
	for x := 0; x < n; x++ {
		used := [5]bool{}
		for _, y := range g[x] {
			used[ans[y]] = true
		}
		for c := 1; c < 5; c++ {
			if !used[c] {
				ans[x] = c
				break
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function gardenNoAdj(n: number, paths: number[][]): number[] {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [x, y] of paths) {
        g[x - 1].push(y - 1);
        g[y - 1].push(x - 1);
    }
    const ans: number[] = Array(n).fill(0);
    for (let x = 0; x < n; ++x) {
        const used: boolean[] = Array(5).fill(false);
        for (const y of g[x]) {
            used[ans[y]] = true;
        }
        for (let c = 1; c < 5; ++c) {
            if (!used[c]) {
                ans[x] = c;
                break;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn garden_no_adj(n: i32, paths: Vec<Vec<i32>>) -> Vec<i32> {
        let n = n as usize;
        let mut g = vec![vec![]; n];

        for path in paths {
            let (x, y) = (path[0] as usize - 1, path[1] as usize - 1);
            g[x].push(y);
            g[y].push(x);
        }

        let mut ans = vec![0; n];
        for x in 0..n {
            let mut used = [false; 5];
            for &y in &g[x] {
                used[ans[y] as usize] = true;
            }
            for c in 1..5 {
                if !used[c] {
                    ans[x] = c as i32;
                    break;
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
