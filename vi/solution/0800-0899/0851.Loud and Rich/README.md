---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Graph
    - Topological Sort
    - Array
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [851. Loud and Rich](https://leetcode.com/problems/loud-and-rich)

[中文文档](/solution/0800-0899/0851.Loud%20and%20Rich/README.md)

## Mô tả

<!-- description:start -->

<p>Có một nhóm gồm <code>n</code> người được đánh số từ <code>0</code> đến <code>n - 1</code>; mỗi người có số tiền và mức độ ít nói khác nhau.</p>

<p>Cho mảng <code>richer</code>, trong đó <code>richer[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết <code>a<sub>i</sub></code> có nhiều tiền hơn <code>b<sub>i</sub></code>, và mảng số nguyên <code>quiet</code>, trong đó <code>quiet[i]</code> biểu thị mức độ ít nói của người thứ <code>i<sup>th</sup></code>. Dữ liệu trong richer đều <strong>hợp lý về mặt logic</strong> (tức là không thể suy ra rằng <code>x</code> giàu hơn <code>y</code> đồng thời <code>y</code> cũng giàu hơn <code>x</code>).</p>

<p>Hãy trả về <em>mảng số nguyên </em><code>answer</code><em>, trong đó </em><code>answer[x] = y</code><em> nếu </em><code>y</code><em> là người ít nói nhất (tức là người </em><code>y</code><em> có giá trị </em><code>quiet[y]</code><em> nhỏ nhất) trong số những người chắc chắn có số tiền bằng hoặc nhiều hơn người </em><code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> richer = [[1,0],[2,1],[3,1],[3,7],[4,3],[5,3],[6,3]], quiet = [3,2,5,4,6,1,7,0]
<strong>Đầu ra:</strong> [5,5,2,5,4,5,6,7]
<strong>Giải thích:</strong> 
answer[0] = 5.
Người 5 có nhiều tiền hơn người 3, người 3 có nhiều tiền hơn người 1, và người 1 có nhiều tiền hơn người 0.
Người duy nhất ít nói hơn (có quiet[x] thấp hơn) là người 7, nhưng không rõ người đó có nhiều tiền hơn người 0 hay không.
answer[7] = 7.
Trong số những người chắc chắn có số tiền bằng hoặc nhiều hơn người 7 (có thể là người 3, 4, 5, 6 hoặc 7), người ít nói nhất (có quiet[x] thấp hơn) là người 7.
Các đáp án còn lại có thể tìm được bằng cách lập luận tương tự.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> richer = [], quiet = [0]
<strong>Đầu ra:</strong> [0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == quiet.length</code></li>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>0 &lt;= quiet[i] &lt; n</code></li>
	<li>Tất cả giá trị trong <code>quiet</code> đều <strong>khác nhau</strong>.</li>
	<li><code>0 &lt;= richer.length &lt;= n * (n - 1) / 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i </sub>!= b<sub>i</sub></code></li>
	<li>Tất cả các cặp trong <code>richer</code> đều <strong>khác nhau</strong>.</li>
	<li>Các thông tin trong <code>richer</code> đều nhất quán về mặt logic.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trên DAG biểu diễn quan hệ “giàu hơn”, mỗi người cần tìm người ít nói nhất trong số họ và tất cả người giàu hơn họ. $n\le 500$, nên tìm kiếm lại từ đầu cho từng người sẽ duyệt lặp nhiều subgraph.
>
> Hướng cạnh từ người ít tiền hơn đến người nhiều tiền hơn và memoize kết quả DFS: bắt đầu từ chính người đó, rồi chọn đáp án ít nói hơn trong các neighbor giàu hơn. Mỗi node chỉ được tính một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def loudAndRich(self, richer: List[List[int]], quiet: List[int]) -> List[int]:
        def dfs(i: int):
            if ans[i] != -1:
                return
            ans[i] = i
            for j in g[i]:
                dfs(j)
                if quiet[ans[j]] < quiet[ans[i]]:
                    ans[i] = ans[j]

        g = defaultdict(list)
        for a, b in richer:
            g[b].append(a)
        n = len(quiet)
        ans = [-1] * n
        for i in range(n):
            dfs(i)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int n;
    private int[] quiet;
    private int[] ans;

    public int[] loudAndRich(int[][] richer, int[] quiet) {
        n = quiet.length;
        this.quiet = quiet;
        g = new List[n];
        ans = new int[n];
        Arrays.fill(ans, -1);
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var r : richer) {
            g[r[1]].add(r[0]);
        }
        for (int i = 0; i < n; ++i) {
            dfs(i);
        }
        return ans;
    }

    private void dfs(int i) {
        if (ans[i] != -1) {
            return;
        }
        ans[i] = i;
        for (int j : g[i]) {
            dfs(j);
            if (quiet[ans[j]] < quiet[ans[i]]) {
                ans[i] = ans[j];
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> loudAndRich(vector<vector<int>>& richer, vector<int>& quiet) {
        int n = quiet.size();
        vector<vector<int>> g(n);
        for (auto& r : richer) {
            g[r[1]].push_back(r[0]);
        }
        vector<int> ans(n, -1);
        function<void(int)> dfs = [&](int i) {
            if (ans[i] != -1) {
                return;
            }
            ans[i] = i;
            for (int j : g[i]) {
                dfs(j);
                if (quiet[ans[j]] < quiet[ans[i]]) {
                    ans[i] = ans[j];
                }
            }
        };
        for (int i = 0; i < n; ++i) {
            dfs(i);
        }
        return ans;
    }
};
```

#### Go

```go
func loudAndRich(richer [][]int, quiet []int) []int {
	n := len(quiet)
	g := make([][]int, n)
	ans := make([]int, n)
	for i := range g {
		ans[i] = -1
	}
	for _, r := range richer {
		a, b := r[0], r[1]
		g[b] = append(g[b], a)
	}
	var dfs func(int)
	dfs = func(i int) {
		if ans[i] != -1 {
			return
		}
		ans[i] = i
		for _, j := range g[i] {
			dfs(j)
			if quiet[ans[j]] < quiet[ans[i]] {
				ans[i] = ans[j]
			}
		}
	}
	for i := range ans {
		dfs(i)
	}
	return ans
}
```

#### TypeScript

```ts
function loudAndRich(richer: number[][], quiet: number[]): number[] {
    const n = quiet.length;
    const g: number[][] = new Array(n).fill(0).map(() => []);
    for (const [a, b] of richer) {
        g[b].push(a);
    }
    const ans: number[] = new Array(n).fill(-1);
    const dfs = (i: number) => {
        if (ans[i] != -1) {
            return ans;
        }
        ans[i] = i;
        for (const j of g[i]) {
            dfs(j);
            if (quiet[ans[j]] < quiet[ans[i]]) {
                ans[i] = ans[j];
            }
        }
    };
    for (let i = 0; i < n; ++i) {
        dfs(i);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
