---
comments: true
difficulty: Medium
rating: 1732
source: Biweekly Contest 14 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Tree DP
---

<!-- problem:start -->

# [1273. Delete Tree Nodes 🔒](https://leetcode.com/problems/delete-tree-nodes)

[中文文档](/solution/1200-1299/1273.Delete%20Tree%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây có root là node 0, với các thông tin sau:</p>

<ul>
	<li>Số node là <code>nodes</code>;</li>
	<li>Giá trị của node thứ <code>i<sup>th</sup></code> là <code>value[i]</code>;</li>
	<li>Node cha của node thứ <code>i<sup>th</sup></code> là <code>parent[i]</code>.</li>
</ul>

<p>Xóa mọi subtree có tổng giá trị các node bằng 0.</p>

<p>Trả về <em>số node còn lại trong cây</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1273.Delete%20Tree%20Nodes/images/1421_sample_1.png" style="width: 403px; height: 347px;" />
<pre>
<strong>Input:</strong> nodes = 7, parent = [-1,0,0,1,2,2,2], value = [1,-2,4,0,-2,-1,-1]
<strong>Output:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nodes = 7, parent = [-1,0,0,1,2,2,2], value = [1,-2,4,0,-2,-1,-2]
<strong>Output:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nodes &lt;= 10<sup>4</sup></code></li>
	<li><code>parent.length == nodes</code></li>
	<li><code>0 &lt;= parent[i] &lt;= nodes - 1</code></li>
	<li><code>parent[0] == -1</code>, cho biết <code>0</code> là root.</li>
	<li><code>value.length == nodes</code></li>
	<li><code>-10<sup>5</sup> &lt;= value[i] &lt;= 10<sup>5</sup></code></li>
	<li>Đầu vào được <strong>đảm bảo</strong> biểu diễn một <strong>cây hợp lệ</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Subtree có tổng giá trị bằng $0$ sẽ bị xóa. Vì $n \le 10^4$, ta chỉ cần DFS từ dưới lên: cộng tổng giá trị và số node còn lại của các node con, rồi đặt số node còn lại bằng $0$ nếu tổng của subtree này bằng $0$. Số node còn lại ở root chính là đáp án. Duyệt hậu thứ tự đảm bảo xử lý các node con trước khi quyết định ở node cha.

<!-- thinking:end -->

Đầu tiên, ta chuyển cây thành graph $g$, trong đó $g[i]$ chứa tất cả node con của node $i$.

Tiếp theo, ta định nghĩa hàm $dfs(i)$ trả về số node và tổng trọng số của subtree có root tại node $i$. Đáp án là $dfs(0)[1]$.

Trong hàm này, ta đệ quy tính số node và tổng trọng số của subtree tại mỗi node con $j$, sau đó cộng dồn các giá trị đó. Nếu tổng tích lũy bằng 0, ta đặt số node của subtree này bằng 0. Cuối cùng, trả về số node và tổng trọng số của subtree có root tại node $i$.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node trong cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def deleteTreeNodes(self, nodes: int, parent: List[int], value: List[int]) -> int:
        def dfs(i):
            s, m = value[i], 1
            for j in g[i]:
                t, n = dfs(j)
                s += t
                m += n
            if s == 0:
                m = 0
            return (s, m)

        g = defaultdict(list)
        for i in range(1, nodes):
            g[parent[i]].append(i)
        return dfs(0)[1]
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] value;

    public int deleteTreeNodes(int nodes, int[] parent, int[] value) {
        g = new List[nodes];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 1; i < nodes; ++i) {
            g[parent[i]].add(i);
        }
        this.value = value;
        return dfs(0)[1];
    }

    private int[] dfs(int i) {
        int[] res = new int[] {value[i], 1};
        for (int j : g[i]) {
            int[] t = dfs(j);
            res[0] += t[0];
            res[1] += t[1];
        }
        if (res[0] == 0) {
            res[1] = 0;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int deleteTreeNodes(int nodes, vector<int>& parent, vector<int>& value) {
        vector<vector<int>> g(nodes);
        for (int i = 1; i < nodes; ++i) {
            g[parent[i]].emplace_back(i);
        }
        function<pair<int, int>(int)> dfs = [&](int i) -> pair<int, int> {
            int s = value[i], m = 1;
            for (int j : g[i]) {
                auto [t, n] = dfs(j);
                s += t;
                m += n;
            }
            if (s == 0) {
                m = 0;
            }
            return pair<int, int>{s, m};
        };
        return dfs(0).second;
    }
};
```

#### Go

```go
func deleteTreeNodes(nodes int, parent []int, value []int) int {
	g := make([][]int, nodes)
	for i := 1; i < nodes; i++ {
		g[parent[i]] = append(g[parent[i]], i)
	}
	type pair struct{ s, n int }
	var dfs func(int) pair
	dfs = func(i int) pair {
		s, m := value[i], 1
		for _, j := range g[i] {
			t := dfs(j)
			s += t.s
			m += t.n
		}
		if s == 0 {
			m = 0
		}
		return pair{s, m}
	}
	return dfs(0).n
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
