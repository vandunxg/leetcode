---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [2764. Is Array a Preorder of Some ‌Binary Tree 🔒](https://leetcode.com/problems/is-array-a-preorder-of-some-binary-tree)

[中文文档](/solution/2700-2799/2764.Is%20Array%20a%20Preorder%20of%20Some%20%E2%80%8CBinary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>mảng 2D</strong> số nguyên được <strong>đánh chỉ số từ 0</strong> <code>nodes</code>, hãy xác định liệu mảng đã cho có biểu diễn phép duyệt <strong>preorder</strong> của một cây <strong>nhị phân</strong> nào đó hay không.</p>

<p>Với mỗi chỉ số <code>i</code>, <code>nodes[i] = [id, parentId]</code>, trong đó <code>id</code> là id của node tại chỉ số <code>i</code> và <code>parentId</code> là id của node cha trong cây (nếu node không có node cha thì <code>parentId == -1</code>).</p>

<p>Trả về <code>true</code> <em>nếu mảng đã cho </em><em>biểu diễn phép duyệt preorder của một cây, và</em> <code>false</code> <em>trong trường hợp ngược lại.</em></p>

<p><strong>Lưu ý:</strong> phép duyệt <strong>preorder</strong> của một cây là cách duyệt đệ quy, trong đó trước tiên ta thăm node hiện tại, sau đó thực hiện phép duyệt preorder cho node con trái, và cuối cùng thực hiện phép duyệt preorder cho node con phải.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nodes = [[0,-1],[1,0],[2,0],[3,2],[4,2]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các node đã cho tạo thành cây trong hình bên dưới.
Ta có thể thấy đây là phép duyệt preorder của cây: đầu tiên ta thăm node 0, sau đó thực hiện phép duyệt preorder của node con phải là [1], rồi thực hiện phép duyệt preorder của node con trái là [2,3,4].
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2764.Is%20Array%20a%20Preorder%20of%20Some%20%E2%80%8CBinary%20Tree/images/1.png" style="padding: 10px; background: #fff; border-radius: .5rem; width: 250px; height: 251px;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nodes = [[0,-1],[1,0],[2,0],[3,1],[4,1]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Các node đã cho tạo thành cây trong hình bên dưới.
Trong phép duyệt preorder, đầu tiên ta thăm node 0, sau đó thực hiện phép duyệt preorder của node con phải là [1,3,4]. Tuy nhiên, trong thứ tự đã cho, 2 nằm giữa 1 và 3, nên đây không phải là phép duyệt preorder của cây.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2764.Is%20Array%20a%20Preorder%20of%20Some%20%E2%80%8CBinary%20Tree/images/2.png" style="padding: 10px; background: #fff; border-radius: .5rem; width: 250px; height: 251px;" /></p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nodes.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nodes[i].length == 2</code></li>
	<li><code>0 &lt;= nodes[i][0] &lt;= 10<sup>5</sup></code></li>
	<li><code>-1 &lt;= nodes[i][1] &lt;= 10<sup>5</sup></code></li>
	<li>Input được tạo sao cho <code>nodes</code> tạo thành một cây nhị phân.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với các node có con trỏ đến node cha, ta cần xác định liệu danh sách có phải là phép duyệt preorder của một cây nhị phân hay không. Việc dựng lại cây rồi duyệt sẽ kiểm tra cùng một điều nhưng cần thêm cấu trúc dữ liệu.
>
> Ta tạo danh sách các node con từ các node cha, sau đó thực hiện DFS từ node gốc: node hiện tại phải bằng phần tử thứ $k$ trong danh sách, rồi tiếp tục đệ quy theo thứ tự các node con. Cuối cùng, $k$ phải bằng số lượng node.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPreorder(self, nodes: List[List[int]]) -> bool:
        def dfs(i: int) -> int:
            nonlocal k
            if i != nodes[k][0]:
                return False
            k += 1
            return all(dfs(j) for j in g[i])

        g = defaultdict(list)
        for i, p in nodes:
            g[p].append(i)
        k = 0
        return dfs(nodes[0][0]) and k == len(nodes)
```

#### Java

```java
class Solution {
    private Map<Integer, List<Integer>> g = new HashMap<>();
    private List<List<Integer>> nodes;
    private int k;

    public boolean isPreorder(List<List<Integer>> nodes) {
        this.nodes = nodes;
        for (var node : nodes) {
            g.computeIfAbsent(node.get(1), key -> new ArrayList<>()).add(node.get(0));
        }
        return dfs(nodes.get(0).get(0)) && k == nodes.size();
    }

    private boolean dfs(int i) {
        if (i != nodes.get(k).get(0)) {
            return false;
        }
        ++k;
        for (int j : g.getOrDefault(i, List.of())) {
            if (!dfs(j)) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPreorder(vector<vector<int>>& nodes) {
        int k = 0;
        unordered_map<int, vector<int>> g;
        for (auto& node : nodes) {
            g[node[1]].push_back(node[0]);
        }
        function<bool(int)> dfs = [&](int i) {
            if (i != nodes[k][0]) {
                return false;
            }
            ++k;
            for (int j : g[i]) {
                if (!dfs(j)) {
                    return false;
                }
            }
            return true;
        };
        return dfs(nodes[0][0]) && k == nodes.size();
    }
};
```

#### Go

```go
func isPreorder(nodes [][]int) bool {
	k := 0
	g := map[int][]int{}
	for _, node := range nodes {
		g[node[1]] = append(g[node[1]], node[0])
	}
	var dfs func(int) bool
	dfs = func(i int) bool {
		if i != nodes[k][0] {
			return false
		}
		k++
		for _, j := range g[i] {
			if !dfs(j) {
				return false
			}
		}
		return true
	}
	return dfs(nodes[0][0]) && k == len(nodes)
}
```

#### TypeScript

```ts
function isPreorder(nodes: number[][]): boolean {
    let k = 0;
    const g: Map<number, number[]> = new Map();
    for (const [i, p] of nodes) {
        if (!g.has(p)) {
            g.set(p, []);
        }
        g.get(p)!.push(i);
    }
    const dfs = (i: number): boolean => {
        if (i !== nodes[k][0]) {
            return false;
        }
        ++k;
        for (const j of g.get(i) ?? []) {
            if (!dfs(j)) {
                return false;
            }
        }
        return true;
    };
    return dfs(nodes[0][0]) && k === nodes.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
