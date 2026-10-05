---
comments: true
difficulty: Medium
rating: 1548
source: Weekly Contest 495 Q2
tags:
    - Design
    - Array
    - Hash Table
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3885. Design Event Manager](https://leetcode.com/problems/design-event-manager)

[中文文档](/solution/3800-3899/3885.Design%20Event%20Manager/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một danh sách event ban đầu, trong đó mỗi event có một <code>eventId</code> duy nhất và một <code>priority</code>.</p>

<p>Hãy triển khai lớp <code>EventManager</code>:</p>

<ul>
	<li><code>EventManager(int[][] events)</code> Khởi tạo manager với các event đã cho, trong đó <code>events[i] = [eventId<sub>i</sub>, priority<sub>​​​​​​​i</sub>]</code>.</li>
	<li><code>void updatePriority(int eventId, int newPriority)</code> Cập nhật priority của event <strong>đang hoạt động</strong> có id <code>eventId</code> thành <code>newPriority</code>.</li>
	<li><code>int pollHighest()</code> Xóa và trả về <code>eventId</code> của event <strong>đang hoạt động</strong> có priority <strong>cao nhất</strong>. Nếu có nhiều event đang hoạt động có cùng priority, trả về <code>eventId</code> <strong>nhỏ nhất</strong> trong số đó. Nếu không có event đang hoạt động, trả về -1.</li>
</ul>

<p>Event được gọi là <strong>đang hoạt động</strong> nếu chưa bị xóa bởi <code>pollHighest()</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;EventManager&quot;, &quot;pollHighest&quot;, &quot;updatePriority&quot;, &quot;pollHighest&quot;, &quot;pollHighest&quot;]<br />
[[[[5, 7], [2, 7], [9, 4]]], [], [9, 7], [], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, 2, null, 5, 9] </span></p>

<p><strong>Giải thích</strong></p>
EventManager eventManager = new EventManager([[5,7], [2,7], [9,4]]); // Khởi tạo manager với ba event<br />
eventManager.pollHighest(); // Cả event 5 và 2 đều có priority 7, nên trả về id nhỏ hơn là 2<br />
eventManager.updatePriority(9, 7); // Event 9 lúc này có priority 7<br />
eventManager.pollHighest(); // Các event còn lại có priority cao nhất là 5 và 9, trả về 5<br />
eventManager.pollHighest(); // Trả về 9</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;EventManager&quot;, &quot;pollHighest&quot;, &quot;pollHighest&quot;, &quot;pollHighest&quot;]<br />
[[[[4, 1], [7, 2]]], [], [], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, 7, 4, -1] </span></p>

<p><strong>Giải thích</strong></p>
EventManager eventManager = new EventManager([[4,1], [7,2]]); // Khởi tạo manager với hai event<br />
eventManager.pollHighest(); // Trả về 7<br />
eventManager.pollHighest(); // Trả về 4<br />
eventManager.pollHighest(); // Không còn event nào, trả về -1</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= events.length &lt;= 10<sup>5</sup></code></li>
	<li><code>events[i] = [eventId, priority]</code></li>
	<li><code>1 &lt;= eventId &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= priority &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả giá trị <code>eventId</code> trong <code>events</code> là <strong>duy nhất</strong>.</li>
	<li><code>1 &lt;= newPriority &lt;= 10<sup>9</sup></code></li>
	<li>Trong mỗi lần gọi <code>updatePriority</code>, <code>eventId</code> đều tham chiếu đến một event <strong>đang hoạt động</strong>.</li>
	<li>Tổng số lần gọi <code>updatePriority</code> và <code>pollHighest</code> là <strong>nhiều nhất</strong> <code>10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorted Set

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần lấy event đang hoạt động có priority cao nhất, nếu hòa thì chọn $\textit{eventId}$ nhỏ nhất, đồng thời hỗ trợ cập nhật priority. Có tối đa $10^5$ thao tác.
>
> Phần tử lớn nhất nằm ở một đầu mút của ordered set; khi cập nhật, ta phải xóa key cũ rồi chèn key mới.
>
> Lưu $(-\textit{priority},\textit{eventId})$ để priority cao hơn và id nhỏ hơn đứng trước, đồng thời dùng hash map để ghi nhớ priority hiện tại nhằm xóa phần tử.
>
> Khi poll, ta cũng xóa entry tương ứng khỏi hash map.

<!-- thinking:end -->

Ta định nghĩa một sorted set $\textit{sl}$ để lưu các tuple priority và id $(-\textit{priority}, \textit{eventId})$ của tất cả event đang hoạt động, và một hash map $\textit{d}$ để lưu priority của từng event.

Khi khởi tạo, ta duyệt danh sách event đã cho, thêm tuple priority và id của mỗi event vào sorted set $\textit{sl}$, đồng thời lưu priority của từng event trong hash map $\textit{d}$.

Với thao tác $\textit{updatePriority}(eventId, newPriority)$, trước tiên ta lấy priority cũ của event từ hash map $\textit{d}$, sau đó xóa tuple gồm priority cũ và event id khỏi sorted set $\textit{sl}$, thêm tuple gồm priority mới và event id vào $\textit{sl}$, rồi cập nhật priority của event trong $\textit{d}$.

Với thao tác $\textit{pollHighest}()$, trước tiên ta kiểm tra sorted set $\textit{sl}$ có rỗng hay không. Nếu rỗng, ta trả về -1. Ngược lại, ta lấy event có priority cao nhất, tức phần tử đầu tiên, từ $\textit{sl}$, xóa tuple của event đó, xóa thông tin priority của event khỏi $\textit{d}$, rồi trả về id của event.

Độ phức tạp thời gian khi khởi tạo là $O(n \log n)$, trong đó $n$ là số event ban đầu. Mỗi lần gọi $\textit{updatePriority}$ và $\textit{pollHighest}$ mất $O(\log n)$. Độ phức tạp không gian là $O(n)$, trong đó $n$ là số event đang hoạt động.

<!-- tabs:start -->

#### Python3

```python
class EventManager:

    def __init__(self, events: list[list[int]]):
        self.sl = SortedList()
        self.d = {}
        for eventId, priority in events:
            self.sl.add((-priority, eventId))
            self.d[eventId] = priority

    def updatePriority(self, eventId: int, newPriority: int) -> None:
        old_priority = self.d[eventId]
        self.sl.remove((-old_priority, eventId))
        self.sl.add((-newPriority, eventId))
        self.d[eventId] = newPriority

    def pollHighest(self) -> int:
        if not self.sl:
            return -1
        eventId = self.sl.pop(0)[1]
        self.d.pop(eventId)
        return eventId


# Your EventManager object will be instantiated and called as such:
# obj = EventManager(events)
# obj.updatePriority(eventId,newPriority)
# param_2 = obj.pollHighest()
```

#### Java

```java
class EventManager {
    private TreeSet<int[]> sl;
    private Map<Integer, Integer> d;

    public EventManager(int[][] events) {
        sl = new TreeSet<>((a, b) -> {
            if (a[0] != b[0]) return a[0] - b[0];
            return a[1] - b[1];
        });
        d = new HashMap<>();
        for (int[] e : events) {
            int eventId = e[0], priority = e[1];
            sl.add(new int[] {-priority, eventId});
            d.put(eventId, priority);
        }
    }

    public void updatePriority(int eventId, int newPriority) {
        int old = d.get(eventId);
        sl.remove(new int[] {-old, eventId});
        sl.add(new int[] {-newPriority, eventId});
        d.put(eventId, newPriority);
    }

    public int pollHighest() {
        if (sl.isEmpty()) {
            return -1;
        }
        int[] top = sl.pollFirst();
        int eventId = top[1];
        d.remove(eventId);
        return eventId;
    }
}

/**
 * Your EventManager object will be instantiated and called as such:
 * EventManager obj = new EventManager(events);
 * obj.updatePriority(eventId,newPriority);
 * int param_2 = obj.pollHighest();
 */
```

#### C++

```cpp
class EventManager {
public:
    set<pair<int, int>> sl;
    unordered_map<int, int> d;

    EventManager(vector<vector<int>>& events) {
        for (auto& e : events) {
            int eventId = e[0], priority = e[1];
            sl.insert({-priority, eventId});
            d[eventId] = priority;
        }
    }

    void updatePriority(int eventId, int newPriority) {
        int old = d[eventId];
        sl.erase({-old, eventId});
        sl.insert({-newPriority, eventId});
        d[eventId] = newPriority;
    }

    int pollHighest() {
        if (sl.empty()) {
            return -1;
        }
        auto it = sl.begin();
        int eventId = it->second;
        sl.erase(it);
        d.erase(eventId);
        return eventId;
    }
};

/**
 * Your EventManager object will be instantiated and called as such:
 * EventManager* obj = new EventManager(events);
 * obj->updatePriority(eventId,newPriority);
 * int param_2 = obj->pollHighest();
 */
```

#### Go

```go
import (
	rbt "github.com/emirpasic/gods/v2/trees/redblacktree"
	"cmp"
)

type pair struct{ p, id int }

type EventManager struct {
	sl *rbt.Tree[pair, struct{}]
	d  map[int]int
}

func Constructor(events [][]int) EventManager {
	sl := rbt.NewWith[pair, struct{}](func(a, b pair) int {
		return cmp.Or(a.p-b.p, a.id-b.id)
	})
	d := make(map[int]int)

	for _, e := range events {
		eventId, priority := e[0], e[1]
		sl.Put(pair{-priority, eventId}, struct{}{})
		d[eventId] = priority
	}

	return EventManager{sl: sl, d: d}
}

func (this *EventManager) UpdatePriority(eventId int, newPriority int) {
	old := this.d[eventId]
	this.sl.Remove(pair{-old, eventId})
	this.sl.Put(pair{-newPriority, eventId}, struct{}{})
	this.d[eventId] = newPriority
}

func (this *EventManager) PollHighest() int {
	if this.sl.Size() == 0 {
		return -1
	}
	it := this.sl.Iterator()
	it.First()

	top := it.Key()
	eventId := top.id

	this.sl.Remove(top)
	delete(this.d, eventId)

	return eventId
}

/**
 * Your EventManager object will be instantiated and called as such:
 * obj := Constructor(events);
 * obj.UpdatePriority(eventId,newPriority);
 * param_2 := obj.PollHighest();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
