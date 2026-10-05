---
comments: true
difficulty: Hard
rating: 2139
source: Biweekly Contest 191 Q4
---

<!-- problem:start -->

# [4051. Count Subarrays with Distant Sums](https://leetcode.com/problems/count-subarrays-with-distant-sums)

[中文文档](/solution/4000-4099/4051.Count%20Subarrays%20with%20Distant%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>goal</code>, <code>k</code>.</p>

<p>Một <strong>mảng con</strong> <code>nums[i..j]</code> được xem là <strong>cách xa</strong> nếu <strong>chênh lệch tuyệt đối</strong> giữa tổng của nó và <code>goal</code> <strong>ít nhất</strong> bằng <code>k</code>.</p>

<p>Trả về số lượng <strong>mảng con cách xa</strong>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp và <strong>không rỗng</strong> nằm trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1], goal = 4, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con cách xa với <code>k = 1</code> là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>j</code></th>
			<th style="border: 1px solid black;"><code>nums[i..j]</code></th>
			<th style="border: 1px solid black;">Tổng</th>
			<th style="border: 1px solid black;"><code>abs(sum - goal)</code></th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[2]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>[2, 1]</code></td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, đáp án là 5.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,-1,3], goal = 2, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con cách xa với <code>k = 2</code> là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>j</code></th>
			<th style="border: 1px solid black;"><code>nums[i..j]</code></th>
			<th style="border: 1px solid black;">Tổng</th>
			<th style="border: 1px solid black;"><code>abs(sum - goal)</code></th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[-1]</code></td>
			<td style="border: 1px solid black;">-1</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>[2, -1, 3]</code></td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-3,1,2], goal = 0, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con cách xa với <code>k = 3</code> là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>j</code></th>
			<th style="border: 1px solid black;"><code>nums[i..j]</code></th>
			<th style="border: 1px solid black;">Tổng</th>
			<th style="border: 1px solid black;"><code>abs(sum - goal)</code></th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[-3]</code></td>
			<td style="border: 1px solid black;">-3</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, đáp án là 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= goal &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Có số lượng mảng con tăng theo bậc hai, nên với $n = 10^5$ ta không thể liệt kê tất cả. Phần bù của điều kiện $|sum - \textit{goal}| \ge k$ là $|sum - \textit{goal}| < k$; đếm phần bù rồi lấy tổng số mảng con trừ đi sẽ đơn giản hơn.
>
> Tổng tiền tố biến tổng của một mảng con thành hiệu của hai điểm. Với mỗi điểm kết thúc bên phải, ta cần biết có bao nhiêu tổng tiền tố trước đó nằm trong một khoảng số.
>
> Sau khi sắp xếp các tổng tiền tố, ta tìm kiếm nhị phân các chỉ số trong Fenwick tree, truy vấn khoảng rồi thêm giá trị hiện tại.

<!-- thinking:end -->

Gọi $s$ là mảng tổng tiền tố của $\textit{nums}$ ($s[0] = 0$). Tổng của mảng con $\textit{nums}[L..R-1]$ là $s[R] - s[L]$, và mảng con đó là cách xa khi và chỉ khi $|s[R] - s[L] - \textit{goal}| \ge k$.

Có tổng cộng $\frac{n(n+1)}{2}$ mảng con không rỗng. Ta đếm những mảng con không thỏa điều kiện, tức là $|s[R] - s[L] - \textit{goal}| < k$, rồi lấy tổng số mảng con trừ đi số này.

Bất đẳng thức trên tương đương với

$$
s[R] - \textit{goal} - k < s[L] < s[R] - \textit{goal} + k
$$

tức là $s[L]$ nằm trong đoạn đóng $[s[R] - \textit{goal} - k + 1,\, s[R] - \textit{goal} + k - 1]$.

Ta duyệt các tổng tiền tố $v = s[R]$ từ trái sang phải. Trong các tổng tiền tố đã thêm, ta truy vấn xem có bao nhiêu giá trị nằm trong $[a, b]$ rồi trừ số đó khỏi đáp án, sau đó thêm $v$. Để rời rạc hóa, ta sắp xếp $s$ và xác định chỉ số trong Binary Indexed Tree bằng tìm kiếm nhị phân.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{nums}$.

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
    def distantSubarrays(self, nums: list[int], goal: int, k: int) -> int:
        s = list(accumulate(nums, initial=0))
        st = sorted(s)
        n = len(nums)
        ans = (1 + n) * n // 2
        bit = BinaryIndexedTree(len(st) + 1)
        for v in s:
            a = v - goal - k + 1
            b = v - goal + k - 1

            l = bisect_left(st, a) + 1
            r = bisect_left(st, b + 1)
            if l <= r:
                ans -= bit.query(r) - bit.query(l - 1)
            bit.update(bisect_left(st, v) + 1, 1)
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

    void update(int x, long delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    long query(int x) {
        long s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public long distantSubarrays(int[] nums, int goal, int k) {
        int n = nums.length;
        long[] s = new long[n + 1];

        for (int i = 0; i < n; i++) {
            s[i + 1] = s[i] + nums[i];
        }

        long[] st = s.clone();
        Arrays.sort(st);

        long ans = (long) n * (n + 1) / 2;
        BinaryIndexedTree bit = new BinaryIndexedTree(st.length + 1);

        for (long v : s) {
            long a = v - goal - k + 1L;
            long b = v - goal + k - 1L;

            int l = lowerBound(st, a) + 1;
            int r = lowerBound(st, b + 1);

            if (l <= r) {
                ans -= bit.query(r) - bit.query(l - 1);
            }

            bit.update(lowerBound(st, v) + 1, 1);
        }

        return ans;
    }

    private int lowerBound(long[] nums, long target) {
        int l = 0;
        int r = nums.length;

        while (l < r) {
            int m = (l + r) >>> 1;
            if (nums[m] < target) {
                l = m + 1;
            } else {
                r = m;
            }
        }

        return l;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
    int n;
    vector<long long> c;

public:
    BinaryIndexedTree(int n)
        : n(n)
        , c(n + 1) {}

    void update(int x, long long delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    long long query(int x) {
        long long s = 0;
        while (x) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
};

class Solution {
public:
    long long distantSubarrays(vector<int>& nums, int goal, int k) {
        int n = nums.size();
        vector<long long> s(n + 1);

        for (int i = 0; i < n; i++) {
            s[i + 1] = s[i] + nums[i];
        }

        vector<long long> st = s;
        sort(st.begin(), st.end());

        long long ans = 1LL * n * (n + 1) / 2;
        BinaryIndexedTree bit(st.size() + 1);

        for (long long v : s) {
            long long a = v - goal - k + 1LL;
            long long b = v - goal + k - 1LL;

            int l = lower_bound(st.begin(), st.end(), a) - st.begin() + 1;
            int r = lower_bound(st.begin(), st.end(), b + 1) - st.begin();

            if (l <= r) {
                ans -= bit.query(r) - bit.query(l - 1);
            }

            int pos = lower_bound(st.begin(), st.end(), v) - st.begin() + 1;
            bit.update(pos, 1);
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

func distantSubarrays(nums []int, goal int, k int) int64 {
	n := len(nums)
	s := make([]int, n+1)

	for i, x := range nums {
		s[i+1] = s[i] + x
	}

	st := append([]int(nil), s...)
	sort.Ints(st)

	ans := n * (n + 1) / 2
	bit := NewBinaryIndexedTree(len(st) + 1)

	for _, v := range s {
		a := v - goal - k + 1
		b := v - goal + k - 1

		l := sort.SearchInts(st, a) + 1
		r := sort.SearchInts(st, b+1)

		if l <= r {
			ans -= bit.query(r) - bit.query(l-1)
		}

		bit.update(sort.SearchInts(st, v)+1, 1)
	}

	return int64(ans)
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private readonly n: number;
    private readonly c: number[];

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

function distantSubarrays(nums: number[], goal: number, k: number): number {
    const n = nums.length;
    const s = new Array<number>(n + 1).fill(0);

    for (let i = 0; i < n; i++) {
        s[i + 1] = s[i] + nums[i];
    }

    const st = [...s].sort((a, b) => a - b);

    let ans = (n * (n + 1)) / 2;
    const bit = new BinaryIndexedTree(st.length + 1);

    for (const v of s) {
        const a = v - goal - k + 1;
        const b = v - goal + k - 1;

        const l = _.sortedIndex(st, a) + 1;
        const r = _.sortedIndex(st, b + 1);

        if (l <= r) {
            ans -= bit.query(r) - bit.query(l - 1);
        }

        bit.update(_.sortedIndex(st, v) + 1, 1);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
