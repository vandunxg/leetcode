---
comments: true
difficulty: Medium
tags:
    - Design
    - Queue
    - Array
    - Linked List
---

<!-- problem:start -->

# [622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue)

[中文文档](/solution/0600-0699/0622.Design%20Circular%20Queue/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế cách cài đặt circular queue. Circular queue là cấu trúc dữ liệu tuyến tính, trong đó các thao tác tuân theo nguyên tắc FIFO (First In First Out), và vị trí cuối được nối lại với vị trí đầu để tạo thành một vòng tròn. Cấu trúc này còn được gọi là &quot;Ring Buffer&quot;.</p>

<p>Một ưu điểm của circular queue là có thể tận dụng các vị trí trống ở đầu queue. Với queue thông thường, khi queue đã đầy, ta không thể thêm phần tử tiếp theo dù phía trước queue có chỗ trống. Circular queue cho phép dùng vị trí đó để lưu giá trị mới.</p>

<p>Hãy cài đặt class <code>MyCircularQueue</code>:</p>

<ul>
	<li><code>MyCircularQueue(k)</code> Khởi tạo object với kích thước queue là <code>k</code>.</li>
	<li><code>int Front()</code> Lấy phần tử ở đầu queue. Nếu queue rỗng, trả về <code>-1</code>.</li>
	<li><code>int Rear()</code> Lấy phần tử cuối queue. Nếu queue rỗng, trả về <code>-1</code>.</li>
	<li><code>boolean enQueue(int value)</code> Thêm một phần tử vào circular queue. Trả về <code>true</code> nếu thao tác thành công.</li>
	<li><code>boolean deQueue()</code> Xóa một phần tử khỏi circular queue. Trả về <code>true</code> nếu thao tác thành công.</li>
	<li><code>boolean isEmpty()</code> Kiểm tra circular queue có rỗng hay không.</li>
	<li><code>boolean isFull()</code> Kiểm tra circular queue có đầy hay không.</li>
</ul>

<p>Bạn phải giải bài toán mà không dùng cấu trúc dữ liệu queue có sẵn trong ngôn ngữ lập trình.&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MyCircularQueue&quot;, &quot;enQueue&quot;, &quot;enQueue&quot;, &quot;enQueue&quot;, &quot;enQueue&quot;, &quot;Rear&quot;, &quot;isFull&quot;, &quot;deQueue&quot;, &quot;enQueue&quot;, &quot;Rear&quot;]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
<strong>Đầu ra</strong>
[null, true, true, true, false, 3, true, true, true, 4]

<strong>Giải thích</strong>
MyCircularQueue myCircularQueue = new MyCircularQueue(3);
myCircularQueue.enQueue(1); // return True
myCircularQueue.enQueue(2); // return True
myCircularQueue.enQueue(3); // return True
myCircularQueue.enQueue(4); // return False
myCircularQueue.Rear();     // return 3
myCircularQueue.isFull();   // return True
myCircularQueue.deQueue();  // return True
myCircularQueue.enQueue(4); // return True
myCircularQueue.Rear();     // return 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>0 &lt;= value &lt;= 1000</code></li>
	<li>Có nhiều nhất <code>3000</code> lần gọi đến&nbsp;<code>enQueue</code>, <code>deQueue</code>,&nbsp;<code>Front</code>,&nbsp;<code>Rear</code>,&nbsp;<code>isEmpty</code> và&nbsp;<code>isFull</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng mảng

<!-- thinking:start -->

> **Tư duy**
>
> Circular queue cần hỗ trợ enqueue/dequeue trong $O(1)$ trên mảng có kích thước cố định, đồng thời phải phân biệt được trạng thái rỗng và đầy. Chỉ dùng head và tail thì hai chỉ số có thể trùng nhau khi quay vòng.
>
> Ta cũng lưu $\textit{size}$: ghi tại $(\textit{front}+\textit{size})\bmod k$ và đọc phần tử cuối tại $(\textit{front}+\textit{size}-1)\bmod k$. Chỉ cần kiểm tra size là biết queue rỗng hay đầy.

<!-- thinking:end -->

Ta dùng mảng $q$ có độ dài $k$ để mô phỏng circular queue, cùng một pointer $\textit{front}$ để lưu vị trí phần tử đầu. Ban đầu queue rỗng và $\textit{front}$ bằng $0$. Ta cũng dùng biến $\textit{size}$ để lưu số phần tử trong queue; ban đầu $\textit{size}$ bằng $0$.

Khi gọi method `enQueue`, trước tiên ta kiểm tra queue đã đầy chưa, tức là $\textit{size} = k$. Nếu đầy, ta trả về $\textit{false}$. Nếu chưa, ta chèn phần tử tại vị trí $(\textit{front} + \textit{size}) \bmod k$, rồi cập nhật $\textit{size} = \textit{size} + 1$ để biểu thị số phần tử trong queue đã tăng thêm $1$. Cuối cùng, ta trả về $\textit{true}$.

Khi gọi method `deQueue`, trước tiên ta kiểm tra queue có rỗng không, tức là $\textit{size} = 0$. Nếu rỗng, ta trả về $\textit{false}$. Nếu không, ta cập nhật $\textit{front} = (\textit{front} + 1) \bmod k$ để đánh dấu phần tử đầu đã được lấy ra khỏi queue, rồi giảm $\textit{size}$ đi $1$.

Khi gọi method `Front`, trước tiên ta kiểm tra queue có rỗng không, tức là $\textit{size} = 0$. Nếu rỗng, ta trả về $-1$; nếu không, trả về $q[\textit{front}]$.

Khi gọi method `Rear`, trước tiên ta kiểm tra queue có rỗng không, tức là $\textit{size} = 0$. Nếu rỗng, ta trả về $-1$; nếu không, trả về $q[(\textit{front} + \textit{size} - 1) \bmod k]$.

Khi gọi method `isEmpty`, ta chỉ cần kiểm tra $\textit{size} = 0$.

Khi gọi method `isFull`, ta chỉ cần kiểm tra $\textit{size} = k$.

Các thao tác trên đều có độ phức tạp thời gian $O(1)$. Độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class MyCircularQueue:

    def __init__(self, k: int):
        self.q = [0] * k
        self.size = 0
        self.capacity = k
        self.front = 0

    def enQueue(self, value: int) -> bool:
        if self.isFull():
            return False
        self.q[(self.front + self.size) % self.capacity] = value
        self.size += 1
        return True

    def deQueue(self) -> bool:
        if self.isEmpty():
            return False
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return True

    def Front(self) -> int:
        return -1 if self.isEmpty() else self.q[self.front]

    def Rear(self) -> int:
        if self.isEmpty():
            return -1
        return self.q[(self.front + self.size - 1) % self.capacity]

    def isEmpty(self) -> bool:
        return self.size == 0

    def isFull(self) -> bool:
        return self.size == self.capacity


# Your MyCircularQueue object will be instantiated and called as such:
# obj = MyCircularQueue(k)
# param_1 = obj.enQueue(value)
# param_2 = obj.deQueue()
# param_3 = obj.Front()
# param_4 = obj.Rear()
# param_5 = obj.isEmpty()
# param_6 = obj.isFull()
```

#### Java

```java
class MyCircularQueue {
    private int[] q;
    private int front;
    private int size;
    private int capacity;

    public MyCircularQueue(int k) {
        q = new int[k];
        capacity = k;
    }

    public boolean enQueue(int value) {
        if (isFull()) {
            return false;
        }
        int idx = (front + size) % capacity;
        q[idx] = value;
        ++size;
        return true;
    }

    public boolean deQueue() {
        if (isEmpty()) {
            return false;
        }
        front = (front + 1) % capacity;
        --size;
        return true;
    }

    public int Front() {
        if (isEmpty()) {
            return -1;
        }
        return q[front];
    }

    public int Rear() {
        if (isEmpty()) {
            return -1;
        }
        int idx = (front + size - 1) % capacity;
        return q[idx];
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == capacity;
    }
}

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue obj = new MyCircularQueue(k);
 * boolean param_1 = obj.enQueue(value);
 * boolean param_2 = obj.deQueue();
 * int param_3 = obj.Front();
 * int param_4 = obj.Rear();
 * boolean param_5 = obj.isEmpty();
 * boolean param_6 = obj.isFull();
 */
```

#### C++

```cpp
class MyCircularQueue {
private:
    int front;
    int size;
    int capacity;
    vector<int> q;

public:
    MyCircularQueue(int k) {
        capacity = k;
        q = vector<int>(k);
        front = size = 0;
    }

    bool enQueue(int value) {
        if (isFull()) return false;
        int idx = (front + size) % capacity;
        q[idx] = value;
        ++size;
        return true;
    }

    bool deQueue() {
        if (isEmpty()) return false;
        front = (front + 1) % capacity;
        --size;
        return true;
    }

    int Front() {
        if (isEmpty()) return -1;
        return q[front];
    }

    int Rear() {
        if (isEmpty()) return -1;
        int idx = (front + size - 1) % capacity;
        return q[idx];
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == capacity;
    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```

#### Go

```go
type MyCircularQueue struct {
	front    int
	size     int
	capacity int
	q        []int
}

func Constructor(k int) MyCircularQueue {
	q := make([]int, k)
	return MyCircularQueue{0, 0, k, q}
}

func (this *MyCircularQueue) EnQueue(value int) bool {
	if this.IsFull() {
		return false
	}
	idx := (this.front + this.size) % this.capacity
	this.q[idx] = value
	this.size++
	return true
}

func (this *MyCircularQueue) DeQueue() bool {
	if this.IsEmpty() {
		return false
	}
	this.front = (this.front + 1) % this.capacity
	this.size--
	return true
}

func (this *MyCircularQueue) Front() int {
	if this.IsEmpty() {
		return -1
	}
	return this.q[this.front]
}

func (this *MyCircularQueue) Rear() int {
	if this.IsEmpty() {
		return -1
	}
	idx := (this.front + this.size - 1) % this.capacity
	return this.q[idx]
}

func (this *MyCircularQueue) IsEmpty() bool {
	return this.size == 0
}

func (this *MyCircularQueue) IsFull() bool {
	return this.size == this.capacity
}

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * obj := Constructor(k);
 * param_1 := obj.EnQueue(value);
 * param_2 := obj.DeQueue();
 * param_3 := obj.Front();
 * param_4 := obj.Rear();
 * param_5 := obj.IsEmpty();
 * param_6 := obj.IsFull();
 */
```

#### TypeScript

```ts
class MyCircularQueue {
    private queue: number[];
    private left: number;
    private right: number;
    private capacity: number;

    constructor(k: number) {
        this.queue = new Array(k);
        this.left = 0;
        this.right = 0;
        this.capacity = k;
    }

    enQueue(value: number): boolean {
        if (this.isFull()) {
            return false;
        }
        this.queue[this.right % this.capacity] = value;
        this.right++;
        return true;
    }

    deQueue(): boolean {
        if (this.isEmpty()) {
            return false;
        }
        this.left++;
        return true;
    }

    Front(): number {
        if (this.isEmpty()) {
            return -1;
        }
        return this.queue[this.left % this.capacity];
    }

    Rear(): number {
        if (this.isEmpty()) {
            return -1;
        }
        return this.queue[(this.right - 1) % this.capacity];
    }

    isEmpty(): boolean {
        return this.right - this.left === 0;
    }

    isFull(): boolean {
        return this.right - this.left === this.capacity;
    }
}

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * var obj = new MyCircularQueue(k)
 * var param_1 = obj.enQueue(value)
 * var param_2 = obj.deQueue()
 * var param_3 = obj.Front()
 * var param_4 = obj.Rear()
 * var param_5 = obj.isEmpty()
 * var param_6 = obj.isFull()
 */
```

#### Rust

```rust
struct MyCircularQueue {
    q: Vec<i32>,
    size: usize,
    capacity: usize,
    front: usize,
}

impl MyCircularQueue {
    fn new(k: i32) -> Self {
        MyCircularQueue {
            q: vec![0; k as usize],
            size: 0,
            capacity: k as usize,
            front: 0,
        }
    }

    fn en_queue(&mut self, value: i32) -> bool {
        if self.is_full() {
            return false;
        }
        let rear = (self.front + self.size) % self.capacity;
        self.q[rear] = value;
        self.size += 1;
        true
    }

    fn de_queue(&mut self) -> bool {
        if self.is_empty() {
            return false;
        }
        self.front = (self.front + 1) % self.capacity;
        self.size -= 1;
        true
    }

    fn front(&self) -> i32 {
        if self.is_empty() {
            -1
        } else {
            self.q[self.front]
        }
    }

    fn rear(&self) -> i32 {
        if self.is_empty() {
            -1
        } else {
            let rear = (self.front + self.size - 1) % self.capacity;
            self.q[rear]
        }
    }

    fn is_empty(&self) -> bool {
        self.size == 0
    }

    fn is_full(&self) -> bool {
        self.size == self.capacity
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
