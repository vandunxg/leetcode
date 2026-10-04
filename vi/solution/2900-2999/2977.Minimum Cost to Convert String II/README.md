---
comments: true
difficulty: Hard
rating: 2695
source: Weekly Contest 377 Q4
tags:
    - Graph
    - Trie
    - Array
    - String
    - Dynamic Programming
    - Shortest Path
---

<!-- problem:start -->

# [2977. Minimum Cost to Convert String II](https://leetcode.com/problems/minimum-cost-to-convert-string-ii)

[Tài liệu tiếng Trung](/solution/2900-2999/2977.Minimum%20Cost%20to%20Convert%20String%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>source</code> và <code>target</code> được đánh số từ <strong>0</strong>, cả hai có độ dài <code>n</code> và chỉ gồm các ký tự tiếng Anh <strong>chữ thường</strong>. Ngoài ra, cho hai mảng chuỗi <code>original</code> và <code>changed</code> được đánh số từ <strong>0</strong>, cùng một mảng số nguyên <code>cost</code>, trong đó <code>cost[i]</code> là chi phí chuyển chuỗi <code>original[i]</code> thành chuỗi <code>changed[i]</code>.</p>

<p>Ban đầu, bạn có chuỗi <code>source</code>. Trong một thao tác, bạn có thể chọn một <strong>substring</strong> <code>x</code> trong chuỗi và đổi nó thành <code>y</code> với chi phí <code>z</code> <strong>nếu</strong> tồn tại <strong>bất kỳ</strong> chỉ số <code>j</code> nào sao cho <code>cost[j] == z</code>, <code>original[j] == x</code> và <code>changed[j] == y</code>. Bạn có thể thực hiện <strong>bất kỳ số lượng</strong> thao tác nào, nhưng mọi cặp thao tác phải thỏa mãn <strong>một trong hai</strong> điều kiện sau:</p>

<ul>
	<li>Các substring được chọn trong hai thao tác là <code>source[a..b]</code> và <code>source[c..d]</code>, với <code>b &lt; c</code> <strong>hoặc</strong> <code>d &lt; a</code>. Nói cách khác, các chỉ số được chọn trong hai thao tác là <strong>không giao nhau</strong>.</li>
	<li>Các substring được chọn trong hai thao tác là <code>source[a..b]</code> và <code>source[c..d]</code>, với <code>a == c</code> <strong>và</strong> <code>b == d</code>. Nói cách khác, các chỉ số được chọn trong hai thao tác là <strong>giống hệt nhau</strong>.</li>
</ul>

<p>Hãy trả về <em>chi phí <strong>nhỏ nhất</strong> để chuyển chuỗi </em><code>source</code><em> thành chuỗi </em><code>target</code><em> bằng cách sử dụng <strong>bất kỳ số lượng</strong> thao tác nào</em>. <em>Nếu không thể chuyển đổi</em> <code>source</code> <em>thành</em> <code>target</code>, <em>hãy trả về</em> <code>-1</code>.</p>

<p><strong>Lưu ý</strong> rằng có thể tồn tại các chỉ số <code>i</code>, <code>j</code> sao cho <code>original[j] == original[i]</code> và <code>changed[j] == changed[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;abcd&quot;, target = &quot;acbe&quot;, original = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;c&quot;,&quot;e&quot;,&quot;d&quot;], changed = [&quot;b&quot;,&quot;c&quot;,&quot;b&quot;,&quot;e&quot;,&quot;b&quot;,&quot;e&quot;], cost = [2,5,5,1,2,20]
<strong>Đầu ra:</strong> 28
<strong>Giải thích:</strong> Để chuyển &quot;abcd&quot; thành &quot;acbe&quot;, thực hiện các thao tác sau:
- Đổi substring source[1..1] từ &quot;b&quot; thành &quot;c&quot; với chi phí 5.
- Đổi substring source[2..2] từ &quot;c&quot; thành &quot;e&quot; với chi phí 1.
- Đổi substring source[2..2] từ &quot;e&quot; thành &quot;b&quot; với chi phí 2.
- Đổi substring source[3..3] từ &quot;d&quot; thành &quot;e&quot; với chi phí 20.
Tổng chi phí là 5 + 1 + 2 + 20 = 28.
Có thể chứng minh đây là chi phí nhỏ nhất có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;abcdefgh&quot;, target = &quot;acdeeghh&quot;, original = [&quot;bcd&quot;,&quot;fgh&quot;,&quot;thh&quot;], changed = [&quot;cde&quot;,&quot;thh&quot;,&quot;ghh&quot;], cost = [1,3,5]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Để chuyển &quot;abcdefgh&quot; thành &quot;acdeeghh&quot;, thực hiện các thao tác sau:
- Đổi substring source[1..3] từ &quot;bcd&quot; thành &quot;cde&quot; với chi phí 1.
- Đổi substring source[5..7] từ &quot;fgh&quot; thành &quot;thh&quot; với chi phí 3. Có thể thực hiện thao tác này vì các chỉ số [5,7] không giao nhau với các chỉ số được chọn trong thao tác đầu tiên.
- Đổi substring source[5..7] từ &quot;thh&quot; thành &quot;ghh&quot; với chi phí 5. Có thể thực hiện thao tác này vì các chỉ số [5,7] không giao nhau với các chỉ số được chọn trong thao tác đầu tiên và giống hệt các chỉ số được chọn trong thao tác thứ hai.
Tổng chi phí là 1 + 3 + 5 = 9.
Có thể chứng minh đây là chi phí nhỏ nhất có thể.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;abcdefgh&quot;, target = &quot;addddddd&quot;, original = [&quot;bcd&quot;,&quot;defgh&quot;], changed = [&quot;ddd&quot;,&quot;ddddd&quot;], cost = [100,1578]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể chuyển &quot;abcdefgh&quot; thành &quot;addddddd&quot;.
Nếu chọn substring source[1..3] làm thao tác đầu tiên để đổi &quot;abcdefgh&quot; thành &quot;adddefgh&quot;, bạn không thể chọn substring source[3..7] làm thao tác thứ hai vì nó có chỉ số 3 trùng với thao tác đầu tiên.
Nếu chọn substring source[3..7] làm thao tác đầu tiên để đổi &quot;abcdefgh&quot; thành &quot;abcddddd&quot;, bạn không thể chọn substring source[1..3] làm thao tác thứ hai vì nó có chỉ số 3 trùng với thao tác đầu tiên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= source.length == target.length &lt;= 1000</code></li>
	<li><code>source</code>, <code>target</code> chỉ gồm các ký tự tiếng Anh viết thường.</li>
	<li><code>1 &lt;= cost.length == original.length == changed.length &lt;= 100</code></li>
	<li><code>1 &lt;= original[i].length == changed[i].length &lt;= source.length</code></li>
	<li><code>original[i]</code>, <code>changed[i]</code> chỉ gồm các ký tự tiếng Anh viết thường.</li>
	<li><code>original[i] != changed[i]</code></li>
	<li><code>1 &lt;= cost[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie + thuật toán Floyd + tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Các phép thay thế hiện tại thay thế toàn bộ substring và phải đồng thời khớp với $source$ và $target$. Có nhiều nhất $100$ ứng viên, nên ta dùng trie để gán id, rồi dùng Floyd trên các id đó để tính chi phí của mọi phép thay thế. Phần còn lại là chia đoạn: trả $g[p][q]$ khi cả hai trie đều chấp nhận một cặp, hoặc bỏ qua một ký tự miễn phí khi chúng đã giống nhau.
>
> $dfs(i)$ ghi nhớ kết quả từ vị trí $i$ trở đi, duyệt đồng thời trên hai trie. Độ dài chuỗi là $1000$, nên số state là tuyến tính và mỗi transition đi theo độ dài đoạn khớp.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể xem mỗi chuỗi là một node, và chi phí chuyển đổi giữa mỗi cặp chuỗi là một cạnh có hướng. Trước hết, ta khởi tạo mảng hai chiều $g$ kích thước $26 \times 26$, trong đó $g[i][j]$ biểu diễn chi phí nhỏ nhất để chuyển chuỗi $i$ thành chuỗi $j$. Ban đầu, $g[i][j] = \infty$, và nếu $i = j$ thì $g[i][j] = 0$. Ở đây, ta có thể dùng trie để lưu các chuỗi trong `original` và `changed` cùng với các mã định danh nguyên tương ứng.

Tiếp theo, ta dùng thuật toán Floyd để tính chi phí nhỏ nhất giữa mọi cặp chuỗi.

Sau đó, ta định nghĩa hàm $dfs(i)$ biểu diễn chi phí nhỏ nhất để chuyển chuỗi $source[i..]$ thành chuỗi $target[i..]$. Đáp án là $dfs(0)$.

Quá trình tính hàm $dfs(i)$ như sau:

- Nếu $i \geq |source|$, không cần chuyển đổi, trả về $0$.
- Ngược lại, nếu $source[i] = target[i]$, ta có thể bỏ qua trực tiếp và đệ quy tính $dfs(i + 1)$. Ta cũng có thể duyệt chỉ số $j$ trong đoạn $[i, |source|)$; nếu cả $source[i..j]$ và $target[i..j]$ đều nằm trong trie, và các mã định danh nguyên tương ứng $x$, $y$ đều lớn hơn hoặc bằng $0$, thì ta có thể cộng $dfs(j + 1)$ và $g[x][y]$ để nhận được chi phí của một phương án chuyển đổi, rồi lấy giá trị nhỏ nhất trong tất cả các phương án.

Tóm lại, ta có:

$$
dfs(i) = \begin{cases}
0, & i \geq |source| \\
dfs(i + 1), & source[i] = target[i] \\
\min_{i \leq j < |source|} \{ dfs(j + 1) + g[x][y] \}, & \textit{otherwise}
\end{cases}
$$

Trong đó $x$ và $y$ lần lượt là các mã định danh nguyên của $source[i..j]$ và $target[i..j]$ trong trie.

Để tránh tính toán lặp lại, ta có thể dùng tìm kiếm có ghi nhớ.

Độ phức tạp thời gian là $O(m^3 + n^2 + m \times n)$, và độ phức tạp không gian là $O(m^2 + m \times n + n)$. Trong đó $m$ và $n$ lần lượt là độ dài của các mảng `original` và `source`.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = ["children", "v"]

    def __init__(self):
        self.children: List[Node | None] = [None] * 26
        self.v = -1


class Solution:
    def minimumCost(
        self,
        source: str,
        target: str,
        original: List[str],
        changed: List[str],
        cost: List[int],
    ) -> int:
        m = len(cost)
        g = [[inf] * (m << 1) for _ in range(m << 1)]
        for i in range(m << 1):
            g[i][i] = 0
        root = Node()
        idx = 0

        def insert(w: str) -> int:
            node = root
            for c in w:
                i = ord(c) - ord("a")
                if node.children[i] is None:
                    node.children[i] = Node()
                node = node.children[i]
            if node.v < 0:
                nonlocal idx
                node.v = idx
                idx += 1
            return node.v

        @cache
        def dfs(i: int) -> int:
            if i >= len(source):
                return 0
            res = dfs(i + 1) if source[i] == target[i] else inf
            p = q = root
            for j in range(i, len(source)):
                p = p.children[ord(source[j]) - ord("a")]
                q = q.children[ord(target[j]) - ord("a")]
                if p is None or q is None:
                    break
                if p.v < 0 or q.v < 0:
                    continue
                res = min(res, dfs(j + 1) + g[p.v][q.v])
            return res

        for x, y, z in zip(original, changed, cost):
            x = insert(x)
            y = insert(y)
            g[x][y] = min(g[x][y], z)
        for k in range(idx):
            for i in range(idx):
                if g[i][k] >= inf:
                    continue
                for j in range(idx):
                    # g[i][j] = min(g[i][j], g[i][k] + g[k][j])
                    if g[i][k] + g[k][j] < g[i][j]:
                        g[i][j] = g[i][k] + g[k][j]

        ans = dfs(0)
        return -1 if ans >= inf else ans
```

#### Java

```java
class Node {
    Node[] children = new Node[26];
    int v = -1;
}

class Solution {
    private final long inf = 1L << 60;
    private Node root = new Node();
    private int idx;

    private long[][] g;
    private char[] s;
    private char[] t;
    private Long[] f;

    public long minimumCost(
        String source, String target, String[] original, String[] changed, int[] cost) {
        int m = cost.length;
        g = new long[m << 1][m << 1];
        s = source.toCharArray();
        t = target.toCharArray();
        for (int i = 0; i < g.length; ++i) {
            Arrays.fill(g[i], inf);
            g[i][i] = 0;
        }
        for (int i = 0; i < m; ++i) {
            int x = insert(original[i]);
            int y = insert(changed[i]);
            g[x][y] = Math.min(g[x][y], cost[i]);
        }
        for (int k = 0; k < idx; ++k) {
            for (int i = 0; i < idx; ++i) {
                if (g[i][k] >= inf) {
                    continue;
                }
                for (int j = 0; j < idx; ++j) {
                    g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
        f = new Long[s.length];
        long ans = dfs(0);
        return ans >= inf ? -1 : ans;
    }

    private int insert(String w) {
        Node node = root;
        for (char c : w.toCharArray()) {
            int i = c - 'a';
            if (node.children[i] == null) {
                node.children[i] = new Node();
            }
            node = node.children[i];
        }
        if (node.v < 0) {
            node.v = idx++;
        }
        return node.v;
    }

    private long dfs(int i) {
        if (i >= s.length) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        long res = s[i] == t[i] ? dfs(i + 1) : inf;
        Node p = root, q = root;
        for (int j = i; j < s.length; ++j) {
            p = p.children[s[j] - 'a'];
            q = q.children[t[j] - 'a'];
            if (p == null || q == null) {
                break;
            }
            if (p.v < 0 || q.v < 0) {
                continue;
            }
            long t = g[p.v][q.v];
            if (t < inf) {
                res = Math.min(res, t + dfs(j + 1));
            }
        }
        return f[i] = res;
    }
}
```

#### C++

```cpp
class Node {
public:
    Node* children[26];
    int v = -1;
    Node() {
        fill(children, children + 26, nullptr);
    }
};

class Solution {
private:
    const long long inf = 1LL << 60;
    Node* root = new Node();
    int idx;

    vector<vector<long long>> g;
    string s;
    string t;
    vector<long long> f;

public:
    long long minimumCost(string source, string target, vector<string>& original, vector<string>& changed, vector<int>& cost) {
        int m = cost.size();
        g = vector<vector<long long>>(m << 1, vector<long long>(m << 1, inf));
        s = source;
        t = target;

        for (int i = 0; i < g.size(); ++i) {
            g[i][i] = 0;
        }

        for (int i = 0; i < m; ++i) {
            int x = insert(original[i]);
            int y = insert(changed[i]);
            g[x][y] = min(g[x][y], static_cast<long long>(cost[i]));
        }

        for (int k = 0; k < idx; ++k) {
            for (int i = 0; i < idx; ++i) {
                if (g[i][k] >= inf) {
                    continue;
                }
                for (int j = 0; j < idx; ++j) {
                    g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }

        f = vector<long long>(s.length(), -1);
        long long ans = dfs(0);
        return ans >= inf ? -1 : ans;
    }

private:
    int insert(const string& w) {
        Node* node = root;
        for (char c : w) {
            int i = c - 'a';
            if (node->children[i] == nullptr) {
                node->children[i] = new Node();
            }
            node = node->children[i];
        }
        if (node->v < 0) {
            node->v = idx++;
        }
        return node->v;
    }

    long long dfs(int i) {
        if (i >= s.length()) {
            return 0;
        }
        if (f[i] != -1) {
            return f[i];
        }
        long long res = (s[i] == t[i]) ? dfs(i + 1) : inf;
        Node* p = root;
        Node* q = root;
        for (int j = i; j < s.length(); ++j) {
            p = p->children[s[j] - 'a'];
            q = q->children[t[j] - 'a'];
            if (p == nullptr || q == nullptr) {
                break;
            }
            if (p->v < 0 || q->v < 0) {
                continue;
            }
            long long temp = g[p->v][q->v];
            if (temp < inf) {
                res = min(res, temp + dfs(j + 1));
            }
        }
        return f[i] = res;
    }
};
```

#### Go

```go
type Node struct {
	children [26]*Node
	v        int
}

func newNode() *Node {
	return &Node{v: -1}
}

func minimumCost(source string, target string, original []string, changed []string, cost []int) int64 {
	inf := 1 << 60
	root := newNode()
	idx := 0
	m := len(cost)
	g := make([][]int, m<<1)
	for i := range g {
		g[i] = make([]int, m<<1)
		for j := range g[i] {
			g[i][j] = inf
		}
		g[i][i] = 0
	}
	insert := func(w string) int {
		node := root
		for _, c := range w {
			i := c - 'a'
			if node.children[i] == nil {
				node.children[i] = newNode()
			}
			node = node.children[i]
		}
		if node.v < 0 {
			node.v = idx
			idx++
		}
		return node.v
	}
	for i := range original {
		x := insert(original[i])
		y := insert(changed[i])
		g[x][y] = min(g[x][y], cost[i])
	}
	for k := 0; k < idx; k++ {
		for i := 0; i < idx; i++ {
			if g[i][k] >= inf {
				continue
			}
			for j := 0; j < idx; j++ {
				g[i][j] = min(g[i][j], g[i][k]+g[k][j])
			}
		}
	}
	n := len(source)
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] >= 0 {
			return f[i]
		}
		f[i] = inf
		if source[i] == target[i] {
			f[i] = dfs(i + 1)
		}
		p, q := root, root
		for j := i; j < n; j++ {
			p = p.children[source[j]-'a']
			q = q.children[target[j]-'a']
			if p == nil || q == nil {
				break
			}
			if p.v < 0 || q.v < 0 {
				continue
			}
			f[i] = min(f[i], dfs(j+1)+g[p.v][q.v])
		}
		return f[i]
	}
	ans := dfs(0)
	if ans >= inf {
		ans = -1
	}
	return int64(ans)
}
```

#### TypeScript

```ts
class Node {
    children: (Node | null)[] = Array(26).fill(null);
    v: number = -1;
}

function minimumCost(
    source: string,
    target: string,
    original: string[],
    changed: string[],
    cost: number[],
): number {
    const m = cost.length;
    const n = source.length;
    const g: number[][] = Array.from({ length: m << 1 }, () => Array(m << 1).fill(Infinity));
    const root: Node = new Node();
    let idx: number = 0;
    const f: number[] = Array(n).fill(-1);
    const insert = (w: string): number => {
        let node: Node = root;
        for (const c of w) {
            const i: number = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (node.children[i] === null) {
                node.children[i] = new Node();
            }
            node = node.children[i] as Node;
        }
        if (node.v < 0) {
            node.v = idx++;
        }
        return node.v;
    };

    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i] !== -1) {
            return f[i];
        }
        let res: number = source[i] === target[i] ? dfs(i + 1) : Infinity;
        let p: Node = root;
        let q: Node = root;
        for (let j = i; j < source.length; ++j) {
            p = p.children[source[j].charCodeAt(0) - 'a'.charCodeAt(0)] as Node;
            q = q.children[target[j].charCodeAt(0) - 'a'.charCodeAt(0)] as Node;
            if (p === null || q === null) {
                break;
            }
            if (p.v < 0 || q.v < 0) {
                continue;
            }
            const t: number = g[p.v][q.v];
            res = Math.min(res, t + dfs(j + 1));
        }
        return (f[i] = res);
    };

    for (let i = 0; i < m; ++i) {
        const x: number = insert(original[i]);
        const y: number = insert(changed[i]);
        g[x][y] = Math.min(g[x][y], cost[i]);
    }

    for (let k = 0; k < idx; ++k) {
        for (let i = 0; i < idx; ++i) {
            if (g[i][k] >= Infinity) {
                continue;
            }
            for (let j = 0; j < idx; ++j) {
                g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
            }
        }
    }
    const ans: number = dfs(0);
    return ans >= Infinity ? -1 : ans;
}
```

#### Rust

```rust
use std::cmp::min;

struct Node {
    children: [Option<Box<Node>>; 26],
    v: i32,
}

impl Node {
    fn new() -> Self {
        Self {
            children: std::array::from_fn(|_| None),
            v: -1,
        }
    }
}

impl Solution {
    pub fn minimum_cost(
        source: String,
        target: String,
        original: Vec<String>,
        changed: Vec<String>,
        cost: Vec<i32>,
    ) -> i64 {
        let inf: i64 = 1 << 60;
        let mut root = Box::new(Node::new());
        let mut idx: usize = 0;
        let m = cost.len();
        let n = m << 1;

        let mut g = vec![vec![inf; n]; n];
        for i in 0..n {
            g[i][i] = 0;
        }

        let mut insert = |w: &str, root: &mut Box<Node>, idx: &mut usize| -> usize {
            let mut node: &mut Box<Node> = root;
            for c in w.bytes() {
                let i = (c - b'a') as usize;
                if node.children[i].is_none() {
                    node.children[i] = Some(Box::new(Node::new()));
                }
                node = node.children[i].as_mut().unwrap();
            }
            if node.v < 0 {
                node.v = *idx as i32;
                *idx += 1;
            }
            node.v as usize
        };

        for i in 0..m {
            let x = insert(&original[i], &mut root, &mut idx);
            let y = insert(&changed[i], &mut root, &mut idx);
            g[x][y] = min(g[x][y], cost[i] as i64);
        }

        for k in 0..idx {
            for i in 0..idx {
                if g[i][k] >= inf {
                    continue;
                }
                for j in 0..idx {
                    let v = g[i][k] + g[k][j];
                    if v < g[i][j] {
                        g[i][j] = v;
                    }
                }
            }
        }

        let s = source.into_bytes();
        let t = target.into_bytes();
        let len = s.len();
        let mut f: Vec<Option<i64>> = vec![None; len];

        fn dfs(
            i: usize,
            s: &[u8],
            t: &[u8],
            root: &Box<Node>,
            g: &Vec<Vec<i64>>,
            f: &mut Vec<Option<i64>>,
            inf: i64,
        ) -> i64 {
            if i >= s.len() {
                return 0;
            }
            if let Some(v) = f[i] {
                return v;
            }
            let mut res = if s[i] == t[i] {
                dfs(i + 1, s, t, root, g, f, inf)
            } else {
                inf
            };
            let mut p: Option<&Box<Node>> = Some(root);
            let mut q: Option<&Box<Node>> = Some(root);
            for j in i..s.len() {
                p = p.and_then(|x| x.children[(s[j] - b'a') as usize].as_ref());
                q = q.and_then(|x| x.children[(t[j] - b'a') as usize].as_ref());
                if p.is_none() || q.is_none() {
                    break;
                }
                let pv = p.unwrap().v;
                let qv = q.unwrap().v;
                if pv < 0 || qv < 0 {
                    continue;
                }
                let c = g[pv as usize][qv as usize];
                if c < inf {
                    let v = c + dfs(j + 1, s, t, root, g, f, inf);
                    if v < res {
                        res = v;
                    }
                }
            }
            f[i] = Some(res);
            res
        }

        let ans = dfs(0, &s, &t, &root, &g, &mut f, inf);
        if ans >= inf { -1 } else { ans }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
