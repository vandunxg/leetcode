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

# [327. Count of Range Sum](https://leetcode.com/problems/count-of-range-sum)

[中文文档](/solution/0300-0399/0327.Count%20of%20Range%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và hai số nguyên <code>lower</code>, <code>upper</code>, hãy trả về <em>số đoạn con có tổng nằm trong khoảng</em> <code>[lower, upper]</code>, <em>tính cả hai đầu mút</em>.</p>

<p>Tổng đoạn <code>S(i, j)</code> được định nghĩa là tổng các phần tử trong <code>nums</code> từ chỉ số <code>i</code> đến <code>j</code>, tính cả hai đầu mút, với điều kiện <code>i &lt;= j</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-2,5,-1], lower = -2, upper = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ba đoạn con là [0,0], [2,2] và [0,2], có tổng lần lượt là -2, -1 và 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0], lower = 0, upper = 0
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>-10<sup>5</sup> &lt;= lower &lt;= upper &lt;= 10<sup>5</sup></code></li>
	<li>Đáp án được <strong>đảm bảo</strong> nằm trong phạm vi số nguyên <strong>32-bit</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số mảng con có tổng nằm trong $[lower,upper]$. Tổng mảng con là hiệu của hai prefix sum; dùng hai vòng lặp sẽ tốn $O(n^2)$. Với prefix sum $x$, ta cần đếm số prefix sum trước đó nằm trong $[x-upper,x-lower]$.
>
> Nén tọa độ tất cả giá trị $x$, $x-lower$ và $x-upper$. Fenwick tree lưu tần suất các prefix sum đã gặp: query khoảng tương ứng rồi mới thêm $x$ vào, nhờ đó chỉ đếm các prefix sum xuất hiện trước đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x, v):
        while x <= self.n:
            self.c[x] += v
            x += x & -x

    def query(self, x):
        s = 0
        while x > 0:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def countRangeSum(self, nums: List[int], lower: int, upper: int) -> int:
        s = list(accumulate(nums, initial=0))
        arr = sorted(set(v for x in s for v in (x, x - lower, x - upper)))
        tree = BinaryIndexedTree(len(arr))
        ans = 0
        for x in s:
            l = bisect_left(arr, x - upper) + 1
            r = bisect_left(arr, x - lower) + 1
            ans += tree.query(r) - tree.query(l - 1)
            tree.update(bisect_left(arr, x) + 1, 1)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] += v;
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x != 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        long[] arr = new long[n * 3 + 3];
        for (int i = 0, j = 0; i <= n; ++i, j += 3) {
            arr[j] = s[i];
            arr[j + 1] = s[i] - lower;
            arr[j + 2] = s[i] - upper;
        }
        Arrays.sort(arr);
        int m = 0;
        for (int i = 0; i < arr.length; ++i) {
            if (i == 0 || arr[i] != arr[i - 1]) {
                arr[m++] = arr[i];
            }
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        int ans = 0;
        for (long x : s) {
            int l = search(arr, m, x - upper);
            int r = search(arr, m, x - lower);
            ans += tree.query(r) - tree.query(l - 1);
            tree.update(search(arr, m, x), 1);
        }
        return ans;
    }

    private int search(long[] nums, int r, long x) {
        int l = 0;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l + 1;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int v) {
        while (x <= n) {
            c[x] += v;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }

private:
    int n;
    vector<int> c;
};

class Solution {
public:
    int countRangeSum(vector<int>& nums, int lower, int upper) {
        using ll = long long;
        int n = nums.size();
        ll s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        ll arr[(n + 1) * 3];
        for (int i = 0, j = 0; i <= n; ++i, j += 3) {
            arr[j] = s[i];
            arr[j + 1] = s[i] - lower;
            arr[j + 2] = s[i] - upper;
        }
        sort(arr, arr + (n + 1) * 3);
        int m = unique(arr, arr + (n + 1) * 3) - arr;
        BinaryIndexedTree tree(m);
        int ans = 0;
        for (int i = 0; i <= n; ++i) {
            int l = lower_bound(arr, arr + m, s[i] - upper) - arr + 1;
            int r = lower_bound(arr, arr + m, s[i] - lower) - arr + 1;
            ans += tree.query(r) - tree.query(l - 1);
            tree.update(lower_bound(arr, arr + m, s[i]) - arr + 1, 1);
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

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += x & -x
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= x & -x
	}
	return s
}

func countRangeSum(nums []int, lower int, upper int) (ans int) {
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	arr := make([]int, (n+1)*3)
	for i, j := 0, 0; i <= n; i, j = i+1, j+3 {
		arr[j] = s[i]
		arr[j+1] = s[i] - lower
		arr[j+2] = s[i] - upper
	}
	sort.Ints(arr)
	m := 0
	for i := range arr {
		if i == 0 || arr[i] != arr[i-1] {
			arr[m] = arr[i]
			m++
		}
	}
	arr = arr[:m]
	tree := newBinaryIndexedTree(m)
	for _, x := range s {
		l := sort.SearchInts(arr, x-upper) + 1
		r := sort.SearchInts(arr, x-lower) + 1
		ans += tree.query(r) - tree.query(l-1)
		tree.update(sort.SearchInts(arr, x)+1, 1)
	}
	return
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(0);
    }

    update(x: number, v: number) {
        while (x <= this.n) {
            this.c[x] += v;
            x += x & -x;
        }
    }

    query(x: number): number {
        let s = 0;
        while (x > 0) {
            s += this.c[x];
            x -= x & -x;
        }
        return s;
    }
}

function countRangeSum(nums: number[], lower: number, upper: number): number {
    const n = nums.length;
    const s = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    let arr: number[] = Array((n + 1) * 3);
    for (let i = 0, j = 0; i <= n; ++i, j += 3) {
        arr[j] = s[i];
        arr[j + 1] = s[i] - lower;
        arr[j + 2] = s[i] - upper;
    }
    arr.sort((a, b) => a - b);
    let m = 0;
    for (let i = 0; i < arr.length; ++i) {
        if (i === 0 || arr[i] !== arr[i - 1]) {
            arr[m++] = arr[i];
        }
    }
    arr = arr.slice(0, m);
    const tree = new BinaryIndexedTree(m);
    let ans = 0;
    for (const x of s) {
        const l = search(arr, m, x - upper);
        const r = search(arr, m, x - lower);
        ans += tree.query(r) - tree.query(l - 1);
        tree.update(search(arr, m, x), 1);
    }
    return ans;
}

function search(nums: number[], r: number, x: number): number {
    let l = 0;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (nums[mid] >= x) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l + 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
