---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [3907. Count Smaller Elements With Opposite Parity 🔒](https://leetcode.com/problems/count-smaller-elements-with-opposite-parity)

[中文文档](/solution/3900-3999/3907.Count%20Smaller%20Elements%20With%20Opposite%20Parity/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p><strong>Điểm số</strong> của một chỉ số <code>i</code> được định nghĩa là số lượng chỉ số <code>j</code> thỏa mãn:</p>

<ul>
	<li><code>i &lt; j &lt; n</code></li>
	<li><code>nums[j] &lt; nums[i]</code></li>
	<li><code>nums[i]</code> và <code>nums[j]</code> có parity khác nhau (một số chẵn và số còn lại là số lẻ).</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code> có độ dài <code>n</code>, trong đó <code>answer[i]</code> là điểm số của chỉ số <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2,4,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1,2,0,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, các phần tử <code>nums[1] = 2</code> và <code>nums[2] = 4</code> nhỏ hơn và có parity khác nhau.</li>
	<li>Với <code>i = 1</code>, phần tử <code>nums[3] = 1</code> nhỏ hơn và có parity khác nhau.</li>
	<li>Với <code>i = 2</code>, các phần tử <code>nums[3] = 1</code> và <code>nums[4] = 3</code> nhỏ hơn và có parity khác nhau.</li>
	<li>Không có phần tử hợp lệ nào cho các chỉ số còn lại.</li>
</ul>

<p>Do đó, <code>answer = [2, 1, 2, 0, 0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,0]</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<p>Với <code>i = 0</code> và <code>i = 1</code>, phần tử <code>nums[2] = 1</code> nhỏ hơn và có parity khác nhau. Do đó, <code>answer = [1, 1, 0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có phần tử nào nằm bên phải chỉ số 0, nên điểm số của nó là 0. Do đó, <code>answer = [0]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Danh sách có thứ tự hoặc Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Việc duyệt sang phải để tìm giá trị nhỏ hơn và có parity đối lập tại mỗi chỉ số có độ phức tạp $O(n^2)$, không phù hợp với $n\le 10^5$. Số lượng cần tìm chính là số giá trị có parity đối lập chưa được xét và nhỏ hơn giá trị hiện tại.
>
> Duyệt từ phải sang trái khiến phần bên phải trở thành tập các giá trị chưa được xét. Hai danh sách đã sắp xếp lưu các số chẵn và số lẻ; $\textit{bisect\_left}$ trên danh sách đối lập trả lời truy vấn, sau đó giá trị hiện tại được thêm vào danh sách tương ứng.
>
> Mỗi truy vấn và thao tác thêm đều có độ phức tạp logarit, nên tổng độ phức tạp là $O(n\log n)$.

<!-- thinking:end -->

Ta có thể sử dụng hai danh sách có thứ tự (hoặc Binary Indexed Tree) để quản lý riêng các phần tử chẵn và lẻ. Với mỗi phần tử, ta truy vấn số lượng phần tử nhỏ hơn trong danh sách còn lại, sau đó thêm phần tử hiện tại vào danh sách tương ứng.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSmallerOppositeParity(self, nums: list[int]) -> list[int]:
        n = len(nums)
        ans = [0] * n
        sl = [SortedList(), SortedList()]
        for i in range(n - 1, -1, -1):
            ans[i] = sl[nums[i] & 1 ^ 1].bisect_left(nums[i])
            sl[nums[i] & 1].add(nums[i])
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    int n;
    int[] c;

    BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

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
}

class Solution {
    public int[] countSmallerOppositeParity(int[] nums) {
        int n = nums.length;
        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        int m = 0;
        for (int i = 0; i < n; i++) {
            if (i == 0 || sorted[i] != sorted[i - 1]) {
                sorted[m++] = sorted[i];
            }
        }
        BinaryIndexedTree[] bit = {new BinaryIndexedTree(m), new BinaryIndexedTree(m)};
        int[] ans = new int[n];
        for (int i = n - 1; i >= 0; --i) {
            int x = Arrays.binarySearch(sorted, 0, m, nums[i]) + 1;
            ans[i] = bit[nums[i] & 1 ^ 1].query(x - 1);
            bit[nums[i] & 1].update(x, 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
struct BIT {
    int n;
    vector<int> c;
    BIT(int n)
        : n(n)
        , c(n + 1, 0) {}
    void update(int x, int delta) {
        for (; x <= n; x += x & -x) c[x] += delta;
    }
    int query(int x) {
        int s = 0;
        for (; x > 0; x -= x & -x) s += c[x];
        return s;
    }
};

class Solution {
public:
    vector<int> countSmallerOppositeParity(vector<int>& nums) {
        int n = nums.size();
        vector<int> sorted = nums;
        sort(sorted.begin(), sorted.end());
        sorted.erase(unique(sorted.begin(), sorted.end()), sorted.end());

        int m = sorted.size();
        BIT* bits[2] = {new BIT(m), new BIT(m)};
        vector<int> ans(n);

        for (int i = n - 1; i >= 0; --i) {
            int x = lower_bound(sorted.begin(), sorted.end(), nums[i]) - sorted.begin() + 1;
            ans[i] = bits[nums[i] & 1 ^ 1]->query(x - 1);
            bits[nums[i] & 1]->update(x, 1);
        }
        return ans;
    }
};
```

#### Go

```go
type BIT struct {
	n int
	c []int
}

func newBIT(n int) *BIT {
	return &BIT{n: n, c: make([]int, n+1)}
}

func (b *BIT) update(x, delta int) {
	for ; x <= b.n; x += x & -x {
		b.c[x] += delta
	}
}

func (b *BIT) query(x int) int {
	s := 0
	for ; x > 0; x -= x & -x {
		s += b.c[x]
	}
	return s
}

func countSmallerOppositeParity(nums []int) []int {
	n := len(nums)
	sorted := make([]int, n)
	copy(sorted, nums)
	sort.Ints(sorted)

	m := 0
	if n > 0 {
		m = 1
		for i := 1; i < n; i++ {
			if sorted[i] != sorted[i-1] {
				sorted[m] = sorted[i]
				m++
			}
		}
		sorted = sorted[:m]
	}

	bits := []*BIT{newBIT(m), newBIT(m)}
	ans := make([]int, n)

	for i := n - 1; i >= 0; i-- {
		x := sort.SearchInts(sorted, nums[i]) + 1
		ans[i] = bits[nums[i]&1^1].query(x - 1)
		bits[nums[i]&1].update(x, 1)
	}
	return ans
}
```

#### TypeScript

```ts
class BIT {
    private c: Int32Array;
    constructor(private n: number) {
        this.c = new Int32Array(n + 1);
    }
    update(x: number, delta: number) {
        for (; x <= this.n; x += x & -x) this.c[x] += delta;
    }
    query(x: number): number {
        let s = 0;
        for (; x > 0; x -= x & -x) s += this.c[x];
        return s;
    }
}

function countSmallerOppositeParity(nums: number[]): number[] {
    const n = nums.length;
    const sorted = _.sortedUniq(_.sortBy(nums));
    const m = sorted.length;

    const bits = [new BIT(m), new BIT(m)];
    const ans = new Array(n);

    for (let i = n - 1; i >= 0; i--) {
        const rank = _.sortedIndex(sorted, nums[i]) + 1;
        ans[i] = bits[(nums[i] & 1) ^ 1].query(rank - 1);
        bits[nums[i] & 1].update(rank, 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
