---
comments: true
difficulty: Hard
rating: 2089
source: Biweekly Contest 169 Q4
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Divide and Conquer
    - Prefix Sum
    - Merge Sort
---

<!-- problem:start -->

# [3739. Count Subarrays With Majority Element II](https://leetcode.com/problems/count-subarrays-with-majority-element-ii)

[中文文档](/solution/3700-3799/3739.Count%20Subarrays%20With%20Majority%20Element%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>target</code>.</p>

<p>Trả về số lượng <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> của <code>nums</code> mà <code>target</code> là <strong>phần tử chiếm đa số</strong>.</p>

<p><strong>Phần tử chiếm đa số</strong> của một mảng con là phần tử xuất hiện <strong>nhiều hơn một nửa</strong> số lần trong mảng con đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,3], target = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ có <code>target = 2</code> là phần tử chiếm đa số:</p>

<ul>
	<li><code>nums[1..1] = [2]</code></li>
	<li><code>nums[2..2] = [2]</code></li>
	<li><code>nums[1..2] = [2,2]</code></li>
	<li><code>nums[0..2] = [1,2,2]</code></li>
	<li><code>nums[1..3] = [2,2,3]</code></li>
</ul>

<p>Vậy có 5 mảng con như vậy.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1], target = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích: </strong></p>

<p><strong>​​​​​​​</strong>Tất cả 10 mảng con đều có 1 là phần tử chiếm đa số.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], target = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>target = 4</code> hoàn toàn không xuất hiện trong <code>nums</code>. Do đó, không có mảng con nào mà 4 là phần tử chiếm đa số. Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>​​​​​​​5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​9</sup></code></li>
	<li><code>1 &lt;= target &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Cách đếm theo bậc hai của bài trước không thể mở rộng. Ta ánh xạ $\textit{target}$ thành $+1$ và mọi giá trị khác thành $-1$, khi đó một phần tử chiếm đa số tương đương với tổng mảng con lớn hơn $0$. Với mỗi điểm kết thúc bên phải, ta cần đếm số tổng tiền tố nhỏ hơn, và Fenwick tree có thể duy trì thông tin này trên miền đã dịch $[-n,n]$.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể coi các phần tử trong mảng bằng $\textit{target}$ là $1$, còn các phần tử khác $\textit{target}$ là $-1$. Khi đó, việc $\textit{target}$ là phần tử chiếm đa số trong một mảng con tương đương với việc số lượng số $1$ trong mảng con lớn hơn số lượng số $-1$, hay tổng của mảng con lớn hơn $0$.

Ta có thể liệt kê các mảng con kết thúc tại từng vị trí. Gọi tổng tiền tố tại vị trí hiện tại là $\textit{s}$. Khi đó, số mảng con kết thúc tại vị trí này có tổng lớn hơn $0$ tương đương với số lượng tổng tiền tố nhỏ hơn $\textit{s}$. Ta có thể dùng Binary Indexed Tree để duy trì số lần xuất hiện của các tổng tiền tố, từ đó tính đáp án một cách hiệu quả. Miền giá trị của tổng tiền tố là $[-n, n]$. Ta có thể dịch tất cả tổng tiền tố sang phải $n+1$ đơn vị để biến miền này thành $[1, 2n+1]$.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

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
    def countMajoritySubarrays(self, nums: List[int], target: int) -> int:
        n = len(nums)
        tree = BinaryIndexedTree(n * 2 + 1)
        s = n + 1
        tree.update(s, 1)
        ans = 0
        for x in nums:
            s += 1 if x == target else -1
            ans += tree.query(s - 1)
            tree.update(s, 1)
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
    public long countMajoritySubarrays(int[] nums, int target) {
        int n = nums.length;
        BinaryIndexedTree tree = new BinaryIndexedTree(2 * n + 1);
        int s = n + 1;
        tree.update(s, 1);
        long ans = 0;
        for (int x : nums) {
            s += x == target ? 1 : -1;
            ans += tree.query(s - 1);
            tree.update(s, 1);
        }
        return ans;
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
        , c(n + 1, 0) {}

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
    long long countMajoritySubarrays(vector<int>& nums, int target) {
        int n = nums.size();
        BinaryIndexedTree tree(2 * n + 1);
        int s = n + 1;
        tree.update(s, 1);
        long long ans = 0;
        for (int x : nums) {
            s += (x == target ? 1 : -1);
            ans += tree.query(s - 1);
            tree.update(s, 1);
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
	return &BinaryIndexedTree{
		n: n,
		c: make([]int, n+1),
	}
}

func (t *BinaryIndexedTree) update(x, delta int) {
	for x <= t.n {
		t.c[x] += delta
		x += x & -x
	}
}

func (t *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += t.c[x]
		x -= x & -x
	}
	return s
}

func countMajoritySubarrays(nums []int, target int) int64 {
	n := len(nums)
	tree := NewBinaryIndexedTree(2*n + 1)
	s := n + 1
	tree.update(s, 1)
	var ans int64
	for _, x := range nums {
		if x == target {
			s++
		} else {
			s--
		}
		ans += int64(tree.query(s - 1))
		tree.update(s, 1)
	}
	return ans
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

function countMajoritySubarrays(nums: number[], target: number): number {
    const n = nums.length;
    const tree = new BinaryIndexedTree(2 * n + 1);
    let s = n + 1;
    tree.update(s, 1);
    let ans = 0;
    for (const x of nums) {
        s += x === target ? 1 : -1;
        ans += tree.query(s - 1);
        tree.update(s, 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
