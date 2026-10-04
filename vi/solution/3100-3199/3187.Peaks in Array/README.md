---
comments: true
difficulty: Hard
rating: 2154
source: Weekly Contest 402 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
---

<!-- problem:start -->

# [3187. Peaks in Array](https://leetcode.com/problems/peaks-in-array)

[中文文档](/solution/3100-3199/3187.Peaks%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>peak</strong> trong mảng <code>arr</code> là một phần tử <strong>lớn hơn</strong> phần tử đứng trước và phần tử đứng sau nó trong <code>arr</code>.</p>

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một mảng số nguyên hai chiều <code>queries</code>.</p>

<p>Bạn phải xử lý các truy vấn thuộc hai loại:</p>

<ul>
	<li><code>queries[i] = [1, l<sub>i</sub>, r<sub>i</sub>]</code>, xác định số lượng phần tử <strong>peak</strong> trong <span data-keyword="subarray">mảng con</span> <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.<!-- notionvc: 73b20b7c-e1ab-4dac-86d0-13761094a9ae --></li>
	<li><code>queries[i] = [2, index<sub>i</sub>, val<sub>i</sub>]</code>, thay đổi <code>nums[index<sub>i</sub>]</code> thành <code><font face="monospace">val<sub>i</sub></font></code>.</li>
</ul>

<p>Trả về một mảng <code>answer</code> chứa kết quả của các truy vấn loại đầu tiên theo đúng thứ tự.<!-- notionvc: a9ccef22-4061-4b5a-b4cc-a2b2a0e12f30 --></p>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li>Phần tử <strong>đầu tiên</strong> và <strong>cuối cùng</strong> của một mảng hoặc mảng con<!-- notionvc: fcffef72-deb5-47cb-8719-3a3790102f73 --> <strong>không thể</strong> là một peak.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,4,2,5], queries = [[2,3,4],[1,0,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Truy vấn đầu tiên: Ta thay đổi <code>nums[3]</code> thành 4 và <code>nums</code> trở thành <code>[3,1,4,4,5]</code>.</p>

<p>Truy vấn thứ hai: Số lượng peak trong <code>[3,1,4,4,5]</code> là 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,4,2,1,5], queries = [[2,2,4],[1,0,2],[1,0,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Truy vấn đầu tiên: <code>nums[2]</code> phải trở thành 4, nhưng nó vốn đã là 4.</p>

<p>Truy vấn thứ hai: Số lượng peak trong <code>[4,1,4]</code> là 0.</p>

<p>Truy vấn thứ ba: Số 4 thứ hai là một peak trong <code>[4,1,4,2,1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i][0] == 1</code> hoặc <code>queries[i][0] == 2</code></li>
	<li>Với mọi <code>i</code> thỏa:
	<ul>
		<li><code>queries[i][0] == 1</code>: <code>0 &lt;= queries[i][1] &lt;= queries[i][2] &lt;= nums.length - 1</code></li>
		<li><code>queries[i][0] == 2</code>: <code>0 &lt;= queries[i][1] &lt;= nums.length - 1</code>, <code>1 &lt;= queries[i][2] &lt;= 10<sup>5</sup></code></li>
	</ul>
	</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Một peak là một chỉ số nằm bên trong, có giá trị lớn hơn nghiêm ngặt cả hai phần tử lân cận. Việc đếm peak theo đoạn kết hợp với cập nhật từng điểm sẽ khiến việc dựng lại có độ phức tạp $O(nq)$.
>
> Các peak là các chỉ báo $0/1$, nên Fenwick tree có thể tính tổng trên đoạn. Phép gán tại $idx$ chỉ làm thay đổi các peak ở $idx-1,idx,idx+1$.
>
> Chèn các peak ban đầu. Một truy vấn tính tổng trên $(l+1,r-1)$. Một lần cập nhật trừ ba cờ cũ, ghi giá trị mới, rồi cộng các cờ mới.

<!-- thinking:end -->

Theo mô tả bài toán, với $0 < i < n - 1$, nếu thỏa mãn $nums[i - 1] < nums[i]$ và $nums[i] > nums[i + 1]$, ta có thể xem $nums[i]$ là $1$, nếu không thì là $0$. Vì vậy, với thao tác $1$, tức là truy vấn số lượng phần tử peak trong mảng con $nums[l..r]$, ta chỉ cần truy vấn số lượng $1$ trong đoạn $[l + 1, r - 1]$. Ta có thể dùng binary indexed tree để duy trì số lượng $1$ trong đoạn $[1, n - 1]$.

Với thao tác $1$, tức là cập nhật $nums[idx]$ thành $val$, thao tác này chỉ ảnh hưởng đến các giá trị ở vị trí $idx - 1$, $idx$ và $idx + 1$, nên ta chỉ cần cập nhật ba vị trí này. Cụ thể, trước tiên ta xóa các phần tử peak ở ba vị trí này, sau đó cập nhật giá trị của $nums[idx]$, rồi thêm lại các phần tử peak ở ba vị trí này.

Độ phức tạp thời gian là $O((n + q) \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $q$ lần lượt là độ dài của mảng `nums` và mảng truy vấn `queries`.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = "n", "c"

    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, delta: int) -> None:
        while x <= self.n:
            self.c[x] += delta
            x += x & -x

    def query(self, x: int) -> int:
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def countOfPeaks(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        def update(i: int, val: int):
            if i <= 0 or i >= n - 1:
                return
            if nums[i - 1] < nums[i] and nums[i] > nums[i + 1]:
                tree.update(i, val)

        n = len(nums)
        tree = BinaryIndexedTree(n - 1)
        for i in range(1, n - 1):
            update(i, 1)
        ans = []
        for q in queries:
            if q[0] == 1:
                l, r = q[1] + 1, q[2] - 1
                ans.append(0 if l > r else tree.query(r) - tree.query(l - 1))
            else:
                idx, val = q[1:]
                for i in range(idx - 1, idx + 2):
                    update(i, -1)
                nums[idx] = val
                for i in range(idx - 1, idx + 2):
                    update(i, 1)
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

    public void update(int x, int delta) {
        for (; x <= n; x += x & -x) {
            c[x] += delta;
        }
    }

    public int query(int x) {
        int s = 0;
        for (; x > 0; x -= x & -x) {
            s += c[x];
        }
        return s;
    }
}

class Solution {
    private BinaryIndexedTree tree;
    private int[] nums;

    public List<Integer> countOfPeaks(int[] nums, int[][] queries) {
        int n = nums.length;
        this.nums = nums;
        tree = new BinaryIndexedTree(n - 1);
        for (int i = 1; i < n - 1; ++i) {
            update(i, 1);
        }
        List<Integer> ans = new ArrayList<>();
        for (var q : queries) {
            if (q[0] == 1) {
                int l = q[1] + 1, r = q[2] - 1;
                ans.add(l > r ? 0 : tree.query(r) - tree.query(l - 1));
            } else {
                int idx = q[1], val = q[2];
                for (int i = idx - 1; i <= idx + 1; ++i) {
                    update(i, -1);
                }
                nums[idx] = val;
                for (int i = idx - 1; i <= idx + 1; ++i) {
                    update(i, 1);
                }
            }
        }
        return ans;
    }

    private void update(int i, int val) {
        if (i <= 0 || i >= nums.length - 1) {
            return;
        }
        if (nums[i - 1] < nums[i] && nums[i] > nums[i + 1]) {
            tree.update(i, val);
        }
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int n;
    vector<int> c;

public:
    BinaryIndexedTree(int n)
        : n(n)
        , c(n + 1) {}

    void update(int x, int delta) {
        for (; x <= n; x += x & -x) {
            c[x] += delta;
        }
    }

    int query(int x) {
        int s = 0;
        for (; x > 0; x -= x & -x) {
            s += c[x];
        }
        return s;
    }
};

class Solution {
public:
    vector<int> countOfPeaks(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        BinaryIndexedTree tree(n - 1);
        auto update = [&](int i, int val) {
            if (i <= 0 || i >= n - 1) {
                return;
            }
            if (nums[i - 1] < nums[i] && nums[i] > nums[i + 1]) {
                tree.update(i, val);
            }
        };
        for (int i = 1; i < n - 1; ++i) {
            update(i, 1);
        }
        vector<int> ans;
        for (auto& q : queries) {
            if (q[0] == 1) {
                int l = q[1] + 1, r = q[2] - 1;
                ans.push_back(l > r ? 0 : tree.query(r) - tree.query(l - 1));
            } else {
                int idx = q[1], val = q[2];
                for (int i = idx - 1; i <= idx + 1; ++i) {
                    update(i, -1);
                }
                nums[idx] = val;
                for (int i = idx - 1; i <= idx + 1; ++i) {
                    update(i, 1);
                }
            }
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

func NewBinaryIndexedTree(n int) *BinaryIndexedTree {
	return &BinaryIndexedTree{n: n, c: make([]int, n+1)}
}

func (bit *BinaryIndexedTree) update(x, delta int) {
	for ; x <= bit.n; x += x & -x {
		bit.c[x] += delta
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	s := 0
	for ; x > 0; x -= x & -x {
		s += bit.c[x]
	}
	return s
}

func countOfPeaks(nums []int, queries [][]int) (ans []int) {
	n := len(nums)
	tree := NewBinaryIndexedTree(n - 1)
	update := func(i, val int) {
		if i <= 0 || i >= n-1 {
			return
		}
		if nums[i-1] < nums[i] && nums[i] > nums[i+1] {
			tree.update(i, val)
		}
	}
	for i := 1; i < n-1; i++ {
		update(i, 1)
	}
	for _, q := range queries {
		if q[0] == 1 {
			l, r := q[1]+1, q[2]-1
			t := 0
			if l <= r {
				t = tree.query(r) - tree.query(l-1)
			}
			ans = append(ans, t)
		} else {
			idx, val := q[1], q[2]
			for i := idx - 1; i <= idx+1; i++ {
				update(i, -1)
			}
			nums[idx] = val
			for i := idx - 1; i <= idx+1; i++ {
				update(i, 1)
			}
		}
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

    update(x: number, delta: number): void {
        for (; x <= this.n; x += x & -x) {
            this.c[x] += delta;
        }
    }

    query(x: number): number {
        let s = 0;
        for (; x > 0; x -= x & -x) {
            s += this.c[x];
        }
        return s;
    }
}

function countOfPeaks(nums: number[], queries: number[][]): number[] {
    const n = nums.length;
    const tree = new BinaryIndexedTree(n - 1);
    const update = (i: number, val: number): void => {
        if (i <= 0 || i >= n - 1) {
            return;
        }
        if (nums[i - 1] < nums[i] && nums[i] > nums[i + 1]) {
            tree.update(i, val);
        }
    };
    for (let i = 1; i < n - 1; ++i) {
        update(i, 1);
    }
    const ans: number[] = [];
    for (const q of queries) {
        if (q[0] === 1) {
            const [l, r] = [q[1] + 1, q[2] - 1];
            ans.push(l > r ? 0 : tree.query(r) - tree.query(l - 1));
        } else {
            const [idx, val] = [q[1], q[2]];
            for (let i = idx - 1; i <= idx + 1; ++i) {
                update(i, -1);
            }
            nums[idx] = val;
            for (let i = idx - 1; i <= idx + 1; ++i) {
                update(i, 1);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_of_peaks(mut nums: Vec<i32>, queries: Vec<Vec<i32>>) -> Vec<i32> {
        struct BinaryIndexedTree {
            n: usize,
            c: Vec<i32>,
        }

        impl BinaryIndexedTree {
            fn new(n: usize) -> Self {
                Self { n, c: vec![0; n + 1] }
            }

            fn update(&mut self, mut x: usize, delta: i32) {
                while x <= self.n {
                    self.c[x] += delta;
                    x += x & (!x + 1);
                }
            }

            fn query(&self, mut x: usize) -> i32 {
                let mut s = 0;
                while x > 0 {
                    s += self.c[x];
                    x &= x - 1;
                }
                s
            }
        }

        let n = nums.len();
        let mut tree = BinaryIndexedTree::new(n - 1);

        let mut update = |i: usize, val: i32, nums: &Vec<i32>, tree: &mut BinaryIndexedTree| {
            if i == 0 || i >= n - 1 {
                return;
            }
            if nums[i - 1] < nums[i] && nums[i] > nums[i + 1] {
                tree.update(i, val);
            }
        };

        for i in 1..n - 1 {
            update(i, 1, &nums, &mut tree);
        }

        let mut ans = Vec::new();
        for q in queries {
            if q[0] == 1 {
                let l = (q[1] + 1).max(1) as usize;
                let r = (q[2] - 1).max(0) as usize;
                if l > r {
                    ans.push(0);
                } else {
                    ans.push(tree.query(r) - tree.query(l - 1));
                }
            } else {
                let idx = q[1] as usize;
                let val = q[2];
                let left = if idx > 0 { idx - 1 } else { 0 };
                let right = usize::min(idx + 1, n - 1);
                for i in left..=right {
                    update(i, -1, &nums, &mut tree);
                }
                nums[idx] = val;
                for i in left..=right {
                    update(i, 1, &nums, &mut tree);
                }
            }
        }

        ans
    }
}
```

#### JavaScript

```js
class BinaryIndexedTree {
    constructor(n) {
        this.n = n;
        this.c = Array(n + 1).fill(0);
    }

    update(x, delta) {
        for (; x <= this.n; x += x & -x) {
            this.c[x] += delta;
        }
    }

    query(x) {
        let s = 0;
        for (; x > 0; x -= x & -x) {
            s += this.c[x];
        }
        return s;
    }
}

/**
 * @param {number[]} nums
 * @param {number[][]} queries
 * @return {number[]}
 */
var countOfPeaks = function (nums, queries) {
    const n = nums.length;
    const tree = new BinaryIndexedTree(n - 1);

    const update = (i, val) => {
        if (i <= 0 || i >= n - 1) {
            return;
        }
        if (nums[i - 1] < nums[i] && nums[i] > nums[i + 1]) {
            tree.update(i, val);
        }
    };

    for (let i = 1; i < n - 1; ++i) {
        update(i, 1);
    }

    const ans = [];
    for (const q of queries) {
        if (q[0] === 1) {
            const l = q[1] + 1;
            const r = q[2] - 1;
            ans.push(l > r ? 0 : tree.query(r) - tree.query(l - 1));
        } else {
            const idx = q[1];
            const val = q[2];
            for (let i = idx - 1; i <= idx + 1; ++i) {
                update(i, -1);
            }
            nums[idx] = val;
            for (let i = idx - 1; i <= idx + 1; ++i) {
                update(i, 1);
            }
        }
    }

    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
