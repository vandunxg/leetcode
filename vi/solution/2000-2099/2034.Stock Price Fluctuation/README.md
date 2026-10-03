---
comments: true
difficulty: Medium
rating: 1831
source: Weekly Contest 262 Q3
tags:
    - Design
    - Hash Table
    - Data Stream
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2034. Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation)

[中文文档](/solution/2000-2099/2034.Stock%20Price%20Fluctuation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một luồng <strong>bản ghi</strong> về một loại cổ phiếu. Mỗi bản ghi gồm một <strong>timestamp</strong> và <strong>price</strong> tương ứng của cổ phiếu tại timestamp đó.</p>

<p>Đáng tiếc là do thị trường chứng khoán biến động, các bản ghi không được gửi theo thứ tự. Tệ hơn nữa, một số bản ghi có thể không chính xác. Một bản ghi khác có cùng timestamp có thể xuất hiện sau đó trong luồng để <strong>điều chỉnh</strong> price của bản ghi sai trước đó.</p>

<p>Hãy thiết kế một thuật toán có thể:</p>

<ul>
	<li><strong>Cập nhật</strong> price của cổ phiếu tại một timestamp cụ thể, <strong>điều chỉnh</strong> price từ các bản ghi trước đó tại timestamp này.</li>
	<li>Tìm <strong>price mới nhất</strong> của cổ phiếu dựa trên các bản ghi hiện tại. <strong>Price mới nhất</strong> là price tại timestamp mới nhất đã được ghi nhận.</li>
	<li>Tìm <strong>price lớn nhất</strong> mà cổ phiếu từng có dựa trên các bản ghi hiện tại.</li>
	<li>Tìm <strong>price nhỏ nhất</strong> mà cổ phiếu từng có dựa trên các bản ghi hiện tại.</li>
</ul>

<p>Hãy triển khai class <code>StockPrice</code>:</p>

<ul>
	<li><code>StockPrice()</code> Khởi tạo đối tượng không có bản ghi price.</li>
	<li><code>void update(int timestamp, int price)</code> Cập nhật <code>price</code> của cổ phiếu tại <code>timestamp</code> đã cho.</li>
	<li><code>int current()</code> Trả về <strong>price mới nhất</strong> của cổ phiếu.</li>
	<li><code>int maximum()</code> Trả về <strong>price lớn nhất</strong> của cổ phiếu.</li>
	<li><code>int minimum()</code> Trả về <strong>price nhỏ nhất</strong> của cổ phiếu.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;StockPrice&quot;, &quot;update&quot;, &quot;update&quot;, &quot;current&quot;, &quot;maximum&quot;, &quot;update&quot;, &quot;maximum&quot;, &quot;update&quot;, &quot;minimum&quot;]
[[], [1, 10], [2, 5], [], [], [1, 3], [], [4, 2], []]
<strong>Đầu ra</strong>
[null, null, null, 5, 10, null, 5, null, 2]

<strong>Giải thích</strong>
StockPrice stockPrice = new StockPrice();
stockPrice.update(1, 10); // Timestamps are [1] with corresponding prices [10].
stockPrice.update(2, 5);  // Timestamps are [1,2] with corresponding prices [10,5].
stockPrice.current();     // return 5, the latest timestamp is 2 with the price being 5.
stockPrice.maximum();     // return 10, the maximum price is 10 at timestamp 1.
stockPrice.update(1, 3);  // The previous timestamp 1 had the wrong price, so it is updated to 3.
                          // Timestamps are [1,2] with corresponding prices [3,5].
stockPrice.maximum();     // return 5, the maximum price is 5 after the correction.
stockPrice.update(4, 2);  // Timestamps are [1,2,4] with corresponding prices [3,5,2].
stockPrice.minimum();     // return 2, the minimum price is 2 at timestamp 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= timestamp, price &lt;= 10<sup>9</sup></code></li>
    <li>Có nhiều nhất <code>10<sup>5</sup></code> lần gọi <strong>tổng cộng</strong> đến <code>update</code>, <code>current</code>, <code>maximum</code> và <code>minimum</code>.</li>
    <li><code>current</code>, <code>maximum</code> và <code>minimum</code> chỉ được gọi <strong>sau khi</strong> <code>update</code> đã được gọi <strong>ít nhất một lần</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần điều chỉnh các timestamp trước đó và truy vấn price hiện tại, lớn nhất và nhỏ nhất trong $10^5$ phép toán. Chỉ dùng hash map thì không thể lấy các giá trị cực trị; chỉ dùng sorted set thì không thể ghi đè theo thời gian.
>
> Map $d$ lưu timestamp$\to$price; ordered multiset $ls$ lưu các price hiện tại. Khi update, xóa price cũ nếu timestamp đã tồn tại, thêm price mới, rồi theo dõi timestamp mới nhất $last$.
>
> current là $d[last]$; các giá trị cực trị là hai đầu của $ls$.

<!-- thinking:end -->

Ta định nghĩa các cấu trúc dữ liệu hoặc biến sau:

- `d`: hash table lưu timestamp và price tương ứng;
- `ls`: ordered set lưu tất cả price;
- `last`: timestamp của lần update cuối cùng.

Sau đó, ta có thể thực hiện các thao tác sau:

- `update(timestamp, price)`: cập nhật price tương ứng với timestamp `timestamp` thành `price`. Nếu `timestamp` đã tồn tại, trước tiên ta cần xóa price tương ứng khỏi ordered set, rồi cập nhật nó thành `price`. Nếu không, ta cập nhật trực tiếp thành `price`. Sau đó, ta cập nhật `last` thành `max(last, timestamp)`. Độ phức tạp thời gian là O(log n).
- `current()`: trả về price tương ứng với `last`. Độ phức tạp thời gian là $O(1)$.
- `maximum()`: trả về giá trị lớn nhất trong ordered set. Độ phức tạp thời gian là $O(\log n)$.
- `minimum()`: trả về giá trị nhỏ nhất trong ordered set. Độ phức tạp thời gian là $O(\log n)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số phép toán `update`.

<!-- tabs:start -->

#### Python3

```python
class StockPrice:
    def __init__(self):
        self.d = {}
        self.ls = SortedList()
        self.last = 0

    def update(self, timestamp: int, price: int) -> None:
        if timestamp in self.d:
            self.ls.remove(self.d[timestamp])
        self.d[timestamp] = price
        self.ls.add(price)
        self.last = max(self.last, timestamp)

    def current(self) -> int:
        return self.d[self.last]

    def maximum(self) -> int:
        return self.ls[-1]

    def minimum(self) -> int:
        return self.ls[0]


# Your StockPrice object will be instantiated and called as such:
# obj = StockPrice()
# obj.update(timestamp,price)
# param_2 = obj.current()
# param_3 = obj.maximum()
# param_4 = obj.minimum()
```

#### Java

```java
class StockPrice {
    private Map<Integer, Integer> d = new HashMap<>();
    private TreeMap<Integer, Integer> ls = new TreeMap<>();
    private int last;

    public StockPrice() {
    }

    public void update(int timestamp, int price) {
        if (d.containsKey(timestamp)) {
            int old = d.get(timestamp);
            if (ls.merge(old, -1, Integer::sum) == 0) {
                ls.remove(old);
            }
        }
        d.put(timestamp, price);
        ls.merge(price, 1, Integer::sum);
        last = Math.max(last, timestamp);
    }

    public int current() {
        return d.get(last);
    }

    public int maximum() {
        return ls.lastKey();
    }

    public int minimum() {
        return ls.firstKey();
    }
}

/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice obj = new StockPrice();
 * obj.update(timestamp,price);
 * int param_2 = obj.current();
 * int param_3 = obj.maximum();
 * int param_4 = obj.minimum();
 */
```

#### C++

```cpp
class StockPrice {
public:
    StockPrice() {
    }

    void update(int timestamp, int price) {
        if (d.count(timestamp)) {
            ls.erase(ls.find(d[timestamp]));
        }
        d[timestamp] = price;
        ls.insert(price);
        last = max(last, timestamp);
    }

    int current() {
        return d[last];
    }

    int maximum() {
        return *ls.rbegin();
    }

    int minimum() {
        return *ls.begin();
    }

private:
    unordered_map<int, int> d;
    multiset<int> ls;
    int last = 0;
};

/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice* obj = new StockPrice();
 * obj->update(timestamp,price);
 * int param_2 = obj->current();
 * int param_3 = obj->maximum();
 * int param_4 = obj->minimum();
 */
```

#### Go

```go
type StockPrice struct {
	d    map[int]int
	ls   *redblacktree.Tree
	last int
}

func Constructor() StockPrice {
	return StockPrice{
		d:    make(map[int]int),
		ls:   redblacktree.NewWithIntComparator(),
		last: 0,
	}
}

func (this *StockPrice) Update(timestamp int, price int) {
	merge := func(rbt *redblacktree.Tree, key, value int) {
		if v, ok := rbt.Get(key); ok {
			nxt := v.(int) + value
			if nxt == 0 {
				rbt.Remove(key)
			} else {
				rbt.Put(key, nxt)
			}
		} else {
			rbt.Put(key, value)
		}
	}
	if v, ok := this.d[timestamp]; ok {
		merge(this.ls, v, -1)
	}
	this.d[timestamp] = price
	merge(this.ls, price, 1)
	this.last = max(this.last, timestamp)
}

func (this *StockPrice) Current() int {
	return this.d[this.last]
}

func (this *StockPrice) Maximum() int {
	return this.ls.Right().Key.(int)
}

func (this *StockPrice) Minimum() int {
	return this.ls.Left().Key.(int)
}

/**
 * Your StockPrice object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Update(timestamp,price);
 * param_2 := obj.Current();
 * param_3 := obj.Maximum();
 * param_4 := obj.Minimum();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
