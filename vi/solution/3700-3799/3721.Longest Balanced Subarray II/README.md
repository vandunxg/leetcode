---
comments: true
difficulty: Hard
rating: 2723
source: Weekly Contest 472 Q4
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Divide and Conquer
    - Prefix Sum
---

<!-- problem:start -->

# [3721. Longest Balanced Subarray II](https://leetcode.com/problems/longest-balanced-subarray-ii)

[中文文档](/solution/3700-3799/3721.Longest%20Balanced%20Subarray%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> được gọi là <strong>cân bằng</strong> nếu số lượng <strong>số chẵn phân biệt</strong> trong mảng con bằng số lượng <strong>số lẻ phân biệt</strong>.</p>

<p>Trả về độ dài của mảng con cân bằng <strong>dài nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng dài nhất là <code>[2, 5, 4, 3]</code>.</li>
	<li>Mảng này có 2 số chẵn phân biệt là <code>[2, 4]</code> và 2 số lẻ phân biệt là <code>[5, 3]</code>. Do đó, đáp án là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,2,5,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng dài nhất là <code>[3, 2, 2, 5, 4]</code>.</li>
	<li>Mảng này có 2 số chẵn phân biệt là <code>[2, 4]</code> và 2 số lẻ phân biệt là <code>[3, 5]</code>. Do đó, đáp án là 5.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng dài nhất là <code>[2, 3, 2]</code>.</li>
	<li>Mảng này có 1 số chẵn phân biệt là <code>[2]</code> và 1 số lẻ phân biệt là <code>[3]</code>. Do đó, đáp án là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree + Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Cách duyệt bậc hai của bài trước không phù hợp khi kích thước dữ liệu lớn. Gán số lẻ thành $+1$ và số chẵn thành $-1$, điều kiện cân bằng trở thành một đoạn có tổng bằng không; khi một giá trị lặp lại, ta phải hủy đóng góp trước đó của nó. Các cập nhật này là phép cộng trên đoạn, còn ta cần tìm tổng tiền tố sớm nhất bằng tổng hiện tại, nên có thể dùng segment tree với giá trị min/max và lazy tag để hỗ trợ.

<!-- thinking:end -->

Ta có thể chuyển bài toán thành bài toán tổng tiền tố. Đặt biến tổng tiền tố $\textit{now}$, biểu diễn hiệu giữa số lẻ và số chẵn trong mảng con hiện tại:

$$
\textit{now} = \text{distinct odd numbers} - \text{distinct even numbers}
$$

Với phần tử lẻ, ghi nhận giá trị $+1$, còn với phần tử chẵn, ghi nhận giá trị $-1$. Dùng hash table $\textit{last}$ để lưu vị trí xuất hiện gần nhất của mỗi số. Nếu một số xuất hiện nhiều lần, ta cần hủy đóng góp trước đó của nó.

Để tính hiệu quả độ dài mảng con mỗi khi thêm phần tử ở đầu phải, ta dùng segment tree để duy trì giá trị nhỏ nhất và lớn nhất của tổng tiền tố trên đoạn, đồng thời hỗ trợ phép cộng trên đoạn và truy vấn tìm kiếm nhị phân trên segment tree. Khi duyệt đến đầu phải $i$, trước tiên cập nhật đóng góp của phần tử hiện tại, sau đó dùng segment tree để tìm vị trí sớm nhất $pos$ mà tổng tiền tố hiện tại $\textit{now}$ xuất hiện. Độ dài mảng con hiện tại là $i - pos$, từ đó cập nhật đáp án:

$$
\textit{ans} = \max(\textit{ans}, i - pos)
$$

Độ phức tạp thời gian là $O(n \log n)$, trong đó $n$ là độ dài mảng. Mỗi lần cập nhật hoặc truy vấn segment tree mất $O(\log n)$, và việc duyệt qua đầu phải $n$ lần cho tổng độ phức tạp thời gian là $O(n \log n)$. Độ phức tạp không gian là $O(n)$, vì các node của segment tree và hash table đều chiếm $O(n)$ không gian.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestBalanced(self, nums: List[int]) -> int:
        n = len(nums)

        class Node:
            __slots__ = ("l", "r", "mn", "mx", "lazy")
            def __init__(self):
                self.l = self.r = 0
                self.mn = self.mx = 0
                self.lazy = 0

        tr = [Node() for _ in range((n + 1) * 4)]

        def build(u: int, l: int, r: int):
            tr[u].l, tr[u].r = l, r
            tr[u].mn = tr[u].mx = tr[u].lazy = 0
            if l == r:
                return
            mid = (l + r) >> 1
            build(u << 1, l, mid)
            build(u << 1 | 1, mid + 1, r)

        def apply(u: int, v: int):
            tr[u].mn += v
            tr[u].mx += v
            tr[u].lazy += v

        def pushdown(u: int):
            if tr[u].lazy:
                apply(u << 1, tr[u].lazy)
                apply(u << 1 | 1, tr[u].lazy)
                tr[u].lazy = 0

        def pushup(u: int):
            tr[u].mn = min(tr[u << 1].mn, tr[u << 1 | 1].mn)
            tr[u].mx = max(tr[u << 1].mx, tr[u << 1 | 1].mx)

        def modify(u: int, l: int, r: int, v: int):
            if tr[u].l >= l and tr[u].r <= r:
                apply(u, v)
                return
            pushdown(u)
            mid = (tr[u].l + tr[u].r) >> 1
            if l <= mid:
                modify(u << 1, l, r, v)
            if r > mid:
                modify(u << 1 | 1, l, r, v)
            pushup(u)

        def query(u: int, target: int) -> int:
            if tr[u].l == tr[u].r:
                return tr[u].l
            pushdown(u)
            if tr[u << 1].mn <= target <= tr[u << 1].mx:
                return query(u << 1, target)
            return query(u << 1 | 1, target)

        build(1, 0, n)

        last = {}
        now = ans = 0

        for i, x in enumerate(nums, start=1):
            det = 1 if (x & 1) else -1
            if x in last:
                modify(1, last[x], n, -det)
                now -= det
            last[x] = i
            modify(1, i, n, det)
            now += det
            pos = query(1, now)
            ans = max(ans, i - pos)

        return ans
```

#### Java

```java
/**
 *
 * Idea:
 * - Treat each distinct odd number as +1, and each distinct even number as -1
 * - Maintain prefix sums representing the balance
 * - When a number appears again, remove its previous contribution
 * - Use a segment tree to maintain min/max prefix sums with range add
 * - Binary search on the segment tree to find the earliest equal prefix sum
 */
class Solution {

    /**
     * Segment tree node
     */
    static class Node {
        int l, r; // segment range
        int mn, mx; // minimum / maximum prefix sum
        int lazy; // lazy propagation (range add)
    }

    /**
     * Segment tree supporting:
     * - range add
     * - find the smallest index with a given prefix sum
     */
    static class SegmentTree {
        Node[] tr;

        SegmentTree(int n) {
            tr = new Node[n << 2];
            for (int i = 0; i < tr.length; i++) {
                tr[i] = new Node();
            }
            build(1, 0, n);
        }

        // Build an empty tree with all prefix sums = 0
        void build(int u, int l, int r) {
            tr[u].l = l;
            tr[u].r = r;
            tr[u].mn = tr[u].mx = 0;
            tr[u].lazy = 0;
            if (l == r) return;
            int mid = (l + r) >> 1;
            build(u << 1, l, mid);
            build(u << 1 | 1, mid + 1, r);
        }

        // Add value v to all prefix sums in [l, r]
        void modify(int u, int l, int r, int v) {
            if (tr[u].l >= l && tr[u].r <= r) {
                apply(u, v);
                return;
            }
            pushdown(u);
            int mid = (tr[u].l + tr[u].r) >> 1;
            if (l <= mid) modify(u << 1, l, r, v);
            if (r > mid) modify(u << 1 | 1, l, r, v);
            pushup(u);
        }

        // Binary search on the segment tree
        // Find the smallest index where prefix sum == target
        int query(int u, int target) {
            if (tr[u].l == tr[u].r) {
                return tr[u].l;
            }
            pushdown(u);
            int left = u << 1;
            int right = u << 1 | 1;
            if (tr[left].mn <= target && target <= tr[left].mx) {
                return query(left, target);
            }
            return query(right, target);
        }

        // Apply range add
        void apply(int u, int v) {
            tr[u].mn += v;
            tr[u].mx += v;
            tr[u].lazy += v;
        }

        // Update from children
        void pushup(int u) {
            tr[u].mn = Math.min(tr[u << 1].mn, tr[u << 1 | 1].mn);
            tr[u].mx = Math.max(tr[u << 1].mx, tr[u << 1 | 1].mx);
        }

        // Push lazy tag down
        void pushdown(int u) {
            if (tr[u].lazy != 0) {
                apply(u << 1, tr[u].lazy);
                apply(u << 1 | 1, tr[u].lazy);
                tr[u].lazy = 0;
            }
        }
    }

    public int longestBalanced(int[] nums) {
        int n = nums.length;
        SegmentTree st = new SegmentTree(n);

        // last[x] = last position where value x appeared
        Map<Integer, Integer> last = new HashMap<>();

        int now = 0; // current prefix sum
        int ans = 0; // answer

        // Enumerate the right endpoint
        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            int det = (x & 1) == 1 ? 1 : -1;

            // Remove previous contribution if x appeared before
            if (last.containsKey(x)) {
                st.modify(1, last.get(x), n, -det);
                now -= det;
            }

            // Add current contribution
            last.put(x, i);
            st.modify(1, i, n, det);
            now += det;

            // Find earliest position with the same prefix sum
            int pos = st.query(1, now);
            ans = Math.max(ans, i - pos);
        }

        return ans;
    }
}
```

#### C++

```cpp
/*
 * Segment tree node.
 * It maintains:
 *  - [l, r] : the covered index range
 *  - mn     : minimum prefix sum in this range
 *  - mx     : maximum prefix sum in this range
 *  - lazy   : lazy propagation value (range add)
 */
class Node {
public:
    int l = 0, r = 0;
    int mn = 0, mx = 0;
    int lazy = 0;
};

/*
 * Segment Tree that supports:
 *  1. Range add
 *  2. Query the smallest index whose prefix sum equals a given value
 */
class SegmentTree {
public:
    SegmentTree(int n) {
        tr.resize(n << 2);
        for (int i = 0; i < (int) tr.size(); ++i) {
            tr[i] = new Node();
        }
        // Build the tree on range [0, n]
        build(1, 0, n);
    }

    /*
     * Add value v to all prefix sums in range [l, r]
     */
    void modify(int u, int l, int r, int v) {
        if (tr[u]->l >= l && tr[u]->r <= r) {
            apply(u, v);
            return;
        }
        pushdown(u);
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (l <= mid) modify(u << 1, l, r, v);
        if (r > mid) modify(u << 1 | 1, l, r, v);
        pushup(u);
    }

    /*
     * Binary search on the segment tree.
     * Find the smallest index pos such that prefix_sum[pos] == target.
     *
     * The key observation:
     * If target is within [mn, mx] of a segment, then such a position
     * must exist inside this segment.
     */
    int query(int u, int target) {
        if (tr[u]->l == tr[u]->r) {
            return tr[u]->l;
        }
        pushdown(u);
        int lc = u << 1, rc = u << 1 | 1;
        if (tr[lc]->mn <= target && target <= tr[lc]->mx) {
            return query(lc, target);
        }
        return query(rc, target);
    }

private:
    vector<Node*> tr;

    /*
     * Build an empty segment tree.
     * Initial prefix sums are all zero.
     */
    void build(int u, int l, int r) {
        tr[u]->l = l;
        tr[u]->r = r;
        tr[u]->mn = tr[u]->mx = 0;
        tr[u]->lazy = 0;
        if (l == r) return;
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    /*
     * Apply a range add to node u.
     */
    void apply(int u, int v) {
        tr[u]->mn += v;
        tr[u]->mx += v;
        tr[u]->lazy += v;
    }

    /*
     * Push information from children to parent.
     */
    void pushup(int u) {
        tr[u]->mn = min(tr[u << 1]->mn, tr[u << 1 | 1]->mn);
        tr[u]->mx = max(tr[u << 1]->mx, tr[u << 1 | 1]->mx);
    }

    /*
     * Push lazy tag down to children.
     */
    void pushdown(int u) {
        if (tr[u]->lazy != 0) {
            apply(u << 1, tr[u]->lazy);
            apply(u << 1 | 1, tr[u]->lazy);
            tr[u]->lazy = 0;
        }
    }
};

class Solution {
public:
    int longestBalanced(vector<int>& nums) {
        int n = nums.size();
        SegmentTree st(n);

        /*
         * last[x] = the most recent position where value x appeared
         */
        unordered_map<int, int> last;

        int now = 0; // current prefix sum
        int ans = 0; // answer

        /*
         * Enumerate the right endpoint of the subarray
         */
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];

            /*
             * Contribution of x:
             *  +1 if x is odd
             *  -1 if x is even
             */
            int det = (x & 1) ? 1 : -1;

            /*
             * If x appeared before, remove its previous contribution
             */
            if (last.count(x)) {
                st.modify(1, last[x], n, -det);
                now -= det;
            }

            /*
             * Add current contribution of x
             */
            last[x] = i;
            st.modify(1, i, n, det);
            now += det;

            /*
             * Find the smallest position pos such that
             * prefix_sum[pos] == now
             */
            int pos = st.query(1, now);

            /*
             * Update the maximum balanced subarray length
             */
            ans = max(ans, i - pos);
        }

        return ans;
    }
};
```

#### Go

```go
// Segment tree node
type Node struct {
	l, r   int // segment range
	mn, mx int // minimum / maximum prefix sum
	lazy   int // lazy propagation (range add)
}

// Segment tree
type SegmentTree struct {
	tr []Node
}

// Build a segment tree for range [0, n]
func NewSegmentTree(n int) *SegmentTree {
	st := &SegmentTree{
		tr: make([]Node, n<<2),
	}
	st.build(1, 0, n)
	return st
}

// Initialize all prefix sums to 0
func (st *SegmentTree) build(u, l, r int) {
	st.tr[u] = Node{l: l, r: r, mn: 0, mx: 0, lazy: 0}
	if l == r {
		return
	}
	mid := (l + r) >> 1
	st.build(u<<1, l, mid)
	st.build(u<<1|1, mid+1, r)
}

// Add v to all prefix sums in [l, r]
func (st *SegmentTree) modify(u, l, r, v int) {
	if st.tr[u].l >= l && st.tr[u].r <= r {
		st.apply(u, v)
		return
	}
	st.pushdown(u)
	mid := (st.tr[u].l + st.tr[u].r) >> 1
	if l <= mid {
		st.modify(u<<1, l, r, v)
	}
	if r > mid {
		st.modify(u<<1|1, l, r, v)
	}
	st.pushup(u)
}

// Binary search on the segment tree
// Find the smallest index where prefix sum == target
func (st *SegmentTree) query(u, target int) int {
	if st.tr[u].l == st.tr[u].r {
		return st.tr[u].l
	}
	st.pushdown(u)
	left, right := u<<1, u<<1|1
	if st.tr[left].mn <= target && target <= st.tr[left].mx {
		return st.query(left, target)
	}
	return st.query(right, target)
}

// Apply range addition
func (st *SegmentTree) apply(u, v int) {
	st.tr[u].mn += v
	st.tr[u].mx += v
	st.tr[u].lazy += v
}

// Update from children
func (st *SegmentTree) pushup(u int) {
	st.tr[u].mn = min(st.tr[u<<1].mn, st.tr[u<<1|1].mn)
	st.tr[u].mx = max(st.tr[u<<1].mx, st.tr[u<<1|1].mx)
}

// Push lazy value down
func (st *SegmentTree) pushdown(u int) {
	if st.tr[u].lazy != 0 {
		v := st.tr[u].lazy
		st.apply(u<<1, v)
		st.apply(u<<1|1, v)
		st.tr[u].lazy = 0
	}
}

// Main function
func longestBalanced(nums []int) int {
	n := len(nums)
	st := NewSegmentTree(n)

	// last[x] = last position where value x appeared
	last := make(map[int]int)

	now := 0 // current prefix sum
	ans := 0 // answer

	// Enumerate right endpoint
	for i := 1; i <= n; i++ {
		x := nums[i-1]
		det := -1
		if x&1 == 1 {
			det = 1
		}

		// Remove previous contribution if x appeared before
		if pos, ok := last[x]; ok {
			st.modify(1, pos, n, -det)
			now -= det
		}

		// Add current contribution
		last[x] = i
		st.modify(1, i, n, det)
		now += det

		// Find earliest position with same prefix sum
		pos := st.query(1, now)
		ans = max(ans, i-pos)
	}

	return ans
}
```

#### TypeScript

```ts
function longestBalanced(nums: number[]): number {
    const n = nums.length;

    interface Node {
        l: number;
        r: number;
        mn: number;
        mx: number;
        lazy: number;
    }

    const tr: Node[] = Array.from({ length: (n + 1) * 4 }, () => ({
        l: 0,
        r: 0,
        mn: 0,
        mx: 0,
        lazy: 0,
    }));

    function build(u: number, l: number, r: number) {
        tr[u].l = l;
        tr[u].r = r;
        if (l === r) return;
        const mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build((u << 1) | 1, mid + 1, r);
    }

    function apply(u: number, v: number) {
        tr[u].mn += v;
        tr[u].mx += v;
        tr[u].lazy += v;
    }

    function pushdown(u: number) {
        if (tr[u].lazy !== 0) {
            apply(u << 1, tr[u].lazy);
            apply((u << 1) | 1, tr[u].lazy);
            tr[u].lazy = 0;
        }
    }

    function pushup(u: number) {
        tr[u].mn = Math.min(tr[u << 1].mn, tr[(u << 1) | 1].mn);
        tr[u].mx = Math.max(tr[u << 1].mx, tr[(u << 1) | 1].mx);
    }

    function modify(u: number, l: number, r: number, v: number) {
        if (tr[u].l >= l && tr[u].r <= r) {
            apply(u, v);
            return;
        }
        pushdown(u);
        const mid = (tr[u].l + tr[u].r) >> 1;
        if (l <= mid) modify(u << 1, l, r, v);
        if (r > mid) modify((u << 1) | 1, l, r, v);
        pushup(u);
    }

    function query(u: number, target: number): number {
        if (tr[u].l === tr[u].r) return tr[u].l;
        pushdown(u);
        if (tr[u << 1].mn <= target && target <= tr[u << 1].mx) {
            return query(u << 1, target);
        }
        return query((u << 1) | 1, target);
    }

    build(1, 0, n);

    const last = new Map<number, number>();
    let now = 0,
        ans = 0;

    nums.forEach((x, idx) => {
        const i = idx + 1;
        const det = x & 1 ? 1 : -1;
        if (last.has(x)) {
            modify(1, last.get(x)!, n, -det);
            now -= det;
        }
        last.set(x, i);
        modify(1, i, n, det);
        now += det;
        const pos = query(1, now);
        ans = Math.max(ans, i - pos);
    });

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
