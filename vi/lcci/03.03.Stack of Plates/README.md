---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [03.03. Stack of Plates](https://leetcode.cn/problems/stack-of-plates-lcci)

[中文文档](/lcci/03.03.Stack%20of%20Plates/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy hình dung một chồng đĩa (theo nghĩa đen). Nếu chồng đĩa quá cao, nó có thể bị đổ. Vì vậy, trong thực tế, chúng ta có thể sẽ bắt đầu một chồng mới khi chồng trước đó vượt quá một ngưỡng nào đó. Hãy triển khai một cấu trúc dữ liệu <code>SetOfStacks</code> mô phỏng điều này.&nbsp;<code>SetOfStacks</code> phải được tạo thành từ nhiều stack và phải tạo một stack mới khi stack trước đó vượt quá capacity. <code>SetOfStacks.push()</code> và <code>SetOfStacks.pop()</code> phải hoạt động giống hệt một stack đơn (nghĩa là <code>pop()</code> phải trả về các giá trị giống như khi chỉ có một stack). Câu hỏi mở rộng: Hãy triển khai một hàm <code>popAt(int index)</code> thực hiện thao tác pop trên một stack con cụ thể.</p>
<p>Bạn phải xóa stack con khi nó trở nên rỗng. <code>pop</code>, <code>popAt</code> phải trả về -1 khi không có phần tử nào để lấy ra.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>:

[&quot;StackOfPlates&quot;, &quot;push&quot;, &quot;push&quot;, &quot;popAt&quot;, &quot;pop&quot;, &quot;pop&quot;]

[[1], [1], [2], [1], [], []]

<strong> Đầu ra</strong>:

[null, null, null, 2, 1, -1]

<strong> Giải thích</strong>:

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>:

[&quot;StackOfPlates&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;popAt&quot;, &quot;popAt&quot;, &quot;popAt&quot;]

[[2], [1], [2], [3], [0], [0], [0]]

<strong> Đầu ra</strong>:

[null, null, null, null, 2, 1, 3]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một stack đã đầy phải bắt đầu một stack đĩa mới, còn `popAt` nhắm tới một stack cụ thể. Tự tính chỉ số trên một mảng duy nhất rất dễ sai khi một stack ở giữa biến mất.
>
> Mô hình hóa cấu trúc này thành một danh sách các stack có giới hạn, trong đó $stk[-1]$ là stack hiện tại.
>
> `push` thêm một danh sách mới khi stack cuối cùng đầy; `pop` gọi `popAt` trên chỉ số cuối; `popAt` lấy phần tử khỏi stack đó và xóa stack nếu nó rỗng. $cap=0$ sẽ từ chối mọi lần push.

<!-- thinking:end -->

Chúng ta có thể dùng một danh sách các stack $stk$ để mô phỏng quá trình này, ban đầu $stk$ rỗng.

- Khi gọi phương thức `push`, nếu $cap$ bằng 0 thì trả về ngay. Nếu không, khi $stk$ rỗng hoặc độ dài của stack cuối cùng trong $stk$ lớn hơn hoặc bằng $cap$, hãy tạo một stack mới. Sau đó thêm phần tử $val$ vào stack cuối cùng trong $stk$. Độ phức tạp thời gian là $O(1)$.
- Khi gọi phương thức `pop`, trả về kết quả của `popAt(|stk| - 1)`. Độ phức tạp thời gian là $O(1)$.
- Khi gọi phương thức `popAt`, nếu $index$ không nằm trong khoảng $[0, |stk|)$ thì trả về -1. Nếu không, trả về phần tử trên cùng của $stk[index]$ và lấy nó ra. Nếu $stk[index]$ rỗng sau khi lấy phần tử, hãy xóa nó khỏi $stk$. Độ phức tạp thời gian là $O(1)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số phần tử.

<!-- tabs:start -->

#### Python3

```python
class StackOfPlates:
    def __init__(self, cap: int):
        self.cap = cap
        self.stk = []

    def push(self, val: int) -> None:
        if self.cap == 0:
            return
        if not self.stk or len(self.stk[-1]) >= self.cap:
            self.stk.append([])
        self.stk[-1].append(val)

    def pop(self) -> int:
        return self.popAt(len(self.stk) - 1)

    def popAt(self, index: int) -> int:
        ans = -1
        if 0 <= index < len(self.stk):
            ans = self.stk[index].pop()
            if not self.stk[index]:
                self.stk.pop(index)
        return ans


# Your StackOfPlates object will be instantiated and called as such:
# obj = StackOfPlates(cap)
# obj.push(val)
# param_2 = obj.pop()
# param_3 = obj.popAt(index)
```

#### Java

```java
class StackOfPlates {
    private List<Deque<Integer>> stk = new ArrayList<>();
    private int cap;

    public StackOfPlates(int cap) {
        this.cap = cap;
    }

    public void push(int val) {
        if (cap == 0) {
            return;
        }
        if (stk.isEmpty() || stk.get(stk.size() - 1).size() >= cap) {
            stk.add(new ArrayDeque<>());
        }
        stk.get(stk.size() - 1).push(val);
    }

    public int pop() {
        return popAt(stk.size() - 1);
    }

    public int popAt(int index) {
        int ans = -1;
        if (index >= 0 && index < stk.size()) {
            ans = stk.get(index).pop();
            if (stk.get(index).isEmpty()) {
                stk.remove(index);
            }
        }
        return ans;
    }
}

/**
 * Your StackOfPlates object will be instantiated and called as such:
 * StackOfPlates obj = new StackOfPlates(cap);
 * obj.push(val);
 * int param_2 = obj.pop();
 * int param_3 = obj.popAt(index);
 */
```

#### C++

```cpp
class StackOfPlates {
public:
    StackOfPlates(int cap) {
        this->cap = cap;
    }

    void push(int val) {
        if (!cap) {
            return;
        }
        if (stk.empty() || stk.back().size() >= cap) {
            stk.emplace_back(stack<int>());
        }
        stk.back().push(val);
    }

    int pop() {
        return popAt(stk.size() - 1);
    }

    int popAt(int index) {
        int ans = -1;
        if (index >= 0 && index < stk.size()) {
            ans = stk[index].top();
            stk[index].pop();
            if (stk[index].empty()) {
                stk.erase(stk.begin() + index);
            }
        }
        return ans;
    }

private:
    int cap;
    vector<stack<int>> stk;
};

/**
 * Your StackOfPlates object will be instantiated and called as such:
 * StackOfPlates* obj = new StackOfPlates(cap);
 * obj->push(val);
 * int param_2 = obj->pop();
 * int param_3 = obj->popAt(index);
 */
```

#### Go

```go
type StackOfPlates struct {
	stk [][]int
	cap int
}

func Constructor(cap int) StackOfPlates {
	return StackOfPlates{[][]int{}, cap}
}

func (this *StackOfPlates) Push(val int) {
	if this.cap == 0 {
		return
	}
	if len(this.stk) == 0 || len(this.stk[len(this.stk)-1]) >= this.cap {
		this.stk = append(this.stk, []int{})
	}
	this.stk[len(this.stk)-1] = append(this.stk[len(this.stk)-1], val)
}

func (this *StackOfPlates) Pop() int {
	return this.PopAt(len(this.stk) - 1)
}

func (this *StackOfPlates) PopAt(index int) int {
	ans := -1
	if index >= 0 && index < len(this.stk) {
		t := this.stk[index]
		ans = t[len(t)-1]
		this.stk[index] = this.stk[index][:len(t)-1]
		if len(this.stk[index]) == 0 {
			this.stk = append(this.stk[:index], this.stk[index+1:]...)
		}
	}
	return ans
}

/**
 * Your StackOfPlates object will be instantiated and called as such:
 * obj := Constructor(cap);
 * obj.Push(val);
 * param_2 := obj.Pop();
 * param_3 := obj.PopAt(index);
 */
```

#### TypeScript

```ts
class StackOfPlates {
    private cap: number;
    private stacks: number[][];
    constructor(cap: number) {
        this.cap = cap;
        this.stacks = [];
    }
    push(val: number): void {
        if (this.cap === 0) {
            return;
        }
        const n = this.stacks.length;
        const stack = this.stacks[n - 1];
        if (stack == null || stack.length === this.cap) {
            this.stacks.push([val]);
        } else {
            stack.push(val);
        }
    }
    pop(): number {
        const n = this.stacks.length;
        if (n === 0) {
            return -1;
        }
        const stack = this.stacks[n - 1];
        const res = stack.pop();
        if (stack.length === 0) {
            this.stacks.pop();
        }
        return res;
    }
    popAt(index: number): number {
        if (index >= this.stacks.length) {
            return -1;
        }
        const stack = this.stacks[index];
        const res = stack.pop();
        if (stack.length === 0) {
            this.stacks.splice(index, 1);
        }
        return res;
    }
}
/**
 * Your StackOfPlates object will be instantiated and called as such:
 * var obj = new StackOfPlates(cap)
 * obj.push(val)
 * var param_2 = obj.pop()
 * var param_3 = obj.popAt(index)
 */
```

#### Swift

```swift
class StackOfPlates {
    private var stacks: [[Int]]
    private var cap: Int

    init(_ cap: Int) {
        self.cap = cap
        self.stacks = []
    }

    func push(_ val: Int) {
        if cap == 0 {
            return
        }
        if stacks.isEmpty || stacks.last!.count >= cap {
            stacks.append([])
        }
        stacks[stacks.count - 1].append(val)
    }

    func pop() -> Int {
        return popAt(stacks.count - 1)
    }

    func popAt(_ index: Int) -> Int {
        guard index >= 0, index < stacks.count, !stacks[index].isEmpty else {
            return -1
        }
        let value = stacks[index].removeLast()
        if stacks[index].isEmpty {
            stacks.remove(at: index)
        }
        return value
    }
}

/**
 * Your StackOfPlates object will be instantiated and called as such:
 * let obj = new StackOfPlates(cap);
 * obj.push(val);
 * let param_2 = obj.pop();
 * let param_3 = obj.popAt(index);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
