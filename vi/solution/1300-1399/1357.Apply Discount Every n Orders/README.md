---
comments: true
difficulty: Medium
rating: 1429
source: Biweekly Contest 20 Q2
tags:
    - Design
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1357. Apply Discount Every n Orders](https://leetcode.com/problems/apply-discount-every-n-orders)

[中文文档](/solution/1300-1399/1357.Apply%20Discount%20Every%20n%20Orders/README.md)

## Mô tả

<!-- description:start -->

<p>Một siêu thị có đông khách hàng. Các sản phẩm được bán tại đây được biểu diễn bằng hai mảng số nguyên song song <code>products</code> và <code>prices</code>, trong đó sản phẩm thứ <code>i<sup>th</sup></code> có ID là <code>products[i]</code> và giá là <code>prices[i]</code>.</p>

<p>Khi khách hàng thanh toán, hóa đơn được biểu diễn bằng hai mảng số nguyên song song <code>product</code> và <code>amount</code>. Sản phẩm thứ <code>j<sup>th</sup></code> mà họ mua có ID là <code>product[j]</code>, còn <code>amount[j]</code> là số lượng sản phẩm đã mua. Tổng phụ được tính bằng tổng của từng <code>amount[j] * (price of the j<sup>th</sup> product)</code>.</p>

<p>Siêu thị tổ chức khuyến mãi: cứ mỗi khách hàng thứ <code>n<sup>th</sup></code> thanh toán sẽ được giảm giá theo <strong>tỷ lệ phần trăm</strong>. Mức giảm được cho bởi <code>discount</code>, tức khách hàng được giảm <code>discount</code> phần trăm trên tổng phụ. Cụ thể, nếu tổng phụ là <code>bill</code>, số tiền thực trả là <code>bill * ((100 - discount) / 100)</code>.</p>

<p>Cài đặt class <code>Cashier</code>:</p>

<ul>
	<li><code>Cashier(int n, int discount, int[] products, int[] prices)</code> khởi tạo đối tượng với <code>n</code>, mức <code>discount</code>, danh sách <code>products</code> và giá tương ứng <code>prices</code>.</li>
	<li><code>double getBill(int[] product, int[] amount)</code> trả về tổng hóa đơn cuối cùng sau khi áp dụng giảm giá (nếu có). Chấp nhận đáp án sai lệch không quá <code>10<sup>-5</sup></code> so với giá trị thực.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Cashier&quot;,&quot;getBill&quot;,&quot;getBill&quot;,&quot;getBill&quot;,&quot;getBill&quot;,&quot;getBill&quot;,&quot;getBill&quot;,&quot;getBill&quot;]
[[3,50,[1,2,3,4,5,6,7],[100,200,300,400,300,200,100]],[[1,2],[1,2]],[[3,7],[10,10]],[[1,2,3,4,5,6,7],[1,1,1,1,1,1,1]],[[4],[10]],[[7,3],[10,10]],[[7,5,3,1,6,4,2],[10,10,10,9,9,9,7]],[[2,3,5],[5,3,2]]]
<strong>Đầu ra</strong>
[null,500.0,4000.0,800.0,4000.0,4000.0,7350.0,2500.0]
<strong>Giải thích</strong>
Cashier cashier = new Cashier(3,50,[1,2,3,4,5,6,7],[100,200,300,400,300,200,100]);
cashier.getBill([1,2],[1,2]);                        // return 500.0. 1<sup>st</sup> customer, no discount.
                                                     // bill = 1 * 100 + 2 * 200 = 500.
cashier.getBill([3,7],[10,10]);                      // return 4000.0. 2<sup>nd</sup> customer, no discount.
                                                     // bill = 10 * 300 + 10 * 100 = 4000.
cashier.getBill([1,2,3,4,5,6,7],[1,1,1,1,1,1,1]);    // return 800.0. 3<sup>rd</sup> customer, 50% discount.
                                                     // Original bill = 1600
                                                     // Actual bill = 1600 * ((100 - 50) / 100) = 800.
cashier.getBill([4],[10]);                           // return 4000.0. 4<sup>th</sup> customer, no discount.
cashier.getBill([7,3],[10,10]);                      // return 4000.0. 5<sup>th</sup> customer, no discount.
cashier.getBill([7,5,3,1,6,4,2],[10,10,10,9,9,9,7]); // return 7350.0. 6<sup>th</sup> customer, 50% discount.
                                                     // Original bill = 14700, but with
                                                     // Actual bill = 14700 * ((100 - 50) / 100) = 7350.
cashier.getBill([2,3,5],[5,3,2]);                    // return 2500.0.  7<sup>th</sup> customer, no discount.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= discount &lt;= 100</code></li>
	<li><code>1 &lt;= products.length &lt;= 200</code></li>
	<li><code>prices.length == products.length</code></li>
	<li><code>1 &lt;= products[i] &lt;= 200</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 1000</code></li>
	<li>Các phần tử trong <code>products</code> là <strong>duy nhất</strong>.</li>
	<li><code>1 &lt;= product.length &lt;= products.length</code></li>
	<li><code>amount.length == product.length</code></li>
	<li><code>product[j]</code> có trong <code>products</code>.</li>
	<li><code>1 &lt;= amount[j] &lt;= 1000</code></li>
	<li>Các phần tử của <code>product</code> là <strong>duy nhất</strong>.</li>
	<li><code>getBill</code> được gọi nhiều nhất <code>1000</code> lần.</li>
	<li>Chấp nhận đáp án sai lệch không quá <code>10<sup>-5</sup></code> so với giá trị thực.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Cứ mỗi khách hàng thứ $n$ sẽ được giảm giá trên toàn bộ hóa đơn. Ta lưu ánh xạ từ product ID sang giá trong hash table một lần khi khởi tạo. Bộ đếm lấy modulo $n$ để áp dụng giảm giá khi giá trị trở về $0$, nếu không thì trả về tổng chưa giảm.

<!-- thinking:end -->

Ta dùng hash table $d$ để lưu product ID và đơn giá, ánh xạ từng phần tử trong `products` với giá tương ứng trong `prices` khi khởi tạo.

Ta cũng duy trì bộ đếm khách hàng $i$, khởi tạo bằng $0$.

Với mỗi lần gọi `getBill`:

1. Tăng bộ đếm rồi lấy modulo: $i = (i + 1) \bmod n$, để xác định khách hàng hiện tại đang thanh toán;
2. Duyệt các ID sản phẩm đã mua và số lượng tương ứng, tính tổng hóa đơn $x = \sum_j d[\textit{product}[j]] \times \textit{amount}[j]$;
3. Nếu $i = 0$, khách hiện tại là khách hàng thứ $n$ và toàn bộ hóa đơn được giảm giá; trả về $x - \dfrac{\textit{discount} \times x}{100}$. Nếu không, trả về trực tiếp $x$.

Độ phức tạp thời gian khởi tạo là $O(n)$, với $n$ là số loại sản phẩm. Mỗi lần gọi `getBill` mất $O(m)$ thời gian, với $m$ là số loại sản phẩm trong lần mua hiện tại. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Cashier:

    def __init__(self, n: int, discount: int, products: List[int], prices: List[int]):
        self.i = 0
        self.n = n
        self.discount = discount
        self.d = {a: b for a, b in zip(products, prices)}

    def getBill(self, product: List[int], amount: List[int]) -> float:
        self.i = (self.i + 1) % self.n
        x = sum(self.d[a] * b for a, b in zip(product, amount))
        if self.i == 0:
            return x - (self.discount * x) / 100
        return x


# Your Cashier object will be instantiated and called as such:
# obj = Cashier(n, discount, products, prices)
# param_1 = obj.getBill(product,amount)
```

#### Java

```java
class Cashier {
    private int i;
    private int n;
    private int discount;
    private Map<Integer, Integer> d;

    public Cashier(int n, int discount, int[] products, int[] prices) {
        this.i = 0;
        this.n = n;
        this.discount = discount;
        this.d = new HashMap<>();
        for (int j = 0; j < products.length; j++) {
            this.d.put(products[j], prices[j]);
        }
    }

    public double getBill(int[] product, int[] amount) {
        this.i = (this.i + 1) % this.n;
        double x = 0;
        for (int j = 0; j < product.length; j++) {
            x += this.d.get(product[j]) * amount[j];
        }
        if (this.i == 0) {
            return x - (this.discount * x) / 100.0;
        }
        return x;
    }
}

/**
 * Your Cashier object will be instantiated and called as such:
 * Cashier obj = new Cashier(n, discount, products, prices);
 * double param_1 = obj.getBill(product,amount);
 */
```

#### C++

```cpp
class Cashier {
public:
    int i;
    int n;
    int discount;
    unordered_map<int, int> d;

    Cashier(int n, int discount, vector<int>& products, vector<int>& prices) {
        this->i = 0;
        this->n = n;
        this->discount = discount;
        for (int j = 0; j < products.size(); j++) {
            d[products[j]] = prices[j];
        }
    }

    double getBill(vector<int> product, vector<int> amount) {
        i = (i + 1) % n;
        double x = 0;
        for (int j = 0; j < product.size(); j++) {
            x += d[product[j]] * amount[j];
        }
        if (i == 0) {
            return x - (discount * x) / 100.0;
        }
        return x;
    }
};

/**
 * Your Cashier object will be instantiated and called as such:
 * Cashier* obj = new Cashier(n, discount, products, prices);
 * double param_1 = obj->getBill(product,amount);
 */
```

#### Go

```go
type Cashier struct {
	i        int
	n        int
	discount int
	d        map[int]int
}

func Constructor(n int, discount int, products []int, prices []int) Cashier {
	d := make(map[int]int)
	for i := 0; i < len(products); i++ {
		d[products[i]] = prices[i]
	}
	return Cashier{i: 0, n: n, discount: discount, d: d}
}

func (this *Cashier) GetBill(product []int, amount []int) float64 {
	this.i = (this.i + 1) % this.n
	x := 0
	for i := 0; i < len(product); i++ {
		x += this.d[product[i]] * amount[i]
	}
	if this.i == 0 {
		return float64(x) - float64(this.discount)*float64(x)/100.0
	}
	return float64(x)
}

/**
 * Your Cashier object will be instantiated and called as such:
 * obj := Constructor(n, discount, products, prices);
 * param_1 := obj.GetBill(product,amount);
 */
```

#### TypeScript

```ts
class Cashier {
    i: number;
    n: number;
    discount: number;
    d: Map<number, number>;

    constructor(n: number, discount: number, products: number[], prices: number[]) {
        this.i = 0;
        this.n = n;
        this.discount = discount;
        this.d = new Map();
        for (let j = 0; j < products.length; j++) {
            this.d.set(products[j], prices[j]);
        }
    }

    getBill(product: number[], amount: number[]): number {
        this.i = (this.i + 1) % this.n;
        let x = 0;
        for (let j = 0; j < product.length; j++) {
            x += (this.d.get(product[j]) || 0) * amount[j];
        }
        if (this.i === 0) {
            return x - (this.discount * x) / 100;
        }
        return x;
    }
}

/**
 * Your Cashier object will be instantiated and called as such:
 * var obj = new Cashier(n, discount, products, prices)
 * var param_1 = obj.getBill(product,amount)
 */
```

#### Rust

```rust
use std::cell::Cell;
use std::collections::HashMap;

struct Cashier {
    i: Cell<i32>,
    n: i32,
    discount: i32,
    d: HashMap<i32, i32>,
}

impl Cashier {
    fn new(n: i32, discount: i32, products: Vec<i32>, prices: Vec<i32>) -> Self {
        let mut d = HashMap::new();
        for i in 0..products.len() {
            d.insert(products[i], prices[i]);
        }
        Cashier {
            i: Cell::new(0),
            n,
            discount,
            d,
        }
    }

    fn get_bill(&self, product: Vec<i32>, amount: Vec<i32>) -> f64 {
        let mut x = 0i64;
        let mut i = self.i.get();
        i = (i + 1) % self.n;
        self.i.set(i);

        for j in 0..product.len() {
            x += (self.d[&product[j]] as i64) * (amount[j] as i64);
        }

        if i == 0 {
            return x as f64 - (self.discount as f64) * (x as f64) / 100.0;
        }
        x as f64
    }
}

// Your Cashier object will be instantiated and called as such:
// let obj = Cashier::new(n, discount, products, prices);
// let ret_1: f64 = obj.get_bill(product, amount);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
