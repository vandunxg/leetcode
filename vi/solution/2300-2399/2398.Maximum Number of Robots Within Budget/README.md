---
comments: true
difficulty: Hard
rating: 1917
source: Biweekly Contest 86 Q4
tags:
    - Queue
    - Array
    - Binary Search
    - Prefix Sum
    - Sliding Window
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2398. Maximum Number of Robots Within Budget](https://leetcode.com/problems/maximum-number-of-robots-within-budget)

[中文文档](/solution/2300-2399/2398.Maximum%20Number%20of%20Robots%20Within%20Budget/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> robot. Cho hai mảng số nguyên <strong>đánh chỉ số từ 0</strong> là <code>chargeTimes</code> và <code>runningCosts</code>, cả hai đều có độ dài <code>n</code>. Robot thứ <code>i<sup>th</sup></code> tốn <code>chargeTimes[i]</code> đơn vị chi phí để sạc và tốn <code>runningCosts[i]</code> đơn vị chi phí để chạy. Bạn cũng được cho một số nguyên <code>budget</code>.</p>

<p><strong>Tổng chi phí</strong> để chạy <code>k</code> robot được chọn bằng <code>max(chargeTimes) + k * sum(runningCosts)</code>, trong đó <code>max(chargeTimes)</code> là chi phí sạc lớn nhất trong số <code>k</code> robot và <code>sum(runningCosts)</code> là tổng chi phí chạy của <code>k</code> robot.</p>

<p>Trả về <em>số lượng <strong>lớn nhất</strong> các robot <strong>liên tiếp</strong> mà bạn có thể chạy sao cho tổng chi phí <strong>không</strong> vượt quá </em><code>budget</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> chargeTimes = [3,6,1,3,4], runningCosts = [2,1,3,4,5], budget = 25
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Có thể chạy từng robot riêng lẻ và mọi cặp robot liên tiếp mà không vượt quá ngân sách.
Để thu được đáp án 3, xét 3 robot đầu tiên. Tổng chi phí sẽ là max(3,6,1) + 3 * sum(2,1,3) = 6 + 3 * 6 = 24, nhỏ hơn 25.
Có thể chứng minh rằng không thể chạy nhiều hơn 3 robot liên tiếp mà không vượt quá ngân sách, nên ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> chargeTimes = [11,12,19], runningCosts = [10,8,7], budget = 19
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không robot nào có thể chạy mà không vượt quá ngân sách, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>chargeTimes.length == runningCosts.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= chargeTimes[i], runningCosts[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= budget &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Monotonic Queue

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí của một nhóm robot liên tiếp bằng thời gian sạc lớn nhất trong cửa sổ cộng với độ dài cửa sổ nhân tổng chi phí chạy. Vì $n \le 5 \times 10^4$; duyệt để tìm giá trị lớn nhất trong mọi cửa sổ sẽ quá chậm. Chi phí tăng theo độ dài, nên đầu trái chỉ di chuyển sang phải.
>
> Mở rộng đầu phải bằng một deque đơn điệu để tìm thời gian sạc lớn nhất và dùng một biến tổng $s$ cho chi phí chạy. Khi vượt ngân sách, loại các chỉ số đã ra khỏi cửa sổ và di chuyển đầu trái. Độ rộng lớn nhất chính là đáp án.

<!-- thinking:end -->

Bài toán về bản chất là tìm giá trị lớn nhất trong một cửa sổ trượt, có thể giải bằng monotonic queue.

Ta chỉ cần dùng binary search để liệt kê kích thước cửa sổ $k$ và tìm $k$ lớn nhất thỏa mãn yêu cầu của bài toán.

Trong quá trình triển khai, ta không cần thực sự dùng binary search để liệt kê. Chỉ cần chuyển cửa sổ cố định thành cửa sổ không cố định bằng hai con trỏ.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số robot trong bài toán.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumRobots(
        self, chargeTimes: List[int], runningCosts: List[int], budget: int
    ) -> int:
        q = deque()
        ans = s = l = 0
        for r, (t, c) in enumerate(zip(chargeTimes, runningCosts)):
            s += c
            while q and chargeTimes[q[-1]] <= t:
                q.pop()
            q.append(r)
            while q and (r - l + 1) * s + chargeTimes[q[0]] > budget:
                if q[0] == l:
                    q.popleft()
                s -= runningCosts[l]
                l += 1
            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int maximumRobots(int[] chargeTimes, int[] runningCosts, long budget) {
        Deque<Integer> q = new ArrayDeque<>();
        int n = chargeTimes.length;
        int ans = 0;
        long s = 0;
        for (int l = 0, r = 0; r < n; ++r) {
            s += runningCosts[r];
            while (!q.isEmpty() && chargeTimes[q.peekLast()] <= chargeTimes[r]) {
                q.pollLast();
            }
            q.offerLast(r);
            while (!q.isEmpty() && (r - l + 1) * s + chargeTimes[q.peekFirst()] > budget) {
                if (q.peekFirst() == l) {
                    q.pollFirst();
                }
                s -= runningCosts[l++];
            }
            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumRobots(vector<int>& chargeTimes, vector<int>& runningCosts, long long budget) {
        deque<int> q;
        long long s = 0;
        int ans = 0;
        int n = chargeTimes.size();
        for (int l = 0, r = 0; r < n; ++r) {
            s += runningCosts[r];
            while (q.size() && chargeTimes[q.back()] <= chargeTimes[r]) {
                q.pop_back();
            }
            q.push_back(r);
            while (q.size() && (r - l + 1) * s + chargeTimes[q.front()] > budget) {
                if (q.front() == l) {
                    q.pop_front();
                }
                s -= runningCosts[l++];
            }
            ans = max(ans, r - l + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumRobots(chargeTimes []int, runningCosts []int, budget int64) (ans int) {
	q := Deque{}
	s := int64(0)
	l := 0
	for r, t := range chargeTimes {
		s += int64(runningCosts[r])
		for !q.Empty() && chargeTimes[q.Back()] <= t {
			q.PopBack()
		}
		q.PushBack(r)
		for !q.Empty() && int64(r-l+1)*s+int64(chargeTimes[q.Front()]) > budget {
			if q.Front() == l {
				q.PopFront()
			}
			s -= int64(runningCosts[l])
			l++
		}
		ans = max(ans, r-l+1)
	}
	return
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
function maximumRobots(chargeTimes: number[], runningCosts: number[], budget: number): number {
    const q = new Deque<number>();
    const n = chargeTimes.length;
    let [ans, s] = [0, 0];
    for (let l = 0, r = 0; r < n; ++r) {
        s += runningCosts[r];
        while (!q.isEmpty() && chargeTimes[q.backValue()!] <= chargeTimes[r]) {
            q.popBack();
        }
        q.pushBack(r);
        while (!q.isEmpty() && (r - l + 1) * s + chargeTimes[q.frontValue()!] > budget) {
            if (q.frontValue() === l) {
                q.popFront();
            }
            s -= runningCosts[l++];
        }
        ans = Math.max(ans, r - l + 1);
    }
    return ans;
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
