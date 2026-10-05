---
comments: true
difficulty: Hard
rating: 2288
source: Weekly Contest 499 Q4
tags:
    - Segment Tree
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3915. Maximum Sum of Alternating Subsequence With Distance at Least K](https://leetcode.com/problems/maximum-sum-of-alternating-subsequence-with-distance-at-least-k)

[中文文档](/solution/3900-3999/3915.Maximum%20Sum%20of%20Alternating%20Subsequence%20With%20Distance%20at%20Least%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Hãy chọn một <strong><span data-keyword="subsequence-sequence">dãy con</span></strong> có các chỉ số <code>0 &lt;= i<sub>1</sub> &lt; i<sub>2</sub> &lt; ... &lt; i<sub>m</sub> &lt; n</code> sao cho:</p>

<ul>
	<li>Với mọi <code>1 &lt;= t &lt; m</code>, <code>i<sub>t+1</sub> - i<sub>t</sub> &gt;= k</code>.</li>
	<li>Các giá trị được chọn tạo thành một dãy <strong>luân phiên nghiêm ngặt</strong>. Nói cách khác, một trong hai điều kiện sau phải đúng:
	<ul>
		<li><code>nums[i<sub>1</sub>] &lt; nums[i<sub>2</sub>] &gt; nums[i<sub>3</sub>] &lt; ...</code>, hoặc</li>
		<li><code>nums[i<sub>1</sub>] &gt; nums[i<sub>2</sub>] &lt; nums[i<sub>3</sub>] &gt; ...</code></li>
	</ul>
	</li>
</ul>

<p>Một <strong>dãy con</strong> có độ dài 1 cũng được xem là <strong>luân phiên nghiêm ngặt</strong>. Điểm của một dãy con <strong>hợp lệ</strong> là <strong>tổng</strong> các giá trị được chọn.</p>

<p>Hãy trả về một số nguyên biểu thị <strong>điểm</strong> <strong>lớn nhất</strong> có thể đạt được của một dãy con hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lựa chọn tối ưu là các chỉ số <code>[0, 2]</code>, cho các giá trị <code>[5, 2]</code>.</p>

<ul>
	<li>Điều kiện khoảng cách được thỏa mãn vì <code>2 - 0 = 2 &gt;= k</code>.</li>
	<li>Các giá trị luân phiên nghiêm ngặt vì <code>5 &gt; 2</code>.</li>
</ul>

<p>Điểm là <code>5 + 2 = 7</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,4,2,4], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lựa chọn tối ưu là các chỉ số <code>[0, 1, 3, 4]</code>, cho các giá trị <code>[3, 5, 2, 4]</code>.</p>

<ul>
	<li>Điều kiện khoảng cách được thỏa mãn vì hiệu giữa mỗi cặp chỉ số liên tiếp được chọn ít nhất là <code>k = 1</code>.</li>
	<li>Các giá trị luân phiên nghiêm ngặt vì <code>3 &lt; 5 &gt; 2 &lt; 4</code>.</li>
</ul>

<p>Điểm là <code>3 + 5 + 2 + 4 = 14</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con hợp lệ duy nhất là <code>[5]</code>. Dãy con có 1 phần tử luôn luân phiên nghiêm ngặt, nên điểm là 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Quy hoạch động cho dãy con nếu liệt kê mọi phần tử trước đó cách ít nhất $k$ sẽ có độ phức tạp $O(n^2)$ và không đáp ứng được với $n\le 10^5$. Các phép chuyển còn yêu cầu phần tử trước đó phải lớn hơn hoặc nhỏ hơn.
>
> Gọi $f[i][0]$ là dãy con luân phiên tốt nhất kết thúc tại $i$ ở vị trí trũng, và $f[i][1]$ là dãy ở vị trí đỉnh. Một vị trí trũng chỉ có thể nối sau một vị trí đỉnh lớn hơn, còn một vị trí đỉnh chỉ có thể nối sau một vị trí trũng nhỏ hơn; đồng thời các chỉ số phải cách nhau ít nhất $k$.
>
> Hai cây Fenwick lưu các giá trị lớn nhất trên prefix của $f[\cdot][0]$ và suffix của $f[\cdot][1]$ trên miền giá trị đã rời rạc hóa. Tại chỉ số $i$, trước tiên ta thêm trạng thái ở $i-k$, sau đó thực hiện truy vấn; vì vậy mọi phần tử trước được dùng trong phép chuyển đều cách ít nhất $k$.

<!-- thinking:end -->

**Định nghĩa trạng thái**

Gọi $f[i][0]$ là tổng lớn nhất của một dãy con hợp lệ kết thúc tại chỉ số $i$, trong đó phần tử cuối là một **vị trí trũng** (phần tử tiếp theo phải lớn hơn để duy trì tính luân phiên), và $f[i][1]$ là tổng lớn nhất trong đó phần tử cuối là một **vị trí đỉnh** (phần tử tiếp theo phải nhỏ hơn).

**Phép chuyển**

Khi chuyển trạng thái, ta liệt kê chỉ số trước đó $j$ thỏa mãn $j \leq i - k$:

- Trạng thái $f[i][0]$ (vị trí trũng): chuyển từ $f[j][1]$, yêu cầu $\text{nums}[j] > \text{nums}[i]$, tức là truy vấn giá trị lớn nhất của $f[\cdot][1]$ trên miền giá trị $(\text{nums}[i],\ +\infty)$:

$$f[i][0] = \text{nums}[i] + \max\!\left(0,\ \max_{\substack{j \leq i-k \\ \text{nums}[j] > \text{nums}[i]}} f[j][1]\right)$$

- Trạng thái $f[i][1]$ (vị trí đỉnh): chuyển từ $f[j][0]$, yêu cầu $\text{nums}[j] < \text{nums}[i]$, tức là truy vấn giá trị lớn nhất của $f[\cdot][0]$ trên miền giá trị $[1,\ \text{nums}[i]-1]$:

$$f[i][1] = \text{nums}[i] + \max\!\left(0,\ \max_{\substack{j \leq i-k \\ \text{nums}[j] < \text{nums}[i]}} f[j][0]\right)$$

Đáp án cuối cùng là $\max_{0 \leq i < n}\max(f[i][0],\ f[i][1])$.

**Tối ưu hóa**

Các phép chuyển cần những truy vấn giá trị lớn nhất động trên prefix/suffix của miền giá trị. Ta có thể duy trì chúng hiệu quả bằng hai **Binary Indexed Tree (BIT)**:

- BIT $\text{bit}_0$: được đánh chỉ số theo giá trị, duy trì giá trị lớn nhất trên prefix của $f[\cdot][0]$, dùng để truy vấn các trường hợp $\text{nums}[j] < \text{nums}[i]$.
- BIT $\text{bit}_1$: được đánh chỉ số bởi $m + 1 - \textit{rank}$ (đảo ngược, với $m$ là số lượng giá trị phân biệt), duy trì giá trị lớn nhất trên prefix của $f[\cdot][1]$, tương đương với giá trị lớn nhất trên suffix của miền giá trị, dùng để truy vấn các trường hợp $\text{nums}[j] > \text{nums}[i]$.

Để chỉ các chỉ số $j \leq i - k$ tham gia vào phép chuyển, khi xử lý chỉ số $i$, trước tiên ta thêm trạng thái của chỉ số $i - k$ vào các BIT, sau đó mới truy vấn.

Độ phức tạp thời gian là $O(n \log m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ là độ dài của mảng và $m$ là số lượng giá trị phân biệt.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, val: int) -> None:
        while x <= self.n:
            self.c[x] = max(self.c[x], val)
            x += x & -x

    def query(self, x: int) -> int:
        ans = 0
        while x > 0:
            ans = max(ans, self.c[x])
            x -= x & -x
        return ans


class Solution:
    def maxAlternatingSum(self, nums: List[int], k: int) -> int:
        vals = sorted(set(nums))
        m = len(vals)
        rank = {v: i + 1 for i, v in enumerate(vals)}
        bit0 = BinaryIndexedTree(m)
        bit1 = BinaryIndexedTree(m)
        n = len(nums)
        f = [[0, 0] for _ in range(n)]
        ans = 0
        for i, x in enumerate(nums):
            if i >= k:
                r = rank[nums[i - k]]
                bit0.update(r, f[i - k][0])
                bit1.update(m + 1 - r, f[i - k][1])
            r = rank[x]
            f[i][0] = x + bit1.query(m - r)
            f[i][1] = x + bit0.query(r - 1)
            ans = max(ans, f[i][0], f[i][1])
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private final int n;
    private final long[] c;

    BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new long[n + 1];
    }

    void update(int x, long val) {
        while (x <= n) {
            c[x] = Math.max(c[x], val);
            x += x & -x;
        }
    }

    long query(int x) {
        long ans = 0;
        while (x > 0) {
            ans = Math.max(ans, c[x]);
            x -= x & -x;
        }
        return ans;
    }
}

class Solution {
    public long maxAlternatingSum(int[] nums, int k) {
        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        int m = 0;
        for (int i = 0; i < sorted.length; ++i) {
            if (i == 0 || sorted[i] != sorted[i - 1]) {
                sorted[m++] = sorted[i];
            }
        }
        BinaryIndexedTree bit0 = new BinaryIndexedTree(m);
        BinaryIndexedTree bit1 = new BinaryIndexedTree(m);
        int n = nums.length;
        long[][] f = new long[n][2];
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            if (i >= k) {
                int r = rank(sorted, m, nums[i - k]);
                bit0.update(r, f[i - k][0]);
                bit1.update(m + 1 - r, f[i - k][1]);
            }
            int r = rank(sorted, m, nums[i]);
            f[i][0] = nums[i] + bit1.query(m - r);
            f[i][1] = nums[i] + bit0.query(r - 1);
            ans = Math.max(ans, Math.max(f[i][0], f[i][1]));
        }
        return ans;
    }

    private int rank(int[] sorted, int m, int x) {
        int l = 0, r = m;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (sorted[mid] >= x) {
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
    explicit BinaryIndexedTree(int n)
        : n(n)
        , c(n + 1) {}

    void update(int x, long long val) {
        while (x <= n) {
            c[x] = max(c[x], val);
            x += x & -x;
        }
    }

    long long query(int x) {
        long long ans = 0;
        while (x > 0) {
            ans = max(ans, c[x]);
            x -= x & -x;
        }
        return ans;
    }

private:
    int n;
    vector<long long> c;
};

class Solution {
public:
    long long maxAlternatingSum(vector<int>& nums, int k) {
        vector<int> sorted = nums;
        ranges::sort(sorted);
        sorted.erase(unique(sorted.begin(), sorted.end()), sorted.end());
        int m = sorted.size();
        auto rank = [&](int x) {
            return ranges::lower_bound(sorted, x) - sorted.begin() + 1;
        };
        BinaryIndexedTree bit0(m), bit1(m);
        int n = nums.size();
        vector<array<long long, 2>> f(n);
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            if (i >= k) {
                int r = rank(nums[i - k]);
                bit0.update(r, f[i - k][0]);
                bit1.update(m + 1 - r, f[i - k][1]);
            }
            int r = rank(nums[i]);
            f[i][0] = nums[i] + bit1.query(m - r);
            f[i][1] = nums[i] + bit0.query(r - 1);
            ans = max({ans, f[i][0], f[i][1]});
        }
        return ans;
    }
};
```

#### Go

```go
type fenwick []int64

func (f fenwick) update(i int, val int64) {
	for ; i < len(f); i += i & -i {
		f[i] = max(f[i], val)
	}
}

func (f fenwick) query(i int) (res int64) {
	for ; i > 0; i &= i - 1 {
		res = max(res, f[i])
	}
	return
}

func maxAlternatingSum(nums []int, k int) (ans int64) {
	sorted := slices.Clone(nums)
	slices.Sort(sorted)
	sorted = slices.Compact(sorted)
	m := len(sorted)
	bit0 := make(fenwick, m+1)
	bit1 := make(fenwick, m+1)
	n := len(nums)
	f := make([][2]int64, n)
	for i, x := range nums {
		if i >= k {
			r := sort.SearchInts(sorted, nums[i-k]) + 1
			bit0.update(r, f[i-k][0])
			bit1.update(m+1-r, f[i-k][1])
		}
		r := sort.SearchInts(sorted, x) + 1
		f[i][0] = int64(x) + bit1.query(m-r)
		f[i][1] = int64(x) + bit0.query(r-1)
		ans = max(ans, f[i][0], f[i][1])
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
