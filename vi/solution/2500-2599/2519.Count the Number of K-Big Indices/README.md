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
    - Merge Sort
---

<!-- problem:start -->

# [2519. Count the Number of K-Big Indices 🔒](https://leetcode.com/problems/count-the-number-of-k-big-indices)

[中文文档](/solution/2500-2599/2519.Count%20the%20Number%20of%20K-Big%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong> và một số nguyên dương <code>k</code>.</p>

<p>Một chỉ số <code>i</code> được gọi là <strong>k-big</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Tồn tại ít nhất <code>k</code> chỉ số khác nhau <code>idx1</code> sao cho <code>idx1 &lt; i</code> và <code>nums[idx1] &lt; nums[i]</code>.</li>
	<li>Tồn tại ít nhất <code>k</code> chỉ số khác nhau <code>idx2</code> sao cho <code>idx2 &gt; i</code> và <code>nums[idx2] &lt; nums[i]</code>.</li>
</ul>

<p>Trả về <em>số lượng chỉ số k-big</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,6,5,2,3], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong nums chỉ có hai chỉ số 2-big:
- i = 2 --&gt; Có hai idx1 hợp lệ: 0 và 1. Có ba idx2 hợp lệ: 2, 3 và 4.
- i = 3 --&gt; Có hai idx1 hợp lệ: 0 và 1. Có hai idx2 hợp lệ: 3 và 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1], k = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chỉ số 3-big nào trong nums.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số $i$ là $k$-big khi và chỉ khi có ít nhất $k$ giá trị nhỏ hơn nó nằm bên trái, đồng thời điều kiện này cũng đúng ở bên phải. Duyệt cả hai phía cho từng $i$ sẽ có độ phức tạp bậc hai khi $n\le 10^5$.
>
> Có thể xem các giá trị là nằm trong khoảng không vượt quá $n$. Hai cây Fenwick lưu tần suất. Trước tiên, chèn mọi giá trị vào cây bên phải; khi duyệt từ trái sang phải, xóa giá trị hiện tại khỏi cây bên phải, truy vấn số giá trị nhỏ hơn $<v$ ở mỗi phía, rồi chèn $v$ vào cây bên trái. Mỗi chỉ số tốn $O(\log n)$.

<!-- thinking:end -->

Ta duy trì hai binary indexed tree, một cây lưu số phần tử nhỏ hơn phần tử tại vị trí hiện tại ở bên trái, cây còn lại lưu số phần tử nhỏ hơn phần tử tại vị trí hiện tại ở bên phải.

Ta duyệt qua mảng. Với vị trí hiện tại, nếu số phần tử nhỏ hơn phần tử tại vị trí hiện tại ở bên trái lớn hơn hoặc bằng $k$, đồng thời số phần tử nhỏ hơn phần tử tại vị trí hiện tại ở bên phải cũng lớn hơn hoặc bằng $k$, thì vị trí hiện tại là `k-big`, và ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng.

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
    def kBigIndices(self, nums: List[int], k: int) -> int:
        n = len(nums)
        tree1 = BinaryIndexedTree(n)
        tree2 = BinaryIndexedTree(n)
        for v in nums:
            tree2.update(v, 1)
        ans = 0
        for v in nums:
            tree2.update(v, -1)
            ans += tree1.query(v - 1) >= k and tree2.query(v - 1) >= k
            tree1.update(v, 1)
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

    public void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
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
    public int kBigIndices(int[] nums, int k) {
        int n = nums.length;
        BinaryIndexedTree tree1 = new BinaryIndexedTree(n);
        BinaryIndexedTree tree2 = new BinaryIndexedTree(n);
        for (int v : nums) {
            tree2.update(v, 1);
        }
        int ans = 0;
        for (int v : nums) {
            tree2.update(v, -1);
            if (tree1.query(v - 1) >= k && tree2.query(v - 1) >= k) {
                ++ans;
            }
            tree1.update(v, 1);
        }
        return ans;
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

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
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
    int kBigIndices(vector<int>& nums, int k) {
        int n = nums.size();
        BinaryIndexedTree* tree1 = new BinaryIndexedTree(n);
        BinaryIndexedTree* tree2 = new BinaryIndexedTree(n);
        for (int& v : nums) {
            tree2->update(v, 1);
        }
        int ans = 0;
        for (int& v : nums) {
            tree2->update(v, -1);
            ans += tree1->query(v - 1) >= k && tree2->query(v - 1) >= k;
            tree1->update(v, 1);
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

func kBigIndices(nums []int, k int) (ans int) {
	n := len(nums)
	tree1 := newBinaryIndexedTree(n)
	tree2 := newBinaryIndexedTree(n)
	for _, v := range nums {
		tree2.update(v, 1)
	}
	for _, v := range nums {
		tree2.update(v, -1)
		if tree1.query(v-1) >= k && tree2.query(v-1) >= k {
			ans++
		}
		tree1.update(v, 1)
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
        this.c = new Array(n + 1).fill(0);
    }

    update(x: number, delta: number): void {
        while (x <= this.n) {
            this.c[x] += delta;
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

function kBigIndices(nums: number[], k: number): number {
    const n = Math.max(...nums);
    const tree1 = new BinaryIndexedTree(n);
    const tree2 = new BinaryIndexedTree(n);

    for (const v of nums) {
        tree2.update(v, 1);
    }

    let ans = 0;
    for (const v of nums) {
        tree2.update(v, -1);
        if (tree1.query(v - 1) >= k && tree2.query(v - 1) >= k) {
            ans++;
        }
        tree1.update(v, 1);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
