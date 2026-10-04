---
comments: true
difficulty: Hard
tags:
    - Queue
    - Array
    - Dynamic Programming
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2969. Minimum Number of Coins for Fruits II 🔒](https://leetcode.com/problems/minimum-number-of-coins-for-fruits-ii)

[中文文档](/solution/2900-2999/2969.Minimum%20Number%20of%20Coins%20for%20Fruits%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang ở một chợ trái cây có bày bán nhiều loại trái cây ngoại lai khác nhau.</p>

<p>Bạn được cho một mảng <code>prices</code> <strong>đánh chỉ số từ 1</strong>, trong đó <code>prices[i]</code> là số coin cần dùng để mua quả thứ <code>i<sup>th</sup></code>.</p>

<p>Chợ trái cây có ưu đãi sau:</p>

<ul>
	<li>Nếu bạn mua quả thứ <code>i<sup>th</sup></code> với <code>prices[i]</code> coin, bạn có thể nhận miễn phí <code>i</code> quả tiếp theo.</li>
</ul>

<p><strong>Lưu ý</strong> rằng ngay cả khi bạn <strong>có thể</strong> nhận miễn phí quả <code>j</code>, bạn vẫn có thể mua nó với <code>prices[j]</code> coin để nhận một ưu đãi mới.</p>

<p>Hãy trả về <em><strong>số coin nhỏ nhất</strong> cần dùng để có được tất cả các quả</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [3,1,2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Bạn có thể nhận được các quả như sau:
- Mua quả thứ 1<sup>st</sup> với 3 coin, và bạn được phép nhận miễn phí quả thứ 2<sup>nd</sup>.
- Mua quả thứ 2<sup>nd</sup> với 1 coin, và bạn được phép nhận miễn phí quả thứ 3<sup>rd</sup>.
- Nhận miễn phí quả thứ 3<sup>rd</sup>.
Lưu ý rằng dù được phép nhận miễn phí quả thứ 2<sup>nd</sup>, việc mua nó vẫn tối ưu hơn.
Có thể chứng minh rằng 4 là số coin nhỏ nhất cần dùng để có được tất cả các quả.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,10,1,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bạn có thể nhận được các quả như sau:
- Mua quả thứ 1<sup>st</sup> với 1 coin, và bạn được phép nhận miễn phí quả thứ 2<sup>nd</sup>.
- Nhận quả thứ 2<sup>nd</sup> miễn phí.
- Mua quả thứ 3<sup>rd</sup> với 1 coin, và bạn được phép nhận miễn phí quả thứ 4<sup>th</sup>.
- Nhận quả thứ 4<sup>t</sup><sup>h</sup> miễn phí.
Có thể chứng minh rằng 2 là số coin nhỏ nhất cần dùng để có được tất cả các quả.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Công thức truy hồi giống với bài “Fruits I”: $f[i]=prices[i-1]+\min_{i+1 \le j \le 2i+1} f[j]$, nhưng $n$ là $10^5$, nên vòng lặp đôi sẽ không thể đáp ứng. Khi $i$ giảm, đầu phải của cửa sổ cũng giảm, và hàng đợi đơn điệu có thể lấy giá trị nhỏ nhất trong thời gian trung bình $O(1)$.
>
> Duyệt từ cuối về đầu, loại các chỉ số lớn hơn $2i+1$, cộng phần tử đầu vào $prices[i-1]$, rồi duy trì hàng đợi tăng dần theo chi phí. Sau khi cập nhật tại chỗ, $prices[0]$ là đáp án.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số coin nhỏ nhất cần dùng để mua tất cả các quả bắt đầu từ quả thứ $i$. Vì vậy, đáp án là $f[1]$.

Công thức chuyển trạng thái là $f[i] = \min_{i + 1 \le j \le 2i + 1} f[j] + prices[i - 1]$.

Trong phần cài đặt, ta tính từ cuối về đầu và có thể trực tiếp thực hiện chuyển trạng thái trên mảng $prices$, nhờ đó tiết kiệm không gian.

Độ phức tạp thời gian của phương pháp trên là $O(n^2)$. Vì $n$ trong bài toán này có thể lên tới $10^5$, phương pháp sẽ bị quá thời gian.

Quan sát công thức chuyển trạng thái, ta thấy với mỗi $i$, cần tìm giá trị nhỏ nhất của $f[i + 1], f[i + 2], \cdots, f[2i + 1]$, và khi $i$ giảm, phạm vi các giá trị này cũng thu hẹp. Đây chính là bài toán tìm giá trị nhỏ nhất trong một sliding window có phạm vi thu hẹp, có thể tối ưu bằng hàng đợi đơn điệu.

Ta tính từ cuối về đầu, duy trì một hàng đợi tăng dần $q$, trong đó hàng đợi lưu các chỉ số. Nếu phần tử đầu của $q$ lớn hơn $i \times 2 + 1$, điều đó có nghĩa là các phần tử sau $i$ sẽ không được dùng, nên ta loại phần tử đầu khỏi hàng đợi. Nếu $i$ không lớn hơn $(n - 1) / 2$, ta cộng $prices[q[0] - 1]$ vào $prices[i - 1]$, sau đó thêm $i$ vào cuối hàng đợi. Nếu giá của quả tương ứng với phần tử cuối của $q$ lớn hơn hoặc bằng $prices[i - 1]$, ta loại phần tử cuối cho đến khi giá của quả tương ứng với phần tử cuối nhỏ hơn $prices[i - 1]$ hoặc hàng đợi rỗng, rồi thêm $i$ vào cuối hàng đợi.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $prices$.

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
