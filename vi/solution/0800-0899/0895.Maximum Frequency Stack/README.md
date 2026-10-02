---
comments: true
difficulty: Hard
tags:
    - Stack
    - Design
    - Hash Table
    - Ordered Set
---

<!-- problem:start -->

# [895. Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack)

[中文文档](/solution/0800-0899/0895.Maximum%20Frequency%20Stack/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một cấu trúc dữ liệu dạng stack, hỗ trợ push phần tử vào stack và pop phần tử xuất hiện nhiều nhất.</p>

<p>Hãy triển khai class <code>FreqStack</code>:</p>

<ul>
	<li><code>FreqStack()</code> khởi tạo một frequency stack rỗng.</li>
	<li><code>void push(int val)</code> thêm số nguyên <code>val</code> lên đầu stack.</li>
	<li><code>int pop()</code> xóa và trả về phần tử xuất hiện nhiều nhất trong stack.
	<ul>
		<li>Nếu có nhiều phần tử cùng có tần suất cao nhất, phần tử gần đầu stack nhất sẽ được xóa và trả về.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;FreqStack&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;pop&quot;]
[[], [5], [7], [5], [7], [4], [5], [], [], [], []]
<strong>Output</strong>
[null, null, null, null, null, null, null, 5, 7, 5, 4]

<strong>Giải thích</strong>
FreqStack freqStack = new FreqStack();
freqStack.push(5); // The stack is [5]
freqStack.push(7); // The stack is [5,7]
freqStack.push(5); // The stack is [5,7,5]
freqStack.push(7); // The stack is [5,7,5,7]
freqStack.push(4); // The stack is [5,7,5,7,4]
freqStack.push(5); // The stack is [5,7,5,7,4,5]
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,5,7,4].
freqStack.pop();   // return 7, as 5 and 7 is the most frequent, but 7 is closest to the top. The stack becomes [5,7,5,4].
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,4].
freqStack.pop();   // return 4, as 4, 5 and 7 is the most frequent, but 4 is closest to the top. The stack becomes [5,7].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= val &lt;= 10<sup>9</sup></code></li>
	<li>Sẽ có nhiều nhất <code>2 * 10<sup>4</sup></code> lần gọi <code>push</code> và <code>pop</code>.</li>
	<li>Đảm bảo stack có ít nhất một phần tử trước khi gọi <code>pop</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần pop, cần lấy giá trị đang có tần suất cao nhất; nếu hòa thì ưu tiên phần tử mới hơn. Có thể có đến $2\cdot 10^4$ thao tác nên duyệt stack sẽ quá chậm. Ta cần theo dõi cả tần suất lẫn timestamp.
>
> Một map lưu tần suất; heap lưu $(-\textit{freq},-\textit{ts},val)$. Sau khi pop, giảm tần suất của giá trị đó. Các bản ghi cũ trong heap được giữ lại, nhưng sẽ không được chọn khi có bản ghi mới hơn với tần suất cao hơn.

<!-- thinking:end -->

Theo đề bài, ta cần thiết kế cấu trúc dữ liệu hỗ trợ pop phần tử có tần suất cao nhất. Nếu nhiều phần tử có cùng tần suất, phần tử gần đầu stack nhất sẽ được pop.

Ta có thể dùng hash table $cnt$ để lưu tần suất của mỗi phần tử, và priority queue (max heap) $q$ để quản lý tần suất cùng timestamp tương ứng của các phần tử.

Khi thực hiện push, trước tiên ta tăng timestamp hiện tại, tức $ts \gets ts + 1$; sau đó tăng tần suất của phần tử $val$, tức $cnt[val] \gets cnt[val] + 1$; cuối cùng, thêm bộ ba $(cnt[val], ts, val)$ vào priority queue $q$. Độ phức tạp thời gian của thao tác push là $O(\log n)$.

Khi thực hiện pop, ta lấy trực tiếp một phần tử khỏi priority queue $q$. Các phần tử trong $q$ được sắp xếp theo tần suất giảm dần, nên phần tử được lấy ra chắc chắn có tần suất cao nhất. Nếu nhiều phần tử có cùng tần suất, phần tử gần đầu stack nhất sẽ được lấy ra, tức phần tử có timestamp lớn nhất. Sau đó, ta giảm tần suất của phần tử đó, tức $cnt[val] \gets cnt[val] - 1$. Độ phức tạp thời gian của thao tác pop là $O(\log n)$.

<!-- tabs:start -->

#### Python3

```python
class FreqStack:
    def __init__(self):
        self.cnt = defaultdict(int)
        self.q = []
        self.ts = 0

    def push(self, val: int) -> None:
        self.ts += 1
        self.cnt[val] += 1
        heappush(self.q, (-self.cnt[val], -self.ts, val))

    def pop(self) -> int:
        val = heappop(self.q)[2]
        self.cnt[val] -= 1
        return val


# Your FreqStack object will be instantiated and called as such:
# obj = FreqStack()
# obj.push(val)
# param_2 = obj.pop()
```

#### Java

```java
class FreqStack {
    private Map<Integer, Integer> cnt = new HashMap<>();
    private PriorityQueue<int[]> q
        = new PriorityQueue<>((a, b) -> a[0] == b[0] ? b[1] - a[1] : b[0] - a[0]);
    private int ts;

    public FreqStack() {
    }

    public void push(int val) {
        cnt.put(val, cnt.getOrDefault(val, 0) + 1);
        q.offer(new int[] {cnt.get(val), ++ts, val});
    }

    public int pop() {
        int val = q.poll()[2];
        cnt.put(val, cnt.get(val) - 1);
        return val;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(val);
 * int param_2 = obj.pop();
 */
```

#### C++

```cpp
class FreqStack {
public:
    FreqStack() {
    }

    void push(int val) {
        ++cnt[val];
        q.emplace(cnt[val], ++ts, val);
    }

    int pop() {
        auto [a, b, val] = q.top();
        q.pop();
        --cnt[val];
        return val;
    }

private:
    unordered_map<int, int> cnt;
    priority_queue<tuple<int, int, int>> q;
    int ts = 0;
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```

#### Go

```go
type FreqStack struct {
	cnt map[int]int
	q   hp
	ts  int
}

func Constructor() FreqStack {
	return FreqStack{map[int]int{}, hp{}, 0}
}

func (this *FreqStack) Push(val int) {
	this.cnt[val]++
	this.ts++
	heap.Push(&this.q, tuple{this.cnt[val], this.ts, val})
}

func (this *FreqStack) Pop() int {
	val := heap.Pop(&this.q).(tuple).val
	this.cnt[val]--
	return val
}

type tuple struct{ cnt, ts, val int }
type hp []tuple

func (h hp) Len() int { return len(h) }
func (h hp) Less(i, j int) bool {
	return h[i].cnt > h[j].cnt || h[i].cnt == h[j].cnt && h[i].ts > h[j].ts
}
func (h hp) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)   { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }

/**
 * Your FreqStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(val);
 * param_2 := obj.Pop();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Heap giữ lại các bản ghi tần suất đã cũ. Có thể gom nhóm theo tần suất: mức $f$ là một stack chứa các giá trị hiện có tần suất $f$.
>
> Push thêm giá trị vào mức $f+1$ và cập nhật tần suất lớn nhất; pop lấy phần tử trên cùng ở mức cao nhất, rồi giảm mức tối đa nếu stack tại mức đó đã rỗng. Mỗi thao tác có độ phức tạp amortized $O(1)$.

<!-- thinking:end -->

Ở Lời giải 1, để pop phần tử cần tìm, ta duy trì priority queue và phải thao tác trên đó mỗi lần, với độ phức tạp thời gian $O(\log n)$. Nếu tìm được phần tử cần lấy trong $O(1)$, độ phức tạp mỗi thao tác của toàn bộ cấu trúc dữ liệu có thể giảm xuống $O(1)$.

Ta có thể dùng biến $mx$ để lưu tần suất lớn nhất hiện tại, hash table $d$ để lưu danh sách phần tử ứng với từng tần suất, và như ở Lời giải 1, hash table $cnt$ để lưu tần suất của mỗi phần tử.

Khi thực hiện push, ta tăng tần suất của phần tử, tức $cnt[val] \gets cnt[val] + 1$, rồi thêm $val$ vào danh sách ứng với tần suất đó trong hash table $d$, tức $d[cnt[val]].push(val)$. Nếu tần suất của phần tử hiện tại lớn hơn $mx$, ta cập nhật $mx$, tức $mx \gets cnt[val]$. Độ phức tạp thời gian của thao tác push là $O(1)$.

Khi thực hiện pop, ta lấy danh sách các phần tử có tần suất $mx$ từ hash table $d$, pop phần tử cuối cùng $val$ trong danh sách rồi xóa phần tử đó khỏi danh sách, tức $d[mx].pop()$. Cuối cùng, ta giảm tần suất của $val$, tức $cnt[val] \gets cnt[val] - 1$. Nếu danh sách $d[mx]$ rỗng, nghĩa là mọi phần tử có tần suất lớn nhất hiện tại đã được pop; ta cần giảm $mx$, tức $mx \gets mx - 1$. Độ phức tạp thời gian của thao tác pop là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class FreqStack:
    def __init__(self):
        self.cnt = defaultdict(int)
        self.d = defaultdict(list)
        self.mx = 0

    def push(self, val: int) -> None:
        self.cnt[val] += 1
        self.d[self.cnt[val]].append(val)
        self.mx = max(self.mx, self.cnt[val])

    def pop(self) -> int:
        val = self.d[self.mx].pop()
        self.cnt[val] -= 1
        if not self.d[self.mx]:
            self.mx -= 1
        return val


# Your FreqStack object will be instantiated and called as such:
# obj = FreqStack()
# obj.push(val)
# param_2 = obj.pop()
```

#### Java

```java
class FreqStack {
    private Map<Integer, Integer> cnt = new HashMap<>();
    private Map<Integer, Deque<Integer>> d = new HashMap<>();
    private int mx;

    public FreqStack() {
    }

    public void push(int val) {
        cnt.put(val, cnt.getOrDefault(val, 0) + 1);
        int t = cnt.get(val);
        d.computeIfAbsent(t, k -> new ArrayDeque<>()).push(val);
        mx = Math.max(mx, t);
    }

    public int pop() {
        int val = d.get(mx).pop();
        cnt.put(val, cnt.get(val) - 1);
        if (d.get(mx).isEmpty()) {
            --mx;
        }
        return val;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(val);
 * int param_2 = obj.pop();
 */
```

#### C++

```cpp
class FreqStack {
public:
    FreqStack() {
    }

    void push(int val) {
        ++cnt[val];
        d[cnt[val]].push(val);
        mx = max(mx, cnt[val]);
    }

    int pop() {
        int val = d[mx].top();
        --cnt[val];
        d[mx].pop();
        if (d[mx].empty()) --mx;
        return val;
    }

private:
    unordered_map<int, int> cnt;
    unordered_map<int, stack<int>> d;
    int mx = 0;
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```

#### Go

```go
type FreqStack struct {
	cnt map[int]int
	d   map[int][]int
	mx  int
}

func Constructor() FreqStack {
	return FreqStack{map[int]int{}, map[int][]int{}, 0}
}

func (this *FreqStack) Push(val int) {
	this.cnt[val]++
	this.d[this.cnt[val]] = append(this.d[this.cnt[val]], val)
	this.mx = max(this.mx, this.cnt[val])
}

func (this *FreqStack) Pop() int {
	val := this.d[this.mx][len(this.d[this.mx])-1]
	this.d[this.mx] = this.d[this.mx][:len(this.d[this.mx])-1]
	this.cnt[val]--
	if len(this.d[this.mx]) == 0 {
		this.mx--
	}
	return val
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(val);
 * param_2 := obj.Pop();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
