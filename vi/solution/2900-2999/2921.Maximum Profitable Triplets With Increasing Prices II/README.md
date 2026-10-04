---
comments: true
difficulty: Hard
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
---

<!-- problem:start -->

# [2921. Maximum Profitable Triplets With Increasing Prices II 🔒](https://leetcode.com/problems/maximum-profitable-triplets-with-increasing-prices-ii)

[中文文档](/solution/2900-2999/2921.Maximum%20Profitable%20Triplets%20With%20Increasing%20Prices%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng <strong>0-indexed</strong> <code>prices</code> và <code>profits</code> có cùng độ dài <code>n</code>. Có <code>n</code> mặt hàng trong một cửa hàng, trong đó mặt hàng <code>i<sup>th</sup></code> có giá <code>prices[i]</code> và lợi nhuận <code>profits[i]</code>.</p>

<p>Cần chọn ba mặt hàng thỏa mãn điều kiện sau:</p>

<ul>
	<li><code>prices[i] &lt; prices[j] &lt; prices[k]</code> với <code>i &lt; j &lt; k</code>.</li>
</ul>

<p>Nếu chọn các mặt hàng có chỉ số <code>i</code>, <code>j</code> và <code>k</code> thỏa mãn điều kiện trên, lợi nhuận sẽ là <code>profits[i] + profits[j] + profits[k]</code>.</p>

<p>Hãy trả về <em><strong>lợi nhuận lớn nhất</strong> có thể nhận được, và </em><code>-1</code><em> nếu không thể chọn ba mặt hàng thỏa mãn điều kiện đã cho.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [10,2,3,4], profits = [100,2,7,10]
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Không thể chọn mặt hàng có chỉ số i=0 vì không có chỉ số j và k nào thỏa mãn điều kiện.
Vì vậy, bộ ba duy nhất có thể chọn là các mặt hàng có chỉ số 1, 2 và 3, và đây là một lựa chọn hợp lệ vì prices[1] &lt; prices[2] &lt; prices[3].
Đáp án là tổng lợi nhuận của chúng, bằng 2 + 7 + 10 = 19.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,2,3,4,5], profits = [1,5,3,4,6]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Có thể chọn bất kỳ bộ ba mặt hàng nào vì với mọi bộ ba chỉ số i, j và k thỏa mãn i &lt; j &lt; k, điều kiện đều được đáp ứng.
Do đó, lợi nhuận lớn nhất có thể nhận được là tổng lợi nhuận của 3 mặt hàng có lợi nhuận cao nhất, tương ứng với các chỉ số 1, 3 và 4.
Đáp án là tổng của chúng, bằng 5 + 4 + 6 = 15.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [4,3,2,1], profits = [33,20,19,87]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể chọn bộ ba chỉ số nào thỏa mãn điều kiện, nên trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= prices.length == profits.length &lt;= 50000</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 5000</code></li>
	<li><code>1 &lt;= profits[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Đây là phương pháp 2 của phần I: cố định mặt hàng ở giữa, truy vấn lợi nhuận tốt nhất trong các mặt hàng rẻ hơn ở bên trái và đắt hơn ở bên phải. Với $n$ lớn hơn, cần dùng Fenwick tree thay vì duyệt $O(n^2)$.
>
> $tree1$ chèn lợi nhuận từ trái sang phải và truy vấn $[1,price-1]$; $tree2$ đảo ngược giá từ phải sang trái để điều kiện “lớn hơn $price$” trở thành một tiền tố lấy giá trị lớn nhất. Cập nhật $left+profit+right$ khi cả hai phía đều dương.

<!-- thinking:end -->

Ta có thể dùng hai Binary Indexed Tree (BIT) để duy trì lợi nhuận lớn nhất ở bên trái và bên phải của từng mức giá. Sau đó, duyệt qua mức giá ở giữa, truy vấn lợi nhuận lớn nhất ở cả hai phía thông qua BIT, rồi lấy giá trị lớn nhất cuối cùng.

Độ phức tạp thời gian là $O(n \times \log M)$, độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng $prices$, còn $M$ là giá trị lớn nhất trong mảng $prices$. Trong bài toán này, $M \le 5000$.

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

<!-- problem:end -->
