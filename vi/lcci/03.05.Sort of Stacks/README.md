---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [03.05. Sort of Stacks](https://leetcode.cn/problems/sort-of-stacks-lcci)

[Tài liệu tiếng Trung](/lcci/03.05.Sort%20of%20Stacks/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một chương trình sắp xếp một stack sao cho phần tử nhỏ nhất nằm ở trên cùng. Bạn có thể sử dụng thêm một stack tạm thời, nhưng không được sao chép các phần tử sang bất kỳ cấu trúc dữ liệu nào khác (chẳng hạn như mảng). Stack hỗ trợ các thao tác sau: <code>push</code>, <code>pop</code>, <code>peek</code> và <code>isEmpty</code>. Khi stack rỗng, <code>peek</code> phải trả về -1.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Input</strong>:

[&quot;SortedStack&quot;, &quot;push&quot;, &quot;push&quot;, &quot;peek&quot;, &quot;pop&quot;, &quot;peek&quot;]

[[], [1], [2], [], [], []]

<strong> Output</strong>:

[null,null,null,1,null,2]

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Input</strong>:

[&quot;SortedStack&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;push&quot;, &quot;pop&quot;, &quot;isEmpty&quot;]

[[], [], [], [1], [], []]

<strong> Output</strong>:

[null,null,null,null,null,true]

</pre>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li>Tổng số phần tử trong stack nằm trong khoảng [0, 5000].</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack + Stack phụ trợ

<!-- thinking:start -->

> **Tư duy**
>
> Phần tử trên cùng phải luôn là phần tử nhỏ nhất hiện tại. Việc chỉ sắp xếp khi truy vấn không phù hợp với API của stack; một stack thứ hai cho phép khôi phục thứ tự ngay khi chèn.
>
> Để đặt $val$ vào đúng vị trí, ta lần lượt pop mọi phần tử trên cùng nhỏ hơn nó (những phần tử cần nằm phía trên $val$) sang một nơi tạm, push $val$, rồi đưa các phần tử đó trở lại.
>
> Stack phụ trợ $t$ giữ các phần tử nhỏ hơn để $stk$ luôn không giảm từ trên xuống dưới. `pop`/`peek`/`isEmpty` chỉ cần thao tác với phần tử trên cùng.

<!-- thinking:end -->

Ta định nghĩa một stack $stk$ để lưu các phần tử.

Trong thao tác `push`, ta định nghĩa một stack phụ trợ $t$ để lưu các phần tử trong $stk$ nhỏ hơn phần tử hiện tại. Ta pop tất cả phần tử nhỏ hơn phần tử hiện tại khỏi $stk$ và lưu chúng vào $t$, sau đó push phần tử hiện tại vào $stk$, cuối cùng pop toàn bộ phần tử từ $t$ và push chúng trở lại $stk$. Độ phức tạp thời gian là $O(n)$.

Trong thao tác `pop`, ta chỉ cần kiểm tra xem $stk$ có rỗng hay không. Nếu không rỗng, ta pop phần tử trên cùng. Độ phức tạp thời gian là $O(1)$.

Trong thao tác `peek`, ta chỉ cần kiểm tra xem $stk$ có rỗng hay không. Nếu rỗng, ta trả về -1; ngược lại, ta trả về phần tử trên cùng. Độ phức tạp thời gian là $O(1)$.

Trong thao tác `isEmpty`, ta chỉ cần kiểm tra xem $stk$ có rỗng hay không. Độ phức tạp thời gian là $O(1)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số phần tử trong stack.

<!-- tabs:start -->

#### Python3

```python
class SortedStack:

    def __init__(self):
        self.stk = []

    def push(self, val: int) -> None:
        t = []
        while self.stk and self.stk[-1] < val:
            t.append(self.stk.pop())
        self.stk.append(val)
        while t:
            self.stk.append(t.pop())

    def pop(self) -> None:
        if not self.isEmpty():
            self.stk.pop()

    def peek(self) -> int:
        return -1 if self.isEmpty() else self.stk[-1]

    def isEmpty(self) -> bool:
        return not self.stk


# Your SortedStack object will be instantiated and called as such:
# obj = SortedStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.peek()
# param_4 = obj.isEmpty()
```

#### Java

```java
class SortedStack {
    private Deque<Integer> stk = new ArrayDeque<>();

    public SortedStack() {
    }

    public void push(int val) {
        Deque<Integer> t = new ArrayDeque<>();
        while (!stk.isEmpty() && stk.peek() < val) {
            t.push(stk.pop());
        }
        stk.push(val);
        while (!t.isEmpty()) {
            stk.push(t.pop());
        }
    }

    public void pop() {
        if (!isEmpty()) {
            stk.pop();
        }
    }

    public int peek() {
        return isEmpty() ? -1 : stk.peek();
    }

    public boolean isEmpty() {
        return stk.isEmpty();
    }
}

/**
 * Your SortedStack object will be instantiated and called as such:
 * SortedStack obj = new SortedStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.isEmpty();
 */
```

#### C++

```cpp
class SortedStack {
public:
    SortedStack() {
    }

    void push(int val) {
        stack<int> t;
        while (!stk.empty() && stk.top() < val) {
            t.push(stk.top());
            stk.pop();
        }
        stk.push(val);
        while (!t.empty()) {
            stk.push(t.top());
            t.pop();
        }
    }

    void pop() {
        if (!isEmpty()) {
            stk.pop();
        }
    }

    int peek() {
        return isEmpty() ? -1 : stk.top();
    }

    bool isEmpty() {
        return stk.empty();
    }

private:
    stack<int> stk;
};

/**
 * Your SortedStack object will be instantiated and called as such:
 * SortedStack* obj = new SortedStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->isEmpty();
 */
```

#### Go

```go
type SortedStack struct {
	stk []int
}

func Constructor() SortedStack {
	return SortedStack{}
}

func (this *SortedStack) Push(val int) {
	t := make([]int, 0)
	for len(this.stk) > 0 && this.stk[len(this.stk)-1] < val {
		t = append(t, this.stk[len(this.stk)-1])
		this.stk = this.stk[:len(this.stk)-1]
	}
	this.stk = append(this.stk, val)
	for i := len(t) - 1; i >= 0; i-- {
		this.stk = append(this.stk, t[i])
	}
}

func (this *SortedStack) Pop() {
	if !this.IsEmpty() {
		this.stk = this.stk[:len(this.stk)-1]
	}
}

func (this *SortedStack) Peek() int {
	if this.IsEmpty() {
		return -1
	}
	return this.stk[len(this.stk)-1]
}

func (this *SortedStack) IsEmpty() bool {
	return len(this.stk) == 0
}

/**
 * Your SortedStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(val);
 * obj.Pop();
 * param_3 := obj.Peek();
 * param_4 := obj.IsEmpty();
 */
```

#### TypeScript

```ts
class SortedStack {
    private stk: number[] = [];
    constructor() {}

    push(val: number): void {
        const t: number[] = [];
        while (this.stk.length > 0 && this.stk.at(-1)! < val) {
            t.push(this.stk.pop()!);
        }
        this.stk.push(val);
        while (t.length > 0) {
            this.stk.push(t.pop()!);
        }
    }

    pop(): void {
        if (!this.isEmpty()) {
            this.stk.pop();
        }
    }

    peek(): number {
        return this.isEmpty() ? -1 : this.stk.at(-1)!;
    }

    isEmpty(): boolean {
        return this.stk.length === 0;
    }
}

/**
 * Your SortedStack object will be instantiated and called as such:
 * var obj = new SortedStack()
 * obj.push(val)
 * obj.pop()
 * var param_3 = obj.peek()
 * var param_4 = obj.isEmpty()
 */
```

#### Rust

```rust
use std::collections::VecDeque;

struct SortedStack {
    stk: VecDeque<i32>,
}

impl SortedStack {
    fn new() -> Self {
        SortedStack {
            stk: VecDeque::new(),
        }
    }

    fn push(&mut self, val: i32) {
        let mut t = VecDeque::new();
        while let Some(top) = self.stk.pop_back() {
            if top < val {
                t.push_back(top);
            } else {
                self.stk.push_back(top);
                break;
            }
        }
        self.stk.push_back(val);
        while let Some(top) = t.pop_back() {
            self.stk.push_back(top);
        }
    }

    fn pop(&mut self) {
        if !self.is_empty() {
            self.stk.pop_back();
        }
    }

    fn peek(&self) -> i32 {
        if self.is_empty() {
            -1
        } else {
            *self.stk.back().unwrap()
        }
    }

    fn is_empty(&self) -> bool {
        self.stk.is_empty()
    }
}
```

#### Swift

```swift
class SortedStack {
    private var stk: [Int] = []

    init() {}

    func push(_ val: Int) {
        var temp: [Int] = []
        while let top = stk.last, top < val {
            temp.append(stk.removeLast())
        }
        stk.append(val)
        while let last = temp.popLast() {
            stk.append(last)
        }
    }

    func pop() {
        if !isEmpty() {
            stk.removeLast()
        }
    }

    func peek() -> Int {
        return isEmpty() ? -1 : stk.last!
    }

    func isEmpty() -> Bool {
        return stk.isEmpty
    }
}

/**
 * Your SortedStack object will be instantiated and called as such:
 * let obj = new SortedStack();
 * obj.push(val);
 * obj.pop();
 * let param_3 = obj.peek();
 * var myVar: Bool;
 * myVar = obj.isEmpty();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
