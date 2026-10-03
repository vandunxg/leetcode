---
comments: true
difficulty: Hard
rating: 2030
source: Biweekly Contest 88 Q4
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

# [2426. Number of Pairs Satisfying Inequality](https://leetcode.com/problems/number-of-pairs-satisfying-inequality)

[中文文档](/solution/2400-2499/2426.Number%20of%20Pairs%20Satisfying%20Inequality/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp hai mảng số nguyên <code>nums1</code> và <code>nums2</code> được đánh chỉ số từ <strong>0</strong>, mỗi mảng có kích thước <code>n</code>, cùng một số nguyên <code>diff</code>. Hãy tìm số lượng <strong>cặp</strong> <code>(i, j)</code> sao cho:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt;= n - 1</code> <strong>và</strong></li>
	<li><code>nums1[i] - nums1[j] &lt;= nums2[i] - nums2[j] + diff</code>.</li>
</ul>

<p>Trả về <em><strong>số lượng cặp</strong> thỏa mãn các điều kiện.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [3,2,5], nums2 = [2,2,1], diff = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Có 3 cặp thỏa mãn các điều kiện:
1. i = 0, j = 1: 3 - 2 &lt;= 2 - 2 + 1. Vì i &lt; j và 1 &lt;= 1, cặp này thỏa mãn các điều kiện.
2. i = 0, j = 2: 3 - 5 &lt;= 2 - 1 + 1. Vì i &lt; j và -2 &lt;= 2, cặp này thỏa mãn các điều kiện.
3. i = 1, j = 2: 2 - 5 &lt;= 2 - 1 + 1. Vì i &lt; j và -3 &lt;= 2, cặp này thỏa mãn các điều kiện.
Do đó, ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [3,-1], nums2 = [-2,2], diff = -1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không tồn tại cặp nào thỏa mãn các điều kiện, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums1[i], nums2[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= diff &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Với $i<j$, không thể dùng hai vòng lặp để xét bất đẳng thức $nums1[i]-nums2[i]\le nums1[j]-nums2[j]+\textit{diff}$ khi $n\le 10^5$. Đặt $v=a-b$, ta cần đếm các giá trị trước đó thỏa mãn $v_i\le v_j+\textit{diff}$.
>
> Sau khi dịch chỉ số, Fenwick tree lưu các giá trị $v$ đã gặp. Với mỗi $j$ từ trái sang phải, ta truy vấn prefix đến $v_j+\textit{diff}$, sau đó thêm $v_j$ vào cây.

<!-- thinking:end -->

Ta có thể biến đổi bất đẳng thức trong đề bài thành $nums1[i] - nums2[i] \leq nums1[j] - nums2[j] + diff$. Vì vậy, nếu tính hiệu giữa các phần tử tương ứng của hai mảng và thu được một mảng khác $nums$, bài toán trở thành đếm số cặp trong $nums$ thỏa mãn $nums[i] \leq nums[j] + diff$.

Ta có thể duyệt $j$ từ nhỏ đến lớn, tìm xem có bao nhiêu số đứng trước nó thỏa mãn $nums[i] \leq nums[j] + diff$, từ đó tính được số lượng cặp. Ta có thể sử dụng Binary Indexed Tree để duy trì prefix sum, nhờ đó tìm được số lượng số đứng trước thỏa mãn $nums[i] \leq nums[j] + diff$ trong thời gian $O(\log n)$.

Độ phức tạp thời gian là $O(n \times \log n)$.

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
        while x:
            s += self.c[x]
            x -= BinaryIndexedTree.lowbit(x)
        return s


class Solution:
    def numberOfPairs(self, nums1: List[int], nums2: List[int], diff: int) -> int:
        tree = BinaryIndexedTree(10**5)
        ans = 0
        for a, b in zip(nums1, nums2):
            v = a - b
            ans += tree.query(v + diff + 40000)
            tree.update(v + 40000, 1)
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

    public static final int lowbit(int x) {
        return x & -x;
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
}

class Solution {
    public long numberOfPairs(int[] nums1, int[] nums2, int diff) {
        BinaryIndexedTree tree = new BinaryIndexedTree(100000);
        long ans = 0;
        for (int i = 0; i < nums1.length; ++i) {
            int v = nums1[i] - nums2[i];
            ans += tree.query(v + diff + 40000);
            tree.update(v + 40000, 1);
        }
        return ans;
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
    long long numberOfPairs(vector<int>& nums1, vector<int>& nums2, int diff) {
        BinaryIndexedTree* tree = new BinaryIndexedTree(1e5);
        long long ans = 0;
        for (int i = 0; i < nums1.size(); ++i) {
            int v = nums1[i] - nums2[i];
            ans += tree->query(v + diff + 40000);
            tree->update(v + 40000, 1);
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

func numberOfPairs(nums1 []int, nums2 []int, diff int) int64 {
	tree := newBinaryIndexedTree(100000)
	ans := 0
	for i := range nums1 {
		v := nums1[i] - nums2[i]
		ans += tree.query(v + diff + 40000)
		tree.update(v+40000, 1)
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
