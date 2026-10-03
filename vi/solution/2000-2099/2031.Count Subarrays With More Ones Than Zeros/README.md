---
comments: true
difficulty: Medium
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Hash Table
    - Binary Search
    - Divide and Conquer
    - Ordered Set
    - Merge Sort
---

<!-- problem:start -->

# [2031. Count Subarrays With More Ones Than Zeros 🔒](https://leetcode.com/problems/count-subarrays-with-more-ones-than-zeros)

[中文文档](/solution/2000-2099/2031.Count%20Subarrays%20With%20More%20Ones%20Than%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng nhị phân <code>nums</code> chỉ chứa các số nguyên <code>0</code> và <code>1</code>. Hãy trả về <em>số lượng <strong>mảng con</strong> trong nums có <strong>nhiều hơn</strong> </em><code>1</code>&#39;<em> so với </em><code>0</code><em>&#39;s. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> </em><code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,1,0,1]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Các mảng con kích thước 1 có nhiều số 1 hơn số 0 là: [1], [1], [1]
Các mảng con kích thước 2 có nhiều số 1 hơn số 0 là: [1,1]
Các mảng con kích thước 3 có nhiều số 1 hơn số 0 là: [0,1,1], [1,1,0], [1,0,1]
Các mảng con kích thước 4 có nhiều số 1 hơn số 0 là: [1,1,0,1]
Các mảng con kích thước 5 có nhiều số 1 hơn số 0 là: [0,1,1,0,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có mảng con nào có nhiều số 1 hơn số 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Mảng con kích thước 1 có nhiều số 1 hơn số 0 là: [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Xem $0$ là $-1$, một mảng con có nhiều số 1 hơn số 0 khi và chỉ khi tổng tiền tố tăng nghiêm ngặt. Với mỗi đầu phải, ta đếm các tổng tiền tố trước đó nhỏ hơn tổng hiện tại $s$. Việc này cần một cấu trúc có thể xử lý trong thời gian logarit.
>
> Các tổng tiền tố nằm trong $[-n,n]$; dịch chỉ số thêm $n+1$ cho phép Fenwick tree lưu tần suất. Thêm $0$ trước, query rồi update, lấy modulo $10^9+7$.

<!-- thinking:end -->

Bài toán yêu cầu đếm số mảng con trong đó số lượng $1$ lớn hơn số lượng $0$. Nếu coi $0$ trong mảng là $-1$, bài toán trở thành đếm số mảng con có tổng các phần tử lớn hơn $0$.

Để tính tổng các phần tử trong một mảng con, ta có thể sử dụng tổng tiền tố. Để đếm số mảng con có tổng các phần tử lớn hơn $0$, ta có thể sử dụng Binary Indexed Tree để duy trì số lần xuất hiện của mỗi tổng tiền tố. Ban đầu, số lần xuất hiện của tổng tiền tố $0$ là $1$.

Tiếp theo, ta duyệt mảng $nums$, dùng biến $s$ để ghi lại tổng tiền tố hiện tại và biến $ans$ để ghi lại đáp án. Với mỗi vị trí $i$, ta cập nhật tổng tiền tố $s$, sau đó truy vấn số lần xuất hiện của các tổng tiền tố trong khoảng $[0, s)$ trên Binary Indexed Tree, cộng kết quả vào $ans$, rồi cập nhật số lần xuất hiện của $s$ trên Binary Indexed Tree.

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = ["n", "c"]

    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] += v
            x += x & -x

    def query(self, x: int) -> int:
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def subarraysWithMoreZerosThanOnes(self, nums: List[int]) -> int:
        n = len(nums)
        base = n + 1
        tree = BinaryIndexedTree(n + base)
        tree.update(base, 1)
        mod = 10**9 + 7
        ans = s = 0
        for x in nums:
            s += x or -1
            ans += tree.query(s - 1 + base)
            ans %= mod
            tree.update(s + base, 1)
        return ans
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
        for (; x <= n; x += x & -x) {
            c[x] += v;
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
    public int subarraysWithMoreZerosThanOnes(int[] nums) {
        int n = nums.length;
        int base = n + 1;
        BinaryIndexedTree tree = new BinaryIndexedTree(n + base);
        tree.update(base, 1);
        final int mod = (int) 1e9 + 7;
        int ans = 0, s = 0;
        for (int x : nums) {
            s += x == 0 ? -1 : 1;
            ans += tree.query(s - 1 + base);
            ans %= mod;
            tree.update(s + base, 1);
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

    void update(int x, int v) {
        for (; x <= n; x += x & -x) {
            c[x] += v;
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
    int subarraysWithMoreZerosThanOnes(vector<int>& nums) {
        int n = nums.size();
        int base = n + 1;
        BinaryIndexedTree tree(n + base);
        tree.update(base, 1);
        const int mod = 1e9 + 7;
        int ans = 0, s = 0;
        for (int x : nums) {
            s += (x == 0) ? -1 : 1;
            ans += tree.query(s - 1 + base);
            ans %= mod;
            tree.update(s + base, 1);
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
	return &BinaryIndexedTree{n: n, c: make([]int, n+1)}
}

func (bit *BinaryIndexedTree) update(x, v int) {
	for ; x <= bit.n; x += x & -x {
		bit.c[x] += v
	}
}

func (bit *BinaryIndexedTree) query(x int) (s int) {
	for ; x > 0; x -= x & -x {
		s += bit.c[x]
	}
	return
}

func subarraysWithMoreZerosThanOnes(nums []int) (ans int) {
	n := len(nums)
	base := n + 1
	tree := newBinaryIndexedTree(n + base)
	tree.update(base, 1)
	const mod = int(1e9) + 7
	s := 0
	for _, x := range nums {
		if x == 0 {
			s--
		} else {
			s++
		}
		ans += tree.query(s - 1 + base)
		ans %= mod
		tree.update(s+base, 1)
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

    update(x: number, v: number): void {
        for (; x <= this.n; x += x & -x) {
            this.c[x] += v;
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

function subarraysWithMoreZerosThanOnes(nums: number[]): number {
    const n: number = nums.length;
    const base: number = n + 1;
    const tree: BinaryIndexedTree = new BinaryIndexedTree(n + base);
    tree.update(base, 1);
    const mod: number = 1e9 + 7;
    let ans: number = 0;
    let s: number = 0;
    for (const x of nums) {
        s += x || -1;
        ans += tree.query(s - 1 + base);
        ans %= mod;
        tree.update(s + base, 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tổng tiền tố + Tập hợp có thứ tự

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cần dịch chỉ số cho Fenwick tree. Một danh sách đã sắp xếp có thể tìm kiếm nhị phân trên tổng tiền tố gốc: `bisect_left(s)` chính là số lượng tổng trước đó nhỏ hơn nghiêm ngặt.
>
> Công thức truy hồi vẫn giữ nguyên; một multiset có thứ tự thay thế cho tree.

<!-- thinking:end -->

Coi $0$ là $-1$, lưu các tổng tiền tố trong một danh sách đã sắp xếp, rồi tìm kiếm nhị phân số lượng tổng trước đó nhỏ hơn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarraysWithMoreZerosThanOnes(self, nums: List[int]) -> int:
        sl = SortedList([0])
        mod = 10**9 + 7
        ans = s = 0
        for x in nums:
            s += x or -1
            ans += sl.bisect_left(s)
            ans %= mod
            sl.add(s)
        return ans
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
