---
comments: true
difficulty: Medium
rating: 1708
source: Biweekly Contest 118 Q3
tags:
    - Queue
    - Array
    - Dynamic Programming
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2944. Minimum Number of Coins for Fruits](https://leetcode.com/problems/minimum-number-of-coins-for-fruits)

[中文文档](/solution/2900-2999/2944.Minimum%20Number%20of%20Coins%20for%20Fruits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>prices</code> <strong>đánh chỉ số từ 0</strong>, trong đó <code>prices[i]</code> là số coin cần dùng để mua quả thứ <code>(i + 1)<sup>th</sup></code>.</p>

<p>Chợ trái cây có phần thưởng sau cho mỗi quả:</p>

<ul>
	<li>Nếu bạn mua quả thứ <code>(i + 1)<sup>th</sup></code> với <code>prices[i]</code> coin, bạn có thể nhận miễn phí bất kỳ số lượng nào trong <code>i</code> quả tiếp theo.</li>
</ul>

<p><strong>Lưu ý</strong> rằng ngay cả khi bạn <strong>có thể</strong> nhận miễn phí quả <code>j</code>, bạn vẫn có thể mua nó với <code>prices[j - 1]</code> coin để nhận phần thưởng của quả đó.</p>

<p>Hãy trả về <strong>số coin nhỏ nhất</strong> cần dùng để có được tất cả các quả.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mua quả thứ 1<sup>st</sup> với <code>prices[0] = 3</code> coin, bạn được phép nhận miễn phí quả thứ 2<sup>nd</sup>.</li>
	<li>Mua quả thứ 2<sup>nd</sup> với <code>prices[1] = 1</code> coin, bạn được phép nhận miễn phí quả thứ 3<sup>rd</sup>.</li>
	<li>Nhận miễn phí quả thứ 3<sup>rd</sup>.</li>
</ul>

<p>Lưu ý rằng dù có thể nhận miễn phí quả thứ 2<sup>nd</sup> như phần thưởng khi mua quả thứ 1<sup>st</sup>, việc mua quả thứ 2 để nhận phần thưởng của nó vẫn tối ưu hơn.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [1,10,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mua quả thứ 1<sup>st</sup> với <code>prices[0] = 1</code> coin, bạn được phép nhận miễn phí quả thứ 2<sup>nd</sup>.</li>
	<li>Nhận miễn phí quả thứ 2<sup>nd</sup>.</li>
	<li>Mua quả thứ 3<sup>rd</sup> với <code>prices[2] = 1</code> coin, bạn được phép nhận miễn phí quả thứ 4<sup>th</sup>.</li>
	<li>Nhận miễn phí quả thứ 4<sup>t</sup><sup>h</sup>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [26,18,6,12,49,7,45,45]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">39</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mua quả thứ 1<sup>st</sup> với <code>prices[0] = 26</code> coin, bạn được phép nhận miễn phí quả thứ 2<sup>nd</sup>.</li>
	<li>Nhận miễn phí quả thứ 2<sup>nd</sup>.</li>
	<li>Mua quả thứ 3<sup>rd</sup> với <code>prices[2] = 6</code> coin, bạn được phép nhận miễn phí các quả thứ 4<sup>th</sup>, 5<sup>th</sup> và 6<sup>th</sup> (ba quả tiếp theo).</li>
	<li>Nhận miễn phí quả thứ 4<sup>t</sup><sup>h</sup>.</li>
	<li>Nhận miễn phí quả thứ 5<sup>t</sup><sup>h</sup>.</li>
	<li>Mua quả thứ 6<sup>th</sup> với <code>prices[5] = 7</code> coin, bạn được phép nhận miễn phí quả thứ 8<sup>th</sup> và thứ 9<sup>th</sup>.</li>
	<li>Nhận miễn phí quả thứ 7<sup>t</sup><sup>h</sup>.</li>
	<li>Nhận miễn phí quả thứ 8<sup>t</sup><sup>h</sup>.</li>
</ul>

<p>Lưu ý rằng dù có thể nhận miễn phí quả thứ 6<sup>th</sup> như phần thưởng khi mua quả thứ 3<sup>rd</sup>, việc mua quả thứ 6 để nhận phần thưởng của nó vẫn tối ưu hơn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 1000</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mua quả có chỉ số bắt đầu từ 1 là $i$ sẽ mở khóa $i$ quả tiếp theo để nhận miễn phí. $n \le 1000$. Từ $i$, lần mua tiếp theo là một $j \in [i+1,2i+1]$.
>
> $dfs(i)$ lưu kết quả nhỏ nhất này; khi $2i \ge n$, việc mua quả $i$ sẽ bao phủ phần còn lại. Bắt đầu từ $1$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i)$ là số coin nhỏ nhất cần dùng để mua tất cả các quả bắt đầu từ quả thứ $i$. Đáp án là $\textit{dfs}(1)$.

Logic thực thi của hàm $\textit{dfs}(i)$ như sau:

- Nếu $i \times 2 \geq n$, điều đó có nghĩa là mua quả thứ $(i - 1)$ là đủ, các quả còn lại có thể nhận miễn phí, nên trả về $\textit{prices}[i - 1]$.
- Nếu không, ta có thể mua quả thứ $i$, sau đó chọn một quả $j$ để bắt đầu mua trong số các quả từ $i + 1$ đến $2i + 1$. Do đó, $\textit{dfs}(i) = \textit{prices}[i - 1] + \min_{i + 1 \le j \le 2i + 1} \textit{dfs}(j)$.

Để tránh tính toán lặp lại, ta dùng memoization để lưu các kết quả đã tính. Khi gặp lại cùng một trạng thái, ta trả về ngay kết quả tương ứng.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{prices}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCoins(self, prices: List[int]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i * 2 >= len(prices):
                return prices[i - 1]
            return prices[i - 1] + min(dfs(j) for j in range(i + 1, i * 2 + 2))

        return dfs(1)
```

#### Java

```java
class Solution {
    private int[] prices;
    private int[] f;
    private int n;

    public int minimumCoins(int[] prices) {
        n = prices.length;
        f = new int[n + 1];
        this.prices = prices;
        return dfs(1);
    }

    private int dfs(int i) {
        if (i * 2 >= n) {
            return prices[i - 1];
        }
        if (f[i] == 0) {
            f[i] = 1 << 30;
            for (int j = i + 1; j <= i * 2 + 1; ++j) {
                f[i] = Math.min(f[i], prices[i - 1] + dfs(j));
            }
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCoins(vector<int>& prices) {
        int n = prices.size();
        int f[n + 1];
        memset(f, 0x3f, sizeof(f));
        function<int(int)> dfs = [&](int i) {
            if (i * 2 >= n) {
                return prices[i - 1];
            }
            if (f[i] == 0x3f3f3f3f) {
                for (int j = i + 1; j <= i * 2 + 1; ++j) {
                    f[i] = min(f[i], prices[i - 1] + dfs(j));
                }
            }
            return f[i];
        };
        return dfs(1);
    }
};
```

#### Go

```go
func minimumCoins(prices []int) int {
	n := len(prices)
	f := make([]int, n+1)
	var dfs func(int) int
	dfs = func(i int) int {
		if i*2 >= n {
			return prices[i-1]
		}
		if f[i] == 0 {
			f[i] = 1 << 30
			for j := i + 1; j <= i*2+1; j++ {
				f[i] = min(f[i], dfs(j)+prices[i-1])
			}
		}
		return f[i]
	}
	return dfs(1)
}
```

#### TypeScript

```ts
function minimumCoins(prices: number[]): number {
    const n = prices.length;
    const f: number[] = Array(n + 1).fill(0);
    const dfs = (i: number): number => {
        if (i * 2 >= n) {
            return prices[i - 1];
        }
        if (f[i] === 0) {
            f[i] = 1 << 30;
            for (let j = i + 1; j <= i * 2 + 1; ++j) {
                f[i] = Math.min(f[i], prices[i - 1] + dfs(j));
            }
        }
        return f[i];
    };
    return dfs(1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization triển khai cùng công thức truy hồi theo hướng từ dưới lên: $f[i]$ vẫn là chi phí từ $i$ đến cuối, với $j \in [i+1,2i+1]$. Tính từ cuối về đầu giúp loại bỏ stack đệ quy và tương ứng với phương pháp 1.

<!-- thinking:end -->

Ta có thể chuyển bài toán tìm kiếm có nhớ trong Lời giải 1 thành dạng quy hoạch động.

Tương tự Lời giải 1, ta định nghĩa $f[i]$ là số coin nhỏ nhất cần dùng để mua tất cả các quả bắt đầu từ quả thứ $i$. Đáp án là $f[1]$.

Công thức chuyển trạng thái là $f[i] = \min_{i + 1 \le j \le 2i + 1} f[j] + \textit{prices}[i - 1]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{prices}$.

Trong phần cài đặt, ta có thể dùng trực tiếp mảng $\textit{prices}$ để lưu mảng $f$, nhờ đó tối ưu độ phức tạp không gian xuống còn $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCoins(self, prices: List[int]) -> int:
        n = len(prices)
        for i in range((n - 1) // 2, 0, -1):
            prices[i - 1] += min(prices[i : i * 2 + 1])
        return prices[0]
```

#### Java

```java
class Solution {
    public int minimumCoins(int[] prices) {
        int n = prices.length;
        for (int i = (n - 1) / 2; i > 0; --i) {
            int mi = 1 << 30;
            for (int j = i; j <= i * 2; ++j) {
                mi = Math.min(mi, prices[j]);
            }
            prices[i - 1] += mi;
        }
        return prices[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCoins(vector<int>& prices) {
        int n = prices.size();
        for (int i = (n - 1) / 2; i; --i) {
            prices[i - 1] += *min_element(prices.begin() + i, prices.begin() + 2 * i + 1);
        }
        return prices[0];
    }
};
```

#### Go

```go
func minimumCoins(prices []int) int {
	for i := (len(prices) - 1) / 2; i > 0; i-- {
		prices[i-1] += slices.Min(prices[i : i*2+1])
	}
	return prices[0]
}
```

#### TypeScript

```ts
function minimumCoins(prices: number[]): number {
    for (let i = (prices.length - 1) >> 1; i; --i) {
        prices[i - 1] += Math.min(...prices.slice(i, i * 2 + 1));
    }
    return prices[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động + Tối ưu hóa bằng hàng đợi đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 2 tốn $O(n)$ cho mỗi $i$ để tìm giá trị nhỏ nhất trong một đoạn của $f[j]$, tức là tổng cộng $O(n^2)$, mức này chấp nhận được khi $n=1000$. Khi $i$ giảm, cửa sổ $[i+1,2i+1]$ thu hẹp ở phía phải, nên ta có thể dùng hàng đợi đơn điệu để lưu các chỉ số ứng viên.
>
> Khi duyệt ngược, loại các phần tử đầu lớn hơn $2i+1$, cộng $prices[q[0]-1]$ vào $prices[i-1]$, rồi duy trì hàng đợi tăng dần theo giá trị của $prices$. Sau khi cập nhật tại chỗ, $prices[0]$ là đáp án.

<!-- thinking:end -->

Quan sát công thức chuyển trạng thái trong Lời giải 2, ta thấy với mỗi $i$, cần tìm giá trị nhỏ nhất trong $f[i + 1], f[i + 2], \cdots, f[2i + 1]$. Khi $i$ giảm, phạm vi các giá trị này cũng thu hẹp. Đây chính là bài toán tìm giá trị nhỏ nhất trong một sliding window có phạm vi thu hẹp, có thể tối ưu bằng hàng đợi đơn điệu.

Ta tính từ cuối về đầu, duy trì một hàng đợi tăng dần $q$, trong đó hàng đợi lưu các chỉ số. Nếu phần tử đầu của $q$ lớn hơn $i \times 2 + 1$, điều đó có nghĩa là các phần tử sau $i$ sẽ không được dùng, nên ta loại phần tử đầu khỏi hàng đợi. Nếu $i$ không lớn hơn $(n - 1) / 2$, ta cộng $\textit{prices}[q[0] - 1]$ vào $\textit{prices}[i - 1]$, sau đó thêm $i$ vào cuối hàng đợi. Nếu giá của quả tương ứng với phần tử cuối của $q$ lớn hơn hoặc bằng $\textit{prices}[i - 1]$, ta loại phần tử cuối cho đến khi giá của quả tương ứng với phần tử cuối nhỏ hơn $\textit{prices}[i - 1]$ hoặc hàng đợi rỗng, rồi thêm $i$ vào cuối hàng đợi.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{prices}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCoins(self, prices: List[int]) -> int:
        n = len(prices)
        q = deque()
        for i in range(n, 0, -1):
            while q and q[0] > i * 2 + 1:
                q.popleft()
            if i <= (n - 1) // 2:
                prices[i - 1] += prices[q[0] - 1]
            while q and prices[q[-1] - 1] >= prices[i - 1]:
                q.pop()
            q.append(i)
        return prices[0]
```

#### Java

```java
class Solution {
    public int minimumCoins(int[] prices) {
        int n = prices.length;
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = n; i > 0; --i) {
            while (!q.isEmpty() && q.peek() > i * 2 + 1) {
                q.poll();
            }
            if (i <= (n - 1) / 2) {
                prices[i - 1] += prices[q.peek() - 1];
            }
            while (!q.isEmpty() && prices[q.peekLast() - 1] >= prices[i - 1]) {
                q.pollLast();
            }
            q.offer(i);
        }
        return prices[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCoins(vector<int>& prices) {
        int n = prices.size();
        deque<int> q;
        for (int i = n; i; --i) {
            while (q.size() && q.front() > i * 2 + 1) {
                q.pop_front();
            }
            if (i <= (n - 1) / 2) {
                prices[i - 1] += prices[q.front() - 1];
            }
            while (q.size() && prices[q.back() - 1] >= prices[i - 1]) {
                q.pop_back();
            }
            q.push_back(i);
        }
        return prices[0];
    }
};
```

#### Go

```go
func minimumCoins(prices []int) int {
	n := len(prices)
	q := Deque{}
	for i := n; i > 0; i-- {
		for q.Size() > 0 && q.Front() > i*2+1 {
			q.PopFront()
		}
		if i <= (n-1)/2 {
			prices[i-1] += prices[q.Front()-1]
		}
		for q.Size() > 0 && prices[q.Back()-1] >= prices[i-1] {
			q.PopBack()
		}
		q.PushBack(i)
	}
	return prices[0]
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
```

#### TypeScript

```ts
function minimumCoins(prices: number[]): number {
    const n = prices.length;
    const q = new Deque<number>();
    for (let i = n; i; --i) {
        while (q.getSize() && q.frontValue()! > i * 2 + 1) {
            q.popFront();
        }
        if (i <= (n - 1) >> 1) {
            prices[i - 1] += prices[q.frontValue()! - 1];
        }
        while (q.getSize() && prices[q.backValue()! - 1] >= prices[i - 1]) {
            q.popBack();
        }
        q.pushBack(i);
    }
    return prices[0];
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
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
