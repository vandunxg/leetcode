---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Binary Search
    - Ordered Set
    - Monotonic Stack
---

<!-- problem:start -->

# [456. 132 Pattern](https://leetcode.com/problems/132-pattern)

[中文文档](/solution/0400-0499/0456.132%20Pattern/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng gồm <code>n</code> số nguyên <code>nums</code>. <strong>132 pattern</strong> là một dãy con gồm ba số <code>nums[i]</code>, <code>nums[j]</code> và <code>nums[k]</code> sao cho <code>i &lt; j &lt; k</code> và <code>nums[i] &lt; nums[k] &lt; nums[j]</code>.</p>

<p>Trả về <code>true</code><em> nếu </em><code>nums</code><em> có </em><strong>132 pattern</strong><em>; nếu không, trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Dãy không có 132 pattern.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,4,2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Dãy có 132 pattern: [1, 4, 2].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,3,2,0]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Dãy có ba 132 pattern: [-1, 3, 2], [-1, 3, 0] và [-1, 2, 0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm $i<j<k$ sao cho $nums[i]<nums[k]<nums[j]$. Duyệt ba vòng lặp tốn $O(n^3)$; cố định $j$ rồi quét hai phía vẫn tốn $O(n^2)$.
>
> Duyệt từ phải sang trái bằng một decreasing stack. $vk$ là giá trị lớn nhất đã pop khỏi stack (một ứng viên cho $nums[k]$, nhỏ hơn đỉnh hiện tại). Nếu giá trị hiện tại nhỏ hơn $vk$, nó có thể là $nums[i]$.
>
> Stack lưu các đỉnh ứng viên $nums[j]$. Cập nhật $vk$ trước khi push để bảo đảm $k$ nằm bên phải $j$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def find132pattern(self, nums: List[int]) -> bool:
        vk = -inf
        stk = []
        for x in nums[::-1]:
            if x < vk:
                return True
            while stk and stk[-1] < x:
                vk = stk.pop()
            stk.append(x)
        return False
```

#### Java

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        int vk = -(1 << 30);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = nums.length - 1; i >= 0; --i) {
            if (nums[i] < vk) {
                return true;
            }
            while (!stk.isEmpty() && stk.peek() < nums[i]) {
                vk = stk.pop();
            }
            stk.push(nums[i]);
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        int vk = INT_MIN;
        stack<int> stk;
        for (int i = nums.size() - 1; ~i; --i) {
            if (nums[i] < vk) {
                return true;
            }
            while (!stk.empty() && stk.top() < nums[i]) {
                vk = stk.top();
                stk.pop();
            }
            stk.push(nums[i]);
        }
        return false;
    }
};
```

#### Go

```go
func find132pattern(nums []int) bool {
	vk := -(1 << 30)
	stk := []int{}
	for i := len(nums) - 1; i >= 0; i-- {
		if nums[i] < vk {
			return true
		}
		for len(stk) > 0 && stk[len(stk)-1] < nums[i] {
			vk = stk[len(stk)-1]
			stk = stk[:len(stk)-1]
		}
		stk = append(stk, nums[i])
	}
	return false
}
```

#### TypeScript

```ts
function find132pattern(nums: number[]): boolean {
    let vk = -Infinity;
    const stk: number[] = [];
    for (let i = nums.length - 1; i >= 0; --i) {
        if (nums[i] < vk) {
            return true;
        }
        while (stk.length && stk[stk.length - 1] < nums[i]) {
            vk = stk.pop()!;
        }
        stk.push(nums[i]);
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn find132pattern(nums: Vec<i32>) -> bool {
        let n = nums.len();
        let mut vk = i32::MIN;
        let mut stk = vec![];
        for i in (0..n).rev() {
            if nums[i] < vk {
                return true;
            }
            while !stk.is_empty() && stk.last().unwrap() < &nums[i] {
                vk = stk.pop().unwrap();
            }
            stk.push(nums[i]);
        }
        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chạy tuyến tính nhưng phụ thuộc vào stack duyệt từ phải sang trái. Một cách khác là lưu giá trị nhỏ nhất của prefix, rồi quét từ phải và dùng Fenwick tree để kiểm tra xem đã gặp giá trị nào trong khoảng $(\textit{left}[i],nums[i])$ chưa.
>
> Sau khi nén tọa độ, cây đếm các giá trị đã quét ở bên phải. Cách này có thêm hệ số logarit; cấu trúc dữ liệu đóng vai trò đếm số giá trị trong một khoảng thay vì dùng monotone stack.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] += delta
            x += x & -x

    def query(self, x):
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def find132pattern(self, nums: List[int]) -> bool:
        s = sorted(set(nums))
        n = len(nums)
        left = [inf] * (n + 1)
        for i, x in enumerate(nums):
            left[i + 1] = min(left[i], x)
        tree = BinaryIndexedTree(len(s))
        for i in range(n - 1, -1, -1):
            x = bisect_left(s, nums[i]) + 1
            y = bisect_left(s, left[i]) + 1
            if x > y and tree.query(x - 1) > tree.query(y):
                return True
            tree.update(x, 1)
        return False
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] += v;
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public boolean find132pattern(int[] nums) {
        int[] s = nums.clone();
        Arrays.sort(s);
        int n = nums.length;
        int m = 0;
        int[] left = new int[n + 1];
        left[0] = 1 << 30;
        for (int i = 0; i < n; ++i) {
            left[i + 1] = Math.min(left[i], nums[i]);
            if (i == 0 || s[i] != s[i - 1]) {
                s[m++] = s[i];
            }
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        for (int i = n - 1; i >= 0; --i) {
            int x = search(s, m, nums[i]);
            int y = search(s, m, left[i]);
            if (x > y && tree.query(x - 1) > tree.query(y)) {
                return true;
            }
            tree.update(x, 1);
        }
        return false;
    }

    private int search(int[] nums, int r, int x) {
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
    BinaryIndexedTree(int n) {
        this->n = n;
        this->c = vector<int>(n + 1, 0);
    }

    void update(int x, int val) {
        while (x <= n) {
            c[x] += val;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
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
    bool find132pattern(vector<int>& nums) {
        vector<int> s = nums;
        sort(s.begin(), s.end());
        s.erase(unique(s.begin(), s.end()), s.end());
        BinaryIndexedTree tree(s.size());
        int n = nums.size();
        int left[n + 1];
        memset(left, 63, sizeof(left));
        for (int i = 0; i < n; ++i) {
            left[i + 1] = min(left[i], nums[i]);
        }
        for (int i = nums.size() - 1; ~i; --i) {
            int x = lower_bound(s.begin(), s.end(), nums[i]) - s.begin() + 1;
            int y = lower_bound(s.begin(), s.end(), left[i]) - s.begin() + 1;
            if (x > y && tree.query(x - 1) > tree.query(y)) {
                return true;
            }
            tree.update(x, 1);
        }
        return false;
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

func (this *BinaryIndexedTree) update(x, val int) {
	for x <= this.n {
		this.c[x] += val
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

func find132pattern(nums []int) bool {
	n := len(nums)
	s := make([]int, n)
	left := make([]int, n+1)
	left[0] = 1 << 30
	copy(s, nums)
	sort.Ints(s)
	m := 0
	for i := 0; i < n; i++ {
		left[i+1] = min(left[i], nums[i])
		if i == 0 || s[i] != s[i-1] {
			s[m] = s[i]
			m++
		}
	}
	s = s[:m]
	tree := newBinaryIndexedTree(m)
	for i := n - 1; i >= 0; i-- {
		x := sort.SearchInts(s, nums[i]) + 1
		y := sort.SearchInts(s, left[i]) + 1
		if x > y && tree.query(x-1) > tree.query(y) {
			return true
		}
		tree.update(x, 1)
	}
	return false
}
```

#### TypeScript

```ts
class BinaryIndextedTree {
    n: number;
    c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = new Array(n + 1).fill(0);
    }

    update(x: number, val: number): void {
        while (x <= this.n) {
            this.c[x] += val;
            x += x & -x;
        }
    }

    query(x: number): number {
        let s = 0;
        while (x) {
            s += this.c[x];
            x -= x & -x;
        }
        return s;
    }
}

function find132pattern(nums: number[]): boolean {
    let s: number[] = [...nums];
    s.sort((a, b) => a - b);
    const n = nums.length;
    const left: number[] = new Array(n + 1).fill(1 << 30);
    let m = 0;
    for (let i = 0; i < n; ++i) {
        left[i + 1] = Math.min(left[i], nums[i]);
        if (i == 0 || s[i] != s[i - 1]) {
            s[m++] = s[i];
        }
    }
    s = s.slice(0, m);
    const tree = new BinaryIndextedTree(m);
    for (let i = n - 1; i >= 0; --i) {
        const x = search(s, nums[i]);
        const y = search(s, left[i]);
        if (x > y && tree.query(x - 1) > tree.query(y)) {
            return true;
        }
        tree.update(x, 1);
    }
    return false;
}

function search(nums: number[], x: number): number {
    let l = 0,
        r = nums.length - 1;
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
