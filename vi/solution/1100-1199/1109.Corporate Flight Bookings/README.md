---
comments: true
difficulty: Medium
rating: 1569
source: Weekly Contest 144 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1109. Corporate Flight Bookings](https://leetcode.com/problems/corporate-flight-bookings)

[中文文档](/solution/1100-1199/1109.Corporate%20Flight%20Bookings/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> chuyến bay được đánh số từ <code>1</code> đến <code>n</code>.</p>

<p>Bạn được cho mảng đặt chỗ chuyến bay <code>bookings</code>, trong đó <code>bookings[i] = [first<sub>i</sub>, last<sub>i</sub>, seats<sub>i</sub>]</code> biểu thị việc đặt <code>seats<sub>i</sub></code> chỗ cho <strong>mỗi chuyến bay</strong> từ <code>first<sub>i</sub></code> đến <code>last<sub>i</sub></code> (<strong>bao gồm cả hai đầu</strong>).</p>

<p>Trả về <em>mảng </em><code>answer</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là tổng số chỗ đã đặt cho chuyến bay </em><code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
<strong>Output:</strong> [10,55,45,25,25]
<strong>Giải thích:</strong>
Nhãn chuyến bay:        1   2   3   4   5
Đặt chỗ 1:  10  10
Đặt chỗ 2:      20  20
Đặt chỗ 3:      25  25  25  25
Tổng số chỗ:         10  55  45  25  25
Vậy, answer = [10,55,45,25,25]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> bookings = [[1,2,10],[2,2,15]], n = 2
<strong>Output:</strong> [10,25]
<strong>Giải thích:</strong>
Nhãn chuyến bay:        1   2
Đặt chỗ 1:  10  10
Đặt chỗ 2:      15
Tổng số chỗ:         10  25
Vậy, answer = [10,25]

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= bookings.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>bookings[i].length == 3</code></li>
	<li><code>1 &lt;= first<sub>i</sub> &lt;= last<sub>i</sub> &lt;= n</code></li>
	<li><code>1 &lt;= seats<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt đặt chỗ cộng cùng một lượng trên đoạn đóng $[\textit{first},\textit{last}]$. Cập nhật từng chuyến bay trong đoạn sẽ tốn thời gian tỷ lệ với tổng độ dài các đoạn, trường hợp xấu nhất là $O(nm)$.
>
> Mảng hiệu cộng $\textit{seats}$ tại đầu trái và trừ lượng đó tại $\textit{last}+1$, nên mỗi lần cập nhật đoạn chỉ cần hai lần cập nhật điểm. Cuối cùng, tính prefix sum để khôi phục số chỗ đã đặt trên mỗi chuyến bay.

<!-- thinking:end -->

Mỗi lượt đặt chỗ dành `seats` chỗ trên tất cả chuyến bay thuộc đoạn `[first, last]`. Vì vậy, ta có thể dùng mảng hiệu. Với mỗi lượt đặt, cộng `seats` tại vị trí `first` và trừ `seats` tại vị trí `last + 1`. Cuối cùng, tính prefix sum của mảng hiệu để tìm tổng số chỗ đã đặt cho từng chuyến bay.

Độ phức tạp thời gian là $O(n)$, với $n$ là số chuyến bay. Không tính bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def corpFlightBookings(self, bookings: List[List[int]], n: int) -> List[int]:
        ans = [0] * n
        for first, last, seats in bookings:
            ans[first - 1] += seats
            if last < n:
                ans[last] -= seats
        return list(accumulate(ans))
```

#### Java

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int[] ans = new int[n];
        for (var e : bookings) {
            int first = e[0], last = e[1], seats = e[2];
            ans[first - 1] += seats;
            if (last < n) {
                ans[last] -= seats;
            }
        }
        for (int i = 1; i < n; ++i) {
            ans[i] += ans[i - 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        vector<int> ans(n);
        for (auto& e : bookings) {
            int first = e[0], last = e[1], seats = e[2];
            ans[first - 1] += seats;
            if (last < n) {
                ans[last] -= seats;
            }
        }
        for (int i = 1; i < n; ++i) {
            ans[i] += ans[i - 1];
        }
        return ans;
    }
};
```

#### Go

```go
func corpFlightBookings(bookings [][]int, n int) []int {
	ans := make([]int, n)
	for _, e := range bookings {
		first, last, seats := e[0], e[1], e[2]
		ans[first-1] += seats
		if last < n {
			ans[last] -= seats
		}
	}
	for i := 1; i < n; i++ {
		ans[i] += ans[i-1]
	}
	return ans
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn corp_flight_bookings(bookings: Vec<Vec<i32>>, n: i32) -> Vec<i32> {
        let mut ans = vec![0; n as usize];

        // Build the difference vector first
        for b in &bookings {
            let (l, r) = ((b[0] as usize) - 1, (b[1] as usize) - 1);
            ans[l] += b[2];
            if r < (n as usize) - 1 {
                ans[r + 1] -= b[2];
            }
        }

        // Build the prefix sum vector based on the difference vector
        for i in 1..n as usize {
            ans[i] += ans[i - 1];
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} bookings
 * @param {number} n
 * @return {number[]}
 */
var corpFlightBookings = function (bookings, n) {
    const ans = new Array(n).fill(0);
    for (const [first, last, seats] of bookings) {
        ans[first - 1] += seats;
        if (last < n) {
            ans[last] -= seats;
        }
    }
    for (let i = 1; i < n; ++i) {
        ans[i] += ans[i - 1];
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Binary Indexed Tree + Ý tưởng mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mảng hiệu ở Lời giải 1 cần một lượt tính prefix cuối cùng và xử lý offline. Fenwick tree hỗ trợ cộng tại một điểm và query prefix trong $O(\log n)$: cộng tại $\textit{first}$, trừ tại $\textit{last}+1$, khi đó prefix tại $i$ chính là số chỗ của chuyến bay $i$. Bài này không cần query online, nên cách này chỉ là một phương án cài đặt khác với thêm hệ số log.

<!-- thinking:end -->

Ta cũng có thể kết hợp binary indexed tree với ý tưởng mảng hiệu để thực hiện các thao tác trên. Mỗi lượt đặt chỗ có thể xem là đặt `seats` chỗ cho mọi chuyến bay trong đoạn `[first, last]`. Vì vậy, với mỗi lượt đặt, ta cộng `seats` tại vị trí `first` và trừ `seats` tại vị trí `last + 1` trong binary indexed tree. Cuối cùng, tính prefix sum tại từng vị trí để tìm tổng số chỗ đã đặt cho mỗi chuyến bay.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số chuyến bay.

Sau đây là phần giới thiệu cơ bản về binary indexed tree:

Binary indexed tree, còn được gọi là "Binary Indexed Tree" hoặc Fenwick tree, hỗ trợ hiệu quả hai thao tác sau:

1. **Cập nhật một điểm** `update(x, delta)`: Cộng giá trị delta vào phần tử tại vị trí x trong dãy;
1. **Truy vấn tổng prefix** `query(x)`: Truy vấn tổng trên đoạn `[1,...x]` của dãy, tức prefix sum đến vị trí x.

Độ phức tạp thời gian của mỗi thao tác là $O(\log n)$.

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
    def corpFlightBookings(self, bookings: List[List[int]], n: int) -> List[int]:
        tree = BinaryIndexedTree(n)
        for first, last, seats in bookings:
            tree.update(first, seats)
            tree.update(last + 1, -seats)
        return [tree.query(i + 1) for i in range(n)]
```

#### Java

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (var e : bookings) {
            int first = e[0], last = e[1], seats = e[2];
            tree.update(first, seats);
            tree.update(last + 1, -seats);
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = tree.query(i + 1);
        }
        return ans;
    }
}

class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
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
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }

private:
    int n;
    vector<int> c;
};

class Solution {
public:
    vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
        BinaryIndexedTree* tree = new BinaryIndexedTree(n);
        for (auto& e : bookings) {
            int first = e[0], last = e[1], seats = e[2];
            tree->update(first, seats);
            tree->update(last + 1, -seats);
        }
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            ans[i] = tree->query(i + 1);
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

func newBinaryIndexedTree(n int) *BinaryIndexedTree {
	c := make([]int, n+1)
	return &BinaryIndexedTree{n, c}
}

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += x & -x
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= x & -x
	}
	return s
}

func corpFlightBookings(bookings [][]int, n int) []int {
	tree := newBinaryIndexedTree(n)
	for _, e := range bookings {
		first, last, seats := e[0], e[1], e[2]
		tree.update(first, seats)
		tree.update(last+1, -seats)
	}
	ans := make([]int, n)
	for i := range ans {
		ans[i] = tree.query(i + 1)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
