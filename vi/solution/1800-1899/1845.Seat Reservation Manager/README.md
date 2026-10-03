---
comments: true
difficulty: Medium
rating: 1428
source: Biweekly Contest 51 Q2
tags:
    - Design
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1845. Seat Reservation Manager](https://leetcode.com/problems/seat-reservation-manager)

[中文文档](/solution/1800-1899/1845.Seat%20Reservation%20Manager/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một hệ thống quản lý trạng thái đặt chỗ của <code>n</code> ghế được đánh số từ <code>1</code> đến <code>n</code>.</p>

<p>Cài đặt lớp <code>SeatManager</code>:</p>

<ul>
	<li><code>SeatManager(int n)</code> khởi tạo một đối tượng <code>SeatManager</code> quản lý <code>n</code> ghế được đánh số từ <code>1</code> đến <code>n</code>. Ban đầu tất cả ghế đều còn trống.</li>
	<li><code>int reserve()</code> lấy ghế chưa được đặt có số <strong>nhỏ nhất</strong>, đặt ghế đó và trả về số ghế.</li>
	<li><code>void unreserve(int seatNumber)</code> hủy đặt ghế có số <code>seatNumber</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;SeatManager&quot;, &quot;reserve&quot;, &quot;reserve&quot;, &quot;unreserve&quot;, &quot;reserve&quot;, &quot;reserve&quot;, &quot;reserve&quot;, &quot;reserve&quot;, &quot;unreserve&quot;]
[[5], [], [], [2], [], [], [], [], [5]]
<strong>Đầu ra</strong>
[null, 1, 2, null, 2, 3, 4, 5, null]

<strong>Giải thích</strong>
SeatManager seatManager = new SeatManager(5); // Initializes a SeatManager with 5 seats.
seatManager.reserve();    // All seats are available, so return the lowest numbered seat, which is 1.
seatManager.reserve();    // The available seats are [2,3,4,5], so return the lowest of them, which is 2.
seatManager.unreserve(2); // Unreserve seat 2, so now the available seats are [2,3,4,5].
seatManager.reserve();    // The available seats are [2,3,4,5], so return the lowest of them, which is 2.
seatManager.reserve();    // The available seats are [3,4,5], so return the lowest of them, which is 3.
seatManager.reserve();    // The available seats are [4,5], so return the lowest of them, which is 4.
seatManager.reserve();    // The only available seat is seat 5, so return 5.
seatManager.unreserve(5); // Unreserve seat 5, so now the available seats are [5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= seatNumber &lt;= n</code></li>
	<li>Với mỗi lần gọi <code>reserve</code>, đảm bảo có ít nhất một ghế chưa được đặt.</li>
	<li>Với mỗi lần gọi <code>unreserve</code>, đảm bảo <code>seatNumber</code> đang được đặt.</li>
	<li>Tổng số lần gọi <code>reserve</code> và <code>unreserve</code> <strong>tổng cộng</strong> không vượt quá <code>10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Ta luôn phải đặt ghế trống nhỏ nhất và có thể hủy đặt ghế sau đó. Duyệt mảng boolean để tìm chỉ số nhỏ nhất của ghế trống sẽ tốn $O(n)$ cho mỗi lần gọi.
>
> Lưu số các ghế trống trong một min-heap: $\textit{reserve}$ lấy phần tử đầu, còn $\textit{unreserve}$ đưa số ghế trở lại. Cả hai thao tác đều có độ phức tạp logarit.

<!-- thinking:end -->

Ta định nghĩa một priority queue (min-heap) $\textit{q}$ để lưu số của tất cả ghế còn trống. Ban đầu, ta thêm tất cả số ghế từ $1$ đến $n$ vào $\textit{q}$.

Khi gọi phương thức `reserve`, ta lấy phần tử đầu của $\textit{q}$, chính là số ghế trống nhỏ nhất.

Khi gọi phương thức `unreserve`, ta thêm lại số ghế vào $\textit{q}$.

Về độ phức tạp thời gian, độ phức tạp khởi tạo là $O(n)$ hoặc $O(n \times \log n)$, còn độ phức tạp thời gian của cả hai phương thức `reserve` và `unreserve` đều là $O(\log n)$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class SeatManager:
    def __init__(self, n: int):
        self.q = list(range(1, n + 1))

    def reserve(self) -> int:
        return heappop(self.q)

    def unreserve(self, seatNumber: int) -> None:
        heappush(self.q, seatNumber)


# Your SeatManager object will be instantiated and called as such:
# obj = SeatManager(n)
# param_1 = obj.reserve()
# obj.unreserve(seatNumber)
```

#### Java

```java
class SeatManager {
    private PriorityQueue<Integer> q = new PriorityQueue<>();

    public SeatManager(int n) {
        for (int i = 1; i <= n; ++i) {
            q.offer(i);
        }
    }

    public int reserve() {
        return q.poll();
    }

    public void unreserve(int seatNumber) {
        q.offer(seatNumber);
    }
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager obj = new SeatManager(n);
 * int param_1 = obj.reserve();
 * obj.unreserve(seatNumber);
 */
```

#### C++

```cpp
class SeatManager {
public:
    SeatManager(int n) {
        for (int i = 1; i <= n; ++i) {
            q.push(i);
        }
    }

    int reserve() {
        int seat = q.top();
        q.pop();
        return seat;
    }

    void unreserve(int seatNumber) {
        q.push(seatNumber);
    }

private:
    priority_queue<int, vector<int>, greater<int>> q;
};

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager* obj = new SeatManager(n);
 * int param_1 = obj->reserve();
 * obj->unreserve(seatNumber);
 */
```

#### Go

```go
type SeatManager struct {
	q hp
}

func Constructor(n int) SeatManager {
	q := hp{}
	for i := 1; i <= n; i++ {
		heap.Push(&q, i)
	}
	return SeatManager{q}
}

func (this *SeatManager) Reserve() int {
	return heap.Pop(&this.q).(int)
}

func (this *SeatManager) Unreserve(seatNumber int) {
	heap.Push(&this.q, seatNumber)
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * obj := Constructor(n);
 * param_1 := obj.Reserve();
 * obj.Unreserve(seatNumber);
 */
```

#### TypeScript

```ts
class SeatManager {
    private q = new MinPriorityQueue<number>();
    constructor(n: number) {
        for (let i = 1; i <= n; i++) {
            this.q.enqueue(i);
        }
    }

    reserve(): number {
        return this.q.dequeue();
    }

    unreserve(seatNumber: number): void {
        this.q.enqueue(seatNumber);
    }
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * var obj = new SeatManager(n)
 * var param_1 = obj.reserve()
 * obj.unreserve(seatNumber)
 */
```

#### C#

```cs
public class SeatManager {
    private PriorityQueue<int, int> q = new PriorityQueue<int, int>();

    public SeatManager(int n) {
        for (int i = 1; i <= n; ++i) {
            q.Enqueue(i, i);
        }
    }

    public int Reserve() {
        return q.Dequeue();
    }

    public void Unreserve(int seatNumber) {
        q.Enqueue(seatNumber, seatNumber);
    }
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager obj = new SeatManager(n);
 * int param_1 = obj.Reserve();
 * obj.Unreserve(seatNumber);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
