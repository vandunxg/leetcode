---
comments: true
difficulty: Hard
rating: 2327
source: Weekly Contest 372 Q4
tags:
    - Stack
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Monotonic Stack
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2940. Find Building Where Alice and Bob Can Meet](https://leetcode.com/problems/find-building-where-alice-and-bob-can-meet)

[中文文档](/solution/2900-2999/2940.Find%20Building%20Where%20Alice%20and%20Bob%20Can%20Meet/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>heights</code> được <strong>đánh chỉ số từ 0</strong>, trong đó <code>heights[i]</code> biểu diễn chiều cao của tòa nhà thứ <code>i<sup>th</sup></code>.</p>

<p>Nếu một người đang ở tòa nhà <code>i</code>, họ có thể di chuyển đến bất kỳ tòa nhà nào khác <code>j</code> khi và chỉ khi <code>i &lt; j</code> và <code>heights[i] &lt; heights[j]</code>.</p>

<p>Bạn cũng được cho một mảng khác là <code>queries</code>, trong đó <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>. Trong truy vấn thứ <code>i<sup>th</sup></code>, Alice đang ở tòa nhà <code>a<sub>i</sub></code> còn Bob đang ở tòa nhà <code>b<sub>i</sub></code>.</p>

<p>Trả về <em>một mảng</em> <code>ans</code> <em>trong đó</em> <code>ans[i]</code> <em>là <strong>chỉ số của tòa nhà ngoài cùng bên trái</strong> nơi Alice và Bob có thể gặp nhau trong</em> <code>i<sup>th</sup></code> <em>truy vấn</em>. <em>Nếu Alice và Bob không thể di chuyển đến cùng một tòa nhà trong truy vấn</em> <code>i</code>, <em>gán</em> <code>ans[i]</code> <em>bằng</em> <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [6,4,8,5,2,7], queries = [[0,1],[0,3],[2,4],[3,4],[2,2]]
<strong>Đầu ra:</strong> [2,5,-1,5,2]
<strong>Giải thích:</strong> Trong truy vấn đầu tiên, Alice và Bob có thể di chuyển đến tòa nhà 2 vì heights[0] &lt; heights[2] và heights[1] &lt; heights[2].
Trong truy vấn thứ hai, Alice và Bob có thể di chuyển đến tòa nhà 5 vì heights[0] &lt; heights[5] và heights[3] &lt; heights[5].
Trong truy vấn thứ ba, Alice không thể gặp Bob vì Alice không thể di chuyển đến bất kỳ tòa nhà nào khác.
Trong truy vấn thứ tư, Alice và Bob có thể di chuyển đến tòa nhà 5 vì heights[3] &lt; heights[5] và heights[4] &lt; heights[5].
Trong truy vấn thứ năm, Alice và Bob đã ở cùng một tòa nhà.
Với ans[i] != -1, có thể chứng minh rằng ans[i] là tòa nhà ngoài cùng bên trái nơi Alice và Bob có thể gặp nhau.
Với ans[i] == -1, có thể chứng minh rằng không tồn tại tòa nhà nào nơi Alice và Bob có thể gặp nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [5,3,8,2,6,1,4,6], queries = [[0,7],[3,5],[5,2],[3,0],[1,6]]
<strong>Đầu ra:</strong> [7,6,-1,4,6]
<strong>Giải thích:</strong> Trong truy vấn đầu tiên, Alice có thể di chuyển trực tiếp đến tòa nhà của Bob vì heights[0] &lt; heights[7].
Trong truy vấn thứ hai, Alice và Bob có thể di chuyển đến tòa nhà 6 vì heights[3] &lt; heights[6] và heights[5] &lt; heights[6].
Trong truy vấn thứ ba, Alice không thể gặp Bob vì Bob không thể di chuyển đến bất kỳ tòa nhà nào khác.
Trong truy vấn thứ tư, Alice và Bob có thể di chuyển đến tòa nhà 4 vì heights[3] &lt; heights[4] và heights[0] &lt; heights[4].
Trong truy vấn thứ năm, Alice có thể di chuyển trực tiếp đến tòa nhà của Bob vì heights[1] &lt; heights[6].
Với ans[i] != -1, có thể chứng minh rằng ans[i] là tòa nhà ngoài cùng bên trái nơi Alice và Bob có thể gặp nhau.
Với ans[i] == -1, có thể chứng minh rằng không tồn tại tòa nhà nào nơi Alice và Bob có thể gặp nhau.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= heights.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= heights.length - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Họ sẽ gặp nhau tại một tòa nhà nào đó $j \ge \max(a,b)$ cao hơn người đang ở tòa nhà thấp hơn (hoặc đơn giản là ngay tại tòa nhà bên phải nếu tòa nhà đó đã cao hơn). Duyệt phần đuôi cho từng truy vấn sẽ quá chậm với $n,q \le 5 \times 10^4$.
>
> Xử lý các truy vấn theo thứ tự giảm dần của điểm cuối bên phải và thêm các tòa nhà nằm bên phải $r$ vào một Fenwick tree, được lập chỉ mục theo chiều cao đã nén và lưu chỉ số nhỏ nhất. Truy vấn tòa nhà có chỉ số nhỏ nhất và cao hơn $heights[l]$ sẽ cho ta tòa nhà hai người gặp nhau.

<!-- thinking:end -->

Gọi $queries[i] = [l_i, r_i]$, trong đó $l_i \le r_i$. Nếu $l_i = r_i$ hoặc $heights[l_i] < heights[r_i]$, đáp án là $r_i$. Ngược lại, ta cần tìm $j$ nhỏ nhất trong tất cả các $j > r_i$ và $heights[j] > heights[l_i]$.

Ta có thể sắp xếp $queries$ theo thứ tự giảm dần của $r_i$, đồng thời dùng con trỏ $j$ để trỏ đến chỉ số hiện tại đang được duyệt trong $heights$.

Tiếp theo, ta duyệt từng truy vấn $queries[i] = (l, r)$. Với truy vấn hiện tại, nếu $j > r$, ta lặp để thêm $heights[j]$ vào binary indexed tree. Binary indexed tree duy trì chỉ số nhỏ nhất của chiều cao trong phần đuôi (sau khi rời rạc hóa). Sau đó, ta kiểm tra xem $l = r$ hoặc $heights[l] < heights[r]$. Nếu đúng, đáp án của truy vấn hiện tại là $r$. Ngược lại, ta truy vấn chỉ số nhỏ nhất của $heights[l]$ trong binary indexed tree, đó là đáp án của truy vấn hiện tại.

Độ phức tạp thời gian là $O((n + m) \times \log n + m \times \log m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của $heights$ và $queries$.

Bài toán tương tự:

- [2736. Maximum Sum Queries](https://github.com/doocs/leetcode/blob/main/solution/2700-2799/2736.Maximum%20Sum%20Queries/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = ["n", "c"]

    def __init__(self, n: int):
        self.n = n
        self.c = [inf] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = min(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        mi = inf
        while x:
            mi = min(mi, self.c[x])
            x -= x & -x
        return -1 if mi == inf else mi


class Solution:
    def leftmostBuildingQueries(
        self, heights: List[int], queries: List[List[int]]
    ) -> List[int]:
        n, m = len(heights), len(queries)
        for i in range(m):
            queries[i] = [min(queries[i]), max(queries[i])]
        j = n - 1
        s = sorted(set(heights))
        ans = [-1] * m
        tree = BinaryIndexedTree(n)
        for i in sorted(range(m), key=lambda i: -queries[i][1]):
            l, r = queries[i]
            while j > r:
                k = n - bisect_left(s, heights[j]) + 1
                tree.update(k, j)
                j -= 1
            if l == r or heights[l] < heights[r]:
                ans[i] = r
            else:
                k = n - bisect_left(s, heights[l])
                ans[i] = tree.query(k)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private final int inf = 1 << 30;
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
        Arrays.fill(c, inf);
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] = Math.min(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int mi = inf;
        while (x > 0) {
            mi = Math.min(mi, c[x]);
            x -= x & -x;
        }
        return mi == inf ? -1 : mi;
    }
}

class Solution {
    public int[] leftmostBuildingQueries(int[] heights, int[][] queries) {
        int n = heights.length;
        int m = queries.length;
        for (int i = 0; i < m; ++i) {
            if (queries[i][0] > queries[i][1]) {
                queries[i] = new int[] {queries[i][1], queries[i][0]};
            }
        }
        Integer[] idx = new Integer[m];
        for (int i = 0; i < m; ++i) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> queries[j][1] - queries[i][1]);
        int[] s = heights.clone();
        Arrays.sort(s);
        int[] ans = new int[m];
        int j = n - 1;
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (int i : idx) {
            int l = queries[i][0], r = queries[i][1];
            while (j > r) {
                int k = n - Arrays.binarySearch(s, heights[j]) + 1;
                tree.update(k, j);
                --j;
            }
            if (l == r || heights[l] < heights[r]) {
                ans[i] = r;
            } else {
                int k = n - Arrays.binarySearch(s, heights[l]);
                ans[i] = tree.query(k);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int inf = 1 << 30;
    int n;
    vector<int> c;

public:
    BinaryIndexedTree(int n) {
        this->n = n;
        c.resize(n + 1, inf);
    }

    void update(int x, int v) {
        while (x <= n) {
            c[x] = min(c[x], v);
            x += x & -x;
        }
    }

    int query(int x) {
        int mi = inf;
        while (x > 0) {
            mi = min(mi, c[x]);
            x -= x & -x;
        }
        return mi == inf ? -1 : mi;
    }
};

class Solution {
public:
    vector<int> leftmostBuildingQueries(vector<int>& heights, vector<vector<int>>& queries) {
        int n = heights.size(), m = queries.size();
        for (auto& q : queries) {
            if (q[0] > q[1]) {
                swap(q[0], q[1]);
            }
        }
        vector<int> idx(m);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return queries[j][1] < queries[i][1];
        });
        vector<int> s = heights;
        sort(s.begin(), s.end());
        s.erase(unique(s.begin(), s.end()), s.end());
        vector<int> ans(m);
        int j = n - 1;
        BinaryIndexedTree tree(n);
        for (int i : idx) {
            int l = queries[i][0], r = queries[i][1];
            while (j > r) {
                int k = s.end() - lower_bound(s.begin(), s.end(), heights[j]) + 1;
                tree.update(k, j);
                --j;
            }
            if (l == r || heights[l] < heights[r]) {
                ans[i] = r;
            } else {
                int k = s.end() - lower_bound(s.begin(), s.end(), heights[l]);
                ans[i] = tree.query(k);
            }
        }
        return ans;
    }
};
```

#### Go

```go
const inf int = 1 << 30

type BinaryIndexedTree struct {
	n int
	c []int
}

func NewBinaryIndexedTree(n int) BinaryIndexedTree {
	c := make([]int, n+1)
	for i := range c {
		c[i] = inf
	}
	return BinaryIndexedTree{n: n, c: c}
}

func (bit *BinaryIndexedTree) update(x, v int) {
	for x <= bit.n {
		bit.c[x] = min(bit.c[x], v)
		x += x & -x
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	mi := inf
	for x > 0 {
		mi = min(mi, bit.c[x])
		x -= x & -x
	}
	if mi == inf {
		return -1
	}
	return mi
}

func leftmostBuildingQueries(heights []int, queries [][]int) []int {
	n, m := len(heights), len(queries)
	for _, q := range queries {
		if q[0] > q[1] {
			q[0], q[1] = q[1], q[0]
		}
	}
	idx := make([]int, m)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return queries[idx[j]][1] < queries[idx[i]][1] })
	s := make([]int, n)
	copy(s, heights)
	sort.Ints(s)
	ans := make([]int, m)
	tree := NewBinaryIndexedTree(n)
	j := n - 1
	for _, i := range idx {
		l, r := queries[i][0], queries[i][1]
		for ; j > r; j-- {
			k := n - sort.SearchInts(s, heights[j]) + 1
			tree.update(k, j)
		}
		if l == r || heights[l] < heights[r] {
			ans[i] = r
		} else {
			k := n - sort.SearchInts(s, heights[l])
			ans[i] = tree.query(k)
		}
	}
	return ans
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];
    private inf: number = 1 << 30;

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(this.inf);
    }

    update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] = Math.min(this.c[x], v);
            x += x & -x;
        }
    }

    query(x: number): number {
        let mi = this.inf;
        while (x > 0) {
            mi = Math.min(mi, this.c[x]);
            x -= x & -x;
        }
        return mi === this.inf ? -1 : mi;
    }
}

function leftmostBuildingQueries(heights: number[], queries: number[][]): number[] {
    const n = heights.length;
    const m = queries.length;
    for (const q of queries) {
        if (q[0] > q[1]) {
            [q[0], q[1]] = [q[1], q[0]];
        }
    }
    const idx: number[] = Array(m)
        .fill(0)
        .map((_, i) => i);
    idx.sort((i, j) => queries[j][1] - queries[i][1]);
    const tree = new BinaryIndexedTree(n);
    const ans: number[] = Array(m).fill(-1);
    const s = [...heights];
    s.sort((a, b) => a - b);
    const search = (x: number) => {
        let [l, r] = [0, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (s[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    let j = n - 1;
    for (const i of idx) {
        const [l, r] = queries[i];
        while (j > r) {
            const k = n - search(heights[j]) + 1;
            tree.update(k, j);
            --j;
        }
        if (l === r || heights[l] < heights[r]) {
            ans[i] = r;
        } else {
            const k = n - search(heights[l]);
            ans[i] = tree.query(k);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
