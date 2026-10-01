---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [03.02. Min Stack](https://leetcode.cn/problems/min-stack-lcci)

[中文文档](/lcci/03.02.Min%20Stack/README.md)

## Mô tả

<!-- description:start -->

<p>Làm thế nào để thiết kế một stack mà ngoài push và pop còn có hàm min trả về phần tử nhỏ nhất? Các thao tác push, pop và min đều phải chạy trong thời gian 0(1).</p>

<p><strong>Ví dụ: </strong></p>

<pre>

MinStack minStack = new MinStack();

minStack.push(-2);

minStack.push(0);

minStack.push(-3);

minStack.getMin();   --&gt; return -3.

minStack.pop();

minStack.top();      --&gt; return 0.

minStack.getMin();   --&gt; return -2.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai stack

<!-- thinking:start -->

> **Tư duy**
>
> Quét toàn bộ stack để tìm `getMin` có độ phức tạp tuyến tính, không đáp ứng yêu cầu `push`/`pop` có thời gian $O(1)$.
>
> Giá trị nhỏ nhất của mỗi prefix đã được biết tại thời điểm push và được khôi phục khi pop, nên ta có thể dùng một stack thứ hai để lưu giá trị nhỏ nhất tích lũy song song với stack đầu tiên.
>
> `mins` bắt đầu với $+\infty$; mỗi `push` ghi lại $\min(mins[-1], val)$, còn `pop` loại bỏ phần tử trên cùng của cả hai stack. `getMin` đọc phần tử trên cùng của `mins` trong $O(1)$.

<!-- thinking:end -->

Chúng ta dùng hai stack để triển khai cấu trúc này, trong đó `stk1` dùng để lưu dữ liệu, còn `stk2` dùng để lưu giá trị nhỏ nhất hiện tại trong stack. Ban đầu, `stk2` lưu một giá trị rất lớn.

- Khi push phần tử `x` vào stack, ta push `x` vào `stk1`, đồng thời push `min(x, stk2[-1])` vào `stk2`.
- Khi pop một phần tử khỏi stack, ta pop phần tử trên cùng của cả `stk1` và `stk2`.
- Khi cần lấy phần tử trên cùng của stack hiện tại, ta chỉ cần trả về phần tử trên cùng của `stk1`.
- Khi cần lấy giá trị nhỏ nhất trong stack hiện tại, ta chỉ cần trả về phần tử trên cùng của `stk2`.

Độ phức tạp thời gian của mỗi thao tác là $O(1)$, còn độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class MinStack:
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.s = []
        self.mins = [inf]

    def push(self, val: int) -> None:
        self.s.append(val)
        self.mins.append(min(self.mins[-1], val))

    def pop(self) -> None:
        self.s.pop()
        self.mins.pop()

    def top(self) -> int:
        return self.s[-1]

    def getMin(self) -> int:
        return self.mins[-1]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

#### Java

```java
class MinStack {
    private Deque<Integer> stk1 = new ArrayDeque<>();
    private Deque<Integer> stk2 = new ArrayDeque<>();

    /** initialize your data structure here. */
    public MinStack() {
        stk2.push(Integer.MAX_VALUE);
    }

    public void push(int x) {
        stk1.push(x);
        stk2.push(Math.min(x, stk2.peek()));
    }

    public void pop() {
        stk1.pop();
        stk2.pop();
    }

    public int top() {
        return stk1.peek();
    }

    public int getMin() {
        return stk2.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

#### C++

```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
        stk2.push(INT_MAX);
    }

    void push(int x) {
        stk1.push(x);
        stk2.push(min(x, stk2.top()));
    }

    void pop() {
        stk1.pop();
        stk2.pop();
    }

    int top() {
        return stk1.top();
    }

    int getMin() {
        return stk2.top();
    }

private:
    stack<int> stk1;
    stack<int> stk2;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

#### Go

```go
type MinStack struct {
	stk1 []int
	stk2 []int
}

/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{[]int{}, []int{math.MaxInt32}}
}

func (this *MinStack) Push(x int) {
	this.stk1 = append(this.stk1, x)
	this.stk2 = append(this.stk2, min(x, this.stk2[len(this.stk2)-1]))
}

func (this *MinStack) Pop() {
	this.stk1 = this.stk1[:len(this.stk1)-1]
	this.stk2 = this.stk2[:len(this.stk2)-1]
}

func (this *MinStack) Top() int {
	return this.stk1[len(this.stk1)-1]
}

func (this *MinStack) GetMin() int {
	return this.stk2[len(this.stk2)-1]
}

/**
 * Your MinStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * obj.Pop();
 * param_3 := obj.Top();
 * param_4 := obj.GetMin();
 */
```

#### TypeScript

```ts
class MinStack {
    stack: number[];
    mins: number[];
    constructor() {
        this.stack = [];
        this.mins = [];
    }

    push(x: number): void {
        this.stack.push(x);
        this.mins.push(Math.min(this.getMin(), x));
    }

    pop(): void {
        this.stack.pop();
        this.mins.pop();
    }

    top(): number {
        return this.stack[this.stack.length - 1];
    }

    getMin(): number {
        return this.mins.length == 0 ? Infinity : this.mins[this.mins.length - 1];
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```

#### Rust

```rust
use std::collections::VecDeque;
struct MinStack {
    stack: VecDeque<i32>,
    min_stack: VecDeque<i32>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl MinStack {
    /** initialize your data structure here. */
    fn new() -> Self {
        Self {
            stack: VecDeque::new(),
            min_stack: VecDeque::new(),
        }
    }

    fn push(&mut self, x: i32) {
        self.stack.push_back(x);
        if self.min_stack.is_empty() || *self.min_stack.back().unwrap() >= x {
            self.min_stack.push_back(x);
        }
    }

    fn pop(&mut self) {
        let val = self.stack.pop_back().unwrap();
        if *self.min_stack.back().unwrap() == val {
            self.min_stack.pop_back();
        }
    }

    fn top(&self) -> i32 {
        *self.stack.back().unwrap()
    }

    fn get_min(&self) -> i32 {
        *self.min_stack.back().unwrap()
    }
}
```

#### C#

```cs
public class MinStack {
    private Stack<int> stk1 = new Stack<int>();
    private Stack<int> stk2 = new Stack<int>();

    /** initialize your data structure here. */
    public MinStack() {
        stk2.Push(int.MaxValue);
    }

    public void Push(int x) {
        stk1.Push(x);
        stk2.Push(Math.Min(x, GetMin()));
    }

    public void Pop() {
        stk1.Pop();
        stk2.Pop();
    }

    public int Top() {
        return stk1.Peek();
    }

    public int GetMin() {
        return stk2.Peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.Push(x);
 * obj.Pop();
 * int param_3 = obj.Top();
 * int param_4 = obj.GetMin();
 */
```

#### Swift

```swift
class MinStack {
    private var stk1: [Int]
    private var stk2: [Int]

    init() {
        stk1 = []
        stk2 = [Int.max]
    }

    func push(_ x: Int) {
        stk1.append(x)

        stk2.append(min(x, stk2.last!))
    }

    func pop() {
        stk1.removeLast()
        stk2.removeLast()
    }

    func top() -> Int {
        return stk1.last!
    }

    func getMin() -> Int {
        return stk2.last!
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * let obj = MinStack();
 * obj.push(x);
 * obj.pop();
 * let param_3 = obj.top();
 * let param_4 = obj.getMin();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
