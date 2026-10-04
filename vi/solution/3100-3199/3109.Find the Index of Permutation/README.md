---
comments: true
difficulty: Medium
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

# [3109. Find the Index of Permutation 🔒](https://leetcode.com/problems/find-the-index-of-permutation)

[中文文档](/solution/3100-3199/3109.Find%20the%20Index%20of%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>perm</code> có độ dài <code>n</code>, là một hoán vị của <code>[1, 2, ..., n]</code>, hãy trả về chỉ số của <code>perm</code> trong mảng tất cả các hoán vị của <code>[1, 2, ..., n]</code> được <span data-keyword="lexicographically-sorted-array">sắp xếp theo thứ tự từ điển</span>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về phần dư khi chia cho <strong>modulo</strong> <code>10<sup>9</sup>&nbsp;+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">perm = [1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có hai hoán vị theo thứ tự sau:</p>

<p><code>[1,2]</code>, <code>[2,1]</code><br />
<br />
Và <code>[1,2]</code> có chỉ số 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">perm = [3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có sáu hoán vị theo thứ tự sau:</p>

<p><code>[1,2,3]</code>, <code>[1,3,2]</code>, <code>[2,1,3]</code>, <code>[2,3,1]</code>, <code>[3,1,2]</code>, <code>[3,2,1]</code><br />
<br />
Và <code>[3,1,2]</code> có chỉ số 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == perm.length &lt;= 10<sup>5</sup></code></li>
	<li><code>perm</code> là một hoán vị của <code>[1, 2, ..., n]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số theo thứ tự từ điển của một hoán vị là số hoán vị nhỏ hơn nghiêm ngặt nó. Việc đệ quy qua các giá trị chưa dùng ở mỗi prefix sẽ tăng theo giai thừa và không thể đáp ứng với $n$ vừa phải.
>
> Nếu vị trí $i$ nhận một giá trị chưa dùng nhỏ hơn $perm[i]$, $n-i-1$ vị trí còn lại có thể được sắp xếp tùy ý, đóng góp số đó nhân với $(n-i-1)!$. Cộng các giá trị đóng góp theo từng vị trí sẽ cho ra hạng.
>
> Fenwick tree lưu các giá trị đã gặp, nên số giá trị đã dùng nhỏ hơn giá trị hiện tại có thể lấy bằng một truy vấn prefix. Ta cộng $(perm[i]-1-\textit{query}(perm[i]))\times(n-i-1)!$ từ trái sang phải và đánh dấu $perm[i]$, đạt độ phức tạp $O(n\log n)$.

<!-- thinking:end -->

Theo yêu cầu của bài toán, chúng ta cần tìm xem có bao nhiêu hoán vị có thứ tự từ điển nhỏ hơn hoán vị đã cho.

Chúng ta xét cách tính số hoán vị có thứ tự từ điển nhỏ hơn hoán vị đã cho. Có hai trường hợp:

- Phần tử đầu tiên của hoán vị nhỏ hơn $perm[0]$, khi đó có $(perm[0] - 1) \times (n-1)!$ hoán vị.
- Phần tử đầu tiên của hoán vị bằng $perm[0]$, khi đó chúng ta tiếp tục xét phần tử thứ hai, và cứ như vậy.
- Tổng của tất cả các trường hợp là đáp án.

Chúng ta có thể dùng binary indexed tree để duy trì số phần tử nhỏ hơn phần tử hiện tại trong các phần tử đã duyệt. Với phần tử thứ $i$ của hoán vị đã cho, số phần tử còn lại nhỏ hơn nó là $perm[i] - 1 - tree.query(perm[i])$, và số hoán vị tương ứng là $(perm[i] - 1 - tree.query(perm[i])) \times (n-i-1)!$, được cộng vào đáp án. Sau đó, chúng ta cập nhật binary indexed tree và thêm phần tử hiện tại vào binary indexed tree. Tiếp tục duyệt phần tử tiếp theo cho đến khi duyệt hết tất cả phần tử.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của hoán vị.

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
    def getPermutationIndex(self, perm: List[int]) -> int:
        mod = 10**9 + 7
        ans, n = 0, len(perm)
        tree = BinaryIndexedTree(n + 1)
        f = [1] * n
        for i in range(1, n):
            f[i] = f[i - 1] * i % mod
        for i, x in enumerate(perm):
            cnt = x - 1 - tree.query(x)
            ans += cnt * f[n - i - 1] % mod
            tree.update(x, 1)
        return ans % mod
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

    public void update(int x, int delta) {
        for (; x <= n; x += x & -x) {
            c[x] += delta;
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
    public int getPermutationIndex(int[] perm) {
        final int mod = (int) 1e9 + 7;
        long ans = 0;
        int n = perm.length;
        BinaryIndexedTree tree = new BinaryIndexedTree(n + 1);
        long[] f = new long[n];
        f[0] = 1;
        for (int i = 1; i < n; ++i) {
            f[i] = f[i - 1] * i % mod;
        }
        for (int i = 0; i < n; ++i) {
            int cnt = perm[i] - 1 - tree.query(perm[i]);
            ans = (ans + cnt * f[n - i - 1] % mod) % mod;
            tree.update(perm[i], 1);
        }
        return (int) ans;
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
        , c(n + 1) {}

    void update(int x, int delta) {
        for (; x <= n; x += x & -x) {
            c[x] += delta;
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
    int getPermutationIndex(vector<int>& perm) {
        const int mod = 1e9 + 7;
        using ll = long long;
        ll ans = 0;
        int n = perm.size();
        BinaryIndexedTree tree(n + 1);
        ll f[n];
        f[0] = 1;
        for (int i = 1; i < n; ++i) {
            f[i] = f[i - 1] * i % mod;
        }
        for (int i = 0; i < n; ++i) {
            int cnt = perm[i] - 1 - tree.query(perm[i]);
            ans += cnt * f[n - i - 1] % mod;
            tree.update(perm[i], 1);
        }
        return ans % mod;
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
	return &BinaryIndexedTree{n: n, c: make([]int, n+1)}
}

func (bit *BinaryIndexedTree) update(x, delta int) {
	for ; x <= bit.n; x += x & -x {
		bit.c[x] += delta
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	s := 0
	for ; x > 0; x -= x & -x {
		s += bit.c[x]
	}
	return s
}

func getPermutationIndex(perm []int) (ans int) {
	const mod int = 1e9 + 7
	n := len(perm)
	tree := NewBinaryIndexedTree(n + 1)
	f := make([]int, n)
	f[0] = 1
	for i := 1; i < n; i++ {
		f[i] = f[i-1] * i % mod
	}
	for i, x := range perm {
		cnt := x - 1 - tree.query(x)
		ans += cnt * f[n-1-i] % mod
		tree.update(x, 1)
	}
	return ans % mod
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

    update(x: number, delta: number): void {
        for (; x <= this.n; x += x & -x) {
            this.c[x] += delta;
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

function getPermutationIndex(perm: number[]): number {
    const mod = 1e9 + 7;
    const n = perm.length;
    const tree = new BinaryIndexedTree(n + 1);
    let ans = 0;
    const f: number[] = Array(n).fill(1);
    for (let i = 1; i < n; ++i) {
        f[i] = (f[i - 1] * i) % mod;
    }
    for (let i = 0; i < n; ++i) {
        const cnt = perm[i] - 1 - tree.query(perm[i]);
        ans = (ans + cnt * f[n - i - 1]) % mod;
        tree.update(perm[i], 1);
    }
    return ans % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
