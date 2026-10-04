---
comments: true
difficulty: Hard
rating: 2397
source: Biweekly Contest 98 Q4
tags:
    - Segment Tree
    - Array
---

<!-- problem:start -->

# [2569. Handling Sum Queries After Update](https://leetcode.com/problems/handling-sum-queries-after-update)

[中文文档](/solution/2500-2599/2569.Handling%20Sum%20Queries%20After%20Update/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng <strong>0-indexed</strong> <code>nums1</code> và <code>nums2</code>, cùng một mảng 2 chiều <code>queries</code> gồm các truy vấn. Có ba loại truy vấn:</p>

<ol>
	<li>Với truy vấn loại 1, <code>queries[i]&nbsp;= [1, l, r]</code>. Đảo các giá trị từ <code>0</code> thành <code>1</code> và từ <code>1</code> thành <code>0</code> trong <code>nums1</code>&nbsp;từ chỉ số <code>l</code> đến chỉ số <code>r</code>. Cả <code>l</code> và <code>r</code> đều là <strong>0-indexed</strong>.</li>
	<li>Với truy vấn loại 2, <code>queries[i]&nbsp;= [2, p, 0]</code>. Với mọi chỉ số <code>0 &lt;= i &lt; n</code>, đặt&nbsp;<code>nums2[i] =&nbsp;nums2[i]&nbsp;+ nums1[i]&nbsp;* p</code>.</li>
	<li>Với truy vấn loại 3, <code>queries[i]&nbsp;= [3, 0, 0]</code>. Tìm tổng các phần tử trong <code>nums2</code>.</li>
</ol>

<p>Trả về <em>một mảng chứa tất cả đáp án của các truy vấn loại 3.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,0,1], nums2 = [0,0,0], queries = [[1,1,1],[2,1,0],[3,0,0]]
<strong>Đầu ra:</strong> [3]
<strong>Giải thích:</strong> Sau truy vấn đầu tiên, nums1 trở thành [1,1,1]. Sau truy vấn thứ hai, nums2 trở thành [1,1,1], nên đáp án của truy vấn thứ ba là 3. Do đó, ta trả về [3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1], nums2 = [5], queries = [[2,0,0],[3,0,0]]
<strong>Đầu ra:</strong> [5]
<strong>Giải thích:</strong> Sau truy vấn đầu tiên, nums2 vẫn là [5], nên đáp án của truy vấn thứ hai là 5. Do đó, ta trả về [5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length,nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums1.length = nums2.length</code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code><font face="monospace">queries[i].length = 3</font></code></li>
	<li><code><font face="monospace">0 &lt;= l &lt;= r &lt;= nums1.length - 1</font></code></li>
	<li><code><font face="monospace">0 &lt;= p &lt;= 10<sup>6</sup></font></code></li>
	<li><code>0 &lt;= nums1[i] &lt;= 1</code></li>
	<li><code>0 &lt;= nums2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cây phân đoạn

<!-- thinking:start -->

> **Tư duy**
>
> Có ba thao tác: đảo một đoạn trong $nums1$; cộng $p\times\sum nums1$ vào mọi phần tử của $nums2$; truy vấn $\sum nums2$. Với $n,q\le 10^5$, ta không thể duyệt một đoạn trong mỗi lần đảo.
>
> $\sum nums2$ là một tổng được duy trì duy nhất; thao tác thứ hai chỉ cần tổng trên một đoạn của $nums1$. Cây phân đoạn lưu số lượng số 1 trong mỗi đoạn, cùng lazy flip để thay tổng $s$ bằng $\textit{length}-s$. Thao tác thứ ba chỉ cần ghi nhận tổng hiện tại.

<!-- thinking:end -->

Theo mô tả đề bài:

- Thao tác $1$ là đảo tất cả các số trong đoạn chỉ số $[l,..r]$ của mảng `nums1`, tức là đổi $0$ thành $1$ và $1$ thành $0$.
- Thao tác $3$ là tính tổng tất cả các số trong mảng `nums2`.
- Thao tác $2$ là cộng tổng tất cả các số trong mảng `nums2` với $p$ lần tổng tất cả các số trong mảng `nums1`, tức là $sum(nums2) = sum(nums2) + p * sum(nums1)$.

Vì vậy, thực tế ta chỉ cần duy trì tổng trên các đoạn của mảng `nums1`, có thể thực hiện bằng một cây phân đoạn.

Ta định nghĩa mỗi nút của cây phân đoạn là `Node`, mỗi nút chứa các thuộc tính sau:

- `l`: Biên trái của nút, chỉ số bắt đầu từ $1$.
- `r`: Biên phải của nút, chỉ số bắt đầu từ $1$.
- `s`: Tổng trên đoạn của nút.
- `lazy`: Nhãn lazy của nút.

Cây phân đoạn chủ yếu có các thao tác sau:

- `build(u, l, r)`: Xây dựng cây phân đoạn.
- `pushdown(u)`: Lan truyền nhãn lazy.
- `pushup(u)`: Cập nhật thông tin của nút cha bằng thông tin của các nút con.
- `modify(u, l, r)`: Sửa tổng trên đoạn. Trong bài này, thao tác đó là đảo từng số trong đoạn, nên tổng đoạn $s = r - l + 1 - s$.
- `query(u, l, r)`: Truy vấn tổng trên đoạn.

Đầu tiên, tính tổng tất cả các số trong mảng `nums2`, ký hiệu là $s$.

Khi thực hiện thao tác $1$, ta chỉ cần gọi `modify(1, l + 1, r + 1)`.

Khi thực hiện thao tác $2$, ta cập nhật $s = s + p \times query(1, 1, n)$.

Khi thực hiện thao tác $3$, ta chỉ cần thêm $s$ vào mảng đáp án.

Độ phức tạp thời gian là $O(n + m \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng `nums1` và `queries`.

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self):
        self.l = self.r = 0
        self.s = self.lazy = 0


class SegmentTree:
    def __init__(self, nums):
        self.nums = nums
        n = len(nums)
        self.tr = [Node() for _ in range(n << 2)]
        self.build(1, 1, n)

    def build(self, u, l, r):
        self.tr[u].l, self.tr[u].r = l, r
        if l == r:
            self.tr[u].s = self.nums[l - 1]
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)
        self.pushup(u)

    def modify(self, u, l, r):
        if self.tr[u].l >= l and self.tr[u].r <= r:
            self.tr[u].lazy ^= 1
            self.tr[u].s = self.tr[u].r - self.tr[u].l + 1 - self.tr[u].s
            return
        self.pushdown(u)
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if l <= mid:
            self.modify(u << 1, l, r)
        if r > mid:
            self.modify(u << 1 | 1, l, r)
        self.pushup(u)

    def query(self, u, l, r):
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].s
        self.pushdown(u)
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        res = 0
        if l <= mid:
            res += self.query(u << 1, l, r)
        if r > mid:
            res += self.query(u << 1 | 1, l, r)
        return res

    def pushup(self, u):
        self.tr[u].s = self.tr[u << 1].s + self.tr[u << 1 | 1].s

    def pushdown(self, u):
        if self.tr[u].lazy:
            mid = (self.tr[u].l + self.tr[u].r) >> 1
            self.tr[u << 1].s = mid - self.tr[u].l + 1 - self.tr[u << 1].s
            self.tr[u << 1].lazy ^= 1
            self.tr[u << 1 | 1].s = self.tr[u].r - mid - self.tr[u << 1 | 1].s
            self.tr[u << 1 | 1].lazy ^= 1
            self.tr[u].lazy ^= 1


class Solution:
    def handleQuery(
        self, nums1: List[int], nums2: List[int], queries: List[List[int]]
    ) -> List[int]:
        tree = SegmentTree(nums1)
        s = sum(nums2)
        ans = []
        for op, a, b in queries:
            if op == 1:
                tree.modify(1, a + 1, b + 1)
            elif op == 2:
                s += a * tree.query(1, 1, len(nums1))
            else:
                ans.append(s)
        return ans
```

#### Java

```java
class Node {
    int l, r;
    int s, lazy;
}

class SegmentTree {
    private Node[] tr;
    private int[] nums;

    public SegmentTree(int[] nums) {
        int n = nums.length;
        this.nums = nums;
        tr = new Node[n << 2];
        for (int i = 0; i < tr.length; ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    private void build(int u, int l, int r) {
        tr[u].l = l;
        tr[u].r = r;
        if (l == r) {
            tr[u].s = nums[l - 1];
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    public void modify(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            tr[u].lazy ^= 1;
            tr[u].s = tr[u].r - tr[u].l + 1 - tr[u].s;
            return;
        }
        pushdown(u);
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (l <= mid) {
            modify(u << 1, l, r);
        }
        if (r > mid) {
            modify(u << 1 | 1, l, r);
        }
        pushup(u);
    }

    public int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].s;
        }
        pushdown(u);
        int mid = (tr[u].l + tr[u].r) >> 1;
        int res = 0;
        if (l <= mid) {
            res += query(u << 1, l, r);
        }
        if (r > mid) {
            res += query(u << 1 | 1, l, r);
        }
        return res;
    }

    private void pushup(int u) {
        tr[u].s = tr[u << 1].s + tr[u << 1 | 1].s;
    }

    private void pushdown(int u) {
        if (tr[u].lazy == 1) {
            int mid = (tr[u].l + tr[u].r) >> 1;
            tr[u << 1].s = mid - tr[u].l + 1 - tr[u << 1].s;
            tr[u << 1].lazy ^= 1;
            tr[u << 1 | 1].s = tr[u].r - mid - tr[u << 1 | 1].s;
            tr[u << 1 | 1].lazy ^= 1;
            tr[u].lazy ^= 1;
        }
    }
}

class Solution {
    public long[] handleQuery(int[] nums1, int[] nums2, int[][] queries) {
        SegmentTree tree = new SegmentTree(nums1);
        long s = 0;
        for (int x : nums2) {
            s += x;
        }
        int m = 0;
        for (var q : queries) {
            if (q[0] == 3) {
                ++m;
            }
        }
        long[] ans = new long[m];
        int i = 0;
        for (var q : queries) {
            if (q[0] == 1) {
                tree.modify(1, q[1] + 1, q[2] + 1);
            } else if (q[0] == 2) {
                s += 1L * q[1] * tree.query(1, 1, nums2.length);
            } else {
                ans[i++] = s;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Node {
public:
    int l = 0, r = 0;
    int s = 0, lazy = 0;
};

class SegmentTree {
public:
    SegmentTree(vector<int>& nums) {
        this->nums = nums;
        int n = nums.size();
        tr.resize(n << 2);
        for (int i = 0; i < tr.size(); ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    void modify(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) {
            tr[u]->lazy ^= 1;
            tr[u]->s = tr[u]->r - tr[u]->l + 1 - tr[u]->s;
            return;
        }
        pushdown(u);
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (l <= mid) {
            modify(u << 1, l, r);
        }
        if (r > mid) {
            modify(u << 1 | 1, l, r);
        }
        pushup(u);
    }

    int query(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) {
            return tr[u]->s;
        }
        pushdown(u);
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        int res = 0;
        if (l <= mid) {
            res += query(u << 1, l, r);
        }
        if (r > mid) {
            res += query(u << 1 | 1, l, r);
        }
        return res;
    }

private:
    vector<Node*> tr;
    vector<int> nums;

    void build(int u, int l, int r) {
        tr[u]->l = l;
        tr[u]->r = r;
        if (l == r) {
            tr[u]->s = nums[l - 1];
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    void pushup(int u) {
        tr[u]->s = tr[u << 1]->s + tr[u << 1 | 1]->s;
    }

    void pushdown(int u) {
        if (tr[u]->lazy) {
            int mid = (tr[u]->l + tr[u]->r) >> 1;
            tr[u << 1]->s = mid - tr[u]->l + 1 - tr[u << 1]->s;
            tr[u << 1]->lazy ^= 1;
            tr[u << 1 | 1]->s = tr[u]->r - mid - tr[u << 1 | 1]->s;
            tr[u << 1 | 1]->lazy ^= 1;
            tr[u]->lazy ^= 1;
        }
    }
};

class Solution {
public:
    vector<long long> handleQuery(vector<int>& nums1, vector<int>& nums2, vector<vector<int>>& queries) {
        SegmentTree* tree = new SegmentTree(nums1);
        long long s = 0;
        for (int& x : nums2) {
            s += x;
        }
        vector<long long> ans;
        for (auto& q : queries) {
            if (q[0] == 1) {
                tree->modify(1, q[1] + 1, q[2] + 1);
            } else if (q[0] == 2) {
                s += 1LL * q[1] * tree->query(1, 1, nums1.size());
            } else {
                ans.push_back(s);
            }
        }
        return ans;
    }
};
```

#### Go

```go
type node struct {
	l, r, s, lazy int
}

type segmentTree struct {
	nums []int
	tr   []*node
}

func newSegmentTree(nums []int) *segmentTree {
	n := len(nums)
	tr := make([]*node, n<<2)
	for i := range tr {
		tr[i] = &node{}
	}
	t := &segmentTree{nums, tr}
	t.build(1, 1, n)
	return t
}

func (t *segmentTree) build(u, l, r int) {
	t.tr[u].l, t.tr[u].r = l, r
	if l == r {
		t.tr[u].s = t.nums[l-1]
		return
	}
	mid := (l + r) >> 1
	t.build(u<<1, l, mid)
	t.build(u<<1|1, mid+1, r)
	t.pushup(u)
}

func (t *segmentTree) modify(u, l, r int) {
	if t.tr[u].l >= l && t.tr[u].r <= r {
		t.tr[u].lazy ^= 1
		t.tr[u].s = t.tr[u].r - t.tr[u].l + 1 - t.tr[u].s
		return
	}
	t.pushdown(u)
	mid := (t.tr[u].l + t.tr[u].r) >> 1
	if l <= mid {
		t.modify(u<<1, l, r)
	}
	if r > mid {
		t.modify(u<<1|1, l, r)
	}
	t.pushup(u)
}

func (t *segmentTree) query(u, l, r int) int {
	if t.tr[u].l >= l && t.tr[u].r <= r {
		return t.tr[u].s
	}
	t.pushdown(u)
	mid := (t.tr[u].l + t.tr[u].r) >> 1
	res := 0
	if l <= mid {
		res += t.query(u<<1, l, r)
	}
	if r > mid {
		res += t.query(u<<1|1, l, r)
	}
	return res
}

func (t *segmentTree) pushup(u int) {
	t.tr[u].s = t.tr[u<<1].s + t.tr[u<<1|1].s
}

func (t *segmentTree) pushdown(u int) {
	if t.tr[u].lazy == 1 {
		mid := (t.tr[u].l + t.tr[u].r) >> 1
		t.tr[u<<1].s = mid - t.tr[u].l + 1 - t.tr[u<<1].s
		t.tr[u<<1].lazy ^= 1
		t.tr[u<<1|1].s = t.tr[u].r - mid - t.tr[u<<1|1].s
		t.tr[u<<1|1].lazy ^= 1
		t.tr[u].lazy ^= 1
	}
}

func handleQuery(nums1 []int, nums2 []int, queries [][]int) (ans []int64) {
	tree := newSegmentTree(nums1)
	var s int64
	for _, x := range nums2 {
		s += int64(x)
	}
	for _, q := range queries {
		if q[0] == 1 {
			tree.modify(1, q[1]+1, q[2]+1)
		} else if q[0] == 2 {
			s += int64(q[1] * tree.query(1, 1, len(nums1)))
		} else {
			ans = append(ans, s)
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
