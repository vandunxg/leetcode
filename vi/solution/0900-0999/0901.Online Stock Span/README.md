---
comments: true
difficulty: Medium
tags:
    - Stack
    - Design
    - Data Stream
    - Monotonic Stack
---

<!-- problem:start -->

# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span)

[中文文档](/solution/0900-0999/0901.Online%20Stock%20Span/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế thuật toán ghi nhận giá cổ phiếu hằng ngày và trả về <strong>span</strong> của giá cổ phiếu trong ngày hiện tại.</p>

<p><strong>Span</strong> của giá cổ phiếu trong một ngày là số ngày liên tiếp lớn nhất (bắt đầu từ ngày đó rồi lùi về trước) mà giá cổ phiếu nhỏ hơn hoặc bằng giá của ngày đó.</p>

<ul>
	<li>Ví dụ, nếu giá cổ phiếu trong bốn ngày gần nhất là <code>[7,2,1,2]</code> và giá hôm nay là 2, thì span hôm nay bằng 3 vì tính từ hôm nay trở về trước, giá cổ phiếu nhỏ hơn hoặc bằng 2 trong 3 ngày liên tiếp.</li>
	<li>Tương tự, nếu giá cổ phiếu trong bốn ngày gần nhất là <code>[7,34,1,2]</code> và giá hôm nay là <code>8</code>, thì span hôm nay bằng <code>3</code> vì tính từ hôm nay trở về trước, giá cổ phiếu nhỏ hơn hoặc bằng <code>8</code> trong <code>3</code> ngày liên tiếp.</li>
</ul>

<p>Hãy triển khai class <code>StockSpanner</code>:</p>

<ul>
	<li><code>StockSpanner()</code> khởi tạo đối tượng của class.</li>
	<li><code>int next(int price)</code> trả về <strong>span</strong> của giá cổ phiếu khi biết giá hôm nay là <code>price</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;StockSpanner&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;]
[[], [100], [80], [60], [70], [60], [75], [85]]
<strong>Output</strong>
[null, 1, 1, 1, 2, 1, 4, 6]

<strong>Giải thích</strong>
StockSpanner stockSpanner = new StockSpanner();
stockSpanner.next(100); // return 1
stockSpanner.next(80);  // return 1
stockSpanner.next(60);  // return 1
stockSpanner.next(70);  // return 2
stockSpanner.next(60);  // return 1
stockSpanner.next(75);  // return 4, because the last 4 prices (including today&#39;s price of 75) were less than or equal to today&#39;s price.
stockSpanner.next(85);  // return 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= price &lt;= 10<sup>5</sup></code></li>
	<li>Sẽ có nhiều nhất <code>10<sup>4</sup></code> lần gọi <code>next</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Nếu mỗi truy vấn đều lần ngược từ hôm nay đến khi gặp giá cao hơn, tổng thời gian sẽ tăng theo bậc hai. Không cần đếm lại các span đã được tính: nếu giá ở đầu stack nhỏ hơn hoặc bằng $price$ hôm nay, ta có thể gộp span của nó vào kết quả.
>
> Dùng stack giảm dần theo giá, lưu các cặp $(price, cnt)$. Ta pop và cộng dồn các span đó trước khi push giá mới. Mỗi mức giá chỉ được thêm và lấy ra một lần, nên mỗi truy vấn có độ phức tạp amortized hằng số.

<!-- thinking:end -->

Theo đề bài, với giá $price$ của ngày hiện tại, ta lùi về trước để tìm mức giá đầu tiên cao hơn giá này. Khoảng cách chỉ số $cnt$ giữa hai mức giá chính là span của ngày hiện tại.

Đây là dạng bài monotonic stack kinh điển: tìm phần tử đầu tiên bên trái lớn hơn phần tử hiện tại.

Ta duy trì một stack có giá giảm dần từ đáy lên đỉnh. Mỗi phần tử trong stack là cặp dữ liệu $(price, cnt)$, trong đó $price$ là giá và $cnt$ là span tương ứng.

Khi giá $price$ xuất hiện, ta so sánh nó với phần tử trên đỉnh stack. Nếu giá của phần tử đó nhỏ hơn hoặc bằng $price$, ta cộng span $cnt$ của ngày hiện tại với span của phần tử trên đỉnh rồi pop phần tử đó. Lặp lại cho đến khi giá ở đỉnh stack lớn hơn $price$ hoặc stack rỗng.

Cuối cùng, ta push $(price, cnt)$ vào stack và trả về $cnt$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lần gọi `next(price)`.

<!-- tabs:start -->

#### Python3

```python
class StockSpanner:
    def __init__(self):
        self.stk = []

    def next(self, price: int) -> int:
        cnt = 1
        while self.stk and self.stk[-1][0] <= price:
            cnt += self.stk.pop()[1]
        self.stk.append((price, cnt))
        return cnt


# Your StockSpanner object will be instantiated and called as such:
# obj = StockSpanner()
# param_1 = obj.next(price)
```

#### Java

```java
class StockSpanner {
    private Deque<int[]> stk = new ArrayDeque<>();

    public StockSpanner() {
    }

    public int next(int price) {
        int cnt = 1;
        while (!stk.isEmpty() && stk.peek()[0] <= price) {
            cnt += stk.pop()[1];
        }
        stk.push(new int[] {price, cnt});
        return cnt;
    }
}

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner obj = new StockSpanner();
 * int param_1 = obj.next(price);
 */
```

#### C++

```cpp
class StockSpanner {
public:
    StockSpanner() {
    }

    int next(int price) {
        int cnt = 1;
        while (!stk.empty() && stk.top().first <= price) {
            cnt += stk.top().second;
            stk.pop();
        }
        stk.emplace(price, cnt);
        return cnt;
    }

private:
    stack<pair<int, int>> stk;
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```

#### Go

```go
type StockSpanner struct {
	stk []pair
}

func Constructor() StockSpanner {
	return StockSpanner{[]pair{}}
}

func (this *StockSpanner) Next(price int) int {
	cnt := 1
	for len(this.stk) > 0 && this.stk[len(this.stk)-1].price <= price {
		cnt += this.stk[len(this.stk)-1].cnt
		this.stk = this.stk[:len(this.stk)-1]
	}
	this.stk = append(this.stk, pair{price, cnt})
	return cnt
}

type pair struct{ price, cnt int }

/**
 * Your StockSpanner object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Next(price);
 */
```

#### TypeScript

```ts
class StockSpanner {
    private stk: number[][];

    constructor() {
        this.stk = [];
    }

    next(price: number): number {
        let cnt = 1;
        while (this.stk.length && this.stk.at(-1)[0] <= price) {
            cnt += this.stk.pop()[1];
        }
        this.stk.push([price, cnt]);
        return cnt;
    }
}

/**
 * Your StockSpanner object will be instantiated and called as such:
 * var obj = new StockSpanner()
 * var param_1 = obj.next(price)
 */
```

#### Rust

```rust
use std::collections::VecDeque;
struct StockSpanner {
    stk: VecDeque<(i32, i32)>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl StockSpanner {
    fn new() -> Self {
        Self {
            stk: vec![(i32::MAX, -1)].into_iter().collect(),
        }
    }

    fn next(&mut self, price: i32) -> i32 {
        let mut cnt = 1;
        while self.stk.back().unwrap().0 <= price {
            cnt += self.stk.pop_back().unwrap().1;
        }
        self.stk.push_back((price, cnt));
        cnt
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
