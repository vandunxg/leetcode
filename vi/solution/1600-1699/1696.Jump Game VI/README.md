---
comments: true
difficulty: Medium
rating: 1954
source: Weekly Contest 220 Q3
tags:
    - Queue
    - Array
    - Dynamic Programming
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi)

[中文文档](/solution/1600-1699/1696.Jump%20Game%20VI/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong> và một số nguyên <code>k</code>.</p>

<p>Ban đầu bạn đứng tại chỉ số <code>0</code>. Trong một bước, bạn có thể nhảy tiến tối đa <code>k</code> bước mà không vượt ra ngoài biên của mảng. Nghĩa là, bạn có thể nhảy từ chỉ số <code>i</code> đến bất kỳ chỉ số nào trong đoạn <code>[i + 1, min(n - 1, i + k)]</code>, <strong>bao gồm cả hai đầu mút</strong>.</p>

<p>Bạn muốn đi đến chỉ số cuối cùng của mảng (chỉ số <code>n - 1</code>). <strong>Điểm số</strong> là <strong>tổng</strong> của mọi <code>nums[j]</code> với mỗi chỉ số <code>j</code> đã đi qua trong mảng.</p>

<p>Trả về <em><strong>điểm số lớn nhất</strong> có thể đạt được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<u>1</u>,<u>-1</u>,-2,<u>4</u>,-7,<u>3</u>], k = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Bạn có thể chọn các bước nhảy tạo thành dãy con [1,-1,4,3] (các phần tử được gạch chân ở trên). Tổng là 7.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<u>10</u>,-5,-2,<u>4</u>,0,<u>3</u>], k = 3
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> Bạn có thể chọn các bước nhảy tạo thành dãy con [10,4,3] (các phần tử được gạch chân ở trên). Tổng là 17.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, k &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + tối ưu bằng hàng đợi đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Từ $i$, ta có thể nhảy đến một vị trí trong $(i,i+k]$, cộng giá trị tại vị trí đáp xuống, và cần tối đa hóa điểm số cuối cùng. Công thức ngây thơ $f[i]=nums[i]+\max_{i-k \le j < i} f[j]$ có độ phức tạp $O(nk)$ với $n=10^5$.
>
> Một deque giảm dần lưu các chỉ số trong cửa sổ lớn nhất: $f[q[0]]$ là giá trị tiền nhiệm tốt nhất; loại bỏ ở cuối các giá trị $f$ kém hơn và loại bỏ ở đầu các chỉ số đã ra khỏi cửa sổ.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là điểm số lớn nhất khi đến chỉ số $i$. Giá trị $f[i]$ có thể được chuyển từ $f[j]$, trong đó $j$ thỏa mãn $i - k \leq j \leq i - 1$. Do đó, ta có thể dùng quy hoạch động để giải bài toán này.

Công thức chuyển trạng thái là:

$$
f[i] = \max_{j \in [i - k, i - 1]} f[j] + nums[i]
$$

Ta có thể sử dụng hàng đợi đơn điệu để tối ưu công thức chuyển trạng thái. Cụ thể, ta duy trì một hàng đợi giảm dần, lưu các chỉ số $j$, sao cho các giá trị $f[j]$ tương ứng cũng giảm dần. Khi chuyển trạng thái, ta chỉ cần lấy chỉ số $j$ ở đầu hàng đợi để có giá trị lớn nhất của $f[j]$, sau đó cập nhật $f[i]$ thành $f[j] + nums[i]$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxResult(self, nums: List[int], k: int) -> int:
        n = len(nums)
        f = [0] * n
        q = deque([0])
        for i in range(n):
            if i - q[0] > k:
                q.popleft()
            f[i] = nums[i] + f[q[0]]
            while q and f[q[-1]] <= f[i]:
                q.pop()
            q.append(i)
        return f[-1]
```

#### Java

```java
class Solution {
    public int maxResult(int[] nums, int k) {
        int n = nums.length;
        int[] f = new int[n];
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        for (int i = 0; i < n; ++i) {
            if (i - q.peekFirst() > k) {
                q.pollFirst();
            }
            f[i] = nums[i] + f[q.peekFirst()];
            while (!q.isEmpty() && f[q.peekLast()] <= f[i]) {
                q.pollLast();
            }
            q.offerLast(i);
        }
        return f[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        int f[n];
        f[0] = 0;
        deque<int> q = {0};
        for (int i = 0; i < n; ++i) {
            if (i - q.front() > k) {
                q.pop_front();
            }
            f[i] = nums[i] + f[q.front()];
            while (!q.empty() && f[i] >= f[q.back()]) {
                q.pop_back();
            }
            q.push_back(i);
        }
        return f[n - 1];
    }
};
```

#### Go

```go
func maxResult(nums []int, k int) int {
	n := len(nums)
	f := make([]int, n)
	q := Deque{}
	q.PushBack(0)
	for i := 0; i < n; i++ {
		if i-q.Front() > k {
			q.PopFront()
		}
		f[i] = nums[i] + f[q.Front()]
		for !q.Empty() && f[i] >= f[q.Back()] {
			q.PopBack()
		}
		q.PushBack(i)
	}
	return f[n-1]
}

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
function maxResult(nums: number[], k: number): number {
    const n = nums.length;
    const f: number[] = Array(n).fill(0);
    const q = new Deque<number>();
    q.pushBack(0);
    for (let i = 0; i < n; ++i) {
        if (i - q.frontValue()! > k) {
            q.popFront();
        }
        f[i] = nums[i] + f[q.frontValue()!];
        while (!q.isEmpty() && f[i] >= f[q.backValue()!]) {
            q.popBack();
        }
        q.pushBack(i);
    }
    return f[n - 1];
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
