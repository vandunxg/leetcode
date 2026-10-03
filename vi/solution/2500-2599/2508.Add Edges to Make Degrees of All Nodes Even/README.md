---
comments: true
difficulty: Hard
rating: 2060
source: Weekly Contest 324 Q3
tags:
    - Graph
    - Hash Table
---

<!-- problem:start -->

# [2508. Add Edges to Make Degrees of All Nodes Even](https://leetcode.com/problems/add-edges-to-make-degrees-of-all-nodes-even)

[中文文档](/solution/2500-2599/2508.Add%20Edges%20to%20Make%20Degrees%20of%20All%20Nodes%20Even/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị <strong>vô hướng</strong> gồm <code>n</code> đỉnh được đánh số từ <code>1</code> đến <code>n</code>. Cho số nguyên <code>n</code> và một mảng <strong>2D</strong> <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh giữa các đỉnh <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>. Đồ thị có thể không liên thông.</p>

<p>Bạn có thể thêm <strong>nhiều nhất</strong> hai cạnh bổ sung (có thể không thêm cạnh nào) vào đồ thị này sao cho không có cạnh trùng lặp và không có self-loop.</p>

<p>Trả về <code>true</code><em> nếu có thể làm cho bậc của mỗi đỉnh trong đồ thị là số chẵn, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>Bậc của một đỉnh là số cạnh nối với đỉnh đó.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2508.Add%20Edges%20to%20Make%20Degrees%20of%20All%20Nodes%20Even/images/agraphdrawio.png" style="width: 500px; height: 190px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[1,2],[2,3],[3,4],[4,2],[1,4],[2,5]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hình trên minh họa một cách thêm cạnh hợp lệ.
Mỗi đỉnh trong đồ thị kết quả đều được nối với một số cạnh chẵn.
</pre>

<p><strong>Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2508.Add%20Edges%20to%20Make%20Degrees%20of%20All%20Nodes%20Even/images/aagraphdrawio.png" style="width: 400px; height: 120px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[1,2],[3,4]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hình trên minh họa một cách thêm hai cạnh hợp lệ.</pre>

<p><strong>Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2508.Add%20Edges%20to%20Make%20Degrees%20of%20All%20Nodes%20Even/images/aaagraphdrawio.png" style="width: 150px; height: 158px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[1,2],[1,3],[1,4]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể tạo ra một đồ thị hợp lệ bằng cách thêm nhiều nhất 2 cạnh.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Có thể thêm nhiều nhất hai cạnh mới và các cạnh này không được trùng với cạnh hiện có, đồng thời bậc của mọi đỉnh phải trở thành số chẵn. Bổ đề bắt tay buộc số đỉnh có bậc lẻ phải là số chẵn, và mỗi cạnh làm thay đổi tính chẵn lẻ của hai đỉnh, nên không thể có nhiều hơn bốn đỉnh có bậc lẻ.
>
> Xây dựng các tập kề và thu thập các đỉnh có bậc lẻ vào $vs$. Trường hợp có $0$ đỉnh đã thỏa mãn. Với hai đỉnh, nối chúng nếu chúng không kề nhau; nếu không, tìm một đỉnh thứ ba không kề với cả hai. Với bốn đỉnh, thử ba cách ghép cặp hoàn hảo và chấp nhận nếu một cách ghép sử dụng hai cạnh còn thiếu.

<!-- thinking:end -->

Trước hết, chúng ta xây dựng đồ thị $g$ bằng `edges`, sau đó tìm tất cả các đỉnh có bậc lẻ, ký hiệu là $vs$.

Nếu độ dài của $vs$ là $0$, nghĩa là mọi đỉnh trong đồ thị $g$ đều có bậc chẵn, nên trả về `true`.

Nếu độ dài của $vs$ là $2$, nghĩa là có hai đỉnh có bậc lẻ trong đồ thị $g$. Nếu có thể nối trực tiếp hai đỉnh này bằng một cạnh, làm cho mọi đỉnh trong đồ thị $g$ có bậc chẵn, ta trả về `true`. Nếu không, nếu tìm được một đỉnh thứ ba $c$ sao cho có thể nối $a$ với $c$ và $b$ với $c$, làm cho mọi đỉnh trong đồ thị $g$ có bậc chẵn, ta trả về `true`. Nếu không, ta trả về `false`.

Nếu độ dài của $vs$ là $4$, ta liệt kê tất cả các cặp có thể và kiểm tra xem có tổ hợp nào thỏa mãn điều kiện hay không. Nếu có, ta trả về `true`; nếu không, ta trả về `false`.

Trong các trường hợp khác, ta trả về `false`.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó $n$ và $m$ lần lượt là số đỉnh và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPossible(self, n: int, edges: List[List[int]]) -> bool:
        g = defaultdict(set)
        for a, b in edges:
            g[a].add(b)
            g[b].add(a)
        vs = [i for i, v in g.items() if len(v) & 1]
        if len(vs) == 0:
            return True
        if len(vs) == 2:
            a, b = vs
            if a not in g[b]:
                return True
            return any(a not in g[c] and c not in g[b] for c in range(1, n + 1))
        if len(vs) == 4:
            a, b, c, d = vs
            if a not in g[b] and c not in g[d]:
                return True
            if a not in g[c] and b not in g[d]:
                return True
            if a not in g[d] and b not in g[c]:
                return True
            return False
        return False
```

#### Java

```java
class Solution {
    public boolean isPossible(int n, List<List<Integer>> edges) {
        Set<Integer>[] g = new Set[n + 1];
        Arrays.setAll(g, k -> new HashSet<>());
        for (var e : edges) {
            int a = e.get(0), b = e.get(1);
            g[a].add(b);
            g[b].add(a);
        }
        List<Integer> vs = new ArrayList<>();
        for (int i = 1; i <= n; ++i) {
            if (g[i].size() % 2 == 1) {
                vs.add(i);
            }
        }
        if (vs.size() == 0) {
            return true;
        }
        if (vs.size() == 2) {
            int a = vs.get(0), b = vs.get(1);
            if (!g[a].contains(b)) {
                return true;
            }
            for (int c = 1; c <= n; ++c) {
                if (a != c && b != c && !g[a].contains(c) && !g[c].contains(b)) {
                    return true;
                }
            }
            return false;
        }
        if (vs.size() == 4) {
            int a = vs.get(0), b = vs.get(1), c = vs.get(2), d = vs.get(3);
            if (!g[a].contains(b) && !g[c].contains(d)) {
                return true;
            }
            if (!g[a].contains(c) && !g[b].contains(d)) {
                return true;
            }
            if (!g[a].contains(d) && !g[b].contains(c)) {
                return true;
            }
            return false;
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPossible(int n, vector<vector<int>>& edges) {
        vector<unordered_set<int>> g(n + 1);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].insert(b);
            g[b].insert(a);
        }
        vector<int> vs;
        for (int i = 1; i <= n; ++i) {
            if (g[i].size() % 2) {
                vs.emplace_back(i);
            }
        }
        if (vs.size() == 0) {
            return true;
        }
        if (vs.size() == 2) {
            int a = vs[0], b = vs[1];
            if (!g[a].count(b)) return true;
            for (int c = 1; c <= n; ++c) {
                if (a != b && b != c && !g[a].count(c) && !g[c].count(b)) {
                    return true;
                }
            }
            return false;
        }
        if (vs.size() == 4) {
            int a = vs[0], b = vs[1], c = vs[2], d = vs[3];
            if (!g[a].count(b) && !g[c].count(d)) return true;
            if (!g[a].count(c) && !g[b].count(d)) return true;
            if (!g[a].count(d) && !g[b].count(c)) return true;
            return false;
        }
        return false;
    }
};
```

#### Go

```go
func isPossible(n int, edges [][]int) bool {
	g := make([]map[int]bool, n+1)
	for _, e := range edges {
		a, b := e[0], e[1]
		if g[a] == nil {
			g[a] = map[int]bool{}
		}
		if g[b] == nil {
			g[b] = map[int]bool{}
		}
		g[a][b], g[b][a] = true, true
	}
	vs := []int{}
	for i := 1; i <= n; i++ {
		if len(g[i])%2 == 1 {
			vs = append(vs, i)
		}
	}
	if len(vs) == 0 {
		return true
	}
	if len(vs) == 2 {
		a, b := vs[0], vs[1]
		if !g[a][b] {
			return true
		}
		for c := 1; c <= n; c++ {
			if a != c && b != c && !g[a][c] && !g[c][b] {
				return true
			}
		}
		return false
	}
	if len(vs) == 4 {
		a, b, c, d := vs[0], vs[1], vs[2], vs[3]
		if !g[a][b] && !g[c][d] {
			return true
		}
		if !g[a][c] && !g[b][d] {
			return true
		}
		if !g[a][d] && !g[b][c] {
			return true
		}
		return false
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
