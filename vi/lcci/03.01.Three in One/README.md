---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [03.01. Three in One](https://leetcode.cn/problems/three-in-one-lcci)

[Tài liệu tiếng Trung](/lcci/03.01.Three%20in%20One/README.md)

## Mô tả

<!-- description:start -->

<p>Mô tả cách sử dụng một mảng duy nhất để triển khai ba stack.</p>

<p>Bạn cần triển khai các phương thức&nbsp;<code>push(stackNum, value)</code>、<code>pop(stackNum)</code>、<code>isEmpty(stackNum)</code>、<code>peek(stackNum)</code>.&nbsp;<code>stackNum<font face="sans-serif, Arial, Verdana, Trebuchet MS">&nbsp;</font></code><font face="sans-serif, Arial, Verdana, Trebuchet MS">là chỉ số của stack.&nbsp;</font><code>value</code>&nbsp;là giá trị được push vào stack.</p>

<p>Constructor yêu cầu tham số&nbsp;<code>stackSize</code>, biểu thị kích thước của mỗi stack.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>:

[&quot;TripleInOne&quot;, &quot;push&quot;, &quot;push&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;isEmpty&quot;]

[[1], [0, 1], [0, 2], [0], [0], [0], [0]]

<strong> Đầu ra</strong>:

[null, null, null, 1, -1, -1, true]

<b>Giải thích</b>: Khi stack rỗng, `pop, peek` trả về -1. Khi stack đầy, `push` không thực hiện thao tác nào.

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>:

[&quot;TripleInOne&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;peek&quot;]

[[2], [0, 1], [0, 2], [0, 3], [0], [0], [0], [0]]

<strong> Đầu ra</strong>:

[null, null, null, null, 2, 1, -1, -1]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng mảng

<!-- thinking:start -->

> **Tư duy**
>
> Ba stack có dung lượng cố định phải dùng chung một vùng lưu trữ. Có thể dùng ba mảng riêng, nhưng bài toán yêu cầu một buffer liên tục.
>
> Chia buffer thành ba vùng bằng nhau và lưu kích thước của từng stack. Chỉ số $cap \times stackNum + size$ là vị trí trống tiếp theo.
>
> Mảng có độ dài $3cap+3$; ba ô cuối lưu kích thước. Mỗi thao tác chỉ truy cập một kích thước và một ô trong $O(1)$. `push` bị từ chối khi stack đầy; stack rỗng trả về $-1$.

<!-- thinking:end -->

Chúng ta sử dụng biến $cap$ để biểu thị kích thước của mỗi stack, đồng thời sử dụng mảng $stk$ có độ dài $3 \times \textit{cap} + 3$ để mô phỏng ba stack. $3 \times \textit{cap}$ phần tử đầu tiên của mảng được dùng để lưu các phần tử của stack, còn ba phần tử cuối được dùng để lưu số phần tử trong mỗi stack.

Với thao tác `push`, trước tiên chúng ta kiểm tra stack đã đầy hay chưa. Nếu stack chưa đầy, chúng ta push phần tử vào stack và cập nhật số phần tử trong stack.

Với thao tác `pop`, trước tiên chúng ta kiểm tra stack có rỗng hay không. Nếu stack không rỗng, chúng ta cập nhật số phần tử trong stack và trả về phần tử ở đỉnh stack.

Với thao tác `peek`, trước tiên chúng ta kiểm tra stack có rỗng hay không. Nếu stack không rỗng, chúng ta trả về phần tử ở đỉnh stack.

Với thao tác `isEmpty`, chúng ta trực tiếp kiểm tra stack có rỗng hay không. Với stack $i$, chỉ cần kiểm tra xem $stk[\textit{cap} \times 3 + i]$ có bằng $0$ hay không.

Về độ phức tạp thời gian, độ phức tạp thời gian của mỗi thao tác là $O(1)$. Độ phức tạp không gian là $O(\textit{cap})$, trong đó $\textit{cap}$ là kích thước của stack.

<!-- tabs:start -->

#### Python3

```python
class TripleInOne:

    def __init__(self, stackSize: int):
        self.cap = stackSize
        self.stk = [0] * (self.cap * 3 + 3)

    def push(self, stackNum: int, value: int) -> None:
        if self.stk[self.cap * 3 + stackNum] < self.cap:
            self.stk[self.cap * stackNum + self.stk[self.cap * 3 + stackNum]] = value
            self.stk[self.cap * 3 + stackNum] += 1

    def pop(self, stackNum: int) -> int:
        if self.isEmpty(stackNum):
            return -1
        self.stk[self.cap * 3 + stackNum] -= 1
        return self.stk[self.cap * stackNum + self.stk[self.cap * 3 + stackNum]]

    def peek(self, stackNum: int) -> int:
        if self.isEmpty(stackNum):
            return -1
        return self.stk[self.cap * stackNum + self.stk[self.cap * 3 + stackNum] - 1]

    def isEmpty(self, stackNum: int) -> bool:
        return self.stk[self.cap * 3 + stackNum] == 0


# Your TripleInOne object will be instantiated and called as such:
# obj = TripleInOne(stackSize)
# obj.push(stackNum,value)
# param_2 = obj.pop(stackNum)
# param_3 = obj.peek(stackNum)
# param_4 = obj.isEmpty(stackNum)
```

#### Java

```java
class TripleInOne {
    private int cap;
    private int[] stk;

    public TripleInOne(int stackSize) {
        cap = stackSize;
        stk = new int[cap * 3 + 3];
    }

    public void push(int stackNum, int value) {
        if (stk[cap * 3 + stackNum] < cap) {
            stk[cap * stackNum + stk[cap * 3 + stackNum]] = value;
            ++stk[cap * 3 + stackNum];
        }
    }

    public int pop(int stackNum) {
        if (isEmpty(stackNum)) {
            return -1;
        }
        --stk[cap * 3 + stackNum];
        return stk[cap * stackNum + stk[cap * 3 + stackNum]];
    }

    public int peek(int stackNum) {
        return isEmpty(stackNum) ? -1 : stk[cap * stackNum + stk[cap * 3 + stackNum] - 1];
    }

    public boolean isEmpty(int stackNum) {
        return stk[cap * 3 + stackNum] == 0;
    }
}

/**
 * Your TripleInOne object will be instantiated and called as such:
 * TripleInOne obj = new TripleInOne(stackSize);
 * obj.push(stackNum,value);
 * int param_2 = obj.pop(stackNum);
 * int param_3 = obj.peek(stackNum);
 * boolean param_4 = obj.isEmpty(stackNum);
 */
```

#### C++

```cpp
class TripleInOne {
public:
    TripleInOne(int stackSize) {
        cap = stackSize;
        stk.resize(cap * 3 + 3);
    }

    void push(int stackNum, int value) {
        if (stk[cap * 3 + stackNum] < cap) {
            stk[cap * stackNum + stk[cap * 3 + stackNum]] = value;
            ++stk[cap * 3 + stackNum];
        }
    }

    int pop(int stackNum) {
        if (isEmpty(stackNum)) {
            return -1;
        }
        --stk[cap * 3 + stackNum];
        return stk[cap * stackNum + stk[cap * 3 + stackNum]];
    }

    int peek(int stackNum) {
        return isEmpty(stackNum) ? -1 : stk[cap * stackNum + stk[cap * 3 + stackNum] - 1];
    }

    bool isEmpty(int stackNum) {
        return stk[cap * 3 + stackNum] == 0;
    }

private:
    int cap;
    vector<int> stk;
};

/**
 * Your TripleInOne object will be instantiated and called as such:
 * TripleInOne* obj = new TripleInOne(stackSize);
 * obj->push(stackNum,value);
 * int param_2 = obj->pop(stackNum);
 * int param_3 = obj->peek(stackNum);
 * bool param_4 = obj->isEmpty(stackNum);
 */
```

#### Go

```go
type TripleInOne struct {
	cap int
	stk []int
}

func Constructor(stackSize int) TripleInOne {
	return TripleInOne{stackSize, make([]int, stackSize*3+3)}
}

func (this *TripleInOne) Push(stackNum int, value int) {
	if this.stk[this.cap*3+stackNum] < this.cap {
		this.stk[this.cap*stackNum+this.stk[this.cap*3+stackNum]] = value
		this.stk[this.cap*3+stackNum]++
	}
}

func (this *TripleInOne) Pop(stackNum int) int {
	if this.IsEmpty(stackNum) {
		return -1
	}
	this.stk[this.cap*3+stackNum]--
	return this.stk[this.cap*stackNum+this.stk[this.cap*3+stackNum]]
}

func (this *TripleInOne) Peek(stackNum int) int {
	if this.IsEmpty(stackNum) {
		return -1
	}
	return this.stk[this.cap*stackNum+this.stk[this.cap*3+stackNum]-1]
}

func (this *TripleInOne) IsEmpty(stackNum int) bool {
	return this.stk[this.cap*3+stackNum] == 0
}

/**
 * Your TripleInOne object will be instantiated and called as such:
 * obj := Constructor(stackSize);
 * obj.Push(stackNum,value);
 * param_2 := obj.Pop(stackNum);
 * param_3 := obj.Peek(stackNum);
 * param_4 := obj.IsEmpty(stackNum);
 */
```

#### TypeScript

```ts
class TripleInOne {
    private cap: number;
    private stk: number[];

    constructor(stackSize: number) {
        this.cap = stackSize;
        this.stk = Array<number>(stackSize * 3 + 3).fill(0);
    }

    push(stackNum: number, value: number): void {
        if (this.stk[this.cap * 3 + stackNum] < this.cap) {
            this.stk[this.cap * stackNum + this.stk[this.cap * 3 + stackNum]] = value;
            this.stk[this.cap * 3 + stackNum]++;
        }
    }

    pop(stackNum: number): number {
        if (this.isEmpty(stackNum)) {
            return -1;
        }
        this.stk[this.cap * 3 + stackNum]--;
        return this.stk[this.cap * stackNum + this.stk[this.cap * 3 + stackNum]];
    }

    peek(stackNum: number): number {
        if (this.isEmpty(stackNum)) {
            return -1;
        }
        return this.stk[this.cap * stackNum + this.stk[this.cap * 3 + stackNum] - 1];
    }

    isEmpty(stackNum: number): boolean {
        return this.stk[this.cap * 3 + stackNum] === 0;
    }
}

/**
 * Your TripleInOne object will be instantiated and called as such:
 * var obj = new TripleInOne(stackSize)
 * obj.push(stackNum,value)
 * var param_2 = obj.pop(stackNum)
 * var param_3 = obj.peek(stackNum)
 * var param_4 = obj.isEmpty(stackNum)
 */
```

#### Swift

```swift
class TripleInOne {
    private var cap: Int
    private var stk: [Int]

    init(_ stackSize: Int) {
        self.cap = stackSize
        self.stk = [Int](repeating: 0, count: cap * 3 + 3)
    }

    func push(_ stackNum: Int, _ value: Int) {
        if stk[cap * 3 + stackNum] < cap {
            stk[cap * stackNum + stk[cap * 3 + stackNum]] = value
            stk[cap * 3 + stackNum] += 1
        }
    }

    func pop(_ stackNum: Int) -> Int {
        if isEmpty(stackNum) {
            return -1
        }
        stk[cap * 3 + stackNum] -= 1
        return stk[cap * stackNum + stk[cap * 3 + stackNum]]
    }

    func peek(_ stackNum: Int) -> Int {
        if isEmpty(stackNum) {
            return -1
        }
        return stk[cap * stackNum + stk[cap * 3 + stackNum] - 1]
    }

    func isEmpty(_ stackNum: Int) -> Bool {
        return stk[cap * 3 + stackNum] == 0
    }
}

/**
 * Your TripleInOne object will be instantiated and called as such:
 * let obj = TripleInOne(stackSize)
 * obj.push(stackNum,value)
 * let param_2 = obj.pop(stackNum)
 * let param_3 = obj.peek(stackNum)
 * let param_4 = obj.isEmpty(stackNum)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
