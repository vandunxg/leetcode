---
comments: true
difficulty: Medium
rating: 1806
source: Biweekly Contest 147 Q2
tags:
    - Design
    - Hash Table
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3408. Design Task Manager](https://leetcode.com/problems/design-task-manager)

[中文文档](/solution/3400-3499/3408.Design%20Task%20Manager/README.md)

## Mô tả

<!-- description:start -->

<p>Có một hệ thống quản lý task cho phép người dùng quản lý các task của mình, trong đó mỗi task được gắn với một độ ưu tiên. Hệ thống cần xử lý hiệu quả các thao tác thêm, chỉnh sửa, thực thi và xóa task.</p>

<p>Hãy cài đặt class <code>TaskManager</code>:</p>

<ul>
    <li>
    <p><code>TaskManager(vector&lt;vector&lt;int&gt;&gt;&amp; tasks)</code> khởi tạo task manager với danh sách các bộ ba user-task-priority. Mỗi phần tử trong danh sách đầu vào có dạng <code>[userId, taskId, priority]</code>, dùng để thêm một task có độ ưu tiên tương ứng cho user được chỉ định.</p>
    </li>
    <li>
    <p><code>void add(int userId, int taskId, int priority)</code> thêm task có <code>taskId</code> và <code>priority</code> tương ứng cho user có <code>userId</code>. <strong>Đảm bảo</strong> rằng <code>taskId</code> không <em>tồn tại</em> trong hệ thống.</p>
    </li>
    <li>
    <p><code>void edit(int taskId, int newPriority)</code> cập nhật độ ưu tiên của <code>taskId</code> hiện có thành <code>newPriority</code>. <strong>Đảm bảo</strong> rằng <code>taskId</code> <em>tồn tại</em> trong hệ thống.</p>
    </li>
    <li>
    <p><code>void rmv(int taskId)</code> xóa task được xác định bởi <code>taskId</code> khỏi hệ thống. <strong>Đảm bảo</strong> rằng <code>taskId</code> <em>tồn tại</em> trong hệ thống.</p>
    </li>
    <li>
    <p><code>int execTop()</code> thực thi task có độ ưu tiên <strong>cao nhất</strong> trong tất cả user. Nếu có nhiều task cùng có độ ưu tiên <strong>cao nhất</strong>, thực thi task có <code>taskId</code> lớn nhất. Sau khi thực thi, <strong> </strong><code>taskId</code><strong> </strong> sẽ được <strong>xóa</strong> khỏi hệ thống. Trả về <code>userId</code> gắn với task đã thực thi. Nếu không có task nào, trả về -1.</p>
    </li>
</ul>

<p><strong>Lưu ý</strong> rằng một user có thể được giao nhiều task.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;TaskManager&quot;, &quot;add&quot;, &quot;edit&quot;, &quot;execTop&quot;, &quot;rmv&quot;, &quot;add&quot;, &quot;execTop&quot;]<br />
[[[[1, 101, 10], [2, 102, 20], [3, 103, 15]]], [4, 104, 5], [102, 8], [], [101], [5, 105, 15], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, null, 3, null, null, 5] </span></p>

<p><strong>Giải thích</strong></p>
TaskManager taskManager = new TaskManager([[1, 101, 10], [2, 102, 20], [3, 103, 15]]); // Initializes with three tasks for Users 1, 2, and 3.<br />
taskManager.add(4, 104, 5); // Adds task 104 with priority 5 for User 4.<br />
taskManager.edit(102, 8); // Updates priority of task 102 to 8.<br />
taskManager.execTop(); // return 3. Executes task 103 for User 3.<br />
taskManager.rmv(101); // Removes task 101 from the system.<br />
taskManager.add(5, 105, 15); // Adds task 105 with priority 15 for User 5.<br />
taskManager.execTop(); // return 5. Executes task 105 for User 5.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= tasks.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= userId &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= taskId &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= priority &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= newPriority &lt;= 10<sup>9</sup></code></li>
    <li>Tổng cộng có <strong>nhiều nhất</strong> <code>2 * 10<sup>5</sup></code> lần gọi đến các phương thức <code>add</code>, <code>edit</code>, <code>rmv</code> và <code>execTop</code>.</li>
    <li>Đầu vào được tạo sao cho <code>taskId</code> luôn hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Map + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Có thể có đến $2\times 10^5$ thao tác, trong đó cần tìm task có độ ưu tiên cao nhất (nếu bằng nhau thì chọn task có id lớn nhất), đồng thời chỉnh sửa hoặc xóa theo $\textit{taskId}$. Duyệt qua toàn bộ task ở mỗi lần gọi $\textit{execTop}$ sẽ quá chậm.
>
> Hash map tìm user và độ ưu tiên của một task trong $O(1)$, nhưng không tìm được giá trị lớn nhất trên toàn cục. Heap hoặc ordered set duy trì giá trị lớn nhất, nhưng khi chỉnh sửa vẫn cần tìm tuple cũ.
>
> Ta lưu ánh xạ $\textit{taskId}\mapsto(\textit{userId},\textit{priority})$ trong hash map $\textit{d}$, và $(-\textit{priority},-\textit{taskId})$ trong ordered set để task tốt nhất nằm ở đầu. Mỗi lần cập nhật đều tác động đến cả hai cấu trúc; $\textit{execTop}$ lấy phần tử đầu tiên của set.

<!-- thinking:end -->

Ta dùng một hash map $\text{d}$ để lưu thông tin task, trong đó key là ID của task và value là tuple $(\text{userId}, \text{priority})$ đại diện cho ID user và độ ưu tiên của task.

Ta dùng một ordered set $\text{st}$ để lưu tất cả task hiện có trong hệ thống, trong đó mỗi phần tử là tuple $(-\text{priority}, -\text{taskId})$ đại diện cho độ ưu tiên âm và ID task âm. Việc dùng các giá trị âm giúp task có độ ưu tiên cao nhất và ID task lớn nhất xuất hiện đầu tiên trong ordered set.

Với mỗi thao tác, ta xử lý như sau:

- **Khởi tạo**: Với mỗi task $(\text{userId}, \text{taskId}, \text{priority})$, thêm task đó vào hash map $\text{d}$ và ordered set $\text{st}$.
- **Thêm task**: Thêm task $(\text{userId}, \text{taskId}, \text{priority})$ vào hash map $\text{d}$ và ordered set $\text{st}$.
- **Chỉnh sửa task**: Lấy user ID và độ ưu tiên cũ của task ID đã cho từ hash map $\text{d}$, xóa thông tin task cũ khỏi ordered set $\text{st}$, sau đó thêm thông tin task mới vào cả hash map và ordered set.
- **Xóa task**: Lấy độ ưu tiên của task ID đã cho từ hash map $\text{d}$, xóa thông tin task khỏi ordered set $\text{st}$, rồi xóa task khỏi hash map.
- **Thực thi task có độ ưu tiên cao nhất**: Nếu ordered set $\text{st}$ rỗng, trả về -1. Nếu không, lấy phần tử đầu tiên của ordered set, lấy ID task, lấy user ID tương ứng từ hash map, rồi xóa task khỏi cả hash map và ordered set. Cuối cùng, trả về user ID.

Về độ phức tạp thời gian, việc khởi tạo cần $O(n \log n)$, trong đó $n$ là số task ban đầu. Mỗi thao tác thêm, chỉnh sửa, xóa và thực thi cần $O(\log m)$, trong đó $m$ là số task hiện có trong hệ thống. Vì tổng số thao tác không vượt quá $2 \times 10^5$, độ phức tạp thời gian tổng thể là chấp nhận được. Độ phức tạp không gian là $O(n + m)$ để lưu hash map và ordered set.

<!-- tabs:start -->

#### Python3

```python
class TaskManager:

    def __init__(self, tasks: List[List[int]]):
        self.d = {}
        self.st = SortedList()
        for task in tasks:
            self.add(*task)

    def add(self, userId: int, taskId: int, priority: int) -> None:
        self.d[taskId] = (userId, priority)
        self.st.add((-priority, -taskId))

    def edit(self, taskId: int, newPriority: int) -> None:
        userId, priority = self.d[taskId]
        self.st.discard((-priority, -taskId))
        self.d[taskId] = (userId, newPriority)
        self.st.add((-newPriority, -taskId))

    def rmv(self, taskId: int) -> None:
        _, priority = self.d[taskId]
        self.d.pop(taskId)
        self.st.remove((-priority, -taskId))

    def execTop(self) -> int:
        if not self.st:
            return -1
        taskId = -self.st.pop(0)[1]
        userId, _ = self.d[taskId]
        self.d.pop(taskId)
        return userId


# Your TaskManager object will be instantiated and called as such:
# obj = TaskManager(tasks)
# obj.add(userId,taskId,priority)
# obj.edit(taskId,newPriority)
# obj.rmv(taskId)
# param_4 = obj.execTop()
```

#### Java

```java
class TaskManager {
    private final Map<Integer, int[]> d = new HashMap<>();
    private final TreeSet<int[]> st = new TreeSet<>((a, b) -> {
        if (a[0] == b[0]) {
            return b[1] - a[1];
        }
        return b[0] - a[0];
    });

    public TaskManager(List<List<Integer>> tasks) {
        for (var task : tasks) {
            add(task.get(0), task.get(1), task.get(2));
        }
    }

    public void add(int userId, int taskId, int priority) {
        d.put(taskId, new int[] {userId, priority});
        st.add(new int[] {priority, taskId});
    }

    public void edit(int taskId, int newPriority) {
        var e = d.get(taskId);
        int userId = e[0], priority = e[1];
        st.remove(new int[] {priority, taskId});
        st.add(new int[] {newPriority, taskId});
        d.put(taskId, new int[] {userId, newPriority});
    }

    public void rmv(int taskId) {
        var e = d.remove(taskId);
        int priority = e[1];
        st.remove(new int[] {priority, taskId});
    }

    public int execTop() {
        if (st.isEmpty()) {
            return -1;
        }
        var e = st.pollFirst();
        var t = d.remove(e[1]);
        return t[0];
    }
}

/**
 * Your TaskManager object will be instantiated and called as such:
 * TaskManager obj = new TaskManager(tasks);
 * obj.add(userId,taskId,priority);
 * obj.edit(taskId,newPriority);
 * obj.rmv(taskId);
 * int param_4 = obj.execTop();
 */
```

#### C++

```cpp
class TaskManager {
private:
    unordered_map<int, pair<int, int>> d;
    set<pair<int, int>> st;

public:
    TaskManager(vector<vector<int>>& tasks) {
        for (const auto& task : tasks) {
            add(task[0], task[1], task[2]);
        }
    }

    void add(int userId, int taskId, int priority) {
        d[taskId] = {userId, priority};
        st.insert({-priority, -taskId});
    }

    void edit(int taskId, int newPriority) {
        auto [userId, priority] = d[taskId];
        st.erase({-priority, -taskId});
        st.insert({-newPriority, -taskId});
        d[taskId] = {userId, newPriority};
    }

    void rmv(int taskId) {
        auto [userId, priority] = d[taskId];
        st.erase({-priority, -taskId});
        d.erase(taskId);
    }

    int execTop() {
        if (st.empty()) {
            return -1;
        }
        auto e = *st.begin();
        st.erase(st.begin());
        int taskId = -e.second;
        int userId = d[taskId].first;
        d.erase(taskId);
        return userId;
    }
};

/**
 * Your TaskManager object will be instantiated and called as such:
 * TaskManager* obj = new TaskManager(tasks);
 * obj->add(userId,taskId,priority);
 * obj->edit(taskId,newPriority);
 * obj->rmv(taskId);
 * int param_4 = obj->execTop();
 */
```

#### Go

```go
type TaskManager struct {
    d  map[int][2]int
    st *redblacktree.Tree[int, int]
}

func encode(priority, taskId int) int {
    return (priority << 32) | taskId
}

func comparator(a, b int) int {
    if a > b {
        return -1
    } else if a < b {
        return 1
    }
    return 0
}

func Constructor(tasks [][]int) TaskManager {
    tm := TaskManager{
        d:  make(map[int][2]int),
        st: redblacktree.NewWith[int, int](comparator),
    }
    for _, task := range tasks {
        tm.Add(task[0], task[1], task[2])
    }
    return tm
}

func (this *TaskManager) Add(userId int, taskId int, priority int) {
    this.d[taskId] = [2]int{userId, priority}
    this.st.Put(encode(priority, taskId), taskId)
}

func (this *TaskManager) Edit(taskId int, newPriority int) {
    if e, ok := this.d[taskId]; ok {
        priority := e[1]
        this.st.Remove(encode(priority, taskId))
        this.d[taskId] = [2]int{e[0], newPriority}
        this.st.Put(encode(newPriority, taskId), taskId)
    }
}

func (this *TaskManager) Rmv(taskId int) {
    if e, ok := this.d[taskId]; ok {
        priority := e[1]
        delete(this.d, taskId)
        this.st.Remove(encode(priority, taskId))
    }
}

func (this *TaskManager) ExecTop() int {
    if this.st.Empty() {
        return -1
    }
    it := this.st.Iterator()
    it.Next()
    taskId := it.Value()
    if e, ok := this.d[taskId]; ok {
        delete(this.d, taskId)
        this.st.Remove(it.Key())
        return e[0]
    }
    return -1
}

/**
 * Your TaskManager object will be instantiated and called as such:
 * obj := Constructor(tasks);
 * obj.Add(userId,taskId,priority);
 * obj.Edit(taskId,newPriority);
 * obj.Rmv(taskId);
 * param_4 := obj.ExecTop();
 */
```

#### TypeScript

```ts
class TaskManager {
    private d: Map<number, [number, number]>;
    private pq: PriorityQueue<[number, number]>;

    constructor(tasks: number[][]) {
        this.d = new Map();
        this.pq = new PriorityQueue<[number, number]>((a, b) => {
            if (a[0] === b[0]) {
                return b[1] - a[1];
            }
            return b[0] - a[0];
        });
        for (const task of tasks) {
            this.add(task[0], task[1], task[2]);
        }
    }

    add(userId: number, taskId: number, priority: number): void {
        this.d.set(taskId, [userId, priority]);
        this.pq.enqueue([priority, taskId]);
    }

    edit(taskId: number, newPriority: number): void {
        const e = this.d.get(taskId);
        if (!e) return;
        const userId = e[0];
        this.d.set(taskId, [userId, newPriority]);
        this.pq.enqueue([newPriority, taskId]);
    }

    rmv(taskId: number): void {
        this.d.delete(taskId);
    }

    execTop(): number {
        while (!this.pq.isEmpty()) {
            const [priority, taskId] = this.pq.dequeue();
            const e = this.d.get(taskId);
            if (e && e[1] === priority) {
                this.d.delete(taskId);
                return e[0];
            }
        }
        return -1;
    }
}

/**
 * Your TaskManager object will be instantiated and called as such:
 * var obj = new TaskManager(tasks)
 * obj.add(userId,taskId,priority)
 * obj.edit(taskId,newPriority)
 * obj.rmv(taskId)
 * var param_4 = obj.execTop()
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
