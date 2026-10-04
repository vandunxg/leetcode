---
comments: true
difficulty: Medium
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
---

<!-- problem:start -->

# [2907. Maximum Profitable Triplets With Increasing Prices I 🔒](https://leetcode.com/problems/maximum-profitable-triplets-with-increasing-prices-i)

[中文文档](/solution/2900-2999/2907.Maximum%20Profitable%20Triplets%20With%20Increasing%20Prices%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng <code>prices</code> và <code>profits</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>. Có <code>n</code> mặt hàng trong một cửa hàng, trong đó mặt hàng <code>i<sup>th</sup></code> có giá là <code>prices[i]</code> và lợi nhuận là <code>profits[i]</code>.</p>

<p>Ta phải chọn ba mặt hàng thỏa mãn điều kiện sau:</p>

<ul>
	<li><code>prices[i] &lt; prices[j] &lt; prices[k]</code> với <code>i &lt; j &lt; k</code>.</li>
</ul>

<p>Nếu chọn các mặt hàng có chỉ số <code>i</code>, <code>j</code> và <code>k</code> thỏa mãn điều kiện trên, lợi nhuận sẽ là <code>profits[i] + profits[j] + profits[k]</code>.</p>

<p>Trả về <em><strong>lợi nhuận lớn nhất</strong> có thể nhận được và </em><code>-1</code><em> nếu không thể chọn ba mặt hàng thỏa mãn điều kiện đã cho.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [10,2,3,4], profits = [100,2,7,10]
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Không thể chọn mặt hàng có chỉ số i=0 vì không tồn tại các chỉ số j và k sao cho điều kiện được thỏa mãn.
Vì vậy, bộ ba duy nhất có thể chọn là các mặt hàng có chỉ số 1, 2 và 3, và đây là lựa chọn hợp lệ vì prices[1] &lt; prices[2] &lt; prices[3].
Đáp án là tổng lợi nhuận của chúng, bằng 2 + 7 + 10 = 19.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,2,3,4,5], profits = [1,5,3,4,6]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Ta có thể chọn bất kỳ bộ ba mặt hàng nào vì với mọi bộ ba chỉ số i, j và k thỏa mãn i &lt; j &lt; k, điều kiện đều được thỏa mãn.
Do đó, lợi nhuận lớn nhất có thể nhận được là tổng lợi nhuận của 3 mặt hàng có lợi nhuận cao nhất, tương ứng với các chỉ số 1, 3 và 4.
Đáp án là tổng lợi nhuận của chúng, bằng 5 + 4 + 6 = 15.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [4,3,2,1], profits = [33,20,19,87]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể chọn bộ ba chỉ số nào thỏa mãn điều kiện, nên ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= prices.length == profits.length &lt;= 2000</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= profits[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt phần tử ở giữa

<!-- thinking:start -->

> **Tư duy**
>
> Một bộ ba hợp lệ cần có giá tăng nghiêm ngặt. Sau khi cố định chỉ số ở giữa $j$, phía bên trái cần lợi nhuận lớn nhất trong các mặt hàng rẻ hơn, còn phía bên phải cần lợi nhuận lớn nhất trong các mặt hàng đắt hơn; mỗi phía có thể được duyệt tuyến tính. Với $n$ vừa phải, độ phức tạp $O(n^2)$ là đủ.
>
> Nếu một trong hai phía không tồn tại phần tử phù hợp thì không thể dùng $j$. Đáp án là giá trị lớn nhất của $left+profits[j]+right$ trên mọi $j$ hợp lệ.

<!-- thinking:end -->

Ta có thể duyệt phần tử ở giữa $profits[j]$, sau đó duyệt phần tử bên trái $profits[i]$ và phần tử bên phải $profits[k]$. Với mỗi $profits[j]$, ta cần tìm $profits[i]$ lớn nhất và $profits[k]$ lớn nhất sao cho $prices[i] < prices[j] < prices[k]$. Ta định nghĩa $left$ là giá trị lớn nhất ở bên trái $profits[j]$, còn $right$ là giá trị lớn nhất ở bên phải $profits[j]$. Nếu cả hai tồn tại, ta cập nhật đáp án bằng $ans = \max(ans, left + profits[j] + right)$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int], profits: List[int]) -> int:
        n = len(prices)
        ans = -1
        for j, x in enumerate(profits):
            left = right = 0
            for i in range(j):
                if prices[i] < prices[j] and left < profits[i]:
                    left = profits[i]
            for k in range(j + 1, n):
                if prices[j] < prices[k] and right < profits[k]:
                    right = profits[k]
            if left and right:
                ans = max(ans, left + x + right)
        return ans
```

#### Java

```java
class Solution {
    public int maxProfit(int[] prices, int[] profits) {
        int n = prices.length;
        int ans = -1;
        for (int j = 0; j < n; ++j) {
            int left = 0, right = 0;
            for (int i = 0; i < j; ++i) {
                if (prices[i] < prices[j]) {
                    left = Math.max(left, profits[i]);
                }
            }
            for (int k = j + 1; k < n; ++k) {
                if (prices[j] < prices[k]) {
                    right = Math.max(right, profits[k]);
                }
            }
            if (left > 0 && right > 0) {
                ans = Math.max(ans, left + profits[j] + right);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, vector<int>& profits) {
        int n = prices.size();
        int ans = -1;
        for (int j = 0; j < n; ++j) {
            int left = 0, right = 0;
            for (int i = 0; i < j; ++i) {
                if (prices[i] < prices[j]) {
                    left = max(left, profits[i]);
                }
            }
            for (int k = j + 1; k < n; ++k) {
                if (prices[j] < prices[k]) {
                    right = max(right, profits[k]);
                }
            }
            if (left && right) {
                ans = max(ans, left + profits[j] + right);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxProfit(prices []int, profits []int) int {
	n := len(prices)
	ans := -1
	for j, x := range profits {
		left, right := 0, 0
		for i := 0; i < j; i++ {
			if prices[i] < prices[j] {
				left = max(left, profits[i])
			}
		}
		for k := j + 1; k < n; k++ {
			if prices[j] < prices[k] {
				right = max(right, profits[k])
			}
		}
		if left > 0 && right > 0 {
			ans = max(ans, left+x+right)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxProfit(prices: number[], profits: number[]): number {
    const n = prices.length;
    let ans = -1;
    for (let j = 0; j < n; ++j) {
        let [left, right] = [0, 0];
        for (let i = 0; i < j; ++i) {
            if (prices[i] < prices[j]) {
                left = Math.max(left, profits[i]);
            }
        }
        for (let k = j + 1; k < n; ++k) {
            if (prices[j] < prices[k]) {
                right = Math.max(right, profits[k]);
            }
        }
        if (left > 0 && right > 0) {
            ans = Math.max(ans, left + profits[j] + right);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_profit(prices: Vec<i32>, profits: Vec<i32>) -> i32 {
        let n = prices.len();
        let mut ans = -1;

        for j in 0..n {
            let mut left = 0;
            let mut right = 0;

            for i in 0..j {
                if prices[i] < prices[j] {
                    left = left.max(profits[i]);
                }
            }

            for k in j + 1..n {
                if prices[j] < prices[k] {
                    right = right.max(profits[k]);
                }
            }

            if left > 0 && right > 0 {
                ans = ans.max(left + profits[j] + right);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 duyệt cả hai phía với mỗi chỉ số ở giữa nên có độ phức tạp $O(n^2)$. Truy vấn “lợi nhuận lớn nhất trong các giá nhỏ hơn $p$” là một truy vấn lấy cực đại trên prefix, có thể được lưu trong một Fenwick tree trên miền giá trị.
>
> Ta truy vấn rồi chèn các phần tử từ trái sang phải để tìm các giá trị lớn nhất bên trái; sau đó đảo trục giá và lặp lại từ phải sang trái. Cuối cùng, duyệt các chỉ số ở giữa để hoàn tất với độ phức tạp $O(n \log U)$.

<!-- thinking:end -->

Ta có thể sử dụng hai Binary Indexed Tree (BIT) để lần lượt duy trì lợi nhuận lớn nhất ở bên trái và bên phải của mỗi mức giá. Sau đó, ta duyệt giá ở giữa, truy vấn lợi nhuận lớn nhất ở cả hai phía thông qua BIT, rồi lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(n \times \log M)$, còn độ phức tạp không gian là $O(M)$. Ở đây, $n$ là độ dài của mảng $prices$, còn $M$ là giá trị lớn nhất trong mảng $prices$. Trong bài toán này, $M \le 10^6$.

Vì độ dài của $prices$ không vượt quá $2000$, trong khi giá trị của $prices[i]$ có thể đạt tới $10^6$, ta có thể rời rạc hóa $prices$ để giảm độ phức tạp không gian xuống còn $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = max(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        mx = 0
        while x:
            mx = max(mx, self.c[x])
            x -= x & -x
        return mx


class Solution:
    def maxProfit(self, prices: List[int], profits: List[int]) -> int:
        n = len(prices)
        left = [0] * n
        right = [0] * n

        m = max(prices)
        tree1 = BinaryIndexedTree(m + 1)
        tree2 = BinaryIndexedTree(m + 1)

        for i, x in enumerate(prices):
            left[i] = tree1.query(x - 1)
            tree1.update(x, profits[i])
        for i in range(n - 1, -1, -1):
            x = m + 1 - prices[i]
            right[i] = tree2.query(x - 1)
            tree2.update(x, profits[i])

        return max(
            (l + x + r for l, x, r in zip(left, profits, right) if l and r), default=-1
        )
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
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = Math.max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

class Solution {
    public int maxProfit(int[] prices, int[] profits) {
        int n = prices.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int m = 0;
        for (int x : prices) {
            m = Math.max(m, x);
        }
        BinaryIndexedTree tree1 = new BinaryIndexedTree(m + 1);
        BinaryIndexedTree tree2 = new BinaryIndexedTree(m + 1);
        for (int i = 0; i < n; ++i) {
            int x = prices[i];
            left[i] = tree1.query(x - 1);
            tree1.update(x, profits[i]);
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = m + 1 - prices[i];
            right[i] = tree2.query(x - 1);
            tree2.update(x, profits[i]);
        }
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = Math.max(ans, left[i] + profits[i] + right[i]);
            }
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
    BinaryIndexedTree(int n) {
        this->n = n;
        c.resize(n + 1, 0);
    }

    void update(int x, int v) {
        while (x <= n) {
            c[x] = max(c[x], v);
            x += x & -x;
        }
    }

    int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
};

class Solution {
public:
    int maxProfit(vector<int>& prices, vector<int>& profits) {
        int n = prices.size();
        vector<int> left(n, 0);
        vector<int> right(n, 0);
        int m = *max_element(prices.begin(), prices.end());
        BinaryIndexedTree tree1(m + 1);
        BinaryIndexedTree tree2(m + 1);
        for (int i = 0; i < n; ++i) {
            int x = prices[i];
            left[i] = tree1.query(x - 1);
            tree1.update(x, profits[i]);
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = m + 1 - prices[i];
            right[i] = tree2.query(x - 1);
            tree2.update(x, profits[i]);
        }
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = max(ans, left[i] + profits[i] + right[i]);
            }
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

func NewBinaryIndexedTree(n int) BinaryIndexedTree {
	c := make([]int, n+1)
	return BinaryIndexedTree{n: n, c: c}
}

func (bit *BinaryIndexedTree) update(x, v int) {
	for x <= bit.n {
		bit.c[x] = max(bit.c[x], v)
		x += x & -x
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	mx := 0
	for x > 0 {
		mx = max(mx, bit.c[x])
		x -= x & -x
	}
	return mx
}

func maxProfit(prices []int, profits []int) int {
	n := len(prices)
	left := make([]int, n)
	right := make([]int, n)
	m := slices.Max(prices)

	tree1 := NewBinaryIndexedTree(m + 1)
	tree2 := NewBinaryIndexedTree(m + 1)

	for i, x := range prices {
		left[i] = tree1.query(x - 1)
		tree1.update(x, profits[i])
	}

	for i := n - 1; i >= 0; i-- {
		x := m + 1 - prices[i]
		right[i] = tree2.query(x - 1)
		tree2.update(x, profits[i])
	}

	ans := -1

	for i := 0; i < n; i++ {
		if left[i] > 0 && right[i] > 0 {
			ans = max(ans, left[i]+profits[i]+right[i])
		}
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

    update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] = Math.max(this.c[x], v);
            x += x & -x;
        }
    }

    query(x: number): number {
        let mx = 0;
        while (x > 0) {
            mx = Math.max(mx, this.c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

function maxProfit(prices: number[], profits: number[]): number {
    const n: number = prices.length;
    const left: number[] = Array(n).fill(0);
    const right: number[] = Array(n).fill(0);
    const m = Math.max(...prices);

    const tree1: BinaryIndexedTree = new BinaryIndexedTree(m + 1);
    const tree2: BinaryIndexedTree = new BinaryIndexedTree(m + 1);

    for (let i = 0; i < n; i++) {
        const x: number = prices[i];
        left[i] = tree1.query(x - 1);
        tree1.update(x, profits[i]);
    }

    for (let i = n - 1; i >= 0; i--) {
        const x: number = m + 1 - prices[i];
        right[i] = tree2.query(x - 1);
        tree2.update(x, profits[i]);
    }

    let ans: number = -1;

    for (let i = 0; i < n; i++) {
        if (left[i] > 0 && right[i] > 0) {
            ans = Math.max(ans, left[i] + profits[i] + right[i]);
        }
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
    fn new(n: usize) -> BinaryIndexedTree {
        BinaryIndexedTree {
            n,
            c: vec![0; n + 1],
        }
    }

    fn update(&mut self, x: usize, v: i32) {
        let mut x = x;
        while x <= self.n {
            self.c[x] = self.c[x].max(v);
            x += x & x.wrapping_neg();
        }
    }

    fn query(&self, x: usize) -> i32 {
        let mut x = x;
        let mut mx = 0;
        while x > 0 {
            mx = mx.max(self.c[x]);
            x -= x & x.wrapping_neg();
        }
        mx
    }
}

impl Solution {
    pub fn max_profit(prices: Vec<i32>, profits: Vec<i32>) -> i32 {
        let n = prices.len();
        let mut left = vec![0; n];
        let mut right = vec![0; n];
        let m = prices.iter().cloned().max().unwrap_or(0);

        let mut tree1 = BinaryIndexedTree::new((m as usize) + 1);
        let mut tree2 = BinaryIndexedTree::new((m as usize) + 1);

        for i in 0..n {
            let x = prices[i] as usize;
            left[i] = tree1.query(x - 1);
            tree1.update(x, profits[i]);
        }

        for i in (0..n).rev() {
            let x = (m + 1 - prices[i]) as usize;
            right[i] = tree2.query(x - 1);
            tree2.update(x, profits[i]);
        }

        let mut ans = -1;
        for i in 0..n {
            if left[i] > 0 && right[i] > 0 {
                ans = ans.max(left[i] + profits[i] + right[i]);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 2 đánh chỉ số cho Fenwick tree bằng giá gốc, gây lãng phí không gian khi các mức giá thưa. Ta có thể nén hạng các mức giá khác nhau và dùng hạng làm chỉ số; các truy vấn và cập nhật không thay đổi, đồng thời chỉ cần $O(n)$ bộ nhớ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = max(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        mx = 0
        while x:
            mx = max(mx, self.c[x])
            x -= x & -x
        return mx


class Solution:
    def maxProfit(self, prices: List[int], profits: List[int]) -> int:
        n = len(prices)
        left = [0] * n
        right = [0] * n

        s = sorted(set(prices))
        m = len(s)
        tree1 = BinaryIndexedTree(m + 1)
        tree2 = BinaryIndexedTree(m + 1)

        for i, x in enumerate(prices):
            x = bisect_left(s, x) + 1
            left[i] = tree1.query(x - 1)
            tree1.update(x, profits[i])
        for i in range(n - 1, -1, -1):
            x = m + 1 - (bisect_left(s, prices[i]) + 1)
            right[i] = tree2.query(x - 1)
            tree2.update(x, profits[i])

        return max(
            (l + x + r for l, x, r in zip(left, profits, right) if l and r), default=-1
        )
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
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = Math.max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

class Solution {
    public int maxProfit(int[] prices, int[] profits) {
        int n = prices.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int[] s = prices.clone();
        Arrays.sort(s);
        int m = 0;
        for (int i = 0; i < n; ++i) {
            if (i == 0 || s[i] != s[i - 1]) {
                s[m++] = s[i];
            }
        }
        BinaryIndexedTree tree1 = new BinaryIndexedTree(m + 1);
        BinaryIndexedTree tree2 = new BinaryIndexedTree(m + 1);
        for (int i = 0; i < n; ++i) {
            int x = search(s, prices[i], m);
            left[i] = tree1.query(x - 1);
            tree1.update(x, profits[i]);
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = m + 1 - search(s, prices[i], m);
            right[i] = tree2.query(x - 1);
            tree2.update(x, profits[i]);
        }
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = Math.max(ans, left[i] + profits[i] + right[i]);
            }
        }
        return ans;
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
        return l + 1;
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
    BinaryIndexedTree(int n) {
        this->n = n;
        c.resize(n + 1, 0);
    }

    void update(int x, int v) {
        while (x <= n) {
            c[x] = max(c[x], v);
            x += x & -x;
        }
    }

    int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
};

class Solution {
public:
    int maxProfit(vector<int>& prices, vector<int>& profits) {
        int n = prices.size();
        vector<int> left(n);
        vector<int> right(n);
        vector<int> s = prices;
        sort(s.begin(), s.end());
        s.erase(unique(s.begin(), s.end()), s.end());
        int m = s.size();
        BinaryIndexedTree tree1(m + 1);
        BinaryIndexedTree tree2(m + 1);
        for (int i = 0; i < n; ++i) {
            int x = lower_bound(s.begin(), s.end(), prices[i]) - s.begin() + 1;
            left[i] = tree1.query(x - 1);
            tree1.update(x, profits[i]);
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = m + 1 - (lower_bound(s.begin(), s.end(), prices[i]) - s.begin() + 1);
            right[i] = tree2.query(x - 1);
            tree2.update(x, profits[i]);
        }
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = max(ans, left[i] + profits[i] + right[i]);
            }
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

func NewBinaryIndexedTree(n int) BinaryIndexedTree {
	c := make([]int, n+1)
	return BinaryIndexedTree{n: n, c: c}
}

func (bit *BinaryIndexedTree) update(x, v int) {
	for x <= bit.n {
		bit.c[x] = max(bit.c[x], v)
		x += x & -x
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	mx := 0
	for x > 0 {
		mx = max(mx, bit.c[x])
		x -= x & -x
	}
	return mx
}

func maxProfit(prices []int, profits []int) int {
	n := len(prices)
	left := make([]int, n)
	right := make([]int, n)
	s := make([]int, n)
	copy(s, prices)
	sort.Ints(s)
	m := 0
	for i, x := range s {
		if i == 0 || x != s[i-1] {
			s[m] = x
			m++
		}
	}

	tree1 := NewBinaryIndexedTree(m + 1)
	tree2 := NewBinaryIndexedTree(m + 1)

	for i, x := range prices {
		x = sort.SearchInts(s[:m], x) + 1
		left[i] = tree1.query(x - 1)
		tree1.update(x, profits[i])
	}

	for i := n - 1; i >= 0; i-- {
		x := m + 1 - (sort.SearchInts(s[:m], prices[i]) + 1)
		right[i] = tree2.query(x - 1)
		tree2.update(x, profits[i])
	}

	ans := -1

	for i := 0; i < n; i++ {
		if left[i] > 0 && right[i] > 0 {
			ans = max(ans, left[i]+profits[i]+right[i])
		}
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

    update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] = Math.max(this.c[x], v);
            x += x & -x;
        }
    }

    query(x: number): number {
        let mx = 0;
        while (x > 0) {
            mx = Math.max(mx, this.c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

function maxProfit(prices: number[], profits: number[]): number {
    const n: number = prices.length;
    const left: number[] = Array(n).fill(0);
    const right: number[] = Array(n).fill(0);

    const s = [...prices].sort((a, b) => a - b);
    let m = 0;
    for (let i = 0; i < n; ++i) {
        if (i === 0 || s[i] !== s[i - 1]) {
            s[m++] = s[i];
        }
    }
    s.length = m;

    const tree1: BinaryIndexedTree = new BinaryIndexedTree(m + 1);
    const tree2: BinaryIndexedTree = new BinaryIndexedTree(m + 1);

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
        const x = search(s, prices[i]) + 1;
        left[i] = tree1.query(x - 1);
        tree1.update(x, profits[i]);
    }

    for (let i = n - 1; i >= 0; i--) {
        const x: number = m + 1 - (search(s, prices[i]) + 1);
        right[i] = tree2.query(x - 1);
        tree2.update(x, profits[i]);
    }

    let ans: number = -1;

    for (let i = 0; i < n; i++) {
        if (left[i] > 0 && right[i] > 0) {
            ans = Math.max(ans, left[i] + profits[i] + right[i]);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
