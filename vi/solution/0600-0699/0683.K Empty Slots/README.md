---
comments: true
difficulty: Hard
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Queue
    - Array
    - Ordered Set
    - Sliding Window
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [683. K Empty Slots 🔒](https://leetcode.com/problems/k-empty-slots)

[中文文档](/solution/0600-0699/0683.K%20Empty%20Slots/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> bóng đèn xếp thành một hàng, được đánh số từ <code>1</code> đến <code>n</code>. Ban đầu, tất cả bóng đèn đều tắt. Mỗi ngày ta bật <strong>chính xác một</strong> bóng đèn, cho đến khi tất cả đều sáng sau <code>n</code> ngày.</p>

<p>Cho mảng <code>bulbs</code>&nbsp;có độ dài <code>n</code>, trong đó <code>bulbs[i] = x</code> nghĩa là vào ngày thứ <code>(i+1)<sup>th</sup></code>, ta bật bóng đèn ở vị trí <code>x</code>. Ở đây, <code>i</code>&nbsp;được đánh chỉ số từ&nbsp;<strong>0</strong>, còn <code>x</code>&nbsp;được đánh chỉ số từ&nbsp;<strong>1</strong>.</p>

<p>Cho số nguyên <code>k</code>, hãy trả về&nbsp;<em><strong>ngày sớm nhất</strong> mà có hai bóng đèn <strong>đang sáng</strong>, giữa chúng có <strong>chính xác</strong>&nbsp;<code>k</code> bóng đèn và tất cả đều <strong>đang tắt</strong>. Nếu không có ngày nào như vậy, trả về <code>-1</code>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> bulbs = [1,3,2], k = 1
<strong>Đầu ra:</strong> 2
<b>Giải thích:</b>
Ngày thứ nhất: bulbs[0] = 1, bóng đèn thứ nhất được bật: [1,0,0]
Ngày thứ hai: bulbs[1] = 3, bóng đèn thứ ba được bật: [1,0,1]
Ngày thứ ba: bulbs[2] = 2, bóng đèn thứ hai được bật: [1,1,1]
Ta trả về 2 vì vào ngày thứ hai, có hai bóng đèn đang sáng với một bóng đèn tắt ở giữa.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> bulbs = [1,2,3], k = 1
<strong>Đầu ra:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == bulbs.length</code></li>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= bulbs[i] &lt;= n</code></li>
	<li><code>bulbs</code>&nbsp;là một hoán vị của các số từ&nbsp;<code>1</code>&nbsp;đến&nbsp;<code>n</code>.</li>
	<li><code>0 &lt;= k &lt;= 2 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Các bóng đèn được bật theo thứ tự; ta cần tìm ngày đầu tiên có hai bóng đèn đang sáng, giữa chúng có đúng $k$ vị trí trống. Xét từng cặp bóng đèn đang sáng có độ phức tạp bậc hai.
>
> Fenwick tree đếm số bóng đèn đã bật. Sau khi bật bóng ở vị trí $x$, nếu bóng ở vị trí $x\pm(k+1)$ đã bật và hiệu prefix sum giữa chúng bằng $0$, thì khoảng giữa không có bóng nào đang sáng.

<!-- thinking:end -->

Ta có thể dùng Binary Indexed Tree để duy trì prefix sum của các bóng đèn. Mỗi lần bật một bóng đèn, ta cập nhật vị trí tương ứng trong Binary Indexed Tree. Sau đó, kiểm tra xem $k$ bóng đèn bên trái hoặc bên phải bóng vừa bật đều đang tắt, đồng thời bóng thứ $(k+1)$ theo hướng đó đã bật hay chưa. Nếu một trong hai phía thỏa mãn, trả về ngày hiện tại.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số bóng đèn.

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
    def kEmptySlots(self, bulbs: List[int], k: int) -> int:
        n = len(bulbs)
        tree = BinaryIndexedTree(n)
        vis = [False] * (n + 1)
        for i, x in enumerate(bulbs, 1):
            tree.update(x, 1)
            vis[x] = True
            y = x - k - 1
            if y > 0 and vis[y] and tree.query(x - 1) - tree.query(y) == 0:
                return i
            y = x + k + 1
            if y <= n and vis[y] and tree.query(y - 1) - tree.query(x) == 0:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int kEmptySlots(int[] bulbs, int k) {
        int n = bulbs.length;
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        boolean[] vis = new boolean[n + 1];
        for (int i = 1; i <= n; ++i) {
            int x = bulbs[i - 1];
            tree.update(x, 1);
            vis[x] = true;
            int y = x - k - 1;
            if (y > 0 && vis[y] && tree.query(x - 1) - tree.query(y) == 0) {
                return i;
            }
            y = x + k + 1;
            if (y <= n && vis[y] && tree.query(y - 1) - tree.query(x) == 0) {
                return i;
            }
        }
        return -1;
    }
}

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
        for (; x <= n; x += x & -x) {
            c[x] += delta;
        }
    }

    int query(int x) {
        int s = 0;
        for (; x; x -= x & -x) {
            s += c[x];
        }
        return s;
    }
};

class Solution {
public:
    int kEmptySlots(vector<int>& bulbs, int k) {
        int n = bulbs.size();
        BinaryIndexedTree* tree = new BinaryIndexedTree(n);
        bool vis[n + 1];
        memset(vis, false, sizeof(vis));
        for (int i = 1; i <= n; ++i) {
            int x = bulbs[i - 1];
            tree->update(x, 1);
            vis[x] = true;
            int y = x - k - 1;
            if (y > 0 && vis[y] && tree->query(x - 1) - tree->query(y) == 0) {
                return i;
            }
            y = x + k + 1;
            if (y <= n && vis[y] && tree->query(y - 1) - tree->query(x) == 0) {
                return i;
            }
        }
        return -1;
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
	for ; x <= this.n; x += x & -x {
		this.c[x] += delta
	}
}

func (this *BinaryIndexedTree) query(x int) (s int) {
	for ; x > 0; x -= x & -x {
		s += this.c[x]
	}
	return
}

func kEmptySlots(bulbs []int, k int) int {
	n := len(bulbs)
	tree := newBinaryIndexedTree(n)
	vis := make([]bool, n+1)
	for i, x := range bulbs {
		tree.update(x, 1)
		vis[x] = true
		i++
		y := x - k - 1
		if y > 0 && vis[y] && tree.query(x-1)-tree.query(y) == 0 {
			return i
		}
		y = x + k + 1
		if y <= n && vis[y] && tree.query(y-1)-tree.query(x) == 0 {
			return i
		}
	}
	return -1
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

    public update(x: number, delta: number) {
        for (; x <= this.n; x += x & -x) {
            this.c[x] += delta;
        }
    }

    public query(x: number): number {
        let s = 0;
        for (; x > 0; x -= x & -x) {
            s += this.c[x];
        }
        return s;
    }
}

function kEmptySlots(bulbs: number[], k: number): number {
    const n = bulbs.length;
    const tree = new BinaryIndexedTree(n);
    const vis: boolean[] = Array(n + 1).fill(false);
    for (let i = 1; i <= n; ++i) {
        const x = bulbs[i - 1];
        tree.update(x, 1);
        vis[x] = true;
        let y = x - k - 1;
        if (y > 0 && vis[y] && tree.query(x - 1) - tree.query(y) === 0) {
            return i;
        }
        y = x + k + 1;
        if (y <= n && vis[y] && tree.query(y - 1) - tree.query(x) === 0) {
            return i;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
