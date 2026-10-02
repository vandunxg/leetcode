---
comments: true
difficulty: Medium
tags:
    - Concurrency
---

<!-- problem:start -->

# [1188. Design Bounded Blocking Queue 🔒](https://leetcode.com/problems/design-bounded-blocking-queue)

[中文文档](/solution/1100-1199/1188.Design%20Bounded%20Blocking%20Queue/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy triển khai một blocking queue có giới hạn, thread-safe, với các method sau:</p>

<ul>
	<li><code>BoundedBlockingQueue(int capacity)</code> Khởi tạo queue với sức chứa tối đa là <code>capacity</code>.</li>
	<li><code>void enqueue(int element)</code> Thêm <code>element</code> vào đầu queue. Nếu queue đầy, thread gọi sẽ bị block cho đến khi queue không còn đầy.</li>
	<li><code>int dequeue()</code> Trả về và xóa phần tử ở cuối queue. Nếu queue rỗng, thread gọi sẽ bị block cho đến khi queue không còn rỗng.</li>
	<li><code>int size()</code> Trả về số phần tử hiện có trong queue.</li>
</ul>

<p>Implementation của bạn sẽ được kiểm thử bằng nhiều thread chạy đồng thời. Mỗi thread sẽ là producer chỉ gọi method <code>enqueue</code> hoặc consumer chỉ gọi method <code>dequeue</code>. Method <code>size</code> sẽ được gọi sau mỗi test case.</p>

<p>Không sử dụng implementation bounded blocking queue có sẵn; cách đó sẽ không được chấp nhận trong buổi phỏng vấn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong>
1
1
[&quot;BoundedBlockingQueue&quot;,&quot;enqueue&quot;,&quot;dequeue&quot;,&quot;dequeue&quot;,&quot;enqueue&quot;,&quot;enqueue&quot;,&quot;enqueue&quot;,&quot;enqueue&quot;,&quot;dequeue&quot;]
[[2],[1],[],[],[0],[2],[3],[4],[]]

<strong>Output:</strong>
[1,0,2,2]

<strong>Giải thích:</strong>
Số thread producer = 1
Số thread consumer = 1

BoundedBlockingQueue queue = new BoundedBlockingQueue(2);   // initialize the queue with capacity = 2.

queue.enqueue(1);   // The producer thread enqueues 1 to the queue.
queue.dequeue();    // The consumer thread calls dequeue and returns 1 from the queue.
queue.dequeue();    // Since the queue is empty, the consumer thread is blocked.
queue.enqueue(0);   // The producer thread enqueues 0 to the queue. The consumer thread is unblocked and returns 0 from the queue.
queue.enqueue(2);   // The producer thread enqueues 2 to the queue.
queue.enqueue(3);   // The producer thread enqueues 3 to the queue.
queue.enqueue(4);   // The producer thread is blocked because the queue&#39;s capacity (2) is reached.
queue.dequeue();    // The consumer thread returns 2 from the queue. The producer thread is unblocked and enqueues 4 to the queue.
queue.size();       // 2 elements remaining in the queue. size() is always called at the end of each test case.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong>
3
4
[&quot;BoundedBlockingQueue&quot;,&quot;enqueue&quot;,&quot;enqueue&quot;,&quot;enqueue&quot;,&quot;dequeue&quot;,&quot;dequeue&quot;,&quot;dequeue&quot;,&quot;enqueue&quot;]
[[3],[1],[0],[2],[],[],[],[3]]
<strong>Output:</strong>
[1,0,2,1]

<strong>Giải thích:</strong>
Số thread producer = 3
Số thread consumer = 4

BoundedBlockingQueue queue = new BoundedBlockingQueue(3);   // initialize the queue with capacity = 3.

queue.enqueue(1);   // Producer thread P1 enqueues 1 to the queue.
queue.enqueue(0);   // Producer thread P2 enqueues 0 to the queue.
queue.enqueue(2);   // Producer thread P3 enqueues 2 to the queue.
queue.dequeue();    // Consumer thread C1 calls dequeue.
queue.dequeue();    // Consumer thread C2 calls dequeue.
queue.dequeue();    // Consumer thread C3 calls dequeue.
queue.enqueue(3);   // One of the producer threads enqueues 3 to the queue.
queue.size();       // 1 element remaining in the queue.

Vì có nhiều hơn một thread producer/consumer nên ta không thể biết hệ điều hành sẽ lập lịch các thread theo thứ tự nào, dù input có vẻ ngụ ý một thứ tự cụ thể. Do đó, mọi output sau đều được chấp nhận: [1,0,2], [1,2,0], [0,1,2], [0,2,1], [2,0,1] hoặc [2,1,0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= Number of Prdoucers &lt;= 8</code></li>
	<li><code>1 &lt;= Number of Consumers &lt;= 8</code></li>
	<li><code>1 &lt;= size &lt;= 30</code></li>
	<li><code>0 &lt;= element &lt;= 20</code></li>
	<li>Số lần gọi <code>enqueue</code> <strong>lớn hơn hoặc bằng</strong> số lần gọi <code>dequeue</code>.</li>
	<li>Tổng số lần gọi <code>enque</code>, <code>deque</code> và <code>size</code> không quá <code>40</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khi chạy đồng thời, queue có giới hạn phải block producer lúc queue đầy và consumer lúc queue rỗng. Một semaphore sức chứa kiểm soát `enqueue`, còn semaphore phần tử kiểm soát `dequeue`: enqueue lấy một slot rồi tăng semaphore phần tử; dequeue thực hiện ngược lại. Chỉ truy cập deque sau khi lấy được permit tương ứng, nhờ đó bảo đảm sức chứa và thứ tự FIFO.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
from threading import Semaphore


class BoundedBlockingQueue(object):
    def __init__(self, capacity: int):
        self.s1 = Semaphore(capacity)
        self.s2 = Semaphore(0)
        self.q = deque()

    def enqueue(self, element: int) -> None:
        self.s1.acquire()
        self.q.append(element)
        self.s2.release()

    def dequeue(self) -> int:
        self.s2.acquire()
        ans = self.q.popleft()
        self.s1.release()
        return ans

    def size(self) -> int:
        return len(self.q)
```

#### Java

```java
class BoundedBlockingQueue {
    private Semaphore s1;
    private Semaphore s2;
    private Deque<Integer> q = new ArrayDeque<>();

    public BoundedBlockingQueue(int capacity) {
        s1 = new Semaphore(capacity);
        s2 = new Semaphore(0);
    }

    public void enqueue(int element) throws InterruptedException {
        s1.acquire();
        q.offer(element);
        s2.release();
    }

    public int dequeue() throws InterruptedException {
        s2.acquire();
        int ans = q.poll();
        s1.release();
        return ans;
    }

    public int size() {
        return q.size();
    }
}
```

#### C++

```cpp
#include <semaphore.h>

class BoundedBlockingQueue {
public:
    BoundedBlockingQueue(int capacity) {
        sem_init(&s1, 0, capacity);
        sem_init(&s2, 0, 0);
    }

    void enqueue(int element) {
        sem_wait(&s1);
        q.push(element);
        sem_post(&s2);
    }

    int dequeue() {
        sem_wait(&s2);
        int ans = q.front();
        q.pop();
        sem_post(&s1);
        return ans;
    }

    int size() {
        return q.size();
    }

private:
    queue<int> q;
    sem_t s1, s2;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
