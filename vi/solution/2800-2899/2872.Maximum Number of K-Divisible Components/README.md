---
comments: true
difficulty: Hard
rating: 1967
source: Biweekly Contest 114 Q4
tags:
    - Tree
    - Depth-First Search
---

<!-- problem:start -->

# [2872. Maximum Number of K-Divisible Components](https://leetcode.com/problems/maximum-number-of-k-divisible-components)

[中文文档](/solution/2800-2899/2872.Maximum%20Number%20of%20K-Divisible%20Components/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho số nguyên <code>n</code> và một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>values</code> <strong>đánh chỉ số từ 0</strong> có độ dài <code>n</code>, trong đó <code>values[i]</code> là <strong>giá trị</strong> gắn với node thứ <code>i<sup>th</sup></code>, và một số nguyên <code>k</code>.</p>

<p>Một <strong>cách chia hợp lệ</strong> của cây được tạo ra bằng cách xóa một tập hợp bất kỳ các cạnh, có thể là tập rỗng, khỏi cây sao cho tất cả component thu được đều có tổng giá trị chia hết cho <code>k</code>, trong đó <strong>giá trị của một component liên thông</strong> là tổng giá trị của các node trong component đó.</p>

<p>Trả về <em><strong>số lượng component lớn nhất</strong> trong một cách chia hợp lệ bất kỳ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2872.Maximum%20Number%20of%20K-Divisible%20Components/images/example12-cropped2svg.jpg" style="width: 1024px; height: 453px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[0,2],[1,2],[1,3],[2,4]], values = [1,8,1,4,4], k = 6
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta xóa cạnh nối node 1 với node 2. Cách chia thu được là hợp lệ vì:
- Giá trị của component chứa các node 1 và 3 là values[1] + values[3] = 12.
- Giá trị của component chứa các node 0, 2 và 4 là values[0] + values[2] + values[4] = 6.
Có thể chứng minh rằng không có cách chia hợp lệ nào khác có nhiều hơn 2 component liên thông.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2872.Maximum%20Number%20of%20K-Divisible%20Components/images/example21svg-1.jpg" style="width: 999px; height: 338px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [3,0,6,1,5,2,1], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta xóa cạnh nối node 0 với node 2 và cạnh nối node 0 với node 1. Cách chia thu được là hợp lệ vì:
- Giá trị của component chứa node 0 là values[0] = 3.
- Giá trị của component chứa các node 2, 5 và 6 là values[2] + values[5] + values[6] = 9.
- Giá trị của component chứa các node 1, 3 và 4 là values[1] + values[3] + values[4] = 6.
Có thể chứng minh rằng không có cách chia hợp lệ nào khác có nhiều hơn 3 component liên thông.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
\t<li><code>1 &lt;= n &lt;= 3 * 10<sup>4</sup></code></li>
\t<li><code>edges.length == n - 1</code></li>
\t<li><code>edges[i].length == 2</code></li>
\t<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
\t<li><code>values.length == n</code></li>
\t<li><code>0 &lt;= values[i] &lt;= 10<sup>9</sup></code></li>
\t<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
\t<li>Tổng của <code>values</code> chia hết cho <code>k</code>.</li>
\t<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Tổng giá trị của toàn bộ cây chia hết cho $k$, nên nếu xóa một subtree có tổng cũng chia hết cho $k$ thì các component còn lại vẫn hợp lệ. DFS theo thứ tự từ dưới lên sẽ cộng dồn tổng các subtree và đếm mỗi subtree có tổng là $0$ theo modulo $k$.

<!-- thinking:end -->

Ta nhận thấy đề bài đảm bảo tổng giá trị của tất cả node trong toàn bộ cây chia hết cho $k$. Vì vậy, nếu xóa một subtree có tổng giá trị chia hết cho $k$, tổng giá trị của mỗi component liên thông còn lại cũng phải chia hết cho $k$.

Do đó, ta có thể dùng DFS, bắt đầu từ node gốc để duyệt toàn bộ cây. Với mỗi node, ta tính tổng giá trị của tất cả node trong subtree của nó. Nếu tổng này chia hết cho $k$, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxKDivisibleComponents(
        self, n: int, edges: List[List[int]], values: List[int], k: int
    ) -> int:
        def dfs(i: int, fa: int) -> int:
            s = values[i]
            for j in g[i]:
                if j != fa:
                    s += dfs(j, i)
            nonlocal ans
            ans += s % k == 0
            return s

        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        ans = 0
        dfs(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private int ans;
    private List<Integer>[] g;
    private int[] values;
    private int k;

    public int maxKDivisibleComponents(int n, int[][] edges, int[] values, int k) {
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        this.values = values;
        this.k = k;
        dfs(0, -1);
        return ans;
    }

    private long dfs(int i, int fa) {
        long s = values[i];
        for (int j : g[i]) {
            if (j != fa) {
                s += dfs(j, i);
            }
        }
        ans += s % k == 0 ? 1 : 0;
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxKDivisibleComponents(int n, vector<vector<int>>& edges, vector<int>& values, int k) {
        int ans = 0;
        vector<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        auto dfs = [&](this auto&& dfs, int i, int fa) -> long long {
            long long s = values[i];
            for (int j : g[i]) {
                if (j != fa) {
                    s += dfs(j, i);
                }
            }
            ans += s % k == 0;
            return s;
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func maxKDivisibleComponents(n int, edges [][]int, values []int, k int) (ans int) {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var dfs func(int, int) int
	dfs = func(i, fa int) int {
		s := values[i]
		for _, j := range g[i] {
			if j != fa {
				s += dfs(j, i)
			}
		}
		if s%k == 0 {
			ans++
		}
		return s
	}
	dfs(0, -1)
	return
}
```

#### TypeScript

```ts
function maxKDivisibleComponents(
    n: number,
    edges: number[][],
    values: number[],
    k: number,
): number {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    let ans = 0;
    const dfs = (i: number, fa: number): number => {
        let s = values[i];
        for (const j of g[i]) {
            if (j !== fa) {
                s += dfs(j, i);
            }
        }
        if (s % k === 0) {
            ++ans;
        }
        return s;
    };
    dfs(0, -1);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_k_divisible_components(n: i32, edges: Vec<Vec<i32>>, values: Vec<i32>, k: i32) -> i32 {
        let n = n as usize;
        let mut g = vec![vec![]; n];
        for e in edges {
            let a = e[0] as usize;
            let b = e[1] as usize;
            g[a].push(b);
            g[b].push(a);
        }

        let mut ans = 0;

        fn dfs(i: usize, fa: i32, g: &Vec<Vec<usize>>, values: &Vec<i32>, k: i32, ans: &mut i32) -> i64 {
            let mut s = values[i] as i64;
            for &j in &g[i] {
                if j as i32 != fa {
                    s += dfs(j, i as i32, g, values, k, ans);
                }
            }
            if s % k as i64 == 0 {
                *ans += 1;
            }
            s
        }

        dfs(0, -1, &g, &values, k, &mut ans);
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
