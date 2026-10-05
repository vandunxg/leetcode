---
comments: true
difficulty: Hard
rating: 2155
source: Weekly Contest 513 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Divide and Conquer
    - Prefix Sum
    - Merge Sort
---

<!-- problem:start -->

# [4013. Count Subarrays With Even Odd Ratio II](https://leetcode.com/problems/count-subarrays-with-even-odd-ratio-ii)

[中文文档](/solution/4000-4099/4013.Count%20Subarrays%20With%20Even%20Odd%20Ratio%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>a</code> và <code>b</code>.</p>

<p>Với một <span data-keyword="subarray-nonempty">mảng con</span>, gọi:</p>

<ul>
	<li><code>x</code> là số lượng phần tử chẵn.</li>
	<li><code>y</code> là số lượng phần tử lẻ.</li>
</ul>

<p>Tỷ lệ giữa số phần tử chẵn và số phần tử lẻ trong một mảng con được định nghĩa là <code>x / y</code>, trong đó các tỷ lệ được so sánh theo giá trị hữu tỉ chính xác.</p>

<p>Một mảng con được gọi là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><code>y &gt; 0</code>, và</li>
	<li><code>x / y &lt;= a / b</code>.</li>
</ul>

<p>Trả về số lượng mảng con hợp lệ trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2], a = 3, b = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Giá trị</th>
			<th style="border: 1px solid black;">Số phần tử chẵn</th>
			<th style="border: 1px solid black;">Số phần tử lẻ</th>
			<th style="border: 1px solid black;">Tỷ lệ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..0]</code></td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>0 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..1]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..2]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2, 1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>1 / 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..3]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2, 1, 2]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>2 / 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[1..2]</code></td>
			<td style="border: 1px solid black;"><code>[2, 1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[2..2]</code></td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>0 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[2..3]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy số lượng mảng con hợp lệ là 7.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,1], a = 2, b = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Giá trị</th>
			<th style="border: 1px solid black;">Số phần tử chẵn</th>
			<th style="border: 1px solid black;">Số phần tử lẻ</th>
			<th style="border: 1px solid black;">Tỷ lệ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..2]</code></td>
			<td style="border: 1px solid black;"><code>[2, 2, 1]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>2 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[1..2]</code></td>
			<td style="border: 1px solid black;"><code>[2, 1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[2..2]</code></td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>0 / 1</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy số lượng mảng con hợp lệ là 3.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,2], a = 1, b = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi mảng con đều chứa 0 số lẻ, nên không có mảng con nào hợp lệ.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= a, b &lt;= 10<sup>9</sup>​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê bậc hai của bài trước không thể áp dụng khi $n=10^5$. Điều kiện $y>0$ và $\frac{x}{y}\le\frac{a}{b}$ được biến đổi thành $ay-bx\ge 0$ khi $b>0$; một mảng con toàn số chẵn khiến cùng biểu thức này âm, nên hai ràng buộc được gộp lại.
>
> Ta ánh xạ số lẻ thành $+a$ và số chẵn thành $-b$, rồi đếm các mảng con không rỗng có tổng ít nhất $0$, tức là các cặp prefix thỏa mãn $s[L]\le s[R]$.
>
> Khi duyệt $R$, Fenwick tree trên các giá trị prefix đã nén lưu số lượng $s[L]$ trước đó đã xuất hiện. Ta truy vấn các giá trị $\le s[R]$, sau đó chèn giá trị hiện tại.

<!-- thinking:end -->

Với một mảng con, gọi $x$ là số lượng phần tử chẵn và $y$ là số lượng phần tử lẻ. Bài toán yêu cầu $y > 0$ và $\frac{x}{y} \le \frac{a}{b}$. Vì $b > 0$ và $y > 0$, bất đẳng thức tương đương với $a \cdot y - b \cdot x \ge 0$.

Khi $y = 0$, vì mảng con không rỗng nên ta phải có $x > 0$. Trong trường hợp này, $a \cdot y - b \cdot x = -b \cdot x < 0$, nên bất đẳng thức không đúng. Do đó, hai điều kiện trong đề bài có thể được gộp thành một điều kiện duy nhất: $a \cdot y - b \cdot x \ge 0$.

Ta xem các số lẻ trong $\textit{nums}$ là $a$ và các số chẵn là $-b$, tạo thành một mảng $\textit{arr}$. Khi đó, bài toán ban đầu tương đương với việc đếm số mảng con liên tiếp không rỗng của $\textit{arr}$ có tổng các phần tử ít nhất là $0$.

Gọi $s$ là mảng tổng prefix của $\textit{arr}$. Tổng các phần tử của mảng con $[L, R - 1]$ bằng $s[R] - s[L]$, nên bài toán được chuyển thành: có bao nhiêu cặp chỉ số $(L, R)$ thỏa mãn $0 \le L < R \le n$ và $s[R] - s[L] \ge 0$, tức là $s[L] \le s[R]$?

Ta duyệt $R$ và cần đếm nhanh số chỉ số $L$ ở bên trái $R$ thỏa mãn $s[L] \le s[R]$. Việc này có thể được duy trì bằng Binary Indexed Tree: trước tiên ta rời rạc hóa mọi giá trị trong $s$ (sắp xếp và loại bỏ trùng lặp), sau đó duyệt $s$ từ trái sang phải. Với mỗi giá trị $v = s[R]$, ta truy vấn số phần tử đã chèn không lớn hơn $v$ trong Binary Indexed Tree và cộng vào đáp án, rồi chèn $v$ vào cây.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

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
    def countRatioSubarrays(self, nums: list[int], a: int, b: int) -> int:
        n = len(nums)
        s = [0] * (n + 1)
        for i, x in enumerate(nums):
            s[i + 1] = s[i] + (a if x % 2 else -b)

        st = sorted(set(s))
        bit = BinaryIndexedTree(len(st) + 1)
        ans = 0
        for v in s:
            x = bisect_left(st, v) + 1
            ans += bit.query(x)
            bit.update(x, 1)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private final int n;
    private final int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
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
    public long countRatioSubarrays(int[] nums, int a, int b) {
        int n = nums.length;

        long[] s = new long[n + 1];
        for (int i = 0; i < n; i++) {
            s[i + 1] = s[i] + (nums[i] % 2 == 1 ? a : -b);
        }

        long[] st = s.clone();
        Arrays.sort(st);

        int m = 0;
        for (long x : st) {
            if (m == 0 || st[m - 1] != x) {
                st[m++] = x;
            }
        }

        BinaryIndexedTree bit = new BinaryIndexedTree(m + 1);

        long ans = 0;

        for (long v : s) {
            int x = Arrays.binarySearch(st, 0, m, v) + 1;
            ans += bit.query(x);
            bit.update(x, 1);
        }

        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
    int n;
    vector<int> c;

public:
    BinaryIndexedTree(int n)
        : n(n)
        , c(n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
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
};

class Solution {
public:
    long long countRatioSubarrays(vector<int>& nums, int a, int b) {
        int n = nums.size();

        vector<long long> s(n + 1);
        for (int i = 0; i < n; i++) {
            s[i + 1] = s[i] + (nums[i] % 2 ? a : -b);
        }

        vector<long long> st = s;
        sort(st.begin(), st.end());
        st.erase(unique(st.begin(), st.end()), st.end());

        BinaryIndexedTree bit(st.size() + 1);

        long long ans = 0;

        for (long long v : s) {
            int x = lower_bound(st.begin(), st.end(), v) - st.begin() + 1;
            ans += bit.query(x);
            bit.update(x, 1);
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

func (bit *BinaryIndexedTree) update(x int, delta int) {
	for x <= bit.n {
		bit.c[x] += delta
		x += x & -x
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	sum := 0
	for x > 0 {
		sum += bit.c[x]
		x -= x & -x
	}
	return sum
}

func countRatioSubarrays(nums []int, a int, b int) int64 {
	n := len(nums)

	s := make([]int64, n+1)

	for i, x := range nums {
		if x%2 == 1 {
			s[i+1] = s[i] + int64(a)
		} else {
			s[i+1] = s[i] - int64(b)
		}
	}

	st := append([]int64{}, s...)
	sort.Slice(st, func(i, j int) bool {
		return st[i] < st[j]
	})

	uniq := make([]int64, 0, len(st))
	for _, x := range st {
		if len(uniq) == 0 || uniq[len(uniq)-1] != x {
			uniq = append(uniq, x)
		}
	}

	bit := NewBinaryIndexedTree(len(uniq) + 1)

	var ans int64

	for _, v := range s {
		x := sort.Search(len(uniq), func(i int) bool {
			return uniq[i] >= v
		}) + 1

		ans += int64(bit.query(x))
		bit.update(x, 1)
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
        this.c = new Array(n + 1).fill(0);
    }

    update(x: number, delta: number): void {
        while (x <= this.n) {
            this.c[x] += delta;
            x += x & -x;
        }
    }

    query(x: number): number {
        let sum = 0;
        while (x > 0) {
            sum += this.c[x];
            x -= x & -x;
        }
        return sum;
    }
}

function countRatioSubarrays(nums: number[], a: number, b: number): number {
    const n = nums.length;

    const s = new Array<number>(n + 1).fill(0);

    for (let i = 0; i < n; i++) {
        s[i + 1] = s[i] + (nums[i] % 2 === 1 ? a : -b);
    }

    const st = [...s].sort((x, y) => x - y);

    const uniq: number[] = [];
    for (const x of st) {
        if (uniq.length === 0 || uniq[uniq.length - 1] !== x) {
            uniq.push(x);
        }
    }

    const bit = new BinaryIndexedTree(uniq.length + 1);

    let ans = 0;

    for (const v of s) {
        const x = _.sortedIndex(uniq, v) + 1;

        ans += bit.query(x);
        bit.update(x, 1);
    }

    return ans;
}
```

#### Rust

```rust
struct BinaryIndexedTree {
    n: usize,
    c: Vec<i32>,
}

impl BinaryIndexedTree {
    fn new(n: usize) -> Self {
        Self {
            n,
            c: vec![0; n + 1],
        }
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

impl Solution {
    pub fn count_ratio_subarrays(nums: Vec<i32>, a: i32, b: i32) -> i64 {
        let n = nums.len();

        let mut s = vec![0i64; n + 1];

        for i in 0..n {
            s[i + 1] = s[i]
                + if nums[i] % 2 == 1 {
                    a as i64
                } else {
                    -(b as i64)
                };
        }

        let mut st = s.clone();
        st.sort_unstable();
        st.dedup();

        let mut bit = BinaryIndexedTree::new(st.len() + 1);

        let mut ans = 0i64;

        for v in s {
            let x = match st.binary_search(&v) {
                Ok(i) => i,
                Err(i) => i,
            } + 1;

            ans += bit.query(x) as i64;
            bit.update(x, 1);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
