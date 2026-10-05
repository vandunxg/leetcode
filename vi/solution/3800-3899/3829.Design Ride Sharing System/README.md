---
comments: true
difficulty: Medium
rating: 1593
source: Weekly Contest 487 Q3
tags:
    - Design
    - Queue
    - Hash Table
    - Data Stream
---

<!-- problem:start -->

# [3829. Design Ride Sharing System](https://leetcode.com/problems/design-ride-sharing-system)

[中文文档](/solution/3800-3899/3829.Design%20Ride%20Sharing%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Một hệ thống chia sẻ chuyến đi quản lý các yêu cầu đi xe từ hành khách và tình trạng sẵn sàng của tài xế. Hành khách yêu cầu chuyến đi, còn tài xế trở nên sẵn sàng theo thời gian. Hệ thống cần ghép hành khách và tài xế theo thứ tự họ tham gia.</p>

<p>Hãy triển khai lớp <code>RideSharingSystem</code>:</p>

<ul>
	<li><code>RideSharingSystem()</code>: Khởi tạo hệ thống.</li>
	<li><code>void addRider(int riderId)</code>: Thêm một hành khách mới có <code>riderId</code> đã cho.</li>
	<li><code>void addDriver(int driverId)</code>: Thêm một tài xế mới có <code>driverId</code> đã cho.</li>
	<li><code>int[] matchDriverWithRider()</code>: Ghép tài xế sẵn sàng <strong>đến sớm nhất</strong> với hành khách đang chờ <strong>đến sớm nhất</strong>, rồi xóa cả hai khỏi hệ thống. Trả về một mảng số nguyên có kích thước 2, trong đó <code>result = [driverId, riderId]</code> nếu ghép thành công. Nếu không thể ghép, trả về <code>[-1, -1]</code>.</li>
	<li><code>void cancelRider(int riderId)</code>: Hủy yêu cầu đi xe của hành khách có <code>riderId</code> đã cho <strong>nếu hành khách tồn tại</strong> và <strong>chưa được ghép</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;RideSharingSystem&quot;, &quot;addRider&quot;, &quot;addDriver&quot;, &quot;addRider&quot;, &quot;matchDriverWithRider&quot;, &quot;addDriver&quot;, &quot;cancelRider&quot;, &quot;matchDriverWithRider&quot;, &quot;matchDriverWithRider&quot;]<br />
[[], [3], [2], [1], [], [5], [3], [], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, null, null, [2, 3], null, null, [5, 1], [-1, -1]] </span></p>

<p><strong>Giải thích</strong></p>
RideSharingSystem rideSharingSystem = new RideSharingSystem(); // Khởi tạo hệ thống<br />
rideSharingSystem.addRider(3); // hành khách 3 tham gia hàng đợi<br />
rideSharingSystem.addDriver(2); // tài xế 2 tham gia hàng đợi<br />
rideSharingSystem.addRider(1); // hành khách 1 tham gia hàng đợi<br />
rideSharingSystem.matchDriverWithRider(); // trả về [2, 3]<br />
rideSharingSystem.addDriver(5); // tài xế 5 trở nên sẵn sàng<br />
rideSharingSystem.cancelRider(3); // hành khách 3 đã được ghép, việc hủy không có tác dụng<br />
rideSharingSystem.matchDriverWithRider(); // trả về [5, 1]<br />
rideSharingSystem.matchDriverWithRider(); // trả về [-1, -1]</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;RideSharingSystem&quot;, &quot;addRider&quot;, &quot;addDriver&quot;, &quot;addDriver&quot;, &quot;matchDriverWithRider&quot;, &quot;addRider&quot;, &quot;cancelRider&quot;, &quot;matchDriverWithRider&quot;]<br />
[[], [8], [8], [6], [], [2], [2], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, null, null, [8, 8], null, null, [-1, -1]] </span></p>

<p><strong>Giải thích</strong></p>
RideSharingSystem rideSharingSystem = new RideSharingSystem(); // Khởi tạo hệ thống<br />
rideSharingSystem.addRider(8); // hành khách 8 tham gia hàng đợi<br />
rideSharingSystem.addDriver(8); // tài xế 8 tham gia hàng đợi<br />
rideSharingSystem.addDriver(6); // tài xế 6 tham gia hàng đợi<br />
rideSharingSystem.matchDriverWithRider(); // trả về [8, 8]<br />
rideSharingSystem.addRider(2); // hành khách 2 tham gia hàng đợi<br />
rideSharingSystem.cancelRider(2); // hành khách 2 hủy yêu cầu<br />
rideSharingSystem.matchDriverWithRider(); // trả về [-1, -1]</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= riderId, driverId &lt;= 1000</code></li>
	<li>Mỗi <code>riderId</code> là <strong>duy nhất</strong> trong số các hành khách và được thêm nhiều nhất <strong>một lần</strong>.</li>
	<li>Mỗi <code>driverId</code> là <strong>duy nhất</strong> trong số các tài xế và được thêm nhiều nhất <strong>một lần</strong>.</li>
	<li>Tổng số lời gọi đến <code>addRider</code>​​​​​​​, <code>addDriver</code>, <code>matchDriverWithRider</code> và <code>cancelRider</code> <strong>nhiều nhất</strong> là 1000.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorted Set + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta ghép tài xế đến sớm nhất với hành khách đến sớm nhất và có thể hủy một hành khách chưa được ghép. Có nhiều nhất $1000$ lời gọi, nhưng các truy vấn theo thứ tự cần phần tử có timestamp nhỏ nhất.
>
> Một queue FIFO thông thường sẽ phải duyệt khi hủy phần tử ở giữa. Thay vào đó, ta lưu các phần tử theo timestamp.
>
> Ordered set lưu $(t,\textit{id})$ với một đồng hồ tăng dần toàn cục; hash map lưu $t$ của mỗi hành khách để hủy trong $O(\log n)$.
>
> Nếu một trong hai phía rỗng thì phép ghép thất bại; ngược lại, ta lấy ra hai phần tử nhỏ nhất.

<!-- thinking:end -->

Ta dùng hai sorted set $\textit{riders}$ và $\textit{drivers}$ để lưu lần lượt các hành khách đang chờ và các tài xế đang sẵn sàng. Mỗi phần tử là một tuple $(t, \textit{id})$, biểu diễn ID của hành khách/tài xế và timestamp $t$ tại thời điểm họ tham gia hệ thống. Timestamp $t$ được dùng để phân biệt thứ tự tham gia. Ban đầu, $t = 0$, và mỗi lần thêm một hành khách hoặc tài xế, $t$ tăng thêm $1$.

Ngoài ra, ta dùng một hash table $\textit{d}$ để lưu ánh xạ giữa ID của mỗi hành khách và timestamp của họ, giúp tra cứu khi hủy yêu cầu của hành khách.

Cụ thể:

- Khi thêm hành khách, ta thêm $(t, \textit{riderId})$ vào $\textit{riders}$, đặt $\textit{d}[\textit{riderId}] = t$, rồi tăng $t$ thêm $1$.
- Khi thêm tài xế, ta thêm $(t, \textit{driverId})$ vào $\textit{drivers}$, rồi tăng $t$ thêm $1$.
- Khi ghép tài xế với hành khách, nếu $\textit{riders}$ hoặc $\textit{drivers}$ rỗng, ta trả về $[-1, -1]$. Ngược lại, ta xóa các phần tử có timestamp nhỏ nhất khỏi $\textit{riders}$ và $\textit{drivers}$, lần lượt là $(t_r, \textit{riderId})$ và $(t_d, \textit{driverId})$, rồi trả về $[\textit{driverId}, \textit{riderId}]$.
- Khi hủy yêu cầu của một hành khách, ta tra timestamp $t$ của hành khách thông qua $\textit{d}$, rồi xóa $(t, \textit{riderId})$ khỏi $\textit{riders}$.

Độ phức tạp thời gian là $O(\log n)$ cho mỗi thao tác, trong đó $n$ là số hành khách hoặc tài xế hiện tại. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class RideSharingSystem:

    def __init__(self):
        self.t = 0
        self.riders = SortedList()
        self.drivers = SortedList()
        self.d = defaultdict(int)

    def addRider(self, riderId: int) -> None:
        self.d[riderId] = self.t
        self.riders.add((self.t, riderId))
        self.t += 1

    def addDriver(self, driverId: int) -> None:
        self.drivers.add((self.t, driverId))
        self.t += 1

    def matchDriverWithRider(self) -> List[int]:
        if len(self.riders) < 1 or len(self.drivers) < 1:
            return [-1, -1]
        return [self.drivers.pop(0)[1], self.riders.pop(0)[1]]

    def cancelRider(self, riderId: int) -> None:
        self.riders.discard((self.d[riderId], riderId))


# Your RideSharingSystem object will be instantiated and called as such:
# obj = RideSharingSystem()
# obj.addRider(riderId)
# obj.addDriver(driverId)
# param_3 = obj.matchDriverWithRider()
# obj.cancelRider(riderId)
```

#### Java

```java
class RideSharingSystem {
    private int t;
    private TreeSet<int[]> riders;
    private TreeSet<int[]> drivers;
    private Map<Integer, Integer> d;

    public RideSharingSystem() {
        this.t = 0;
        this.riders = new TreeSet<>(
            (a, b) -> a[0] != b[0] ? Integer.compare(a[0], b[0]) : Integer.compare(a[1], b[1]));
        this.drivers = new TreeSet<>(
            (a, b) -> a[0] != b[0] ? Integer.compare(a[0], b[0]) : Integer.compare(a[1], b[1]));
        this.d = new HashMap<>();
    }

    public void addRider(int riderId) {
        d.put(riderId, t);
        riders.add(new int[] {t, riderId});
        t++;
    }

    public void addDriver(int driverId) {
        drivers.add(new int[] {t, driverId});
        t++;
    }

    public int[] matchDriverWithRider() {
        if (riders.isEmpty() || drivers.isEmpty()) {
            return new int[] {-1, -1};
        }
        int driverId = drivers.pollFirst()[1];
        int riderId = riders.pollFirst()[1];
        return new int[] {driverId, riderId};
    }

    public void cancelRider(int riderId) {
        Integer time = d.get(riderId);
        if (time != null) {
            riders.remove(new int[] {time, riderId});
        }
    }
}

/**
 * Your RideSharingSystem object will be instantiated and called as such:
 * RideSharingSystem obj = new RideSharingSystem();
 * obj.addRider(riderId);
 * obj.addDriver(driverId);
 * int[] param_3 = obj.matchDriverWithRider();
 * obj.cancelRider(riderId);
 */
```

#### C++

```cpp
class RideSharingSystem {
private:
    int t;
    set<pair<int, int>> riders;
    set<pair<int, int>> drivers;
    unordered_map<int, int> d;

public:
    RideSharingSystem() {
        t = 0;
    }

    void addRider(int riderId) {
        d[riderId] = t;
        riders.insert({t, riderId});
        t++;
    }

    void addDriver(int driverId) {
        drivers.insert({t, driverId});
        t++;
    }

    vector<int> matchDriverWithRider() {
        if (riders.empty() || drivers.empty()) {
            return {-1, -1};
        }
        int driverId = drivers.begin()->second;
        int riderId = riders.begin()->second;
        drivers.erase(drivers.begin());
        riders.erase(riders.begin());
        return {driverId, riderId};
    }

    void cancelRider(int riderId) {
        auto it = d.find(riderId);
        if (it != d.end()) {
            riders.erase({it->second, riderId});
        }
    }
};

/**
 * Your RideSharingSystem object will be instantiated and called as such:
 * RideSharingSystem* obj = new RideSharingSystem();
 * obj->addRider(riderId);
 * obj->addDriver(driverId);
 * vector<int> param_3 = obj->matchDriverWithRider();
 * obj->cancelRider(riderId);
 */
```

#### Go

```go
type RideSharingSystem struct {
	t       int
	riders  *redblacktree.Tree[int, int]
	drivers *redblacktree.Tree[int, int]
	d       map[int]int
}

func Constructor() RideSharingSystem {
	return RideSharingSystem{
		t:       0,
		riders:  redblacktree.New[int, int](),
		drivers: redblacktree.New[int, int](),
		d:       make(map[int]int),
	}
}

func (this *RideSharingSystem) AddRider(riderId int) {
	this.d[riderId] = this.t
	this.riders.Put(this.t, riderId)
	this.t++
}

func (this *RideSharingSystem) AddDriver(driverId int) {
	this.drivers.Put(this.t, driverId)
	this.t++
}

func (this *RideSharingSystem) MatchDriverWithRider() []int {
	if this.riders.Empty() || this.drivers.Empty() {
		return []int{-1, -1}
	}

	driverTime, driverId := this.drivers.Left().Key, this.drivers.Left().Value
	riderTime, riderId := this.riders.Left().Key, this.riders.Left().Value

	this.drivers.Remove(driverTime)
	this.riders.Remove(riderTime)

	return []int{driverId, riderId}
}

func (this *RideSharingSystem) CancelRider(riderId int) {
	time, exists := this.d[riderId]
	if !exists {
		return
	}
	this.riders.Remove(time)
}

/**
 * Your RideSharingSystem object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddRider(riderId);
 * obj.AddDriver(driverId);
 * param_3 := obj.MatchDriverWithRider();
 * obj.CancelRider(riderId);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
