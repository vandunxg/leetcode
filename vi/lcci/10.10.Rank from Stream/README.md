---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [10.10. Rank from Stream](https://leetcode.cn/problems/rank-from-stream-lcci)

[中文文档](/lcci/10.10.Rank%20from%20Stream/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy tưởng tượng bạn đang đọc một stream các số nguyên. Định kỳ, bạn muốn tra cứu rank của một số <code>x</code> (số lượng các giá trị nhỏ hơn hoặc bằng <code>x</code>). Hãy triển khai các cấu trúc dữ liệu và thuật toán để hỗ trợ những thao tác này. Cụ thể, hãy triển khai method <code>track (int x)</code>, được gọi mỗi khi một số được sinh ra, và method <code>getRankOfNumber(int x)</code>, trả về số lượng các giá trị nhỏ hơn hoặc bằng <code>x</code>.</p>

<p><b>Lưu ý:&nbsp;</b>Bài toán này hơi khác so với phiên bản gốc trong sách.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong>

[&quot;StreamRank&quot;, &quot;getRankOfNumber&quot;, &quot;track&quot;, &quot;getRankOfNumber&quot;]

[[], [1], [0], [0]]

<strong>Đầu ra:

</strong>[null,0,null,1]

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>x &lt;= 50000</code></li>
	<li>Số lần gọi cả hai method&nbsp;<code>track</code>&nbsp;và&nbsp;<code>getRankOfNumber</code> đều nhỏ hơn hoặc bằng 2000.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Với một stream, ta cần biết có bao nhiêu giá trị đã chèn $\le x$. Nếu mỗi lần query lại sort hoặc quét toàn bộ thì độ phức tạp là tuyến tính.
>
> Miền giá trị có kích thước khoảng $5\times 10^4$, nên Fenwick tree có thể cập nhật và query tổng prefix trong $O(\log U)$.
>
> `track` cộng thêm một giá trị tại $x+1$ (đánh số từ 1); `getRankOfNumber` tính tổng trên đoạn $[1,x+1]$. Kích thước $50010$ bao phủ miền giá trị đã nêu.

<!-- thinking:end -->

Chúng ta có thể sử dụng Binary Indexed Tree (còn gọi là Fenwick Tree) để duy trì số lượng các số nhỏ hơn hoặc bằng số hiện tại trong những số đã được thêm vào.

Chúng ta tạo một Binary Indexed Tree có độ dài là $50010$. Với method `track`, chúng ta tăng số hiện tại lên 1 rồi thêm nó vào Binary Indexed Tree. Với method `getRankOfNumber`, chúng ta query trực tiếp số lượng các số nhỏ hơn hoặc bằng $x + 1$ trong Binary Indexed Tree.

Về độ phức tạp thời gian, cả thao tác update và query của Binary Indexed Tree đều có độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là độ dài của Binary Indexed Tree.

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


class StreamRank:

    def __init__(self):
        self.tree = BinaryIndexedTree(50010)

    def track(self, x: int) -> None:
        self.tree.update(x + 1, 1)

    def getRankOfNumber(self, x: int) -> int:
        return self.tree.query(x + 1)


# Your StreamRank object will be instantiated and called as such:
# obj = StreamRank()
# obj.track(x)
# param_2 = obj.getRankOfNumber(x)
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

class StreamRank {
    private BinaryIndexedTree tree = new BinaryIndexedTree(50010);

    public StreamRank() {

    }

    public void track(int x) {
        tree.update(x + 1, 1);
    }

    public int getRankOfNumber(int x) {
        return tree.query(x + 1);
    }
}

/**
 * Your StreamRank object will be instantiated and called as such:
 * StreamRank obj = new StreamRank();
 * obj.track(x);
 * int param_2 = obj.getRankOfNumber(x);
 */
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

class StreamRank {
public:
    StreamRank() {
    }

    void track(int x) {
        tree->update(x + 1, 1);
    }

    int getRankOfNumber(int x) {
        return tree->query(x + 1);
    }

private:
    BinaryIndexedTree* tree = new BinaryIndexedTree(50010);
};

/**
 * Your StreamRank object will be instantiated and called as such:
 * StreamRank* obj = new StreamRank();
 * obj->track(x);
 * int param_2 = obj->getRankOfNumber(x);
 */
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

type StreamRank struct {
	tree *BinaryIndexedTree
}

func Constructor() StreamRank {
	return StreamRank{NewBinaryIndexedTree(50010)}
}

func (this *StreamRank) Track(x int) {
	this.tree.update(x+1, 1)
}

func (this *StreamRank) GetRankOfNumber(x int) int {
	return this.tree.query(x + 1)
}

/**
 * Your StreamRank object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Track(x);
 * param_2 := obj.GetRankOfNumber(x);
 */
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

class StreamRank {
    private tree: BinaryIndexedTree = new BinaryIndexedTree(50010);

    constructor() {}

    track(x: number): void {
        this.tree.update(x + 1, 1);
    }

    getRankOfNumber(x: number): number {
        return this.tree.query(x + 1);
    }
}

/**
 * Your StreamRank object will be instantiated and called as such:
 * var obj = new StreamRank()
 * obj.track(x)
 * var param_2 = obj.getRankOfNumber(x)
 */
```

#### Swift

```swift
class BinaryIndexedTree {
    private var n: Int
    private var c: [Int]

    init(_ n: Int) {
        self.n = n
        self.c = Array(repeating: 0, count: n + 1)
    }

    func update(_ x: Int, _ delta: Int) {
        var idx = x
        while idx <= n {
            c[idx] += delta
            idx += (idx & -idx)
        }
    }

    func query(_ x: Int) -> Int {
        var sum = 0
        var idx = x
        while idx > 0 {
            sum += c[idx]
            idx -= (idx & -idx)
        }
        return sum
    }
}

class StreamRank {
    private var tree: BinaryIndexedTree

    init() {
        tree = BinaryIndexedTree(50010)
    }

    func track(_ x: Int) {
        tree.update(x + 1, 1)
    }

    func getRankOfNumber(_ x: Int) -> Int {
        return tree.query(x + 1)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
