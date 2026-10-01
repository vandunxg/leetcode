---
comments: true
difficulty: Easy
tags:
    - Design
    - Queue
    - Array
    - Data Stream
---

<!-- problem:start -->

# [346. Moving Average from Data Stream 🔒](https://leetcode.com/problems/moving-average-from-data-stream)

[中文文档](/solution/0300-0399/0346.Moving%20Average%20from%20Data%20Stream/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một luồng số nguyên và kích thước cửa sổ, hãy tính moving average của các số nguyên nằm trong cửa sổ trượt.</p>

<p>Hãy cài đặt class <code>MovingAverage</code>:</p>

<ul>
	<li><code>MovingAverage(int size)</code> Khởi tạo object với kích thước cửa sổ <code>size</code>.</li>
	<li><code>double next(int val)</code> Trả về moving average của <code>size</code> giá trị gần nhất trong luồng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MovingAverage&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;]
[[3], [1], [10], [3], [5]]
<strong>Đầu ra</strong>
[null, 1.0, 5.5, 4.66667, 6.0]

<strong>Giải thích</strong>
MovingAverage movingAverage = new MovingAverage(3);
movingAverage.next(1); // return 1.0 = 1 / 1
movingAverage.next(10); // return 5.5 = (1 + 10) / 2
movingAverage.next(3); // return 4.66667 = (1 + 10 + 3) / 3
movingAverage.next(5); // return 6.0 = (10 + 3 + 5) / 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= size &lt;= 1000</code></li>
	<li><code>-10<sup>5</sup> &lt;= val &lt;= 10<sup>5</sup></code></li>
	<li>Có tối đa <code>10<sup>4</sup></code> lần gọi <code>next</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng vòng

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính moving average trên cửa sổ có kích thước cố định. Tính lại tổng cửa sổ mỗi lần sẽ tốn $O(size)$. Duy trì tổng hiện tại để mỗi lần cập nhật chỉ mất $O(1)$.
>
> Circular buffer ghi đè tại chỉ số $cnt\bmod size$: trừ giá trị cũ rồi cộng giá trị mới. Moving average là $s/\min(cnt,size)$.

<!-- thinking:end -->

Ta dùng biến $\textit{s}$ để tính tổng của $\textit{size}$ phần tử gần nhất và biến $\textit{cnt}$ để lưu tổng số phần tử hiện có. Ngoài ra, dùng mảng $\textit{data}$ có độ dài $\textit{size}$ để lưu giá trị tại từng vị trí.

Khi gọi hàm $\textit{next}$, trước tiên ta tính chỉ số $i$ để lưu $\textit{val}$, sau đó cập nhật tổng $s$, gán giá trị tại chỉ số $i$ thành $\textit{val}$ và tăng số lượng phần tử lên một. Cuối cùng, trả về $\frac{s}{\min(\textit{cnt}, \textit{size})}$.

Độ phức tạp thời gian là $O(1)$, độ phức tạp không gian là $O(n)$, trong đó $n$ chính là số nguyên $\textit{size}$ đã cho.

<!-- tabs:start -->

#### Python3

```python
class MovingAverage:

    def __init__(self, size: int):
        self.s = 0
        self.data = [0] * size
        self.cnt = 0

    def next(self, val: int) -> float:
        i = self.cnt % len(self.data)
        self.s += val - self.data[i]
        self.data[i] = val
        self.cnt += 1
        return self.s / min(self.cnt, len(self.data))


# Your MovingAverage object will be instantiated and called as such:
# obj = MovingAverage(size)
# param_1 = obj.next(val)
```

#### Java

```java
class MovingAverage {
    private int s;
    private int cnt;
    private int[] data;

    public MovingAverage(int size) {
        data = new int[size];
    }

    public double next(int val) {
        int i = cnt % data.length;
        s += val - data[i];
        data[i] = val;
        ++cnt;
        return s * 1.0 / Math.min(cnt, data.length);
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```

#### C++

```cpp
class MovingAverage {
public:
    MovingAverage(int size) {
        data.resize(size);
    }

    double next(int val) {
        int i = cnt % data.size();
        s += val - data[i];
        data[i] = val;
        ++cnt;
        return s * 1.0 / min(cnt, (int) data.size());
    }

private:
    int s = 0;
    int cnt = 0;
    vector<int> data;
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage* obj = new MovingAverage(size);
 * double param_1 = obj->next(val);
 */
```

#### Go

```go
type MovingAverage struct {
	s    int
	cnt  int
	data []int
}

func Constructor(size int) MovingAverage {
	return MovingAverage{
		data: make([]int, size),
	}
}

func (this *MovingAverage) Next(val int) float64 {
	i := this.cnt % len(this.data)
	this.s += val - this.data[i]
	this.data[i] = val
	this.cnt++
	return float64(this.s) / float64(min(this.cnt, len(this.data)))
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * obj := Constructor(size);
 * param_1 := obj.Next(val);
 */
```

#### TypeScript

```ts
class MovingAverage {
    private s: number = 0;
    private cnt: number = 0;
    private data: number[];

    constructor(size: number) {
        this.data = Array(size).fill(0);
    }

    next(val: number): number {
        const i = this.cnt % this.data.length;
        this.s += val - this.data[i];
        this.data[i] = val;
        this.cnt++;
        return this.s / Math.min(this.cnt, this.data.length);
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * var obj = new MovingAverage(size)
 * var param_1 = obj.next(val)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Queue

<!-- thinking:start -->

> **Tư duy**
>
> Rất dễ nhầm khi dùng chỉ số vòng tròn. Có thể dùng queue để lưu cửa sổ: khi queue đầy, pop phần tử đầu và trừ khỏi tổng, sau đó thêm phần tử mới vào cuối. Cách này cho kết quả tương tự nhưng code rõ ràng hơn.

<!-- thinking:end -->

Ta dùng queue $\textit{q}$ để lưu $\textit{size}$ phần tử gần nhất và biến $\textit{s}$ để lưu tổng các phần tử này.

Khi gọi hàm $\textit{next}$, trước tiên kiểm tra độ dài queue $\textit{q}$ có bằng $\textit{size}$ hay không. Nếu bằng, dequeue phần tử đầu của $\textit{q}$ rồi cập nhật $\textit{s}$. Sau đó enqueue $\textit{val}$ và cập nhật $\textit{s}$. Cuối cùng, trả về $\frac{s}{\text{len}(q)}$.

Độ phức tạp thời gian là $O(1)$, độ phức tạp không gian là $O(n)$, trong đó $n$ chính là số nguyên $\textit{size}$ đã cho.

<!-- tabs:start -->

#### Python3

```python
class MovingAverage:
    def __init__(self, size: int):
        self.n = size
        self.s = 0
        self.q = deque()

    def next(self, val: int) -> float:
        if len(self.q) == self.n:
            self.s -= self.q.popleft()
        self.q.append(val)
        self.s += val
        return self.s / len(self.q)


# Your MovingAverage object will be instantiated and called as such:
# obj = MovingAverage(size)
# param_1 = obj.next(val)
```

#### Java

```java
class MovingAverage {
    private Deque<Integer> q = new ArrayDeque<>();
    private int n;
    private int s;

    public MovingAverage(int size) {
        n = size;
    }

    public double next(int val) {
        if (q.size() == n) {
            s -= q.pollFirst();
        }
        q.offer(val);
        s += val;
        return s * 1.0 / q.size();
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```

#### C++

```cpp
class MovingAverage {
public:
    MovingAverage(int size) {
        n = size;
    }

    double next(int val) {
        if (q.size() == n) {
            s -= q.front();
            q.pop();
        }
        q.push(val);
        s += val;
        return (double) s / q.size();
    }

private:
    queue<int> q;
    int s = 0;
    int n;
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage* obj = new MovingAverage(size);
 * double param_1 = obj->next(val);
 */
```

#### Go

```go
type MovingAverage struct {
	q []int
	s int
	n int
}

func Constructor(size int) MovingAverage {
	return MovingAverage{n: size}
}

func (this *MovingAverage) Next(val int) float64 {
	if len(this.q) == this.n {
		this.s -= this.q[0]
		this.q = this.q[1:]
	}
	this.q = append(this.q, val)
	this.s += val
	return float64(this.s) / float64(len(this.q))
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * obj := Constructor(size);
 * param_1 := obj.Next(val);
 */
```

#### TypeScript

```ts
class MovingAverage {
    private q: number[] = [];
    private s: number = 0;
    private n: number;

    constructor(size: number) {
        this.n = size;
    }

    next(val: number): number {
        if (this.q.length === this.n) {
            this.s -= this.q.shift()!;
        }
        this.q.push(val);
        this.s += val;
        return this.s / this.q.length;
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * var obj = new MovingAverage(size)
 * var param_1 = obj.next(val)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
