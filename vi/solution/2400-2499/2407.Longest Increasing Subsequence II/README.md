---
comments: true
difficulty: Hard
rating: 2280
source: Weekly Contest 310 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Queue
    - Array
    - Divide and Conquer
    - Dynamic Programming
    - Monotonic Queue
---

<!-- problem:start -->

# [2407. Longest Increasing Subsequence II](https://leetcode.com/problems/longest-increasing-subsequence-ii)

[中文文档](/solution/2400-2499/2407.Longest%20Increasing%20Subsequence%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Hãy tìm dãy con dài nhất của <code>nums</code> thỏa mãn các yêu cầu sau:</p>

<ul>
	<li>Dãy con <strong>tăng nghiêm ngặt</strong> và</li>
	<li>Hiệu giữa hai phần tử liên tiếp trong dãy con <strong>không vượt quá</strong> <code>k</code>.</li>
</ul>

<p>Trả về <em>độ dài của <strong>dãy con</strong> <strong>dài nhất</strong> thỏa mãn các yêu cầu trên.</em></p>

<p><strong>Dãy con</strong> là một mảng có thể thu được từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,2,1,4,3,4,5,8,15], k = 3
<strong>Output:</strong> 5
<strong>Explanation:</strong>
Dãy con dài nhất thỏa mãn các yêu cầu là [1,3,4,5,8].
Dãy con có độ dài bằng 5, nên ta trả về 5.
Lưu ý rằng dãy con [1,3,4,5,8,15] không thỏa mãn các yêu cầu vì 15 - 8 = 7 lớn hơn 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [7,4,5,1,8,12,4,7], k = 5
<strong>Output:</strong> 4
<strong>Explanation:</strong>
Dãy con dài nhất thỏa mãn các yêu cầu là [4,5,8,12].
Dãy con có độ dài bằng 4, nên ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,5], k = 1
<strong>Output:</strong> 1
<strong>Explanation:</strong>
Dãy con dài nhất thỏa mãn các yêu cầu là [1].
Dãy con có độ dài bằng 1, nên ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> LIS kinh điển có độ phức tạp $O(n^2)$, không đáp ứng được khi $n\le 10^5$, đồng thời hiệu giữa hai giá trị liên tiếp không được vượt quá $k$. Độ dài lớn nhất của dãy kết thúc tại $v$ bằng một cộng với giá trị $f$ lớn nhất trên đoạn $[v-k,v-1]$, vì vậy ta cần truy vấn giá trị lớn nhất trên đoạn và cập nhật một điểm trong miền giá trị.
>
> Các giá trị nằm trong $[1,10^5]$. Segment tree lưu $f[v]$; với mỗi $v$, ta truy vấn rồi cập nhật, đạt độ phức tạp $O(n\log V)$.

<!-- thinking:end -->

Ta giả sử rằng $f[v]$ biểu diễn độ dài của dãy con tăng dần dài nhất kết thúc bằng giá trị $v$.

Ta duyệt từng phần tử $v$ trong mảng $nums$, với công thức chuyển trạng thái: $f[v] = \max(f[v], f[x])$, trong đó miền của $x$ là $[v-k, v-1]$.

Do đó, ta cần một cấu trúc dữ liệu để duy trì giá trị lớn nhất trong một đoạn. Không khó để nghĩ đến việc sử dụng segment tree.

Segment tree chia toàn bộ đoạn thành nhiều đoạn con không liên tục, và số đoạn con không vượt quá $log(width)$. Để cập nhật giá trị của một phần tử, chỉ cần cập nhật $log(width)$ đoạn, và các đoạn này đều nằm trong một đoạn lớn chứa phần tử đó.

- Mỗi node của segment tree biểu diễn một đoạn;
- Segment tree có một node gốc duy nhất, biểu diễn toàn bộ miền thống kê, chẳng hạn như $[1,N]$;
- Mỗi node lá của segment tree biểu diễn một đoạn cơ sở có độ dài $1$, $[x, x]$;
- Với mỗi node bên trong $[l,r]$, node con trái là $[l,mid]$, node con phải là $[mid+1,r]$, trong đó $mid = \left \lfloor \frac{l+r}{2} \right \rfloor$.

Trong bài toán này, thông tin được duy trì tại node của segment tree là giá trị lớn nhất trong đoạn tương ứng.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self):
        self.l = 0
        self.r = 0
        self.v = 0


class SegmentTree:
    def __init__(self, n):
        self.tr = [Node() for _ in range(4 * n)]
        self.build(1, 1, n)

    def build(self, u, l, r):
        self.tr[u].l = l
        self.tr[u].r = r
        if l == r:
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)

    def modify(self, u, x, v):
        if self.tr[u].l == x and self.tr[u].r == x:
            self.tr[u].v = v
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def pushup(self, u):
        self.tr[u].v = max(self.tr[u << 1].v, self.tr[u << 1 | 1].v)

    def query(self, u, l, r):
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].v
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        v = 0
        if l <= mid:
            v = self.query(u << 1, l, r)
        if r > mid:
            v = max(v, self.query(u << 1 | 1, l, r))
        return v


class Solution:
    def lengthOfLIS(self, nums: List[int], k: int) -> int:
        tree = SegmentTree(max(nums))
        ans = 1
        for v in nums:
            t = tree.query(1, v - k, v - 1) + 1
            ans = max(ans, t)
            tree.modify(1, v, t)
        return ans
```

#### Java

```java
class Solution {
    public int lengthOfLIS(int[] nums, int k) {
        int mx = nums[0];
        for (int v : nums) {
            mx = Math.max(mx, v);
        }
        SegmentTree tree = new SegmentTree(mx);
        int ans = 0;
        for (int v : nums) {
            int t = tree.query(1, v - k, v - 1) + 1;
            ans = Math.max(ans, t);
            tree.modify(1, v, t);
        }
        return ans;
    }
}

class Node {
    int l;
    int r;
    int v;
}

class SegmentTree {
    private Node[] tr;

    public SegmentTree(int n) {
        tr = new Node[4 * n];
        for (int i = 0; i < tr.length; ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    public void build(int u, int l, int r) {
        tr[u].l = l;
        tr[u].r = r;
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    public void modify(int u, int x, int v) {
        if (tr[u].l == x && tr[u].r == x) {
            tr[u].v = v;
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

    public void pushup(int u) {
        tr[u].v = Math.max(tr[u << 1].v, tr[u << 1 | 1].v);
    }

    public int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].v;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        int v = 0;
        if (l <= mid) {
            v = query(u << 1, l, r);
        }
        if (r > mid) {
            v = Math.max(v, query(u << 1 | 1, l, r));
        }
        return v;
    }
}
```

#### C++

```cpp
class Node {
public:
    int l;
    int r;
    int v;
};

class SegmentTree {
public:
    vector<Node*> tr;

    SegmentTree(int n) {
        tr.resize(4 * n);
        for (int i = 0; i < tr.size(); ++i) tr[i] = new Node();
        build(1, 1, n);
    }

    void build(int u, int l, int r) {
        tr[u]->l = l;
        tr[u]->r = r;
        if (l == r) return;
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    void modify(int u, int x, int v) {
        if (tr[u]->l == x && tr[u]->r == x) {
            tr[u]->v = v;
            return;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (x <= mid)
            modify(u << 1, x, v);
        else
            modify(u << 1 | 1, x, v);
        pushup(u);
    }

    void pushup(int u) {
        tr[u]->v = max(tr[u << 1]->v, tr[u << 1 | 1]->v);
    }

    int query(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) return tr[u]->v;
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        int v = 0;
        if (l <= mid) v = query(u << 1, l, r);
        if (r > mid) v = max(v, query(u << 1 | 1, l, r));
        return v;
    }
};

class Solution {
public:
    int lengthOfLIS(vector<int>& nums, int k) {
        SegmentTree* tree = new SegmentTree(*max_element(nums.begin(), nums.end()));
        int ans = 1;
        for (int v : nums) {
            int t = tree->query(1, v - k, v - 1) + 1;
            ans = max(ans, t);
            tree->modify(1, v, t);
        }
        return ans;
    }
};
```

#### Go

```go
func lengthOfLIS(nums []int, k int) int {
	mx := slices.Max(nums)
	tree := newSegmentTree(mx)
	ans := 1
	for _, v := range nums {
		t := tree.query(1, v-k, v-1) + 1
		ans = max(ans, t)
		tree.modify(1, v, t)
	}
	return ans
}

type node struct {
	l int
	r int
	v int
}

type segmentTree struct {
	tr []*node
}

func newSegmentTree(n int) *segmentTree {
	tr := make([]*node, n<<2)
	for i := range tr {
		tr[i] = &node{}
	}
	t := &segmentTree{tr}
	t.build(1, 1, n)
	return t
}

func (t *segmentTree) build(u, l, r int) {
	t.tr[u].l, t.tr[u].r = l, r
	if l == r {
		return
	}
	mid := (l + r) >> 1
	t.build(u<<1, l, mid)
	t.build(u<<1|1, mid+1, r)
	t.pushup(u)
}

func (t *segmentTree) modify(u, x, v int) {
	if t.tr[u].l == x && t.tr[u].r == x {
		t.tr[u].v = v
		return
	}
	mid := (t.tr[u].l + t.tr[u].r) >> 1
	if x <= mid {
		t.modify(u<<1, x, v)
	} else {
		t.modify(u<<1|1, x, v)
	}
	t.pushup(u)
}

func (t *segmentTree) query(u, l, r int) int {
	if t.tr[u].l >= l && t.tr[u].r <= r {
		return t.tr[u].v
	}
	mid := (t.tr[u].l + t.tr[u].r) >> 1
	v := 0
	if l <= mid {
		v = t.query(u<<1, l, r)
	}
	if r > mid {
		v = max(v, t.query(u<<1|1, l, r))
	}
	return v
}

func (t *segmentTree) pushup(u int) {
	t.tr[u].v = max(t.tr[u<<1].v, t.tr[u<<1|1].v)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
