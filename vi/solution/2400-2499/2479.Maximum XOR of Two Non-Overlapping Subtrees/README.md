---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Graph
    - Trie
---

<!-- problem:start -->

# [2479. Maximum XOR of Two Non-Overlapping Subtrees 🔒](https://leetcode.com/problems/maximum-xor-of-two-non-overlapping-subtrees)

[中文文档](/solution/2400-2499/2479.Maximum%20XOR%20of%20Two%20Non-Overlapping%20Subtrees/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho số nguyên <code>n</code> và một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh nối node <code>a<sub>i</sub></code> và node <code>b<sub>i</sub></code> trong cây. Gốc của cây là node được đánh số <code>0</code>.</p>

<p>Mỗi node có một <strong>giá trị</strong> tương ứng. Bạn được cho một mảng <code>values</code> có độ dài <code>n</code>, trong đó <code>values[i]</code> là <strong>giá trị</strong> của node thứ <code>i<sup>th</sup></code>.</p>

<p>Hãy chọn hai cây con <strong>không giao nhau</strong> bất kỳ. <strong>Điểm số</strong> của bạn là phép XOR bit của tổng các giá trị trong hai cây con đó.</p>

<p>Hãy trả về <em>kết quả</em> <em><strong>lớn nhất</strong></em> <i>là <strong>điểm số</strong> có thể đạt được</i>. <em>Nếu không thể tìm được hai cây con không giao nhau</em>, hãy trả về <code>0</code>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li><strong>Cây con</strong> của một node là cây gồm node đó và tất cả các node hậu duệ của nó.</li>
	<li>Hai cây con <strong>không giao nhau</strong> nếu chúng không có <strong>bất kỳ</strong> node chung nào.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2479.Maximum%20XOR%20of%20Two%20Non-Overlapping%20Subtrees/images/treemaxxor.png" style="width: 346px; height: 249px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,1],[0,2],[1,3],[1,4],[2,5]], values = [2,8,3,6,2,5]
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Cây con của node 1 có tổng giá trị là 16, còn cây con của node 2 có tổng giá trị là 8, nên nếu chọn hai node này thì điểm số sẽ là 16 XOR 8 = 24. Có thể chứng minh rằng đây là điểm số lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2479.Maximum%20XOR%20of%20Two%20Non-Overlapping%20Subtrees/images/tree3drawio.png" style="width: 240px; height: 261px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1],[1,2]], values = [4,6,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cách nào để chọn hai cây con không giao nhau, vì vậy ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>values.length == n</code></li>
	<li><code>1 &lt;= values[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>edges</code> được đảm bảo biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm XOR lớn nhất của tổng hai cây con không giao nhau, với $n\le 5\times 10^4$. DFS đầu tiên tính $s[i]$. Lượt duyệt thứ hai truy vấn trie trước khi chèn: truy vấn chỉ nhìn thấy các cây con đã hoàn tất và không giao với cây con hiện tại; tổng của cây con hiện tại được chèn sau khi các cây con của nó được duyệt xong.
>
> Tổng có thể đạt khoảng $10^{14}$, vì vậy ta dùng trie nhị phân $48$ bit và tham lam chọn bit đối nghịch.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 2

    def insert(self, x):
        node = self
        for i in range(47, -1, -1):
            v = (x >> i) & 1
            if node.children[v] is None:
                node.children[v] = Trie()
            node = node.children[v]

    def search(self, x):
        node = self
        res = 0
        for i in range(47, -1, -1):
            v = (x >> i) & 1
            if node is None:
                return res
            if node.children[v ^ 1]:
                res = res << 1 | 1
                node = node.children[v ^ 1]
            else:
                res <<= 1
                node = node.children[v]
        return res


class Solution:
    def maxXor(self, n: int, edges: List[List[int]], values: List[int]) -> int:
        def dfs1(i, fa):
            t = values[i]
            for j in g[i]:
                if j != fa:
                    t += dfs1(j, i)
            s[i] = t
            return t

        def dfs2(i, fa):
            nonlocal ans
            ans = max(ans, tree.search(s[i]))
            for j in g[i]:
                if j != fa:
                    dfs2(j, i)
            tree.insert(s[i])

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        s = [0] * n
        dfs1(0, -1)
        ans = 0
        tree = Trie()
        dfs2(0, -1)
        return ans
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[2];

    void insert(long x) {
        Trie node = this;
        for (int i = 47; i >= 0; --i) {
            int v = (int) (x >> i) & 1;
            if (node.children[v] == null) {
                node.children[v] = new Trie();
            }
            node = node.children[v];
        }
    }

    long search(long x) {
        Trie node = this;
        long res = 0;
        for (int i = 47; i >= 0; --i) {
            int v = (int) (x >> i) & 1;
            if (node == null) {
                return res;
            }
            if (node.children[v ^ 1] != null) {
                res = res << 1 | 1;
                node = node.children[v ^ 1];
            } else {
                res <<= 1;
                node = node.children[v];
            }
        }
        return res;
    }
}

class Solution {
    private List<Integer>[] g;
    private int[] vals;
    private long[] s;
    private Trie tree;
    private long ans;

    public long maxXor(int n, int[][] edges, int[] values) {
        g = new List[n];
        s = new long[n];
        vals = values;
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs1(0, -1);
        tree = new Trie();
        dfs2(0, -1);
        return ans;
    }

    private void dfs2(int i, int fa) {
        ans = Math.max(ans, tree.search(s[i]));
        for (int j : g[i]) {
            if (j != fa) {
                dfs2(j, i);
            }
        }
        tree.insert(s[i]);
    }

    private long dfs1(int i, int fa) {
        long t = vals[i];
        for (int j : g[i]) {
            if (j != fa) {
                t += dfs1(j, i);
            }
        }
        s[i] = t;
        return t;
    }
}
```

#### C++

```cpp
using ll = long long;

class Trie {
public:
    vector<Trie*> children;
    string v;
    Trie()
        : children(2) {}

    void insert(ll x) {
        Trie* node = this;
        for (int i = 47; ~i; --i) {
            int v = (x >> i) & 1;
            if (!node->children[v]) node->children[v] = new Trie();
            node = node->children[v];
        }
    }

    ll search(ll x) {
        Trie* node = this;
        ll res = 0;
        for (int i = 47; ~i; --i) {
            if (!node) return res;
            int v = (x >> i) & 1;
            if (node->children[v ^ 1]) {
                res = res << 1 | 1;
                node = node->children[v ^ 1];
            } else {
                res <<= 1;
                node = node->children[v];
            }
        }
        return res;
    }
};

class Solution {
public:
    long long maxXor(int n, vector<vector<int>>& edges, vector<int>& values) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        vector<ll> s(n);
        function<ll(int, int)> dfs1 = [&](int i, int fa) -> ll {
            ll t = values[i];
            for (int j : g[i]) {
                if (j != fa) t += dfs1(j, i);
            }
            s[i] = t;
            return t;
        };
        dfs1(0, -1);
        Trie tree;
        ll ans = 0;
        function<void(int, int)> dfs2 = [&](int i, int fa) {
            ans = max(ans, tree.search(s[i]));
            for (int j : g[i]) {
                if (j != fa) {
                    dfs2(j, i);
                }
            }
            tree.insert(s[i]);
        };
        dfs2(0, -1);
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [2]*Trie
}

func newTrie() *Trie {
	return &Trie{}
}

func (this *Trie) insert(x int) {
	node := this
	for i := 47; i >= 0; i-- {
		v := (x >> i) & 1
		if node.children[v] == nil {
			node.children[v] = newTrie()
		}
		node = node.children[v]
	}
}

func (this *Trie) search(x int) int {
	node := this
	res := 0
	for i := 47; i >= 0; i-- {
		v := (x >> i) & 1
		if node == nil {
			return res
		}
		if node.children[v^1] != nil {
			res = res<<1 | 1
			node = node.children[v^1]
		} else {
			res <<= 1
			node = node.children[v]
		}
	}
	return res
}

func maxXor(n int, edges [][]int, values []int) int64 {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	s := make([]int, n)
	var dfs1 func(i, fa int) int
	dfs1 = func(i, fa int) int {
		t := values[i]
		for _, j := range g[i] {
			if j != fa {
				t += dfs1(j, i)
			}
		}
		s[i] = t
		return t
	}
	dfs1(0, -1)
	ans := 0
	tree := newTrie()
	var dfs2 func(i, fa int)
	dfs2 = func(i, fa int) {
		ans = max(ans, tree.search(s[i]))
		for _, j := range g[i] {
			if j != fa {
				dfs2(j, i)
			}
		}
		tree.insert(s[i])
	}
	dfs2(0, -1)
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
