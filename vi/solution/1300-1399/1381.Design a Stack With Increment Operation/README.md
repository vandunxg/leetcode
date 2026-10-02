---
comments: true
difficulty: Medium
rating: 1285
source: Weekly Contest 180 Q2
tags:
    - Stack
    - Design
    - Array
---

<!-- problem:start -->

# [1381. Design a Stack With Increment Operation](https://leetcode.com/problems/design-a-stack-with-increment-operation)

[中文文档](/solution/1300-1399/1381.Design%20a%20Stack%20With%20Increment%20Operation/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một stack hỗ trợ tăng giá trị các phần tử.</p>

<p>Hãy triển khai class <code>CustomStack</code>:</p>

<ul>
	<li><code>CustomStack(int maxSize)</code> Khởi tạo đối tượng với <code>maxSize</code>, là số phần tử tối đa trong stack.</li>
	<li><code>void push(int x)</code> Thêm <code>x</code> vào đỉnh stack nếu stack chưa đạt <code>maxSize</code>.</li>
	<li><code>int pop()</code> Lấy và trả về phần tử ở đỉnh stack, hoặc trả về <code>-1</code> nếu stack rỗng.</li>
	<li><code>void inc(int k, int val)</code> Tăng <code>k</code> phần tử dưới cùng của stack thêm <code>val</code>. Nếu stack có ít hơn <code>k</code> phần tử, tăng tất cả phần tử trong stack.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;CustomStack&quot;,&quot;push&quot;,&quot;push&quot;,&quot;pop&quot;,&quot;push&quot;,&quot;push&quot;,&quot;push&quot;,&quot;increment&quot;,&quot;increment&quot;,&quot;pop&quot;,&quot;pop&quot;,&quot;pop&quot;,&quot;pop&quot;]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
<strong>Đầu ra</strong>
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
<strong>Giải thích</strong>
CustomStack stk = new CustomStack(3); // Stack is Empty []
stk.push(1);                          // stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.pop();                            // return 2 --&gt; Return top of the stack 2, stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.push(3);                          // stack becomes [1, 2, 3]
stk.push(4);                          // stack still [1, 2, 3], Do not add another elements as size is 4
stk.increment(5, 100);                // stack becomes [101, 102, 103]
stk.increment(2, 100);                // stack becomes [201, 202, 103]
stk.pop();                            // return 103 --&gt; Return top of the stack 103, stack becomes [201, 202]
stk.pop();                            // return 202 --&gt; Return top of the stack 202, stack becomes [201]
stk.pop();                            // return 201 --&gt; Return top of the stack 201, stack becomes []
stk.pop();                            // return -1 --&gt; Stack is empty return -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= maxSize, x, k &lt;= 1000</code></li>
	<li><code>0 &lt;= val &lt;= 100</code></li>
	<li>Mỗi method <code>increment</code>, <code>push</code> và <code>pop</code> được gọi tối đa <code>1000</code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng mảng

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{increment}$ cộng $\textit{val}$ vào $k$ phần tử dưới cùng. Duyệt toàn bộ stack vẫn ổn với $10^3$ lần gọi, nhưng có thể đạt $O(1)$: $\textit{add}[i]$ lưu mức tăng cộng dồn theo cơ chế lazy cho chỉ số $i$ và các vị trí bên dưới. Ta ghi nhận cập nhật tại $\min(k,\textit{size})-1$; khi pop, chuyển mức tăng đó xuống một vị trí rồi xóa nó.

<!-- thinking:end -->

Ta dùng mảng $stk$ để mô phỏng stack và số nguyên $i$ để biểu diễn vị trí của phần tử tiếp theo sẽ được push vào stack. Ngoài ra, cần thêm mảng $add$ để lưu giá trị tăng cộng dồn tại mỗi vị trí.

Khi gọi $push(x)$, nếu $i < maxSize$, ta đặt $x$ vào $stk[i]$ rồi tăng $i$ lên một.

Khi gọi $pop()$, nếu $i \leq 0$, stack đang rỗng nên trả về $-1$. Nếu không, ta giảm $i$ đi một và đáp án là $stk[i] + add[i]$. Sau đó, cộng $add[i]$ vào $add[i - 1]$ rồi đặt $add[i]$ thành 0. Cuối cùng, trả về đáp án.

Khi gọi $increment(k, val)$, nếu $i > 0$, ta cộng $val$ vào $add[\min(i, k) - 1]$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là sức chứa tối đa của stack.

<!-- tabs:start -->

#### Python3

```python
class CustomStack:
    def __init__(self, maxSize: int):
        self.stk = [0] * maxSize
        self.add = [0] * maxSize
        self.i = 0

    def push(self, x: int) -> None:
        if self.i < len(self.stk):
            self.stk[self.i] = x
            self.i += 1

    def pop(self) -> int:
        if self.i <= 0:
            return -1
        self.i -= 1
        ans = self.stk[self.i] + self.add[self.i]
        if self.i > 0:
            self.add[self.i - 1] += self.add[self.i]
        self.add[self.i] = 0
        return ans

    def increment(self, k: int, val: int) -> None:
        i = min(k, self.i) - 1
        if i >= 0:
            self.add[i] += val


# Your CustomStack object will be instantiated and called as such:
# obj = CustomStack(maxSize)
# obj.push(x)
# param_2 = obj.pop()
# obj.increment(k,val)
```

#### Java

```java
class CustomStack {
    private int[] stk;
    private int[] add;
    private int i;

    public CustomStack(int maxSize) {
        stk = new int[maxSize];
        add = new int[maxSize];
    }

    public void push(int x) {
        if (i < stk.length) {
            stk[i++] = x;
        }
    }

    public int pop() {
        if (i <= 0) {
            return -1;
        }
        int ans = stk[--i] + add[i];
        if (i > 0) {
            add[i - 1] += add[i];
        }
        add[i] = 0;
        return ans;
    }

    public void increment(int k, int val) {
        if (i > 0) {
            add[Math.min(i, k) - 1] += val;
        }
    }
}

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack obj = new CustomStack(maxSize);
 * obj.push(x);
 * int param_2 = obj.pop();
 * obj.increment(k,val);
 */
```

#### C++

```cpp
class CustomStack {
public:
    CustomStack(int maxSize) {
        stk.resize(maxSize);
        add.resize(maxSize);
        i = 0;
    }

    void push(int x) {
        if (i < stk.size()) {
            stk[i++] = x;
        }
    }

    int pop() {
        if (i <= 0) {
            return -1;
        }
        int ans = stk[--i] + add[i];
        if (i > 0) {
            add[i - 1] += add[i];
        }
        add[i] = 0;
        return ans;
    }

    void increment(int k, int val) {
        if (i > 0) {
            add[min(k, i) - 1] += val;
        }
    }

private:
    vector<int> stk;
    vector<int> add;
    int i;
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack* obj = new CustomStack(maxSize);
 * obj->push(x);
 * int param_2 = obj->pop();
 * obj->increment(k,val);
 */
```

#### Go

```go
type CustomStack struct {
	stk []int
	add []int
	i   int
}

func Constructor(maxSize int) CustomStack {
	return CustomStack{make([]int, maxSize), make([]int, maxSize), 0}
}

func (this *CustomStack) Push(x int) {
	if this.i < len(this.stk) {
		this.stk[this.i] = x
		this.i++
	}
}

func (this *CustomStack) Pop() int {
	if this.i <= 0 {
		return -1
	}
	this.i--
	ans := this.stk[this.i] + this.add[this.i]
	if this.i > 0 {
		this.add[this.i-1] += this.add[this.i]
	}
	this.add[this.i] = 0
	return ans
}

func (this *CustomStack) Increment(k int, val int) {
	if this.i > 0 {
		this.add[min(k, this.i)-1] += val
	}
}

/**
 * Your CustomStack object will be instantiated and called as such:
 * obj := Constructor(maxSize);
 * obj.Push(x);
 * param_2 := obj.Pop();
 * obj.Increment(k,val);
 */
```

#### TypeScript

```ts
class CustomStack {
    private stk: number[];
    private add: number[];
    private i: number;

    constructor(maxSize: number) {
        this.stk = Array(maxSize).fill(0);
        this.add = Array(maxSize).fill(0);
        this.i = 0;
    }

    push(x: number): void {
        if (this.i < this.stk.length) {
            this.stk[this.i++] = x;
        }
    }

    pop(): number {
        if (this.i <= 0) {
            return -1;
        }
        const ans = this.stk[--this.i] + this.add[this.i];
        if (this.i > 0) {
            this.add[this.i - 1] += this.add[this.i];
        }
        this.add[this.i] = 0;
        return ans;
    }

    increment(k: number, val: number): void {
        if (this.i > 0) {
            this.add[Math.min(this.i, k) - 1] += val;
        }
    }
}

/**
 * Your CustomStack object will be instantiated and called as such:
 * var obj = new CustomStack(maxSize)
 * obj.push(x)
 * var param_2 = obj.pop()
 * obj.increment(k,val)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
