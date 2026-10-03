---
comments: true
difficulty: Hard
rating: 2628
source: Weekly Contest 285 Q4
tags:
    - Segment Tree
    - Array
    - String
    - Ordered Set
---

<!-- problem:start -->

# [2213. Longest Substring of One Repeating Character](https://leetcode.com/problems/longest-substring-of-one-repeating-character)

[中文文档](/solution/2200-2299/2213.Longest%20Substring%20of%20One%20Repeating%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>đánh chỉ số từ 0</strong> <code>s</code>. Bạn cũng được cho một chuỗi <strong>đánh chỉ số từ 0</strong> <code>queryCharacters</code> có độ dài <code>k</code> và một mảng số nguyên <strong>đánh chỉ số từ 0</strong> gồm các <strong>chỉ số</strong> <code>queryIndices</code> có độ dài <code>k</code>, cả hai được dùng để mô tả <code>k</code> truy vấn.</p>

<p>Truy vấn thứ <code>i<sup>th</sup></code> cập nhật ký tự tại chỉ số <code>queryIndices[i]</code> trong <code>s</code> thành ký tự <code>queryCharacters[i]</code>.</p>

<p>Trả về <em>một mảng</em> <code>lengths</code> <em>có độ dài </em><code>k</code><em>, trong đó</em> <code>lengths[i]</code> <em>là <strong>độ dài</strong> của <strong>chuỗi con</strong> dài nhất trong </em><code>s</code><em> chỉ gồm <strong>một ký tự lặp lại</strong> <strong>sau khi</strong></em> <em>thực hiện truy vấn thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;babacc&quot;, queryCharacters = &quot;bcb&quot;, queryIndices = [1,3,3]
<strong>Đầu ra:</strong> [3,3,4]
<strong>Giải thích:</strong>
- Truy vấn thứ <sup>1</sup> cập nhật s = &quot;<u>b<strong>b</strong>b</u>acc&quot;. Chuỗi con dài nhất chỉ gồm một ký tự lặp lại là &quot;bbb&quot; với độ dài 3.
- Truy vấn thứ <sup>2</sup> cập nhật s = &quot;bbb<u><strong>c</strong>cc</u>&quot;.
  Chuỗi con dài nhất chỉ gồm một ký tự lặp lại có thể là &quot;bbb&quot; hoặc &quot;ccc&quot; với độ dài 3.
- Truy vấn thứ <sup>3</sup> cập nhật s = &quot;<u>bbb<strong>b</strong></u>cc&quot;. Chuỗi con dài nhất chỉ gồm một ký tự lặp lại là &quot;bbbb&quot; với độ dài 4.
Do đó, ta trả về [3,3,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abyzz&quot;, queryCharacters = &quot;aa&quot;, queryIndices = [2,1]
<strong>Đầu ra:</strong> [2,3]
<strong>Giải thích:</strong>
- Truy vấn thứ <sup>1</sup> cập nhật s = &quot;ab<strong>a</strong><u>zz</u>&quot;. Chuỗi con dài nhất chỉ gồm một ký tự lặp lại là &quot;zz&quot; với độ dài 2.
- Truy vấn thứ <sup>2</sup> cập nhật s = &quot;<u>a<strong>a</strong>a</u>zz&quot;. Chuỗi con dài nhất chỉ gồm một ký tự lặp lại là &quot;aaa&quot; với độ dài 3.
Do đó, ta trả về [2,3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>k == queryCharacters.length == queryIndices.length</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li><code>queryCharacters</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= queryIndices[i] &lt; s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Sau mỗi lần cập nhật một ký tự, ta phải tìm độ dài dãy liên tiếp dài nhất gồm cùng một ký tự. Vì cả $|s|$ và số truy vấn đều có thể bằng $10^5$, việc quét lại hoặc xây dựng lại các dãy sau mỗi lần cập nhật sẽ quá chậm. Mỗi lần cập nhật chỉ thay đổi các dãy lân cận; ta cần gộp thông tin bên trái và bên phải trong thời gian logarithmic.
>
> Mỗi node của segment tree lưu dãy liên tiếp dài nhất ở tiền tố $lmx$, dãy liên tiếp dài nhất ở hậu tố $rmx$ và dãy liên tiếp dài nhất bên trong $mx$. Khi hai ký tự ở ranh giới giữa khớp nhau, một dãy mới có thể vượt qua điểm giữa; nếu một node con hoàn toàn đồng nhất, nó có thể mở rộng tiền tố hoặc hậu tố của node cha.
>
> Ta cập nhật leaf rồi thực hiện $\textit{pushup}$ trên đường đi; đáp án là $mx$ của root. Mỗi thao tác có độ phức tạp $O(\log n)$.

<!-- thinking:end -->

Segment tree chia toàn bộ đoạn thành nhiều đoạn con không liên tiếp, và số đoạn con không vượt quá $\log(\textit{width})$. Để cập nhật giá trị của một phần tử, ta chỉ cần cập nhật $\log(\textit{width})$ đoạn, và tất cả các đoạn này đều nằm trong một đoạn lớn chứa phần tử đó. Khi sửa đoạn, cần dùng **lazy tag** để đảm bảo hiệu năng.

- Mỗi node của segment tree biểu diễn một đoạn;
- Segment tree có duy nhất một root, biểu diễn toàn bộ phạm vi cần thống kê, chẳng hạn như $[1, n]$;
- Mỗi leaf của segment tree biểu diễn một đoạn cơ sở có độ dài $1$, $[x, x]$;
- Với mỗi node nội bộ $[l, r]$, node con trái là $[l, mid]$, node con phải là $[mid + 1, r]$, trong đó $mid = \frac{l + r}{2}$;

Với bài toán này, thông tin được duy trì tại node của segment tree gồm:

1. Số lượng ký tự liên tiếp dài nhất ở tiền tố, $lmx$;
2. Số lượng ký tự liên tiếp dài nhất ở hậu tố, $rmx$;
3. Số lượng ký tự liên tiếp dài nhất trong đoạn, $mx$.
4. Điểm đầu $l$ và điểm cuối $r$ của đoạn.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n \times \log n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
def max(a: int, b: int) -> int:
    return a if a > b else b


class Node:
    __slots__ = "l", "r", "lmx", "rmx", "mx"

    def __init__(self, l: int, r: int):
        self.l = l
        self.r = r
        self.lmx = self.rmx = self.mx = 1


class SegmentTree:
    __slots__ = "s", "tr"

    def __init__(self, s: str):
        self.s = list(s)
        n = len(s)
        self.tr: List[Node | None] = [None] * (n * 4)
        self.build(1, 1, n)

    def build(self, u: int, l: int, r: int):
        self.tr[u] = Node(l, r)
        if l == r:
            return
        mid = (l + r) // 2
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)
        self.pushup(u)

    def query(self, u: int, l: int, r: int) -> int:
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].mx
        mid = (self.tr[u].l + self.tr[u].r) // 2
        ans = 0
        if r <= mid:
            ans = self.query(u << 1, l, r)
        if l > mid:
            ans = max(ans, self.query(u << 1 | 1, l, r))
        return ans

    def modify(self, u: int, x: int, v: str):
        if self.tr[u].l == self.tr[u].r:
            self.s[x - 1] = v
            return
        mid = (self.tr[u].l + self.tr[u].r) // 2
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def pushup(self, u: int):
        root, left, right = self.tr[u], self.tr[u << 1], self.tr[u << 1 | 1]
        root.lmx = left.lmx
        root.rmx = right.rmx
        root.mx = max(left.mx, right.mx)
        a, b = left.r - left.l + 1, right.r - right.l + 1
        if self.s[left.r - 1] == self.s[right.l - 1]:
            if left.lmx == a:
                root.lmx += right.lmx
            if right.rmx == b:
                root.rmx += left.rmx
            root.mx = max(root.mx, left.rmx + right.lmx)


class Solution:
    def longestRepeating(
        self, s: str, queryCharacters: str, queryIndices: List[int]
    ) -> List[int]:
        tree = SegmentTree(s)
        ans = []
        for x, v in zip(queryIndices, queryCharacters):
            tree.modify(1, x + 1, v)
            ans.append(tree.query(1, 1, len(s)))
        return ans
```

#### Java

```java
class Node {
    int l, r;
    int lmx, rmx, mx;

    Node(int l, int r) {
        this.l = l;
        this.r = r;
        lmx = rmx = mx = 1;
    }
}

class SegmentTree {
    private char[] s;
    private Node[] tr;

    public SegmentTree(char[] s) {
        int n = s.length;
        this.s = s;
        tr = new Node[n << 2];
        build(1, 1, n);
    }

    public void build(int u, int l, int r) {
        tr[u] = new Node(l, r);
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    public void modify(int u, int x, char v) {
        if (tr[u].l == x && tr[u].r == x) {
            s[x - 1] = v;
            return;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (x <= mid) {
            modify(u << 1, x, v);
        } else {
            modify(u << 1 | 1, x, v);
        }
        pushup(u);
    }

    public int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].mx;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        int ans = 0;
        if (r <= mid) {
            ans = query(u << 1, l, r);
        }
        if (l > mid) {
            ans = Math.max(ans, query(u << 1 | 1, l, r));
        }
        return ans;
    }

    private void pushup(int u) {
        Node root = tr[u];
        Node left = tr[u << 1], right = tr[u << 1 | 1];
        root.mx = Math.max(left.mx, right.mx);
        root.lmx = left.lmx;
        root.rmx = right.rmx;
        int a = left.r - left.l + 1;
        int b = right.r - right.l + 1;
        if (s[left.r - 1] == s[right.l - 1]) {
            if (left.lmx == a) {
                root.lmx += right.lmx;
            }
            if (right.rmx == b) {
                root.rmx += left.rmx;
            }
            root.mx = Math.max(root.mx, left.rmx + right.lmx);
        }
    }
}

class Solution {
    public int[] longestRepeating(String s, String queryCharacters, int[] queryIndices) {
        SegmentTree tree = new SegmentTree(s.toCharArray());
        int k = queryIndices.length;
        int[] ans = new int[k];
        int n = s.length();
        for (int i = 0; i < k; ++i) {
            int x = queryIndices[i] + 1;
            char v = queryCharacters.charAt(i);
            tree.modify(1, x, v);
            ans[i] = tree.query(1, 1, n);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Node {
public:
    int l, r;
    int lmx, rmx, mx;

    Node(int l, int r)
        : l(l)
        , r(r)
        , lmx(1)
        , rmx(1)
        , mx(1) {}
};

class SegmentTree {
private:
    string s;
    vector<Node*> tr;

    void build(int u, int l, int r) {
        tr[u] = new Node(l, r);
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    void pushup(int u) {
        Node* root = tr[u];
        Node* left = tr[u << 1];
        Node* right = tr[u << 1 | 1];
        root->mx = max(left->mx, right->mx);
        root->lmx = left->lmx;
        root->rmx = right->rmx;
        int a = left->r - left->l + 1;
        int b = right->r - right->l + 1;
        if (s[left->r - 1] == s[right->l - 1]) {
            if (left->lmx == a) {
                root->lmx += right->lmx;
            }
            if (right->rmx == b) {
                root->rmx += left->rmx;
            }
            root->mx = max(root->mx, left->rmx + right->lmx);
        }
    }

public:
    SegmentTree(const string& s)
        : s(s) {
        int n = s.size();
        tr.resize(n * 4);
        build(1, 1, n);
    }

    void modify(int u, int x, char v) {
        if (tr[u]->l == x && tr[u]->r == x) {
            s[x - 1] = v;
            return;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (x <= mid) {
            modify(u << 1, x, v);
        } else {
            modify(u << 1 | 1, x, v);
        }
        pushup(u);
    }

    int query(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) {
            return tr[u]->mx;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        int ans = 0;
        if (r <= mid) {
            ans = query(u << 1, l, r);
        } else if (l > mid) {
            ans = max(ans, query(u << 1 | 1, l, r));
        }
        return ans;
    }
};

class Solution {
public:
    vector<int> longestRepeating(string s, string queryCharacters, vector<int>& queryIndices) {
        SegmentTree tree(s);
        int k = queryIndices.size();
        vector<int> ans(k);
        int n = s.size();
        for (int i = 0; i < k; ++i) {
            int x = queryIndices[i] + 1;
            char v = queryCharacters[i];
            tree.modify(1, x, v);
            ans[i] = tree.query(1, 1, n);
        }
        return ans;
    }
};
```

#### Go

```go
type Node struct {
	l, r         int
	lmx, rmx, mx int
}

type SegmentTree struct {
	s  []byte
	tr []*Node
}

func NewNode(l, r int) *Node {
	return &Node{l: l, r: r, lmx: 1, rmx: 1, mx: 1}
}

func NewSegmentTree(s string) *SegmentTree {
	n := len(s)
	tree := &SegmentTree{s: []byte(s), tr: make([]*Node, n<<2)}
	tree.build(1, 1, n)
	return tree
}

func (tree *SegmentTree) build(u, l, r int) {
	tree.tr[u] = NewNode(l, r)
	if l == r {
		return
	}
	mid := (l + r) >> 1
	tree.build(u<<1, l, mid)
	tree.build(u<<1|1, mid+1, r)
	tree.pushup(u)
}

func (tree *SegmentTree) modify(u, x int, v byte) {
	if tree.tr[u].l == x && tree.tr[u].r == x {
		tree.s[x-1] = v
		return
	}
	mid := (tree.tr[u].l + tree.tr[u].r) >> 1
	if x <= mid {
		tree.modify(u<<1, x, v)
	} else {
		tree.modify(u<<1|1, x, v)
	}
	tree.pushup(u)
}

func (tree *SegmentTree) query(u, l, r int) int {
	if tree.tr[u].l >= l && tree.tr[u].r <= r {
		return tree.tr[u].mx
	}
	mid := (tree.tr[u].l + tree.tr[u].r) >> 1
	ans := 0
	if r <= mid {
		ans = tree.query(u<<1, l, r)
	} else if l > mid {
		ans = max(ans, tree.query(u<<1|1, l, r))
	} else {
		ans = max(tree.query(u<<1, l, r), tree.query(u<<1|1, l, r))
	}
	return ans
}

func (tree *SegmentTree) pushup(u int) {
	root := tree.tr[u]
	left := tree.tr[u<<1]
	right := tree.tr[u<<1|1]
	root.mx = max(left.mx, right.mx)
	root.lmx = left.lmx
	root.rmx = right.rmx
	a := left.r - left.l + 1
	b := right.r - right.l + 1
	if tree.s[left.r-1] == tree.s[right.l-1] {
		if left.lmx == a {
			root.lmx += right.lmx
		}
		if right.rmx == b {
			root.rmx += left.rmx
		}
		root.mx = max(root.mx, left.rmx+right.lmx)
	}
}

func longestRepeating(s string, queryCharacters string, queryIndices []int) (ans []int) {
	tree := NewSegmentTree(s)
	n := len(s)
	for i, v := range queryCharacters {
		x := queryIndices[i] + 1
		tree.modify(1, x, byte(v))
		ans = append(ans, tree.query(1, 1, n))
	}
	return
}
```

#### TypeScript

```ts
class Node {
    l: number;
    r: number;
    lmx: number;
    rmx: number;
    mx: number;

    constructor(l: number, r: number) {
        this.l = l;
        this.r = r;
        this.lmx = 1;
        this.rmx = 1;
        this.mx = 1;
    }
}

class SegmentTree {
    private s: string[];
    private tr: Node[];

    constructor(s: string) {
        this.s = s.split('');
        this.tr = Array(s.length * 4)
            .fill(null)
            .map(() => new Node(0, 0));
        this.build(1, 1, s.length);
    }

    private build(u: number, l: number, r: number): void {
        this.tr[u] = new Node(l, r);
        if (l === r) {
            return;
        }
        const mid = (l + r) >> 1;
        this.build(u << 1, l, mid);
        this.build((u << 1) | 1, mid + 1, r);
        this.pushup(u);
    }

    public modify(u: number, x: number, v: string): void {
        if (this.tr[u].l === x && this.tr[u].r === x) {
            this.s[x - 1] = v;
            return;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        if (x <= mid) {
            this.modify(u << 1, x, v);
        } else {
            this.modify((u << 1) | 1, x, v);
        }
        this.pushup(u);
    }

    public query(u: number, l: number, r: number): number {
        if (this.tr[u].l >= l && this.tr[u].r <= r) {
            return this.tr[u].mx;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        let ans = 0;
        if (r <= mid) {
            ans = this.query(u << 1, l, r);
        } else if (l > mid) {
            ans = Math.max(ans, this.query((u << 1) | 1, l, r));
        } else {
            ans = Math.max(this.query(u << 1, l, r), this.query((u << 1) | 1, l, r));
        }
        return ans;
    }

    private pushup(u: number): void {
        const root = this.tr[u];
        const left = this.tr[u << 1];
        const right = this.tr[(u << 1) | 1];
        root.mx = Math.max(left.mx, right.mx);
        root.lmx = left.lmx;
        root.rmx = right.rmx;
        const a = left.r - left.l + 1;
        const b = right.r - right.l + 1;
        if (this.s[left.r - 1] === this.s[right.l - 1]) {
            if (left.lmx === a) {
                root.lmx += right.lmx;
            }
            if (right.rmx === b) {
                root.rmx += left.rmx;
            }
            root.mx = Math.max(root.mx, left.rmx + right.lmx);
        }
    }
}

function longestRepeating(s: string, queryCharacters: string, queryIndices: number[]): number[] {
    const tree = new SegmentTree(s);
    const k = queryIndices.length;
    const ans: number[] = new Array(k);
    const n = s.length;
    for (let i = 0; i < k; ++i) {
        const x = queryIndices[i] + 1;
        const v = queryCharacters[i];
        tree.modify(1, x, v);
        ans[i] = tree.query(1, 1, n);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_repeating(
        s: String,
        query_characters: String,
        query_indices: Vec<i32>,
    ) -> Vec<i32> {
        struct Node {
            l: usize,
            r: usize,
            lmx: i32,
            rmx: i32,
            mx: i32,
        }

        struct SegmentTree {
            s: Vec<u8>,
            tr: Vec<Node>,
        }

        impl SegmentTree {
            fn new(s: String) -> Self {
                let n = s.len();
                let mut tree = Self {
                    s: s.into_bytes(),
                    tr: (0..n * 4 + 5)
                        .map(|_| Node {
                            l: 0,
                            r: 0,
                            lmx: 0,
                            rmx: 0,
                            mx: 0,
                        })
                        .collect(),
                };
                tree.build(1, 1, n);
                tree
            }

            fn build(&mut self, u: usize, l: usize, r: usize) {
                self.tr[u] = Node {
                    l,
                    r,
                    lmx: 1,
                    rmx: 1,
                    mx: 1,
                };

                if l == r {
                    return;
                }

                let mid = (l + r) >> 1;
                self.build(u << 1, l, mid);
                self.build(u << 1 | 1, mid + 1, r);
                self.pushup(u);
            }

            fn modify(&mut self, u: usize, x: usize, v: u8) {
                if self.tr[u].l == self.tr[u].r {
                    self.s[x - 1] = v;
                    return;
                }

                let mid = (self.tr[u].l + self.tr[u].r) >> 1;

                if x <= mid {
                    self.modify(u << 1, x, v);
                } else {
                    self.modify(u << 1 | 1, x, v);
                }

                self.pushup(u);
            }

            fn pushup(&mut self, u: usize) {
                let left = u << 1;
                let right = u << 1 | 1;

                let left_lmx = self.tr[left].lmx;
                let left_rmx = self.tr[left].rmx;
                let left_mx = self.tr[left].mx;
                let right_lmx = self.tr[right].lmx;
                let right_rmx = self.tr[right].rmx;
                let right_mx = self.tr[right].mx;

                self.tr[u].lmx = left_lmx;
                self.tr[u].rmx = right_rmx;
                self.tr[u].mx = left_mx.max(right_mx);

                let left_len = self.tr[left].r - self.tr[left].l + 1;
                let right_len = self.tr[right].r - self.tr[right].l + 1;

                if self.s[self.tr[left].r - 1] == self.s[self.tr[right].l - 1] {
                    if left_lmx as usize == left_len {
                        self.tr[u].lmx += right_lmx;
                    }

                    if right_rmx as usize == right_len {
                        self.tr[u].rmx += left_rmx;
                    }

                    self.tr[u].mx = self.tr[u].mx.max(left_rmx + right_lmx);
                }
            }

            fn query(&self) -> i32 {
                self.tr[1].mx
            }
        }

        let mut tree = SegmentTree::new(s);
        let mut ans = Vec::with_capacity(query_indices.len());

        for (x, v) in query_indices.iter().zip(query_characters.bytes()) {
            tree.modify(1, *x as usize + 1, v);
            ans.push(tree.query());
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
