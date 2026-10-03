---
comments: true
difficulty: Medium
rating: 1711
source: Weekly Contest 233 Q2
tags:
    - Array
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1801. Number of Orders in the Backlog](https://leetcode.com/problems/number-of-orders-in-the-backlog)

[中文文档](/solution/1800-1899/1801.Number%20of%20Orders%20in%20the%20Backlog/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>orders</code>, trong đó mỗi <code>orders[i] = [price<sub>i</sub>, amount<sub>i</sub>, orderType<sub>i</sub>]</code> cho biết có <code>amount<sub>i</sub></code><sub> </sub>order thuộc loại <code>orderType<sub>i</sub></code> được đặt ở mức giá <code>price<sub>i</sub></code>. <code>orderType<sub>i</sub></code> có giá trị:</p>

<ul>
	<li><code>0</code> nếu đó là một nhóm order <code>buy</code>, hoặc</li>
	<li><code>1</code> nếu đó là một nhóm order <code>sell</code>.</li>
</ul>

<p>Lưu ý rằng <code>orders[i]</code> biểu diễn một nhóm gồm <code>amount<sub>i</sub></code> order độc lập có cùng mức giá và loại order. Với mọi <code>i</code> hợp lệ, tất cả order được biểu diễn bởi <code>orders[i]</code> sẽ được đặt trước tất cả order được biểu diễn bởi <code>orders[i+1]</code>.</p>

<p>Có một <strong>backlog</strong> gồm các order chưa được thực hiện. Ban đầu backlog rỗng. Khi một order được đặt, các bước sau xảy ra:</p>

<ul>
	<li>Nếu order là order <code>buy</code>, ta xét order <code>sell</code> có giá <strong>nhỏ nhất</strong> trong backlog. Nếu giá của order <code>sell</code> đó <strong>nhỏ hơn hoặc bằng</strong> giá của order <code>buy</code> hiện tại, hai order sẽ khớp và được thực hiện, đồng thời order <code>sell</code> bị xóa khỏi backlog. Nếu không, order <code>buy</code> được thêm vào backlog.</li>
	<li>Ngược lại, nếu order là order <code>sell</code>, ta xét order <code>buy</code> có giá <strong>lớn nhất</strong> trong backlog. Nếu giá của order <code>buy</code> đó <strong>lớn hơn hoặc bằng</strong> giá của order <code>sell</code> hiện tại, hai order sẽ khớp và được thực hiện, đồng thời order <code>buy</code> bị xóa khỏi backlog. Nếu không, order <code>sell</code> được thêm vào backlog.</li>
</ul>

<p>Hãy trả về <em>tổng <strong>số lượng</strong> order trong backlog sau khi đặt tất cả order từ input</em>. Vì giá trị này có thể rất lớn, hãy trả về nó <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1801.Number%20of%20Orders%20in%20the%20Backlog/images/ex1.png" style="width: 450px; height: 479px;" />
<pre>
<strong>Đầu vào:</strong> orders = [[10,5,0],[15,2,1],[25,1,1],[30,4,0]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các bước xử lý order như sau:
- Có 5 order loại buy với giá 10 được đặt. Không có order sell, nên 5 order được thêm vào backlog.
- Có 2 order loại sell với giá 15 được đặt. Không có order buy nào có giá lớn hơn hoặc bằng 15, nên 2 order được thêm vào backlog.
- Có 1 order loại sell với giá 25 được đặt. Trong backlog không có order buy nào có giá lớn hơn hoặc bằng 25, nên order này được thêm vào backlog.
- Có 4 order loại buy với giá 30 được đặt. 2 order đầu tiên khớp với 2 order sell có giá thấp nhất là 15, và 2 order sell này bị xóa khỏi backlog. Order thứ 3<sup>rd</sup> khớp với order sell có giá thấp nhất là 25, và order sell này bị xóa khỏi backlog. Sau đó backlog không còn order sell nào, nên order thứ 4<sup>th</sup> được thêm vào backlog.
Cuối cùng, backlog có 5 order buy với giá 10 và 1 order buy với giá 30. Vậy tổng số order trong backlog là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1801.Number%20of%20Orders%20in%20the%20Backlog/images/ex2.png" style="width: 450px; height: 584px;" />
<pre>
<strong>Đầu vào:</strong> orders = [[7,1000000000,1],[15,3,0],[5,999999995,0],[5,1,1]]
<strong>Đầu ra:</strong> 999999984
<strong>Giải thích:</strong> Các bước xử lý order như sau:
- Có 10<sup>9</sup> order loại sell với giá 7 được đặt. Không có order buy, nên 10<sup>9</sup> order được thêm vào backlog.
- Có 3 order loại buy với giá 15 được đặt. Chúng khớp với 3 order sell có giá thấp nhất là 7, và 3 order sell đó bị xóa khỏi backlog.
- Có 999999995 order loại buy với giá 5 được đặt. Giá thấp nhất của order sell là 7, nên 999999995 order được thêm vào backlog.
- Có 1 order loại sell với giá 5 được đặt. Nó khớp với order buy có giá cao nhất là 5, và order buy đó bị xóa khỏi backlog.
Cuối cùng, backlog có (1000000000-3) order sell với giá 7 và (999999995-1) order buy với giá 5. Vậy tổng số order = 1999999991, bằng 999999984 % (10<sup>9</sup> + 7).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= orders.length &lt;= 10<sup>5</sup></code></li>
	<li><code>orders[i].length == 3</code></li>
	<li><code>1 &lt;= price<sub>i</sub>, amount<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>orderType<sub>i</sub></code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Max-Min Heap) + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi order có thể khớp với các order đối diện đã có trong backlog và có mức giá phù hợp hơn; phần còn lại sẽ được đưa vào backlog. Nếu duyệt tuyến tính toàn bộ backlog cho từng lần khớp, mỗi order tốn $O(n)$ và tổng cộng là $O(n^2)$. Với $n \le 10^5$, cách này không thể chấp nhận.
>
> Một order buy chỉ quan tâm đến order sell rẻ nhất nhưng không vượt quá giá của nó; một order sell chỉ quan tâm đến order buy đắt nhất nhưng không thấp hơn giá của nó. Ta lưu order sell trong min-heap và order buy trong max-heap để đối tác phù hợp nhất luôn nằm ở đỉnh. Xử lý order theo thứ tự input, lấy ra và giảm số lượng, sau đó đưa phần còn lại vào heap. Đáp án là tổng số lượng còn lại modulo $10^9+7$.

<!-- thinking:end -->

Ta có thể dùng priority queue (max-min heap) để duy trì backlog hiện tại, trong đó max heap `buy` duy trì các order mua trong backlog, còn min heap `sell` duy trì các order bán trong backlog. Mỗi phần tử trong heap là một tuple $(price, amount)$, cho biết có `amount` order ở mức giá `price`.

Tiếp theo, ta duyệt mảng order `orders` và mô phỏng theo yêu cầu của đề bài.

Sau khi duyệt xong, ta cộng số lượng order trong `buy` và `sell` để thu được backlog cuối cùng. Lưu ý rằng đáp án có thể rất lớn, nên cần lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của `orders`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getNumberOfBacklogOrders(self, orders: List[List[int]]) -> int:
        buy, sell = [], []
        for p, a, t in orders:
            if t == 0:
                while a and sell and sell[0][0] <= p:
                    x, y = heappop(sell)
                    if a >= y:
                        a -= y
                    else:
                        heappush(sell, (x, y - a))
                        a = 0
                if a:
                    heappush(buy, (-p, a))
            else:
                while a and buy and -buy[0][0] >= p:
                    x, y = heappop(buy)
                    if a >= y:
                        a -= y
                    else:
                        heappush(buy, (x, y - a))
                        a = 0
                if a:
                    heappush(sell, (p, a))
        mod = 10**9 + 7
        return sum(v[1] for v in buy + sell) % mod
```

#### Java

```java
class Solution {
    public int getNumberOfBacklogOrders(int[][] orders) {
        PriorityQueue<int[]> buy = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        PriorityQueue<int[]> sell = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (var e : orders) {
            int p = e[0], a = e[1], t = e[2];
            if (t == 0) {
                while (a > 0 && !sell.isEmpty() && sell.peek()[0] <= p) {
                    var q = sell.poll();
                    int x = q[0], y = q[1];
                    if (a >= y) {
                        a -= y;
                    } else {
                        sell.offer(new int[] {x, y - a});
                        a = 0;
                    }
                }
                if (a > 0) {
                    buy.offer(new int[] {p, a});
                }
            } else {
                while (a > 0 && !buy.isEmpty() && buy.peek()[0] >= p) {
                    var q = buy.poll();
                    int x = q[0], y = q[1];
                    if (a >= y) {
                        a -= y;
                    } else {
                        buy.offer(new int[] {x, y - a});
                        a = 0;
                    }
                }
                if (a > 0) {
                    sell.offer(new int[] {p, a});
                }
            }
        }
        long ans = 0;
        final int mod = (int) 1e9 + 7;
        while (!buy.isEmpty()) {
            ans += buy.poll()[1];
        }
        while (!sell.isEmpty()) {
            ans += sell.poll()[1];
        }
        return (int) (ans % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getNumberOfBacklogOrders(vector<vector<int>>& orders) {
        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> sell;
        priority_queue<pii> buy;
        for (auto& e : orders) {
            int p = e[0], a = e[1], t = e[2];
            if (t == 0) {
                while (a && !sell.empty() && sell.top().first <= p) {
                    auto [x, y] = sell.top();
                    sell.pop();
                    if (a >= y) {
                        a -= y;
                    } else {
                        sell.push({x, y - a});
                        a = 0;
                    }
                }
                if (a) {
                    buy.push({p, a});
                }
            } else {
                while (a && !buy.empty() && buy.top().first >= p) {
                    auto [x, y] = buy.top();
                    buy.pop();
                    if (a >= y) {
                        a -= y;
                    } else {
                        buy.push({x, y - a});
                        a = 0;
                    }
                }
                if (a) {
                    sell.push({p, a});
                }
            }
        }
        long ans = 0;
        while (!buy.empty()) {
            ans += buy.top().second;
            buy.pop();
        }
        while (!sell.empty()) {
            ans += sell.top().second;
            sell.pop();
        }
        const int mod = 1e9 + 7;
        return ans % mod;
    }
};
```

#### Go

```go
func getNumberOfBacklogOrders(orders [][]int) (ans int) {
	sell := hp{}
	buy := hp{}
	for _, e := range orders {
		p, a, t := e[0], e[1], e[2]
		if t == 0 {
			for a > 0 && len(sell) > 0 && sell[0].p <= p {
				q := heap.Pop(&sell).(pair)
				x, y := q.p, q.a
				if a >= y {
					a -= y
				} else {
					heap.Push(&sell, pair{x, y - a})
					a = 0
				}
			}
			if a > 0 {
				heap.Push(&buy, pair{-p, a})
			}
		} else {
			for a > 0 && len(buy) > 0 && -buy[0].p >= p {
				q := heap.Pop(&buy).(pair)
				x, y := q.p, q.a
				if a >= y {
					a -= y
				} else {
					heap.Push(&buy, pair{x, y - a})
					a = 0
				}
			}
			if a > 0 {
				heap.Push(&sell, pair{p, a})
			}
		}
	}
	const mod int = 1e9 + 7
	for len(buy) > 0 {
		ans += heap.Pop(&buy).(pair).a
	}
	for len(sell) > 0 {
		ans += heap.Pop(&sell).(pair).a
	}
	return ans % mod
}

type pair struct{ p, a int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].p < h[j].p }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
