---
comments: true
difficulty: Hard
rating: 2448
source: Weekly Contest 370 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [2926. Maximum Balanced Subsequence Sum](https://leetcode.com/problems/maximum-balanced-subsequence-sum)

[中文文档](/solution/2900-2999/2926.Maximum%20Balanced%20Subsequence%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p>Một <strong>dãy con</strong> của <code>nums</code> có độ dài <code>k</code> và gồm các <strong>chỉ số</strong> <code>i<sub>0</sub>&nbsp;&lt;&nbsp;i<sub>1</sub> &lt;&nbsp;... &lt; i<sub>k-1</sub></code> được gọi là <strong>cân bằng</strong> nếu thỏa mãn điều kiện sau:</p>

<ul>
	<li><code>nums[i<sub>j</sub>] - nums[i<sub>j-1</sub>] &gt;= i<sub>j</sub> - i<sub>j-1</sub></code>, với mọi <code>j</code> trong khoảng <code>[1, k - 1]</code>.</li>
</ul>

<p>Một <strong>dãy con</strong> của <code>nums</code> có độ dài <code>1</code> được coi là cân bằng.</p>

<p>Trả về <em>một số nguyên biểu thị <strong>tổng</strong> <strong>lớn nhất</strong> có thể có của các phần tử trong một dãy con <strong>cân bằng</strong> của </em><code>nums</code>.</p>

<p><strong>Dãy con</strong> của một mảng là một mảng mới <strong>không rỗng</strong>, được tạo ra từ mảng ban đầu bằng cách xóa một số phần tử (<strong>có thể không xóa phần tử nào</strong>) mà không làm thay đổi thứ tự tương đối của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,5,6]
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Trong ví dụ này, có thể chọn dãy con [3,5,6] gồm các chỉ số 0, 2 và 3.
nums[2] - nums[0] &gt;= 2 - 0.
nums[3] - nums[2] &gt;= 3 - 2.
Do đó, đây là một dãy con cân bằng và tổng của nó là lớn nhất trong các dãy con cân bằng của nums.
Dãy con gồm các chỉ số 1, 2 và 3 cũng hợp lệ.
Có thể chứng minh rằng không thể tạo ra dãy con cân bằng có tổng lớn hơn 14.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,-1,-3,8]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Trong ví dụ này, có thể chọn dãy con [5,8] gồm các chỉ số 0 và 3.
nums[3] - nums[0] &gt;= 3 - 0.
Do đó, đây là một dãy con cân bằng và tổng của nó là lớn nhất trong các dãy con cân bằng của nums.
Có thể chứng minh rằng không thể tạo ra dãy con cân bằng có tổng lớn hơn 13.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-2,-1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Trong ví dụ này, có thể chọn dãy con [-1].
Đây là một dãy con cân bằng và tổng của nó là lớn nhất trong các dãy con cân bằng của nums.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện cân bằng $nums[i]-nums[j] \ge i-j$ có thể viết lại thành $nums[i]-i \ge nums[j]-j$. Với $arr[t]=nums[t]-t$, ta cần tìm một dãy chỉ số không giảm, sao cho tổng các giá trị tương ứng trong $nums$ là lớn nhất. Công thức ngây thơ $f[i]=nums[i]+\max_{j<i, arr[j]\le arr[i]} f[j]$ (hoặc chỉ là $nums[i]$) có độ phức tạp $O(n^2)$, không phù hợp với $n \le 10^5$.
>
> Sau khi nén tọa độ $arr$, một Fenwick tree lưu giá trị $f$ tốt nhất trên các giá trị không vượt quá một ngưỡng. Ta duyệt theo thứ tự chỉ số, thực hiện query rồi insert; cuối cùng lấy maximum trên prefix để có đáp án.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể biến đổi bất đẳng thức $nums[i] - nums[j] \ge i - j$ thành $nums[i] - i \ge nums[j] - j$. Vì vậy, ta định nghĩa một mảng mới $arr$, trong đó $arr[i] = nums[i] - i$. Một dãy con cân bằng thỏa mãn $arr[j] \le arr[i]$ với mọi $j < i$. Bài toán được chuyển thành chọn một dãy con tăng trong $arr$ sao cho tổng tương ứng trong $nums$ là lớn nhất.

Giả sử $i$ là chỉ số của phần tử cuối cùng trong dãy con, khi đó ta xét chỉ số $j$ của phần tử ngay trước nó. Nếu $arr[j] \le arr[i]$, ta có thể cân nhắc thêm $j$ vào dãy con.

Do đó, ta định nghĩa $f[i]$ là tổng lớn nhất của các phần tử trong $nums$ khi chỉ số của phần tử cuối cùng trong dãy con là $i$. Đáp án là $\max_{i=0}^{n-1} f[i]$.

Công thức chuyển trạng thái là:

$$
f[i] = \max(\max_{j=0}^{i-1} f[j], 0) + nums[i]
$$

trong đó $j$ thỏa mãn $arr[j] \le arr[i]$.

Ta có thể dùng Binary Indexed Tree để duy trì giá trị lớn nhất trên prefix, tức là với mỗi $arr[i]$, ta duy trì giá trị $f[i]$ lớn nhất trong prefix $arr[0..i]$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n: int):
        self.n = n
        self.c = [-inf] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = max(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        mx = -inf
        while x:
            mx = max(mx, self.c[x])
            x -= x & -x
        return mx


class Solution:
    def maxBalancedSubsequenceSum(self, nums: List[int]) -> int:
        arr = [x - i for i, x in enumerate(nums)]
        s = sorted(set(arr))
        tree = BinaryIndexedTree(len(s))
        for i, x in enumerate(nums):
            j = bisect_left(s, x - i) + 1
            v = max(tree.query(j), 0) + x
            tree.update(j, v)
        return tree.query(len(s))
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private long[] c;
    private final long inf = 1L << 60;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new long[n + 1];
        Arrays.fill(c, -inf);
    }

    public void update(int x, long v) {
        while (x <= n) {
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public long query(int x) {
        long mx = -inf;
        while (x > 0) {
            mx = Math.max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

class Solution {
    public long maxBalancedSubsequenceSum(int[] nums) {
        int n = nums.length;
        int[] arr = new int[n];
        for (int i = 0; i < n; ++i) {
            arr[i] = nums[i] - i;
        }
        Arrays.sort(arr);
        int m = 0;
        for (int i = 0; i < n; ++i) {
            if (i == 0 || arr[i] != arr[i - 1]) {
                arr[m++] = arr[i];
            }
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        for (int i = 0; i < n; ++i) {
            int j = search(arr, nums[i] - i, m) + 1;
            long v = Math.max(tree.query(j), 0) + nums[i];
            tree.update(j, v);
        }
        return tree.query(m);
    }

    private int search(int[] nums, int x, int r) {
        int l = 0;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int n;
    vector<long long> c;
    const long long inf = 1e18;

public:
    BinaryIndexedTree(int n) {
        this->n = n;
        c.resize(n + 1, -inf);
    }

    void update(int x, long long v) {
        while (x <= n) {
            c[x] = max(c[x], v);
            x += x & -x;
        }
    }

    long long query(int x) {
        long long mx = -inf;
        while (x > 0) {
            mx = max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
};

class Solution {
public:
    long long maxBalancedSubsequenceSum(vector<int>& nums) {
        int n = nums.size();
        vector<int> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = nums[i] - i;
        }
        sort(arr.begin(), arr.end());
        arr.erase(unique(arr.begin(), arr.end()), arr.end());
        int m = arr.size();
        BinaryIndexedTree tree(m);
        for (int i = 0; i < n; ++i) {
            int j = lower_bound(arr.begin(), arr.end(), nums[i] - i) - arr.begin() + 1;
            long long v = max(tree.query(j), 0LL) + nums[i];
            tree.update(j, v);
        }
        return tree.query(m);
    }
};
```

#### Go

```go
const inf int = 1e18

type BinaryIndexedTree struct {
	n int
	c []int
}

func NewBinaryIndexedTree(n int) BinaryIndexedTree {
	c := make([]int, n+1)
	for i := range c {
		c[i] = -inf
	}
	return BinaryIndexedTree{n: n, c: c}
}

func (bit *BinaryIndexedTree) update(x, v int) {
	for x <= bit.n {
		bit.c[x] = max(bit.c[x], v)
		x += x & -x
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	mx := -inf
	for x > 0 {
		mx = max(mx, bit.c[x])
		x -= x & -x
	}
	return mx
}

func maxBalancedSubsequenceSum(nums []int) int64 {
	n := len(nums)
	arr := make([]int, n)
	for i, x := range nums {
		arr[i] = x - i
	}
	sort.Ints(arr)
	m := 0
	for i, x := range arr {
		if i == 0 || x != arr[i-1] {
			arr[m] = x
			m++
		}
	}
	arr = arr[:m]
	tree := NewBinaryIndexedTree(m)
	for i, x := range nums {
		j := sort.SearchInts(arr, x-i) + 1
		v := max(tree.query(j), 0) + x
		tree.update(j, v)
	}
	return int64(tree.query(m))
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(-Infinity);
    }

    update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] = Math.max(this.c[x], v);
            x += x & -x;
        }
    }

    query(x: number): number {
        let mx = -Infinity;
        while (x > 0) {
            mx = Math.max(mx, this.c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

function maxBalancedSubsequenceSum(nums: number[]): number {
    const n = nums.length;
    const arr = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        arr[i] = nums[i] - i;
    }
    arr.sort((a, b) => a - b);
    let m = 0;
    for (let i = 0; i < n; ++i) {
        if (i === 0 || arr[i] !== arr[i - 1]) {
            arr[m++] = arr[i];
        }
    }
    arr.length = m;
    const tree = new BinaryIndexedTree(m);
    const search = (nums: number[], x: number): number => {
        let [l, r] = [0, nums.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (let i = 0; i < n; ++i) {
        const j = search(arr, nums[i] - i) + 1;
        const v = Math.max(tree.query(j), 0) + nums[i];
        tree.update(j, v);
    }
    return tree.query(m);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
