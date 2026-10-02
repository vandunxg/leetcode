---
comments: true
difficulty: Hard
rating: 3018
source: Biweekly Contest 43 Q4
tags:
    - Tree
    - Graph
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [1719. Number Of Ways To Reconstruct A Tree](https://leetcode.com/problems/number-of-ways-to-reconstruct-a-tree)

[中文文档](/solution/1700-1799/1719.Number%20Of%20Ways%20To%20Reconstruct%20A%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>pairs</code>, trong đó <code>pairs[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>, và:</p>

<ul>
	<li>Không có phần tử trùng lặp.</li>
	<li><code>x<sub>i</sub> &lt; y<sub>i</sub></code></li>
</ul>

<p>Gọi <code>ways</code> là số cây có gốc thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Cây gồm các node có giá trị xuất hiện trong <code>pairs</code>.</li>
	<li>Cặp <code>[x<sub>i</sub>, y<sub>i</sub>]</code> xuất hiện trong <code>pairs</code> <strong>khi và chỉ khi</strong> <code>x<sub>i</sub></code> là tổ tiên của <code>y<sub>i</sub></code> hoặc <code>y<sub>i</sub></code> là tổ tiên của <code>x<sub>i</sub></code>.</li>
	<li><strong>Lưu ý:</strong> cây không nhất thiết là cây nhị phân.</li>
</ul>

<p>Hai cách được xem là khác nhau nếu có ít nhất một node có parent khác nhau trong hai cách.</p>

<p>Trả về:</p>

<ul>
	<li><code>0</code> if <code>ways == 0</code></li>
	<li><code>1</code> if <code>ways == 1</code></li>
	<li><code>2</code> if <code>ways &gt; 1</code></li>
</ul>

<p><strong>Cây có gốc</strong> là cây có một node gốc duy nhất, và mọi cạnh đều hướng ra từ node gốc.</p>

<p><strong>Tổ tiên</strong> của một node là bất kỳ node nào trên đường đi từ gốc đến node đó (không bao gồm chính node đó). Node gốc không có tổ tiên.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1719.Number%20Of%20Ways%20To%20Reconstruct%20A%20Tree/images/trees2.png" style="width: 208px; height: 221px;" />
<pre>
<strong>Input:</strong> pairs = [[1,2],[2,3]]
<strong>Output:</strong> 1
<strong>Explanation:</strong> Có đúng một cây có gốc hợp lệ, như trong hình phía trên.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1719.Number%20Of%20Ways%20To%20Reconstruct%20A%20Tree/images/tree.png" style="width: 234px; height: 241px;" />
<pre>
<strong>Input:</strong> pairs = [[1,2],[2,3],[1,3]]
<strong>Output:</strong> 2
<strong>Explanation:</strong> Có nhiều cây có gốc hợp lệ. Ba trong số đó được minh họa ở các hình phía trên.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> pairs = [[1,2],[2,3],[2,4],[1,5]]
<strong>Output:</strong> 0
<strong>Explanation:</strong> Không có cây có gốc hợp lệ.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= pairs.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= x<sub>i </sub>&lt; y<sub>i</sub> &lt;= 500</code></li>
	<li>Các phần tử trong <code>pairs</code> là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp mã hóa quan hệ tổ tiên: một cạnh tồn tại khi và chỉ khi một node là tổ tiên của node kia. Ta cần xác định có $0$, $1$ hay nhiều hơn một cây có gốc phù hợp.
>
> Bậc trong các cặp càng lớn thì node càng gần gốc. Sắp xếp các node theo bậc; parent ứng viên của một node phải là hàng xóm có bậc không nhỏ hơn.
>
> Ứng viên phải kề với mọi hàng xóm của node. Có nhiều hơn một node không có parent là không thể. Nếu một cặp parent-child có cùng bậc thì có nhiều cây; nếu không thì chỉ có một cây.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkWays(self, pairs: List[List[int]]) -> int:
        g = [[False] * 510 for _ in range(510)]
        v = defaultdict(list)
        for x, y in pairs:
            g[x][y] = g[y][x] = True
            v[x].append(y)
            v[y].append(x)
        nodes = []
        for i in range(510):
            if v[i]:
                nodes.append(i)
                g[i][i] = True
        nodes.sort(key=lambda x: len(v[x]))
        equal = False
        root = 0
        for i, x in enumerate(nodes):
            j = i + 1
            while j < len(nodes) and not g[x][nodes[j]]:
                j += 1
            if j < len(nodes):
                y = nodes[j]
                if len(v[x]) == len(v[y]):
                    equal = True
                for z in v[x]:
                    if not g[y][z]:
                        return 0
            else:
                root += 1
        if root > 1:
            return 0
        return 2 if equal else 1
```

#### Java

```java
class Solution {
    public int checkWays(int[][] pairs) {
        boolean[][] g = new boolean[510][510];
        List<Integer>[] v = new List[510];
        Arrays.setAll(v, k -> new ArrayList<>());
        for (int[] p : pairs) {
            int x = p[0], y = p[1];
            g[x][y] = true;
            g[y][x] = true;
            v[x].add(y);
            v[y].add(x);
        }
        List<Integer> nodes = new ArrayList<>();
        for (int i = 0; i < 510; ++i) {
            if (!v[i].isEmpty()) {
                nodes.add(i);
                g[i][i] = true;
            }
        }
        nodes.sort(Comparator.comparingInt(a -> v[a].size()));
        boolean equal = false;
        int root = 0;
        for (int i = 0; i < nodes.size(); ++i) {
            int x = nodes.get(i);
            int j = i + 1;
            for (; j < nodes.size() && !g[x][nodes.get(j)]; ++j)
                ;
            if (j < nodes.size()) {
                int y = nodes.get(j);
                if (v[x].size() == v[y].size()) {
                    equal = true;
                }
                for (int z : v[x]) {
                    if (!g[y][z]) {
                        return 0;
                    }
                }
            } else {
                ++root;
            }
        }
        if (root > 1) {
            return 0;
        }
        return equal ? 2 : 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int checkWays(vector<vector<int>>& pairs) {
        vector<vector<bool>> g(510, vector<bool>(510));
        vector<vector<int>> v(510);
        for (auto& p : pairs) {
            int x = p[0], y = p[1];
            g[x][y] = g[y][x] = 1;
            v[x].push_back(y);
            v[y].push_back(x);
        }
        vector<int> nodes;
        for (int i = 1; i <= 500; ++i) {
            if (v[i].size()) {
                nodes.push_back(i);
                g[i][i] = 1;
            }
        }
        sort(nodes.begin(), nodes.end(), [&](int x, int y) -> bool { return v[x].size() < v[y].size(); });
        bool equal = 0;
        int root = 0;
        for (int i = 0; i < nodes.size(); ++i) {
            int x = nodes[i];
            int j = i + 1;
            for (; j < nodes.size() && !g[x][nodes[j]]; ++j)
                ;
            if (j < nodes.size()) {
                int y = nodes[j];
                if (v[x].size() == v[y].size()) equal = 1;
                for (int z : v[x])
                    if (!g[y][z])
                        return 0;
            } else
                ++root;
        }
        if (root > 1) return 0;
        if (equal) return 2;
        return 1;
    }
};
```

#### Go

```go
func checkWays(pairs [][]int) int {
	g := make([][]bool, 510)
	v := make([][]int, 510)
	for i := range g {
		g[i] = make([]bool, 510)
	}
	for _, p := range pairs {
		x, y := p[0], p[1]
		g[x][y] = true
		g[y][x] = true
		v[x] = append(v[x], y)
		v[y] = append(v[y], x)
	}
	var nodes []int
	for i := 1; i <= 500; i++ {
		if len(v[i]) > 0 {
			nodes = append(nodes, i)
			g[i][i] = true
		}
	}
	sort.Slice(nodes, func(i, j int) bool {
		return len(v[nodes[i]]) < len(v[nodes[j]])
	})
	equal := false
	root := 0
	for i, x := range nodes {
		j := i + 1
		for ; j < len(nodes) && !g[x][nodes[j]]; j++ {
		}
		if j < len(nodes) {
			y := nodes[j]
			if len(v[x]) == len(v[y]) {
				equal = true
			}
			for _, z := range v[x] {
				if !g[y][z] {
					return 0
				}
			}
		} else {
			root++
		}
	}
	if root > 1 {
		return 0
	}
	if equal {
		return 2
	}
	return 1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
