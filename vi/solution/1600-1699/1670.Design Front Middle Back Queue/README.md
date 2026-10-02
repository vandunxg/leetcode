---
comments: true
difficulty: Medium
rating: 1610
source: Biweekly Contest 40 Q3
tags:
    - Design
    - Queue
    - Array
    - Linked List
    - Data Stream
    - Doubly-Linked List
---

<!-- problem:start -->

# [1670. Design Front Middle Back Queue](https://leetcode.com/problems/design-front-middle-back-queue)

[中文文档](/solution/1600-1699/1670.Design%20Front%20Middle%20Back%20Queue/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một queue hỗ trợ các thao tác <code>push</code> và <code>pop</code> ở đầu, giữa và cuối.</p>

<p>Triển khai class <code>FrontMiddleBack</code>:</p>

<ul>
	<li><code>FrontMiddleBack()</code> Khởi tạo queue.</li>
	<li><code>void pushFront(int val)</code> Thêm <code>val</code> vào <strong>đầu</strong> queue.</li>
	<li><code>void pushMiddle(int val)</code> Thêm <code>val</code> vào <strong>giữa</strong> queue.</li>
	<li><code>void pushBack(int val)</code> Thêm <code>val</code> vào <strong>cuối</strong> queue.</li>
	<li><code>int popFront()</code> Xóa và trả về phần tử <strong>đầu</strong> queue. Nếu queue rỗng, trả về <code>-1</code>.</li>
	<li><code>int popMiddle()</code> Xóa và trả về phần tử <strong>giữa</strong> queue. Nếu queue rỗng, trả về <code>-1</code>.</li>
	<li><code>int popBack()</code> Xóa và trả về phần tử <strong>cuối</strong> queue. Nếu queue rỗng, trả về <code>-1</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng khi có <b>hai</b> vị trí giữa có thể chọn, thao tác được thực hiện trên vị trí giữa <strong>gần đầu hơn</strong>. Ví dụ:</p>

<ul>
	<li>Thêm <code>6</code> vào giữa <code>[1, 2, 3, 4, 5]</code> cho kết quả <code>[1, 2, <u>6</u>, 3, 4, 5]</code>.</li>
	<li>Xóa phần tử giữa khỏi <code>[1, 2, <u>3</u>, 4, 5, 6]</code> trả về <code>3</code> và cho kết quả <code>[1, 2, 4, 5, 6]</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong>
[&quot;FrontMiddleBackQueue&quot;, &quot;pushFront&quot;, &quot;pushBack&quot;, &quot;pushMiddle&quot;, &quot;pushMiddle&quot;, &quot;popFront&quot;, &quot;popMiddle&quot;, &quot;popMiddle&quot;, &quot;popBack&quot;, &quot;popFront&quot;]
[[], [1], [2], [3], [4], [], [], [], [], []]
<strong>Output:</strong>
[null, null, null, null, null, 1, 3, 4, 2, -1]

<strong>Giải thích:</strong>
FrontMiddleBackQueue q = new FrontMiddleBackQueue();
q.pushFront(1);   // [<u>1</u>]
q.pushBack(2);    // [1, <u>2</u>]
q.pushMiddle(3);  // [1, <u>3</u>, 2]
q.pushMiddle(4);  // [1, <u>4</u>, 3, 2]
q.popFront();     // return 1 -&gt; [4, 3, 2]
q.popMiddle();    // return 3 -&gt; [4, 2]
q.popMiddle();    // return 4 -&gt; [2]
q.popBack();      // return 2 -&gt; []
q.popFront();     // return -1 -&gt; [] (Queue rỗng)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= val &lt;= 10<sup>9</sup></code></li>
	<li>Sẽ có nhiều nhất&nbsp;<code>1000</code>&nbsp;lần gọi các hàm&nbsp;<code>pushFront</code>,&nbsp;<code>pushMiddle</code>,&nbsp;<code>pushBack</code>, <code>popFront</code>, <code>popMiddle</code> và <code>popBack</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai deque

<!-- thinking:start -->

> **Tư duy**
>
> Cần chèn/xóa trong $O(1)$ ở đầu, giữa và cuối; tìm vị trí giữa của một list sẽ tốn thời gian tuyến tính. Chia queue thành hai deque $q_1$ (nửa đầu) và $q_2$ (nửa sau), sao cho $q_2$ dài hơn $q_1$ nhiều nhất một phần tử.
>
> Sau mỗi lần cập nhật, $\texttt{rebalance}$ di chuyển một phần tử. Khi hai deque có cùng kích thước, phần tử giữa là phần tử cuối của $q_1$; nếu không, đó là phần tử đầu của $q_2$.

<!-- thinking:end -->

Ta dùng hai deque, trong đó $q_1$ lưu nửa đầu và $q_2$ lưu nửa sau. Hàm `rebalance` duy trì cân bằng giữa hai queue: độ dài của $q_2$ không nhỏ hơn $q_1$ và chênh lệch độ dài không vượt quá $1$.

Trong các hàm `pushFront`, `pushMiddle` và `pushBack`, ta chỉ cần thêm phần tử vào $q_1$ hoặc $q_2$, rồi gọi hàm `rebalance`.

Với hàm `popFront`, trước hết kiểm tra $q_1$ và $q_2$ có rỗng không. Nếu cả hai đều rỗng, trả về $-1$. Nếu không, nếu $q_1$ không rỗng thì xóa phần tử đầu của $q_1$; ngược lại xóa phần tử đầu của $q_2$, rồi gọi `rebalance`.

Với hàm `popMiddle`, nếu cả $q_1$ và $q_2$ đều rỗng thì trả về $-1$. Nếu không, nếu hai deque có cùng độ dài thì xóa phần tử cuối của $q_1$; ngược lại xóa phần tử đầu của $q_2$, rồi gọi `rebalance`.

Với hàm `popBack`, chỉ cần kiểm tra $q_2$ có rỗng không. Nếu $q_1$ và $q_2$ đều rỗng, trả về $-1$; ngược lại xóa phần tử cuối của $q_2$, rồi gọi `rebalance`.

Độ phức tạp thời gian của các thao tác trên là $O(1)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số phần tử trong queue.

<!-- tabs:start -->

#### Python3

```python
class FrontMiddleBackQueue:
    def __init__(self):
        self.q1 = deque()
        self.q2 = deque()

    def pushFront(self, val: int) -> None:
        self.q1.appendleft(val)
        self.rebalance()

    def pushMiddle(self, val: int) -> None:
        self.q1.append(val)
        self.rebalance()

    def pushBack(self, val: int) -> None:
        self.q2.append(val)
        self.rebalance()

    def popFront(self) -> int:
        if not self.q1 and not self.q2:
            return -1
        if self.q1:
            val = self.q1.popleft()
        else:
            val = self.q2.popleft()
        self.rebalance()
        return val

    def popMiddle(self) -> int:
        if not self.q1 and not self.q2:
            return -1
        if len(self.q1) == len(self.q2):
            val = self.q1.pop()
        else:
            val = self.q2.popleft()
        self.rebalance()
        return val

    def popBack(self) -> int:
        if not self.q2:
            return -1
        val = self.q2.pop()
        self.rebalance()
        return val

    def rebalance(self):
        if len(self.q1) > len(self.q2):
            self.q2.appendleft(self.q1.pop())
        if len(self.q2) > len(self.q1) + 1:
            self.q1.append(self.q2.popleft())


# Your FrontMiddleBackQueue object will be instantiated and called as such:
# obj = FrontMiddleBackQueue()
# obj.pushFront(val)
# obj.pushMiddle(val)
# obj.pushBack(val)
# param_4 = obj.popFront()
# param_5 = obj.popMiddle()
# param_6 = obj.popBack()
```

#### Java

```java
class FrontMiddleBackQueue {
    private Deque<Integer> q1 = new ArrayDeque<>();
    private Deque<Integer> q2 = new ArrayDeque<>();

    public FrontMiddleBackQueue() {
    }

    public void pushFront(int val) {
        q1.offerFirst(val);
        rebalance();
    }

    public void pushMiddle(int val) {
        q1.offerLast(val);
        rebalance();
    }

    public void pushBack(int val) {
        q2.offerLast(val);
        rebalance();
    }

    public int popFront() {
        if (q1.isEmpty() && q2.isEmpty()) {
            return -1;
        }
        int val = q1.isEmpty() ? q2.pollFirst() : q1.pollFirst();
        rebalance();
        return val;
    }

    public int popMiddle() {
        if (q1.isEmpty() && q2.isEmpty()) {
            return -1;
        }
        int val = q1.size() == q2.size() ? q1.pollLast() : q2.pollFirst();
        rebalance();
        return val;
    }

    public int popBack() {
        if (q2.isEmpty()) {
            return -1;
        }
        int val = q2.pollLast();
        rebalance();
        return val;
    }

    private void rebalance() {
        if (q1.size() > q2.size()) {
            q2.offerFirst(q1.pollLast());
        }
        if (q2.size() > q1.size() + 1) {
            q1.offerLast(q2.pollFirst());
        }
    }
}

/**
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * FrontMiddleBackQueue obj = new FrontMiddleBackQueue();
 * obj.pushFront(val);
 * obj.pushMiddle(val);
 * obj.pushBack(val);
 * int param_4 = obj.popFront();
 * int param_5 = obj.popMiddle();
 * int param_6 = obj.popBack();
 */
```

#### C++

```cpp
class FrontMiddleBackQueue {
public:
    FrontMiddleBackQueue() {
    }

    void pushFront(int val) {
        q1.push_front(val);
        rebalance();
    }

    void pushMiddle(int val) {
        q1.push_back(val);
        rebalance();
    }

    void pushBack(int val) {
        q2.push_back(val);
        rebalance();
    }

    int popFront() {
        if (q1.empty() && q2.empty()) return -1;
        int val = 0;
        if (q1.size()) {
            val = q1.front();
            q1.pop_front();
        } else {
            val = q2.front();
            q2.pop_front();
        }
        rebalance();
        return val;
    }

    int popMiddle() {
        if (q1.empty() && q2.empty()) return -1;
        int val = 0;
        if (q1.size() == q2.size()) {
            val = q1.back();
            q1.pop_back();
        } else {
            val = q2.front();
            q2.pop_front();
        }
        rebalance();
        return val;
    }

    int popBack() {
        if (q2.empty()) return -1;
        int val = q2.back();
        q2.pop_back();
        rebalance();
        return val;
    }

private:
    deque<int> q1;
    deque<int> q2;

    void rebalance() {
        if (q1.size() > q2.size()) {
            q2.push_front(q1.back());
            q1.pop_back();
        }
        if (q2.size() > q1.size() + 1) {
            q1.push_back(q2.front());
            q2.pop_front();
        }
    }
};

/**
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * FrontMiddleBackQueue* obj = new FrontMiddleBackQueue();
 * obj->pushFront(val);
 * obj->pushMiddle(val);
 * obj->pushBack(val);
 * int param_4 = obj->popFront();
 * int param_5 = obj->popMiddle();
 * int param_6 = obj->popBack();
 */
```

#### Go

```go
type FrontMiddleBackQueue struct {
	q1, q2 Deque
}

func Constructor() FrontMiddleBackQueue {
	return FrontMiddleBackQueue{}
}

func (this *FrontMiddleBackQueue) PushFront(val int) {
	this.q1.PushFront(val)
	this.rebalance()
}

func (this *FrontMiddleBackQueue) PushMiddle(val int) {
	this.q1.PushBack(val)
	this.rebalance()
}

func (this *FrontMiddleBackQueue) PushBack(val int) {
	this.q2.PushBack(val)
	this.rebalance()
}

func (this *FrontMiddleBackQueue) PopFront() int {
	if this.q1.Empty() && this.q2.Empty() {
		return -1
	}
	var val int
	if !this.q1.Empty() {
		val = this.q1.PopFront()
	} else {
		val = this.q2.PopFront()
	}
	this.rebalance()
	return val
}

func (this *FrontMiddleBackQueue) PopMiddle() int {
	if this.q1.Empty() && this.q2.Empty() {
		return -1
	}
	var val int
	if this.q1.Size() == this.q2.Size() {
		val = this.q1.PopBack()
	} else {
		val = this.q2.PopFront()
	}
	this.rebalance()
	return val
}

func (this *FrontMiddleBackQueue) PopBack() int {
	if this.q2.Empty() {
		return -1
	}
	val := this.q2.PopBack()
	this.rebalance()
	return val
}

func (this *FrontMiddleBackQueue) rebalance() {
	if this.q1.Size() > this.q2.Size() {
		this.q2.PushFront(this.q1.PopBack())
	}
	if this.q2.Size() > this.q1.Size()+1 {
		this.q1.PushBack(this.q2.PopFront())
	}
}

// template
type Deque struct{ l, r []int }

func (q Deque) Empty() bool {
	return len(q.l) == 0 && len(q.r) == 0
}

func (q Deque) Size() int {
	return len(q.l) + len(q.r)
}

func (q *Deque) PushFront(v int) {
	q.l = append(q.l, v)
}

func (q *Deque) PushBack(v int) {
	q.r = append(q.r, v)
}

func (q *Deque) PopFront() (v int) {
	if len(q.l) > 0 {
		q.l, v = q.l[:len(q.l)-1], q.l[len(q.l)-1]
	} else {
		v, q.r = q.r[0], q.r[1:]
	}
	return
}

func (q *Deque) PopBack() (v int) {
	if len(q.r) > 0 {
		q.r, v = q.r[:len(q.r)-1], q.r[len(q.r)-1]
	} else {
		v, q.l = q.l[0], q.l[1:]
	}
	return
}

func (q Deque) Front() int {
	if len(q.l) > 0 {
		return q.l[len(q.l)-1]
	}
	return q.r[0]
}

func (q Deque) Back() int {
	if len(q.r) > 0 {
		return q.r[len(q.r)-1]
	}
	return q.l[0]
}

func (q Deque) Get(i int) int {
	if i < len(q.l) {
		return q.l[len(q.l)-1-i]
	}
	return q.r[i-len(q.l)]
}

/**
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.PushFront(val);
 * obj.PushMiddle(val);
 * obj.PushBack(val);
 * param_4 := obj.PopFront();
 * param_5 := obj.PopMiddle();
 * param_6 := obj.PopBack();
 */
```

#### TypeScript

```ts
class FrontMiddleBackQueue {
    private q1: Deque<number>;
    private q2: Deque<number>;

    constructor() {
        this.q1 = new Deque<number>();
        this.q2 = new Deque<number>();
    }

    pushFront(val: number): void {
        this.q1.pushFront(val);
        this.rebalance();
    }

    pushMiddle(val: number): void {
        this.q1.pushBack(val);
        this.rebalance();
    }

    pushBack(val: number): void {
        this.q2.pushBack(val);
        this.rebalance();
    }

    popFront(): number {
        if (this.q1.isEmpty() && this.q2.isEmpty()) {
            return -1;
        }
        const val = this.q1.isEmpty() ? this.q2.popFront() : this.q1.popFront();
        this.rebalance();
        return val!;
    }

    popMiddle(): number {
        if (this.q1.isEmpty() && this.q2.isEmpty()) {
            return -1;
        }
        const val =
            this.q1.getSize() === this.q2.getSize() ? this.q1.popBack() : this.q2.popFront();
        this.rebalance();
        return val!;
    }

    popBack(): number {
        if (this.q2.isEmpty()) {
            return -1;
        }
        const val = this.q2.popBack();
        this.rebalance();
        return val!;
    }

    private rebalance(): void {
        if (this.q1.getSize() > this.q2.getSize()) {
            this.q2.pushFront(this.q1.popBack()!);
        }
        if (this.q2.getSize() > this.q1.getSize() + 1) {
            this.q1.pushBack(this.q2.popFront()!);
        }
    }
}

class Node<T> {
    value: T;
    next: Node<T> | null;
    prev: Node<T> | null;

    constructor(value: T) {
        this.value = value;
        this.next = null;
        this.prev = null;
    }
}

class Deque<T> {
    private front: Node<T> | null;
    private back: Node<T> | null;
    private size: number;

    constructor() {
        this.front = null;
        this.back = null;
        this.size = 0;
    }

    pushFront(val: T): void {
        const newNode = new Node(val);
        if (this.isEmpty()) {
            this.front = newNode;
            this.back = newNode;
        } else {
            newNode.next = this.front;
            this.front!.prev = newNode;
            this.front = newNode;
        }
        this.size++;
    }

    pushBack(val: T): void {
        const newNode = new Node(val);
        if (this.isEmpty()) {
            this.front = newNode;
            this.back = newNode;
        } else {
            newNode.prev = this.back;
            this.back!.next = newNode;
            this.back = newNode;
        }
        this.size++;
    }

    popFront(): T | undefined {
        if (this.isEmpty()) {
            return undefined;
        }
        const value = this.front!.value;
        this.front = this.front!.next;
        if (this.front !== null) {
            this.front.prev = null;
        } else {
            this.back = null;
        }
        this.size--;
        return value;
    }

    popBack(): T | undefined {
        if (this.isEmpty()) {
            return undefined;
        }
        const value = this.back!.value;
        this.back = this.back!.prev;
        if (this.back !== null) {
            this.back.next = null;
        } else {
            this.front = null;
        }
        this.size--;
        return value;
    }

    frontValue(): T | undefined {
        return this.front?.value;
    }

    backValue(): T | undefined {
        return this.back?.value;
    }

    getSize(): number {
        return this.size;
    }

    isEmpty(): boolean {
        return this.size === 0;
    }
}

/**
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * var obj = new FrontMiddleBackQueue()
 * obj.pushFront(val)
 * obj.pushMiddle(val)
 * obj.pushBack(val)
 * var param_4 = obj.popFront()
 * var param_5 = obj.popMiddle()
 * var param_6 = obj.popBack()
 */
```

#### JavaScript

```js
class FrontMiddleBackQueue {
    constructor() {
        this.q1 = new Deque();
        this.q2 = new Deque();
    }

    pushFront(val) {
        this.q1.pushFront(val);
        this.rebalance();
    }

    pushMiddle(val) {
        this.q1.pushBack(val);
        this.rebalance();
    }

    pushBack(val) {
        this.q2.pushBack(val);
        this.rebalance();
    }

    popFront() {
        if (this.q1.isEmpty() && this.q2.isEmpty()) {
            return -1;
        }
        const val = this.q1.isEmpty() ? this.q2.popFront() : this.q1.popFront();
        this.rebalance();
        return val !== undefined ? val : -1;
    }

    popMiddle() {
        if (this.q1.isEmpty() && this.q2.isEmpty()) {
            return -1;
        }
        const val =
            this.q1.getSize() === this.q2.getSize() ? this.q1.popBack() : this.q2.popFront();
        this.rebalance();
        return val !== undefined ? val : -1;
    }

    popBack() {
        if (this.q2.isEmpty()) {
            return -1;
        }
        const val = this.q2.popBack();
        this.rebalance();
        return val !== undefined ? val : -1;
    }

    rebalance() {
        if (this.q1.getSize() > this.q2.getSize()) {
            this.q2.pushFront(this.q1.popBack());
        }
        if (this.q2.getSize() > this.q1.getSize() + 1) {
            this.q1.pushBack(this.q2.popFront());
        }
    }
}

class Node {
    constructor(value) {
        this.value = value;
        this.next = null;
        this.prev = null;
    }
}

class Deque {
    constructor() {
        this.front = null;
        this.back = null;
        this.size = 0;
    }

    pushFront(val) {
        const newNode = new Node(val);
        if (this.isEmpty()) {
            this.front = newNode;
            this.back = newNode;
        } else {
            newNode.next = this.front;
            this.front.prev = newNode;
            this.front = newNode;
        }
        this.size++;
    }

    pushBack(val) {
        const newNode = new Node(val);
        if (this.isEmpty()) {
            this.front = newNode;
            this.back = newNode;
        } else {
            newNode.prev = this.back;
            this.back.next = newNode;
            this.back = newNode;
        }
        this.size++;
    }

    popFront() {
        if (this.isEmpty()) {
            return undefined;
        }
        const value = this.front.value;
        this.front = this.front.next;
        if (this.front !== null) {
            this.front.prev = null;
        } else {
            this.back = null;
        }
        this.size--;
        return value;
    }

    popBack() {
        if (this.isEmpty()) {
            return undefined;
        }
        const value = this.back.value;
        this.back = this.back.prev;
        if (this.back !== null) {
            this.back.next = null;
        } else {
            this.front = null;
        }
        this.size--;
        return value;
    }

    frontValue() {
        return this.front?.value;
    }

    backValue() {
        return this.back?.value;
    }

    getSize() {
        return this.size;
    }

    isEmpty() {
        return this.size === 0;
    }
}

/**
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * var obj = new FrontMiddleBackQueue()
 * obj.pushFront(val)
 * obj.pushMiddle(val)
 * obj.pushBack(val)
 * var param_4 = obj.popFront()
 * var param_5 = obj.popMiddle()
 * var param_6 = obj.popBack()
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
