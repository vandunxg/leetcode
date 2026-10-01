---
comments: true
difficulty: Hard
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Divide and Conquer
    - Ordered Set
    - Treap
    - Merge Sort
---

<!-- problem:start -->

# [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self)

[中文文档](/solution/0300-0399/0315.Count%20of%20Smaller%20Numbers%20After%20Self/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về mảng số nguyên <code>counts</code>, trong đó <code>counts[i]</code> là số phần tử nhỏ hơn nằm bên phải <code>nums[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,2,6,1]
<strong>Đầu ra:</strong> [2,1,1,0]
<strong>Giải thích:</strong>
Bên phải 5 có <b>2</b> phần tử nhỏ hơn (2 và 1).
Bên phải 2 chỉ có <b>1</b> phần tử nhỏ hơn (1).
Bên phải 6 có <b>1</b> phần tử nhỏ hơn (1).
Bên phải 1 không có phần tử nào nhỏ hơn (<b>0</b> phần tử).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1]
<strong>Đầu ra:</strong> [0]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,-1]
<strong>Đầu ra:</strong> [0,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm số phần tử nhỏ hơn ở phía sau. Duyệt lồng nhau tốn $O(n^2)$. Có thể xem phép đếm này là truy vấn tổng prefix trên tần suất các giá trị đã gặp ở bên phải.
>
> Duyệt từ phải sang trái: nén tọa độ, thêm rank hiện tại vào Fenwick tree, rồi truy vấn các rank nhỏ hơn nghiêm ngặt. Cập nhật trước rồi truy vấn đến $x-1$ vẫn loại trừ các giá trị bằng nhau. Đảo ngược kết quả để khôi phục thứ tự ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    @staticmethod
    def lowbit(x):
        return x & -x

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] += delta
            x += BinaryIndexedTree.lowbit(x)

    def query(self, x):
        s = 0
        while x > 0:
            s += self.c[x]
            x -= BinaryIndexedTree.lowbit(x)
        return s


class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        alls = sorted(set(nums))
        m = {v: i for i, v in enumerate(alls, 1)}
        tree = BinaryIndexedTree(len(m))
        ans = []
        for v in nums[::-1]:
            x = m[v]
            tree.update(x, 1)
            ans.append(tree.query(x - 1))
        return ans[::-1]
```

#### Java

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int v : nums) {
            s.add(v);
        }
        List<Integer> alls = new ArrayList<>(s);
        alls.sort(Comparator.comparingInt(a -> a));
        int n = alls.size();
        Map<Integer, Integer> m = new HashMap<>(n);
        for (int i = 0; i < n; ++i) {
            m.put(alls.get(i), i + 1);
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        LinkedList<Integer> ans = new LinkedList<>();
        for (int i = nums.length - 1; i >= 0; --i) {
            int x = m.get(nums[i]);
            tree.update(x, 1);
            ans.addFirst(tree.query(x - 1));
        }
        return ans;
    }
}

class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += lowbit(x);
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    public static int lowbit(int x) {
        return x & -x;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    int n;
    vector<int> c;

    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += lowbit(x);
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    int lowbit(int x) {
        return x & -x;
    }
};

class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        vector<int> alls(s.begin(), s.end());
        sort(alls.begin(), alls.end());
        unordered_map<int, int> m;
        int n = alls.size();
        for (int i = 0; i < n; ++i) m[alls[i]] = i + 1;
        BinaryIndexedTree* tree = new BinaryIndexedTree(n);
        vector<int> ans(nums.size());
        for (int i = nums.size() - 1; i >= 0; --i) {
            int x = m[nums[i]];
            tree->update(x, 1);
            ans[i] = tree->query(x - 1);
        }
        return ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) *BinaryIndexedTree {
	c := make([]int, n+1)
	return &BinaryIndexedTree{n, c}
}

func (this *BinaryIndexedTree) lowbit(x int) int {
	return x & -x
}

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += this.lowbit(x)
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= this.lowbit(x)
	}
	return s
}

func countSmaller(nums []int) []int {
	s := make(map[int]bool)
	for _, v := range nums {
		s[v] = true
	}
	var alls []int
	for v := range s {
		alls = append(alls, v)
	}
	sort.Ints(alls)
	m := make(map[int]int)
	for i, v := range alls {
		m[v] = i + 1
	}
	ans := make([]int, len(nums))
	tree := newBinaryIndexedTree(len(alls))
	for i := len(nums) - 1; i >= 0; i-- {
		x := m[nums[i]]
		tree.update(x, 1)
		ans[i] = tree.query(x - 1)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Fenwick tree thực hiện truy vấn tổng prefix trên miền giá trị. Segment tree chia miền đó thành các đoạn, hỗ trợ tăng tại một điểm và truy vấn đoạn $[1,x-1]$, tương tự cách 1 nhưng biểu diễn tường minh các node đoạn.

<!-- thinking:end -->

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
        self.tr = [Node() for _ in range(n << 2)]
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
            self.tr[u].v += v
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def query(self, u, l, r):
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].v
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        v = 0
        if l <= mid:
            v += self.query(u << 1, l, r)
        if r > mid:
            v += self.query(u << 1 | 1, l, r)
        return v

    def pushup(self, u):
        self.tr[u].v = self.tr[u << 1].v + self.tr[u << 1 | 1].v


class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        s = sorted(set(nums))
        m = {v: i for i, v in enumerate(s, 1)}
        tree = SegmentTree(len(s))
        ans = []
        for v in nums[::-1]:
            x = m[v]
            ans.append(tree.query(1, 1, x - 1))
            tree.modify(1, x, 1)
        return ans[::-1]
```

#### Java

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int v : nums) {
            s.add(v);
        }
        List<Integer> alls = new ArrayList<>(s);
        alls.sort(Comparator.comparingInt(a -> a));
        int n = alls.size();
        Map<Integer, Integer> m = new HashMap<>(n);
        for (int i = 0; i < n; ++i) {
            m.put(alls.get(i), i + 1);
        }
        SegmentTree tree = new SegmentTree(n);
        LinkedList<Integer> ans = new LinkedList<>();
        for (int i = nums.length - 1; i >= 0; --i) {
            int x = m.get(nums[i]);
            tree.modify(1, x, 1);
            ans.addFirst(tree.query(1, 1, x - 1));
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
            tr[u].v += v;
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
        tr[u].v = tr[u << 1].v + tr[u << 1 | 1].v;
    }

    public int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].v;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        int v = 0;
        if (l <= mid) {
            v += query(u << 1, l, r);
        }
        if (r > mid) {
            v += query(u << 1 | 1, l, r);
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
            tr[u]->v += v;
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
        tr[u]->v = tr[u << 1]->v + tr[u << 1 | 1]->v;
    }

    int query(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) return tr[u]->v;
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        int v = 0;
        if (l <= mid) v += query(u << 1, l, r);
        if (r > mid) v += query(u << 1 | 1, l, r);
        return v;
    }
};

class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        vector<int> alls(s.begin(), s.end());
        sort(alls.begin(), alls.end());
        unordered_map<int, int> m;
        int n = alls.size();
        for (int i = 0; i < n; ++i) m[alls[i]] = i + 1;
        SegmentTree* tree = new SegmentTree(n);
        vector<int> ans(nums.size());
        for (int i = nums.size() - 1; i >= 0; --i) {
            int x = m[nums[i]];
            tree->modify(1, x, 1);
            ans[i] = tree->query(1, 1, x - 1);
        }
        return ans;
    }
};
```

#### Go

```go
type node struct {
	l, r, v int
}

type segmentTree struct {
	tr []node
}

func newSegmentTree(n int) *segmentTree {
	t := &segmentTree{tr: make([]node, n<<2)}
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
}

func (t *segmentTree) modify(u, x, v int) {
	if t.tr[u].l == x && t.tr[u].r == x {
		t.tr[u].v += v
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

func (t *segmentTree) pushup(u int) {
	t.tr[u].v = t.tr[u<<1].v + t.tr[u<<1|1].v
}

func (t *segmentTree) query(u, l, r int) int {
	if t.tr[u].l >= l && t.tr[u].r <= r {
		return t.tr[u].v
	}
	mid := (t.tr[u].l + t.tr[u].r) >> 1
	v := 0
	if l <= mid {
		v += t.query(u<<1, l, r)
	}
	if r > mid {
		v += t.query(u<<1|1, l, r)
	}
	return v
}

func countSmaller(nums []int) []int {
	s := map[int]struct{}{}
	for _, v := range nums {
		s[v] = struct{}{}
	}
	alls := make([]int, 0, len(s))
	for v := range s {
		alls = append(alls, v)
	}
	sort.Ints(alls)
	m := map[int]int{}
	for i, v := range alls {
		m[v] = i + 1
	}
	tree := newSegmentTree(len(alls))
	ans := make([]int, len(nums))
	for i := len(nums) - 1; i >= 0; i-- {
		x := m[nums[i]]
		tree.modify(1, x, 1)
		ans[i] = tree.query(1, 1, x-1)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Merge sort

<!-- thinking:start -->

> **Tư duy**
>
> Hai cách đầu cần nén tọa độ và một tree phụ. Trong lúc merge, khi giá trị bên trái $\le$ giá trị bên phải hiện tại, có đúng $j$ phần tử bên phải đã được lấy ra nhỏ hơn nó và nằm sau nó; cộng $j$ vào kết quả tại chỉ số tương ứng. Không cần tree trên miền giá trị, độ phức tạp thời gian vẫn là $O(n\log n)$.

<!-- thinking:end -->

Trong bước merge của merge sort, khi phần tử bên trái $\textit{left}[i] \leq \textit{right}[j]$,
điều đó có nghĩa là có đúng $j$ phần tử ở nửa phải nhỏ hơn $\textit{left}[i]$,
nên ta cộng $j$ vào số lượng tương ứng với $\textit{left}[i]$.

Khi đã duyệt hết các phần tử bên phải, tất cả phần tử ở mảng phải đều nhỏ hơn
mỗi phần tử còn lại ở mảng trái. Vì vậy, ta cộng độ dài toàn bộ mảng phải
vào số lượng của từng phần tử còn lại bên trái.

**Lưu ý:** Trong C++, merge sort trên mảng rất lớn có thể vượt giới hạn bộ nhớ.
Hãy dùng một mảng buffer để tránh cấp phát bộ nhớ quá nhiều lần.

#### Độ phức tạp

- Độ phức tạp thời gian: $O(n \log n)$, là độ phức tạp thời gian tiêu chuẩn của merge sort.
- Độ phức tạp không gian: $O(n)$, là độ phức tạp không gian tiêu chuẩn của recursion stack.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSmaller(self, nums: list[int]) -> list[int]:
        self.right_smaller_counts = [0] * len(nums)

        nums_indices = [(num, idx) for idx, num in enumerate(nums)]
        self.merge_sort(nums_indices)

        return self.right_smaller_counts

    def combine_arrays(
        self,
        left_nums_indices: list[tuple[int, int]],
        right_nums_indices: list[tuple[int, int]],
    ) -> list[tuple[int, int]]:
        merged_nums_indices: list[tuple[int, int]] = []
        left_idx, right_idx = 0, 0

        while left_idx < len(left_nums_indices) and right_idx < len(right_nums_indices):
            if left_nums_indices[left_idx][0] <= right_nums_indices[right_idx][0]:
                # Iterated left side element finalizes its right smaller count.
                left_num_idx = left_nums_indices[left_idx][1]
                self.right_smaller_counts[left_num_idx] += right_idx

                merged_nums_indices.append(left_nums_indices[left_idx])
                left_idx += 1
                continue

            merged_nums_indices.append(right_nums_indices[right_idx])
            right_idx += 1

        while left_idx < len(left_nums_indices):
            # Iterated left side element finalizes its right smaller count.
            left_num_idx = left_nums_indices[left_idx][1]
            self.right_smaller_counts[left_num_idx] += len(right_nums_indices)

            merged_nums_indices.append(left_nums_indices[left_idx])
            left_idx += 1

        while right_idx < len(right_nums_indices):
            merged_nums_indices.append(right_nums_indices[right_idx])
            right_idx += 1

        return merged_nums_indices

    def merge_sort(self, nums_indices: list[tuple[int, int]]) -> list[tuple[int, int]]:
        if len(nums_indices) == 1:
            return nums_indices  # Single element.

        split_idx = len(nums_indices) // 2

        left_nums_indices = self.merge_sort(nums_indices[:split_idx])
        right_nums_indices = self.merge_sort(nums_indices[split_idx:])

        return self.combine_arrays(left_nums_indices, right_nums_indices)
```

#### C++

```cpp
class Solution {
private:
    vector<int> rightSmallerCounts;
    vector<pair<int, int>> buffer;

    void combineArrays(
        vector<pair<int, int>>& numsIndices, int leftBound, int splitIdx, int rightBound) {
        // Left side array = numsIndices[leftBound: splitIdx].
        // Right side array = numsIndices[splitIdx: rightBound + 1].
        int leftIdx = leftBound, rightIdx = splitIdx;
        int bufferIdx = leftBound;

        while (leftIdx < splitIdx && rightIdx <= rightBound) {
            if (numsIndices[leftIdx].first <= numsIndices[rightIdx].first) {
                // Iterated left side element finalizes its right smaller count.
                int leftNumIdx = numsIndices[leftIdx].second;
                rightSmallerCounts[leftNumIdx] += rightIdx - splitIdx;

                buffer[bufferIdx++] = numsIndices[leftIdx++];
            }

            else
                buffer[bufferIdx++] = numsIndices[rightIdx++];
        }

        while (leftIdx < splitIdx) {
            // Iterated left side element finalizes its right smaller count.
            int leftNumIdx = numsIndices[leftIdx].second;
            rightSmallerCounts[leftNumIdx] += rightIdx - splitIdx;

            buffer[bufferIdx++] = numsIndices[leftIdx++];
        }

        while (rightIdx <= rightBound)
            buffer[bufferIdx++] = numsIndices[rightIdx++];

        for (int idx = leftBound; idx <= rightBound; idx++)
            numsIndices[idx] = buffer[idx]; // Put buffer data back to original array.
    }

    void mergeSort(vector<pair<int, int>>& numsIndices, int leftBound, int rightBound) {
        if (leftBound == rightBound) return; // Single element.

        // Plus 1: ensure splitIdx > leftBound.
        int splitIdx = (leftBound + rightBound + 1) / 2;

        mergeSort(numsIndices, leftBound, splitIdx - 1);
        mergeSort(numsIndices, splitIdx, rightBound);

        combineArrays(numsIndices, leftBound, splitIdx, rightBound);
    }

public:
    vector<int> countSmaller(vector<int>& nums) {
        buffer.resize(nums.size()); // Against memory explosions.

        vector<pair<int, int>> numsIndices(nums.size());
        for (int idx = 0; idx < nums.size(); idx++)
            numsIndices[idx] = {nums[idx], idx};

        rightSmallerCounts.assign(nums.size(), 0);
        mergeSort(numsIndices, 0, nums.size() - 1);
        return rightSmallerCounts;
    }
};
```

#### Go

```go
type Pair struct {
	val   int
	index int
}

var (
	tmp   []Pair
	count []int
)

func countSmaller(nums []int) []int {
	tmp, count = make([]Pair, len(nums)), make([]int, len(nums))
	array := make([]Pair, len(nums))
	for i, v := range nums {
		array[i] = Pair{val: v, index: i}
	}
	sorted(array, 0, len(array)-1)
	return count
}

func sorted(arr []Pair, low, high int) {
	if low >= high {
		return
	}
	mid := low + (high-low)/2
	sorted(arr, low, mid)
	sorted(arr, mid+1, high)
	merge(arr, low, mid, high)
}

func merge(arr []Pair, low, mid, high int) {
	left, right := low, mid+1
	idx := low
	for left <= mid && right <= high {
		if arr[left].val <= arr[right].val {
			count[arr[left].index] += right - mid - 1
			tmp[idx], left = arr[left], left+1
		} else {
			tmp[idx], right = arr[right], right+1
		}
		idx++
	}
	for left <= mid {
		count[arr[left].index] += right - mid - 1
		tmp[idx] = arr[left]
		idx, left = idx+1, left+1
	}
	for right <= high {
		tmp[idx] = arr[right]
		idx, right = idx+1, right+1
	}
	// 排序
	for i := low; i <= high; i++ {
		arr[i] = tmp[i]
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
