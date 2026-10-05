---
comments: true
difficulty: Hard
rating: 2544
source: Weekly Contest 497 Q4
tags:
    - Segment Tree
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3901. Good Subsequence Queries](https://leetcode.com/problems/good-subsequence-queries)

[中文文档](/solution/3900-3999/3901.Good%20Subsequence%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>p</code>.</p>

<p>Một <strong><span data-keyword="subsequence-sequence">dãy con</span> không rỗng</strong> của <code>nums</code> được gọi là <strong>good</strong> nếu:</p>

<ul>
	<li>Độ dài của nó <strong>nhỏ hơn nghiêm ngặt</strong> <code>n</code>.</li>
	<li><strong>Ước chung lớn nhất (GCD)</strong> của các phần tử trong nó <strong>chính xác bằng</strong> <code>p</code>.</li>
</ul>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code> có độ dài <code>q</code>, trong đó mỗi <code>queries[i] = [ind<sub>i</sub>, val<sub>i</sub>]</code> cho biết cần cập nhật <code>nums[ind<sub>i</sub>]</code> thành <code>val<sub>i</sub></code>.</p>

<p>Sau mỗi truy vấn, hãy xác định xem trong mảng hiện tại có tồn tại <strong>bất kỳ dãy con good nào</strong> hay không.</p>

<p>Trả về <strong>số lượng</strong> truy vấn mà trong đó tồn tại một <strong>dãy con good</strong>.</p>
Thuật ngữ <code>gcd(a, b)</code> biểu thị <strong>ước chung lớn nhất</strong> của <code>a</code> và <code>b</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,8,12,16], p = 2, queries = [[0,3],[2,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">i</th>
			<th style="border: 1px solid black;"><code>[ind<sub>i</sub>, val<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Thao tác</th>
			<th style="border: 1px solid black;"><code>nums</code> sau cập nhật</th>
			<th style="border: 1px solid black;">Có dãy con good nào không</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[0, 3]</code></td>
			<td style="border: 1px solid black;">Cập nhật <code>nums[0]</code> thành <code>3</code></td>
			<td style="border: 1px solid black;"><code>[3, 8, 12, 16]</code></td>
			<td style="border: 1px solid black;">Không, vì không có dãy con nào có GCD chính xác bằng <code>p = 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[2, 6]</code></td>
			<td style="border: 1px solid black;">Cập nhật <code>nums[2]</code> thành <code>6</code></td>
			<td style="border: 1px solid black;"><code>[3, 8, 6, 16]</code></td>
			<td style="border: 1px solid black;">Có, dãy con <code>[8, 6]</code> có GCD chính xác bằng <code>p = 2</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,5,7,8], p = 3, queries = [[0,6],[1,9],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">i</th>
			<th style="border: 1px solid black;"><code>[ind<sub>i</sub>, val<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Thao tác</th>
			<th style="border: 1px solid black;"><code>nums</code> sau cập nhật</th>
			<th style="border: 1px solid black;">Có dãy con good nào không</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[0, 6]</code></td>
			<td style="border: 1px solid black;">Cập nhật <code>nums[0]</code> thành <code>6</code></td>
			<td style="border: 1px solid black;"><code>[6, 5, 7, 8]</code></td>
			<td style="border: 1px solid black;">Không, vì không có dãy con nào có GCD chính xác bằng <code>p = 3</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[1, 9]</code></td>
			<td style="border: 1px solid black;">Cập nhật <code>nums[1]</code> thành <code>9</code></td>
			<td style="border: 1px solid black;"><code>[6, 9, 7, 8]</code></td>
			<td style="border: 1px solid black;">Có, dãy con <code>[6, 9]</code> có GCD chính xác bằng <code>p = 3</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>[2, 3]</code></td>
			<td style="border: 1px solid black;">Cập nhật <code>nums[2]</code> thành <code>3</code></td>
			<td style="border: 1px solid black;"><code>[6, 9, 3, 8]</code></td>
			<td style="border: 1px solid black;">Có, dãy con <code>[6, 9, 3]</code> có GCD chính xác bằng <code>p = 3</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,7,9], p = 2, queries = [[1,4],[2,8]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">i</th>
			<th style="border: 1px solid black;"><code>[ind<sub>i</sub>, val<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Thao tác</th>
			<th style="border: 1px solid black;"><code>nums</code> sau cập nhật</th>
			<th style="border: 1px solid black;">Có dãy con good nào không</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[1, 4]</code></td>
			<td style="border: 1px solid black;">Cập nhật <code>nums[1]</code> thành <code>4</code></td>
			<td style="border: 1px solid black;"><code>[5, 4, 9]</code></td>
			<td style="border: 1px solid black;">Không, vì không có dãy con nào có GCD chính xác bằng <code>p = 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[2, 8]</code></td>
			<td style="border: 1px solid black;">Cập nhật <code>nums[2]</code> thành <code>8</code></td>
			<td style="border: 1px solid black;"><code>[5, 4, 8]</code></td>
			<td style="border: 1px solid black;">Không, vì không có dãy con nào có GCD chính xác bằng <code>p = 2</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>queries[i] = [ind<sub>i</sub>, val<sub>i</sub>]</code></li>
	<li><code>1 &lt;= val<sub>i</sub>, p &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= ind<sub>i</sub> &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree + GCD

<!-- thinking:start -->

> **Tư duy**
>
> Tính lại GCD của các dãy con sau mỗi lần cập nhật là không thể với $n,q\le 5\times 10^4$. Chỉ các bội của $p$ mới có thể xuất hiện trong một dãy con có GCD chính xác bằng $p$; các phần tử khác có thể bỏ qua.
>
> Lưu chính $nums[i]$ nếu nó là bội của $p$, và lưu $0$ trong trường hợp ngược lại, sau đó duy trì GCD $g$ của tất cả ứng viên trong một Segment Tree. Nếu $g\neq p$ thì câu trả lời là không; nếu $g=p$ và không phải mọi phần tử đều là bội, toàn bộ tập ứng viên đã có độ dài nhỏ hơn $n$.
>
> Chỉ khi $n\le 6$ và mọi phần tử đều là bội thì ta mới xóa từng chỉ số và truy vấn GCD của các phần tử còn lại. Với $n>6$, GCD chung bằng $p$ đã đảm bảo rằng có thể xóa một phần tử mà GCD vẫn bằng $p$.

<!-- thinking:end -->

Ta chỉ quan tâm đến các số là bội của $p$, vì nếu một số không chia hết cho $p$ thì nó không thể thuộc một dãy con có GCD chính xác bằng $p$.

Do đó, ta có thể coi các vị trí có giá trị không chia hết cho $p$ là $0$, và chỉ duy trì giá trị sau cho mỗi vị trí trong Segment Tree:

- Nếu $\textit{nums}[i]$ chia hết cho $p$, lưu giá trị thực của nó trong Segment Tree.
- Ngược lại, lưu $0$.

Bằng cách này, toàn bộ Segment Tree duy trì GCD của tất cả các bội hiện tại của $p$. Gọi giá trị đó là $g$:

- Nếu $g \ne p$, dù chọn như thế nào thì GCD của toàn bộ các phần tử ứng viên cũng không thể chính xác bằng $p$, nên chắc chắn đáp án là false.
- Nếu $g = p$, tất cả các bội của $p$ khi gộp lại đã có GCD bằng $p$.

Tiếp theo, ta vẫn cần độ dài dãy con nhỏ hơn nghiêm ngặt $n$.

- Nếu không phải mọi phần tử đều chia hết cho $p$, tức số phần tử hợp lệ thỏa mãn $\textit{cnt} < n$, ta có thể lấy trực tiếp tất cả các bội của $p$, và đây đã là một dãy con good có độ dài nhỏ hơn $n$.
- Nếu $\textit{cnt} = n$, mọi phần tử đều chia hết cho $p$, nên ta phải xóa ít nhất một phần tử và kiểm tra xem GCD của các phần tử còn lại có vẫn bằng $p$ hay không.

Ta sử dụng sự thật sau: nếu $n > 6$, mọi phần tử đều chia hết cho $p$, và GCD tổng thể đã bằng $p$, thì luôn có thể xóa một phần tử mà GCD vẫn bằng $p$. Vì vậy, ta chỉ cần vét cạn vị trí bị xóa khi $n \le 6$ và mọi phần tử đều là bội của $p$. Khi đó, ta truy vấn GCD của phần bên trái và phần bên phải bằng Segment Tree, rồi gộp chúng lại.

Segment Tree hỗ trợ hai thao tác:

- Cập nhật điểm: thay đổi một vị trí thành giá trị mới hoặc thành $0$.
- Truy vấn đoạn: lấy GCD của một đoạn cho trước.

Sau mỗi lần cập nhật truy vấn, ta chỉ cần áp dụng các quy tắc trên để xác định xem có tồn tại dãy con good hay không.

Độ phức tạp thời gian là $O((n + q) \times \log n)$. Trong trường hợp xấu nhất, khi $n \le 6$, ta còn liệt kê vị trí bị xóa cho mỗi truy vấn, nhưng đó chỉ là một hệ số hằng số. Vì vậy, tổng độ phức tạp vẫn là $O((n + q) \times \log n)$. Độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{nums}$ và $q$ là số lượng truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = "l", "r", "g"

    def __init__(self, l: int, r: int):
        self.l = l
        self.r = r
        self.g = 0


class SegmentTree:
    __slots__ = "tr"

    def __init__(self, n: int):
        self.tr: list[Node | None] = [None] * (n << 2)
        self.build(1, 1, n)

    def build(self, u: int, l: int, r: int):
        self.tr[u] = Node(l, r)
        if l == r:
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)

    def pushup(self, u: int):
        self.tr[u].g = gcd(self.tr[u << 1].g, self.tr[u << 1 | 1].g)

    def modify(self, u: int, x: int, v: int):
        if self.tr[u].l == self.tr[u].r:
            self.tr[u].g = v
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def query(self, u: int, l: int, r: int) -> int:
        if l > r:
            return 0
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].g
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if r <= mid:
            return self.query(u << 1, l, r)
        if l > mid:
            return self.query(u << 1 | 1, l, r)
        return gcd(self.query(u << 1, l, mid), self.query(u << 1 | 1, mid + 1, r))


class Solution:
    def countGoodSubseq(self, nums: list[int], p: int, queries: list[list[int]]) -> int:
        n = len(nums)
        tree = SegmentTree(n)
        cnt = 0

        for i, x in enumerate(nums, 1):
            if x % p == 0:
                tree.modify(1, i, x)
                cnt += 1

        ans = 0
        for idx, val in queries:
            if nums[idx] % p == 0:
                tree.modify(1, idx + 1, 0)
                cnt -= 1
            if val % p == 0:
                tree.modify(1, idx + 1, val)
                cnt += 1
            nums[idx] = val

            if tree.tr[1].g != p:
                continue

            if cnt < n or n > 6:
                ans += 1
                continue

            for i in range(1, n + 1):
                left_g = tree.query(1, 1, i - 1)
                right_g = tree.query(1, i + 1, n)
                if gcd(left_g, right_g) == p:
                    ans += 1
                    break

        return ans
```

#### Java

```java
class Node {
    int l, r;
    int g;

    Node(int l, int r) {
        this.l = l;
        this.r = r;
        this.g = 0;
    }
}

class SegmentTree {
    Node[] tr;

    SegmentTree(int n) {
        tr = new Node[n << 2];
        build(1, 1, n);
    }

    void build(int u, int l, int r) {
        tr[u] = new Node(l, r);
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    void pushup(int u) {
        tr[u].g = gcd(tr[u << 1].g, tr[u << 1 | 1].g);
    }

    void modify(int u, int x, int v) {
        if (tr[u].l == tr[u].r) {
            tr[u].g = v;
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

    int query(int u, int l, int r) {
        if (l > r) {
            return 0;
        }
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].g;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (r <= mid) {
            return query(u << 1, l, r);
        }
        if (l > mid) {
            return query(u << 1 | 1, l, r);
        }
        return gcd(query(u << 1, l, mid), query(u << 1 | 1, mid + 1, r));
    }

    private int gcd(int a, int b) {
        while (b != 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}

class Solution {
    private int gcd(int a, int b) {
        while (b != 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }

    public int countGoodSubseq(int[] nums, int p, int[][] queries) {
        int n = nums.length;
        SegmentTree tree = new SegmentTree(n);
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[i] % p == 0) {
                tree.modify(1, i + 1, nums[i]);
                ++cnt;
            }
        }

        int ans = 0;
        for (int[] q : queries) {
            int idx = q[0], val = q[1];
            if (nums[idx] % p == 0) {
                tree.modify(1, idx + 1, 0);
                --cnt;
            }
            if (val % p == 0) {
                tree.modify(1, idx + 1, val);
                ++cnt;
            }
            nums[idx] = val;

            if (tree.tr[1].g != p) {
                continue;
            }
            if (cnt < n || n > 6) {
                ++ans;
                continue;
            }
            for (int i = 1; i <= n; ++i) {
                int leftG = tree.query(1, 1, i - 1);
                int rightG = tree.query(1, i + 1, n);
                if (gcd(leftG, rightG) == p) {
                    ++ans;
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
class SegNode {
	int l, r;
	int g;

	Node(int l, int r) {
		this.l = l;
		this.r = r;
		this.g = 0;
	}
}

class SegmentTree {
	Node[] tr;

	SegmentTree(int n) {
		tr = new Node[n << 2];
		build(1, 1, n);
	}

	void build(int u, int l, int r) {
		tr[u] = new Node(l, r);
		if (l == r) {
			return;
		}
		int mid = (l + r) >> 1;
		build(u << 1, l, mid);
		build(u << 1 | 1, mid + 1, r);
	}

	void pushup(int u) {
		tr[u].g = gcd(tr[u << 1].g, tr[u << 1 | 1].g);
	}

	void modify(int u, int x, int v) {
		if (tr[u].l == tr[u].r) {
			tr[u].g = v;
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

	int query(int u, int l, int r) {
		if (l > r) {
			return 0;
		}
		if (tr[u].l >= l && tr[u].r <= r) {
			return tr[u].g;
		}
		int mid = (tr[u].l + tr[u].r) >> 1;
		if (r <= mid) {
			return query(u << 1, l, r);
		}
		if (l > mid) {
			return query(u << 1 | 1, l, r);
		}
		return gcd(query(u << 1, l, mid), query(u << 1 | 1, mid + 1, r));
	}

	private int gcd(int a, int b) {
		while (b != 0) {
			int t = a % b;
			a = b;
			b = t;
		}
		return a;
	}
}

class Solution {
	private int gcd(int a, int b) {
		while (b != 0) {
			int t = a % b;
			a = b;
			b = t;
		}
		return a;
	}

	public int countGoodSubseq(int[] nums, int p, int[][] queries) {
		Object[] norqaveliq = new Object[] {nums, p, queries};
		int n = nums.length;
		SegmentTree tree = new SegmentTree(n);
		int cnt = 0;
		for (int i = 0; i < n; ++i) {
			if (nums[i] % p == 0) {
				tree.modify(1, i + 1, nums[i]);
				++cnt;
			}
		}

		int ans = 0;
		for (int[] q : queries) {
			int idx = q[0], val = q[1];
			if (nums[idx] % p == 0) {
				tree.modify(1, idx + 1, 0);
				--cnt;
			}
			if (val % p == 0) {
				tree.modify(1, idx + 1, val);
				++cnt;
			}
			nums[idx] = val;

			if (tree.tr[1].g != p) {
				continue;
			}
			if (cnt < n || n > 6) {
				++ans;
				continue;
			}
			for (int i = 1; i <= n; ++i) {
				int leftG = tree.query(1, 1, i - 1);
				int rightG = tree.query(1, i + 1, n);
				if (gcd(leftG, rightG) == p) {
					++ans;
					break;
				}
			}
		}
		return ans;
	}
}

```

#### Go

```go
func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}

type Node struct {
	l, r int
	g    int
}

func NewNode(l, r int) *Node {
	return &Node{l: l, r: r, g: 0}
}

type SegmentTree struct {
	tr []*Node
}

func NewSegmentTree(n int) *SegmentTree {
	tree := &SegmentTree{tr: make([]*Node, n<<2)}
	tree.build(1, 1, n)
	return tree
}

func (st *SegmentTree) build(u, l, r int) {
	st.tr[u] = NewNode(l, r)
	if l == r {
		return
	}
	mid := (l + r) >> 1
	st.build(u<<1, l, mid)
	st.build(u<<1|1, mid+1, r)
}

func (st *SegmentTree) pushup(u int) {
	st.tr[u].g = gcd(st.tr[u<<1].g, st.tr[u<<1|1].g)
}

func (st *SegmentTree) modify(u, x, v int) {
	if st.tr[u].l == st.tr[u].r {
		st.tr[u].g = v
		return
	}
	mid := (st.tr[u].l + st.tr[u].r) >> 1
	if x <= mid {
		st.modify(u<<1, x, v)
	} else {
		st.modify(u<<1|1, x, v)
	}
	st.pushup(u)
}

func (st *SegmentTree) query(u, l, r int) int {
	if l > r {
		return 0
	}
	if st.tr[u].l >= l && st.tr[u].r <= r {
		return st.tr[u].g
	}
	mid := (st.tr[u].l + st.tr[u].r) >> 1
	if r <= mid {
		return st.query(u<<1, l, r)
	}
	if l > mid {
		return st.query(u<<1|1, l, r)
	}
	return gcd(st.query(u<<1, l, mid), st.query(u<<1|1, mid+1, r))
}

func countGoodSubseq(nums []int, p int, queries [][]int) int {
	n := len(nums)
	tree := NewSegmentTree(n)
	cnt := 0
	for i, x := range nums {
		if x%p == 0 {
			tree.modify(1, i+1, x)
			cnt++
		}
	}

	ans := 0
	for _, q := range queries {
		idx, val := q[0], q[1]
		if nums[idx]%p == 0 {
			tree.modify(1, idx+1, 0)
			cnt--
		}
		if val%p == 0 {
			tree.modify(1, idx+1, val)
			cnt++
		}
		nums[idx] = val

		if tree.tr[1].g != p {
			continue
		}
		if cnt < n || n > 6 {
			ans++
			continue
		}
		for i := 1; i <= n; i++ {
			leftG := tree.query(1, 1, i-1)
			rightG := tree.query(1, i+1, n)
			if gcd(leftG, rightG) == p {
				ans++
				break
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function gcd(a: number, b: number): number {
    while (b !== 0) {
        [a, b] = [b, a % b];
    }
    return a;
}

class SegNode {
    l: number;
    r: number;
    g: number;

    constructor(l: number, r: number) {
        this.l = l;
        this.r = r;
        this.g = 0;
    }
}

class SegmentTree {
    tr: SegNode[];

    constructor(n: number) {
        this.tr = Array(n << 2);
        this.build(1, 1, n);
    }

    build(u: number, l: number, r: number): void {
        this.tr[u] = new SegNode(l, r);
        if (l === r) {
            return;
        }
        const mid = (l + r) >> 1;
        this.build(u << 1, l, mid);
        this.build((u << 1) | 1, mid + 1, r);
    }

    pushup(u: number): void {
        this.tr[u].g = gcd(this.tr[u << 1].g, this.tr[(u << 1) | 1].g);
    }

    modify(u: number, x: number, v: number): void {
        if (this.tr[u].l === this.tr[u].r) {
            this.tr[u].g = v;
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

    query(u: number, l: number, r: number): number {
        if (l > r) {
            return 0;
        }
        if (this.tr[u].l >= l && this.tr[u].r <= r) {
            return this.tr[u].g;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        if (r <= mid) {
            return this.query(u << 1, l, r);
        }
        if (l > mid) {
            return this.query((u << 1) | 1, l, r);
        }
        return gcd(this.query(u << 1, l, mid), this.query((u << 1) | 1, mid + 1, r));
    }
}

function countGoodSubseq(nums: number[], p: number, queries: number[][]): number {
    const n = nums.length;
    const tree = new SegmentTree(n);
    let cnt = 0;
    for (let i = 0; i < n; ++i) {
        if (nums[i] % p === 0) {
            tree.modify(1, i + 1, nums[i]);
            ++cnt;
        }
    }

    let ans = 0;
    for (const [idx, val] of queries) {
        if (nums[idx] % p === 0) {
            tree.modify(1, idx + 1, 0);
            --cnt;
        }
        if (val % p === 0) {
            tree.modify(1, idx + 1, val);
            ++cnt;
        }
        nums[idx] = val;

        if (tree.tr[1].g !== p) {
            continue;
        }
        if (cnt < n || n > 6) {
            ++ans;
            continue;
        }
        for (let i = 1; i <= n; ++i) {
            const leftG = tree.query(1, 1, i - 1);
            const rightG = tree.query(1, i + 1, n);
            if (gcd(leftG, rightG) === p) {
                ++ans;
                break;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
