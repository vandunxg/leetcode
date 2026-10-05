---
comments: true
difficulty: Medium
tags:
    - Design
    - Hash Table
---

<!-- problem:start -->

# [3822. Design Order Management System 🔒](https://leetcode.com/problems/design-order-management-system)

[中文文档](/solution/3800-3899/3822.Design%20Order%20Management%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được yêu cầu thiết kế một hệ thống quản lý order đơn giản cho một nền tảng giao dịch.</p>

<p>Mỗi order gắn với một <code>orderId</code>, một <code>orderType</code> (<code>&quot;buy&quot;</code> hoặc <code>&quot;sell&quot;</code>) và một <code>price</code>.</p>

<p>Một order được xem là <strong>đang hoạt động</strong> trừ khi bị hủy.</p>

<p>Hãy triển khai lớp <code>OrderManagementSystem</code>:</p>

<ul>
	<li><code>OrderManagementSystem()</code>: Khởi tạo hệ thống quản lý order.</li>
	<li><code>void addOrder(int orderId, string orderType, int price)</code>: Thêm một order <strong>đang hoạt động</strong> mới với các thuộc tính đã cho. <strong>Đảm bảo</strong> rằng <code>orderId</code> là duy nhất.</li>
	<li><code>void modifyOrder(int orderId, int newPrice)</code>: Sửa <strong>price</strong> của một order hiện có. <strong>Đảm bảo</strong> rằng order tồn tại và đang <em>hoạt động</em>.</li>
	<li><code>void cancelOrder(int orderId)</code>: Hủy một order hiện có. <strong>Đảm bảo</strong> rằng order tồn tại và đang <em>hoạt động</em>.</li>
	<li><code>vector&lt;int&gt; getOrdersAtPrice(string orderType, int price)</code>: Trả về các <code>orderId</code> của tất cả order <strong>đang hoạt động</strong> khớp với <code>orderType</code> và <code>price</code> đã cho. Nếu không có order nào như vậy, trả về một danh sách rỗng.</li>
</ul>

<p><strong>Lưu ý:</strong> Thứ tự của các <code>orderId</code> được trả về không quan trọng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;OrderManagementSystem&quot;, &quot;addOrder&quot;, &quot;addOrder&quot;, &quot;addOrder&quot;, &quot;getOrdersAtPrice&quot;, &quot;modifyOrder&quot;, &quot;modifyOrder&quot;, &quot;getOrdersAtPrice&quot;, &quot;cancelOrder&quot;, &quot;cancelOrder&quot;, &quot;getOrdersAtPrice&quot;]<br />
[[], [1, &quot;buy&quot;, 1], [2, &quot;buy&quot;, 1], [3, &quot;sell&quot;, 2], [&quot;buy&quot;, 1], [1, 3], [2, 1], [&quot;buy&quot;, 1], [3], [2], [&quot;buy&quot;, 1]]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, null, null, [2, 1], null, null, [2], null, null, []] </span></p>

<p><strong>Giải thích</strong></p>
OrderManagementSystem orderManagementSystem = new OrderManagementSystem();<br />
orderManagementSystem.addOrder(1, &quot;buy&quot;, 1); // Thêm một buy order với ID 1 ở mức giá 1.<br />
orderManagementSystem.addOrder(2, &quot;buy&quot;, 1); // Thêm một buy order với ID 2 ở mức giá 1.<br />
orderManagementSystem.addOrder(3, &quot;sell&quot;, 2); // Thêm một sell order với ID 3 ở mức giá 2.<br />
orderManagementSystem.getOrdersAtPrice(&quot;buy&quot;, 1); // Cả hai buy order (ID 1 và 2) đều đang hoạt động ở mức giá 1, nên kết quả là <code>[2, 1]</code>.<br />
orderManagementSystem.modifyOrder(1, 3); // Order 1 được cập nhật: mức giá trở thành 3.<br />
orderManagementSystem.modifyOrder(2, 1); // Order 2 được cập nhật, nhưng mức giá vẫn là 1.<br />
orderManagementSystem.getOrdersAtPrice(&quot;buy&quot;, 1); // Chỉ order 2 vẫn là buy order đang hoạt động ở mức giá 1, nên kết quả là <code>[2]</code>.<br />
orderManagementSystem.cancelOrder(3); // Sell order với ID 3 bị hủy và xóa khỏi các order đang hoạt động.<br />
orderManagementSystem.cancelOrder(2); // Buy order với ID 2 bị hủy và xóa khỏi các order đang hoạt động.<br />
orderManagementSystem.getOrdersAtPrice(&quot;buy&quot;, 1); // Không còn buy order đang hoạt động nào ở mức giá 1, nên kết quả là <code>[]</code>.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= orderId &lt;= 2000</code></li>
	<li><code>orderId</code> là <strong>duy nhất</strong> trong tất cả order.</li>
	<li><code>orderType</code> là <code>&quot;buy&quot;</code> hoặc <code>&quot;sell&quot;</code>.</li>
	<li><code>1 &lt;= price &lt;= 10<sup>9</sup></code></li>
	<li>Tổng số lần gọi đến <code>addOrder</code>, <code>modifyOrder</code>, <code>cancelOrder</code> và <code>getOrdersAtPrice</code> không vượt quá <font face="monospace">2000</font>.</li>
	<li>Đối với <code>modifyOrder</code> và <code>cancelOrder</code>, <code>orderId</code> được chỉ định <strong>đảm bảo</strong> tồn tại và đang <em>hoạt động</em>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $2000$ order, và ta cần liệt kê các $\textit{orderId}$ đang hoạt động theo type và price. Duyệt toàn bộ trong mỗi truy vấn vẫn đủ nhanh, nhưng modify và cancel vẫn cần tìm key hiện tại.
>
> Key của truy vấn là $(\textit{orderType},\textit{price})$, trong khi mỗi id cần ánh xạ ngược về cặp đó.
>
> Lưu ánh xạ id $\to (\textit{type},\textit{price})$ trong $\textit{orders}$ và các danh sách ngược trong $\textit{t}$. Khi thêm, ghi vào cả hai; khi modify, xóa khỏi danh sách cũ rồi thêm vào danh sách mới.
>
> Xóa phần tử khỏi list có độ phức tạp tuyến tính, chấp nhận được với $n \le 2000$. Truy vấn trả về nguyên danh sách.

<!-- thinking:end -->

Ta sử dụng một hash table $\textit{orders}$ để lưu thông tin type và price của mỗi order, trong đó key là order ID và value là một tuple $(\textit{orderType}, \textit{price})$. Ngoài ra, ta sử dụng một hash table khác là $\textit{t}$ để lưu danh sách order ID tương ứng với mỗi $(\textit{orderType}, \textit{price})$, trong đó key là một tuple $(\textit{orderType}, \textit{price})$ và value là danh sách order ID.

Khi gọi $\texttt{addOrder}$, ta thêm thông tin order vào $\textit{orders}$ và nối order ID vào danh sách tương ứng trong $\textit{t}$.

Khi gọi $\texttt{modifyOrder}$, trước tiên ta lấy order type và price cũ từ $\textit{orders}$, sau đó cập nhật thông tin price của order. Tiếp theo, ta xóa order ID khỏi danh sách tương ứng trong $\textit{t}$ và thêm nó vào danh sách tương ứng với price mới.

Khi gọi $\texttt{cancelOrder}$, ta lấy thông tin order type và price từ $\textit{orders}$, sau đó xóa order ID khỏi danh sách tương ứng trong $\textit{t}$ và xóa order khỏi $\textit{orders}$.

Khi gọi $\texttt{getOrdersAtPrice}$, ta trực tiếp trả về danh sách order ID tương ứng với truy vấn trong $\textit{t}$.

Trong các thao tác trên, độ phức tạp thời gian để thêm và lấy danh sách order ID là $O(1)$, còn độ phức tạp thời gian để xóa một order ID khỏi danh sách là $O(n)$, trong đó $n$ là độ dài của danh sách tương ứng. Vì tổng số order trong bài toán không vượt quá $2000$, phương pháp này đủ hiệu quả trong thực tế. Độ phức tạp không gian là $O(m)$, trong đó $m$ là tổng số order.

<!-- tabs:start -->

#### Python3

```python
class OrderManagementSystem:

    def __init__(self):
        self.orders = {}
        self.t = defaultdict(list)

    def addOrder(self, orderId: int, orderType: str, price: int) -> None:
        self.orders[orderId] = (orderType, price)
        self.t[(orderType, price)].append(orderId)

    def modifyOrder(self, orderId: int, newPrice: int) -> None:
        orderType, price = self.orders[orderId]
        self.orders[orderId] = (orderType, newPrice)
        self.t[(orderType, price)].remove(orderId)
        self.t[(orderType, newPrice)].append(orderId)

    def cancelOrder(self, orderId: int) -> None:
        orderType, price = self.orders[orderId]
        del self.orders[orderId]
        self.t[(orderType, price)].remove(orderId)

    def getOrdersAtPrice(self, orderType: str, price: int) -> List[int]:
        return self.t[(orderType, price)]


# Your OrderManagementSystem object will be instantiated and called as such:
# obj = OrderManagementSystem()
# obj.addOrder(orderId,orderType,price)
# obj.modifyOrder(orderId,newPrice)
# obj.cancelOrder(orderId)
# param_4 = obj.getOrdersAtPrice(orderType,price)
```

#### Java

```java
class OrderManagementSystem {

    private record Key(String orderType, int price) {
    }

    private final Map<Integer, String> orderTypeMap;
    private final Map<Integer, Integer> priceMap;
    private final Map<Key, List<Integer>> t;

    public OrderManagementSystem() {
        orderTypeMap = new HashMap<>();
        priceMap = new HashMap<>();
        t = new HashMap<>();
    }

    public void addOrder(int orderId, String orderType, int price) {
        orderTypeMap.put(orderId, orderType);
        priceMap.put(orderId, price);
        var key = new Key(orderType, price);
        t.computeIfAbsent(key, _ -> new ArrayList<>()).add(orderId);
    }

    public void modifyOrder(int orderId, int newPrice) {
        var orderType = orderTypeMap.get(orderId);
        var oldPrice = priceMap.get(orderId);
        priceMap.put(orderId, newPrice);
        t.get(new Key(orderType, oldPrice)).remove((Integer) orderId);
        t.computeIfAbsent(new Key(orderType, newPrice), _ -> new ArrayList<>()).add(orderId);
    }

    public void cancelOrder(int orderId) {
        var orderType = orderTypeMap.remove(orderId);
        var price = priceMap.remove(orderId);
        t.get(new Key(orderType, price)).remove((Integer) orderId);
    }

    public int[] getOrdersAtPrice(String orderType, int price) {
        var list = t.getOrDefault(new Key(orderType, price), List.of());
        return list.stream().mapToInt(Integer::intValue).toArray();
    }
}

/**
 * Your OrderManagementSystem object will be instantiated and called as such:
 * OrderManagementSystem obj = new OrderManagementSystem();
 * obj.addOrder(orderId,orderType,price);
 * obj.modifyOrder(orderId,newPrice);
 * obj.cancelOrder(orderId);
 * int[] param_4 = obj.getOrdersAtPrice(orderType,price);
 */
```

#### C++

```cpp
class OrderManagementSystem {
    using Key = pair<string, int>;

    struct KeyHash {
        size_t operator()(const Key& k) const {
            return hash<string>()(k.first) ^ (hash<int>()(k.second) << 1);
        }
    };

    unordered_map<int, string> orderTypeMap;
    unordered_map<int, int> priceMap;
    unordered_map<Key, vector<int>, KeyHash> t;

public:
    OrderManagementSystem() {}

    void addOrder(int orderId, string orderType, int price) {
        orderTypeMap[orderId] = orderType;
        priceMap[orderId] = price;
        t[{orderType, price}].push_back(orderId);
    }

    void modifyOrder(int orderId, int newPrice) {
        string orderType = orderTypeMap[orderId];
        int oldPrice = priceMap[orderId];
        priceMap[orderId] = newPrice;

        auto& oldList = t[{orderType, oldPrice}];
        oldList.erase(find(oldList.begin(), oldList.end(), orderId));

        t[{orderType, newPrice}].push_back(orderId);
    }

    void cancelOrder(int orderId) {
        string orderType = orderTypeMap[orderId];
        int price = priceMap[orderId];

        orderTypeMap.erase(orderId);
        priceMap.erase(orderId);

        auto& list = t[{orderType, price}];
        list.erase(find(list.begin(), list.end(), orderId));
    }

    vector<int> getOrdersAtPrice(string orderType, int price) {
        auto it = t.find({orderType, price});
        if (it == t.end()) return {};
        return it->second;
    }
};

/**
 * Your OrderManagementSystem object will be instantiated and called as such:
 * OrderManagementSystem* obj = new OrderManagementSystem();
 * obj->addOrder(orderId,orderType,price);
 * obj->modifyOrder(orderId,newPrice);
 * obj->cancelOrder(orderId);
 * vector<int> param_4 = obj->getOrdersAtPrice(orderType,price);
 */
```

#### Go

```go
type Key struct {
	orderType string
	price     int
}

type OrderManagementSystem struct {
	orderTypeMap map[int]string
	priceMap     map[int]int
	t            map[Key][]int
}

func Constructor() OrderManagementSystem {
	return OrderManagementSystem{
		orderTypeMap: make(map[int]string),
		priceMap:     make(map[int]int),
		t:            make(map[Key][]int),
	}
}

func (this *OrderManagementSystem) AddOrder(orderId int, orderType string, price int) {
	this.orderTypeMap[orderId] = orderType
	this.priceMap[orderId] = price
	key := Key{orderType, price}
	this.t[key] = append(this.t[key], orderId)
}

func (this *OrderManagementSystem) ModifyOrder(orderId int, newPrice int) {
	orderType := this.orderTypeMap[orderId]
	oldPrice := this.priceMap[orderId]
	this.priceMap[orderId] = newPrice

	oldKey := Key{orderType, oldPrice}
	oldList := this.t[oldKey]
	for i, v := range oldList {
		if v == orderId {
			this.t[oldKey] = append(oldList[:i], oldList[i+1:]...)
			break
		}
	}

	newKey := Key{orderType, newPrice}
	this.t[newKey] = append(this.t[newKey], orderId)
}

func (this *OrderManagementSystem) CancelOrder(orderId int) {
	orderType := this.orderTypeMap[orderId]
	price := this.priceMap[orderId]

	delete(this.orderTypeMap, orderId)
	delete(this.priceMap, orderId)

	key := Key{orderType, price}
	list := this.t[key]
	for i, v := range list {
		if v == orderId {
			this.t[key] = append(list[:i], list[i+1:]...)
			break
		}
	}
}

func (this *OrderManagementSystem) GetOrdersAtPrice(orderType string, price int) []int {
	key := Key{orderType, price}
	return this.t[key]
}

/**
 * Your OrderManagementSystem object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddOrder(orderId,orderType,price);
 * obj.ModifyOrder(orderId,newPrice);
 * obj.CancelOrder(orderId);
 * param_4 := obj.GetOrdersAtPrice(orderType,price);
 */
```

#### TypeScript

```ts
class OrderManagementSystem {
    private orderTypeMap: Map<number, string>;
    private priceMap: Map<number, number>;
    private t: Map<string, number[]>;

    constructor() {
        this.orderTypeMap = new Map();
        this.priceMap = new Map();
        this.t = new Map();
    }

    private key(orderType: string, price: number): string {
        return `${orderType}#${price}`;
    }

    addOrder(orderId: number, orderType: string, price: number): void {
        this.orderTypeMap.set(orderId, orderType);
        this.priceMap.set(orderId, price);

        const k = this.key(orderType, price);
        if (!this.t.has(k)) {
            this.t.set(k, []);
        }
        this.t.get(k)!.push(orderId);
    }

    modifyOrder(orderId: number, newPrice: number): void {
        const orderType = this.orderTypeMap.get(orderId)!;
        const oldPrice = this.priceMap.get(orderId)!;

        this.priceMap.set(orderId, newPrice);

        const oldKey = this.key(orderType, oldPrice);
        const oldList = this.t.get(oldKey)!;
        const idx = oldList.indexOf(orderId);
        if (idx !== -1) {
            oldList.splice(idx, 1);
        }

        const newKey = this.key(orderType, newPrice);
        if (!this.t.has(newKey)) {
            this.t.set(newKey, []);
        }
        this.t.get(newKey)!.push(orderId);
    }

    cancelOrder(orderId: number): void {
        const orderType = this.orderTypeMap.get(orderId)!;
        const price = this.priceMap.get(orderId)!;

        this.orderTypeMap.delete(orderId);
        this.priceMap.delete(orderId);

        const k = this.key(orderType, price);
        const list = this.t.get(k)!;
        const idx = list.indexOf(orderId);
        if (idx !== -1) {
            list.splice(idx, 1);
        }
    }

    getOrdersAtPrice(orderType: string, price: number): number[] {
        return this.t.get(this.key(orderType, price)) ?? [];
    }
}

/**
 * Your OrderManagementSystem object will be instantiated and called as such:
 * var obj = new OrderManagementSystem()
 * obj.addOrder(orderId,orderType,price)
 * obj.modifyOrder(orderId,newPrice)
 * obj.cancelOrder(orderId)
 * var param_4 = obj.getOrdersAtPrice(orderType,price)
 */
```

#### Rust

```rust
use std::collections::HashMap;

struct OrderManagementSystem {
    orders: HashMap<i32, (String, i32)>,
    t: HashMap<(String, i32), Vec<i32>>,
}

impl OrderManagementSystem {

    fn new() -> Self {
        Self {
            orders: HashMap::new(),
            t: HashMap::new(),
        }
    }

    fn add_order(&mut self, order_id: i32, order_type: String, price: i32) {
        self.orders.insert(order_id, (order_type.clone(), price));
        self.t
            .entry((order_type, price))
            .or_insert_with(Vec::new)
            .push(order_id);
    }

    fn modify_order(&mut self, order_id: i32, new_price: i32) {
        if let Some((order_type, old_price)) = self.orders.get(&order_id).cloned() {
            self.orders.insert(order_id, (order_type.clone(), new_price));

            if let Some(v) = self.t.get_mut(&(order_type.clone(), old_price)) {
                if let Some(pos) = v.iter().position(|&x| x == order_id) {
                    v.remove(pos);
                }
            }

            self.t
                .entry((order_type, new_price))
                .or_insert_with(Vec::new)
                .push(order_id);
        }
    }

    fn cancel_order(&mut self, order_id: i32) {
        if let Some((order_type, price)) = self.orders.remove(&order_id) {
            if let Some(v) = self.t.get_mut(&(order_type, price)) {
                if let Some(pos) = v.iter().position(|&x| x == order_id) {
                    v.remove(pos);
                }
            }
        }
    }

    fn get_orders_at_price(&self, order_type: String, price: i32) -> Vec<i32> {
        self.t
            .get(&(order_type, price))
            .cloned()
            .unwrap_or_default()
    }
}

/**
 * Your OrderManagementSystem object will be instantiated and called as such:
 * let obj = OrderManagementSystem::new();
 * obj.add_order(orderId, orderType, price);
 * obj.modify_order(orderId, newPrice);
 * obj.cancel_order(orderId);
 * let ret_4: Vec<i32> = obj.get_orders_at_price(orderType, price);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
