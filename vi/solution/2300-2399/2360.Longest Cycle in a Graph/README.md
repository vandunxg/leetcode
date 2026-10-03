---
comments: true
difficulty: Hard
rating: 1897
source: Weekly Contest 304 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Kosaraju
    - Tarjan
---

<!-- problem:start -->

# [2360. Longest Cycle in a Graph](https://leetcode.com/problems/longest-cycle-in-a-graph)

[中文文档](/solution/2300-2399/2360.Longest%20Cycle%20in%20a%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một đồ thị <strong>có hướng</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>, trong đó mỗi node có <strong>nhiều nhất một</strong> cạnh đi ra.</p>

<p>Đồ thị được biểu diễn bằng một mảng <strong>đánh chỉ số từ 0</strong> <code>edges</code> có kích thước <code>n</code>, trong đó cạnh có hướng từ node <code>i</code> đến node <code>edges[i]</code>. Nếu node <code>i</code> không có cạnh đi ra, thì <code>edges[i] == -1</code>.</p>

<p>Hãy trả về <em>độ dài của chu trình <strong>dài nhất</strong> trong đồ thị</em>. Nếu không tồn tại chu trình nào, trả về <code>-1</code>.</p>

<p>Chu trình là một đường đi bắt đầu và kết thúc tại <strong>cùng một</strong> node.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2360.Longest%20Cycle%20in%20a%20Graph/images/graph4drawio-5.png" style="width: 335px; height: 191px;" />
<pre>
<strong>Đầu vào:</strong> edges = [3,3,4,2,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chu trình dài nhất trong đồ thị là: 2 -&gt; 4 -&gt; 3 -&gt; 2.
Độ dài của chu trình này là 3, nên kết quả trả về là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2360.Longest%20Cycle%20in%20a%20Graph/images/graph4drawio-1.png" style="width: 171px; height: 161px;" />
<pre>
<strong>Đầu vào:</strong> edges = [2,-1,3,1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Đồ thị này không có chu trình nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-1 &lt;= edges[i] &lt; n</code></li>
	<li><code>edges[i] != i</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt từ các node bắt đầu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi node có nhiều nhất một cạnh đi ra, nên đồ thị gồm các đường đi dẫn vào các chu trình. Vì $n \le 10^5$, hãy đi theo successor duy nhất từ mỗi node chưa được duyệt.
>
> Đánh dấu các node và lưu lại đường đi. Dừng ở $-1$ nghĩa là không có chu trình; dừng ở một node thuộc đường đi này cho ta một chu trình bắt đầu từ chỉ số đó đến cuối đường đi. Theo dõi độ dài lớn nhất trong các chu trình đó.

<!-- thinking:end -->

Ta có thể duyệt từng node trong khoảng $[0,..,n-1]$. Nếu một node chưa được duyệt, ta bắt đầu từ node này và tìm các node kề cho đến khi gặp một chu trình hoặc một node đã được duyệt. Nếu gặp một chu trình, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng node.

Các bài toán tương tự:

- [2127. Maximum Employees to Be Invited to a Meeting](https://github.com/doocs/leetcode/blob/main/solution/2100-2199/2127.Maximum%20Employees%20to%20Be%20Invited%20to%20a%20Meeting/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestCycle(self, edges: List[int]) -> int:
        n = len(edges)
        vis = [False] * n
        ans = -1
        for i in range(n):
            if vis[i]:
                continue
            j = i
            cycle = []
            while j != -1 and not vis[j]:
                vis[j] = True
                cycle.append(j)
                j = edges[j]
            if j == -1:
                continue
            m = len(cycle)
            k = next((k for k in range(m) if cycle[k] == j), inf)
            ans = max(ans, m - k)
        return ans
```

#### Java

```java
class Solution {
    public int longestCycle(int[] edges) {
        int n = edges.length;
        boolean[] vis = new boolean[n];
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (vis[i]) {
                continue;
            }
            int j = i;
            List<Integer> cycle = new ArrayList<>();
            for (; j != -1 && !vis[j]; j = edges[j]) {
                vis[j] = true;
                cycle.add(j);
            }
            if (j == -1) {
                continue;
            }
            for (int k = 0; k < cycle.size(); ++k) {
                if (cycle.get(k) == j) {
                    ans = Math.max(ans, cycle.size() - k);
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
    int longestCycle(vector<int>& edges) {
        int n = edges.size();
        vector<bool> vis(n);
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (vis[i]) {
                continue;
            }
            int j = i;
            vector<int> cycle;
            for (; j != -1 && !vis[j]; j = edges[j]) {
                vis[j] = true;
                cycle.push_back(j);
            }
            if (j == -1) {
                continue;
            }
            for (int k = 0; k < cycle.size(); ++k) {
                if (cycle[k] == j) {
                    ans = max(ans, (int) cycle.size() - k);
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
func longestCycle(edges []int) int {
	vis := make([]bool, len(edges))
	ans := -1
	for i := range edges {
		if vis[i] {
			continue
		}
		j := i
		cycle := []int{}
		for ; j != -1 && !vis[j]; j = edges[j] {
			vis[j] = true
			cycle = append(cycle, j)
		}
		if j == -1 {
			continue
		}
		for k := range cycle {
			if cycle[k] == j {
				ans = max(ans, len(cycle)-k)
				break
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestCycle(edges: number[]): number {
    const n = edges.length;
    const vis: boolean[] = Array(n).fill(false);
    let ans = -1;
    for (let i = 0; i < n; ++i) {
        if (vis[i]) {
            continue;
        }
        let j = i;
        const cycle: number[] = [];
        for (; j !== -1 && !vis[j]; j = edges[j]) {
            vis[j] = true;
            cycle.push(j);
        }
        if (j === -1) {
            continue;
        }
        for (let k = 0; k < cycle.length; ++k) {
            if (cycle[k] === j) {
                ans = Math.max(ans, cycle.length - k);
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
    pub fn longest_cycle(edges: Vec<i32>) -> i32 {
        let n = edges.len();
        let mut vis = vec![false; n];
        let mut ans = -1;

        for i in 0..n {
            if vis[i] {
                continue;
            }
            let mut j = i as i32;
            let mut cycle = Vec::new();

            while j != -1 && !vis[j as usize] {
                vis[j as usize] = true;
                cycle.push(j);
                j = edges[j as usize];
            }

            if j == -1 {
                continue;
            }

            for k in 0..cycle.len() {
                if cycle[k] == j {
                    ans = ans.max((cycle.len() - k) as i32);
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
