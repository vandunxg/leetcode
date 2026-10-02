---
comments: true
difficulty: Hard
rating: 2032
source: Weekly Contest 186 Q4
tags:
    - Queue
    - Array
    - Dynamic Programming
    - Sliding Window
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1425. Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum)

[中文文档](/solution/1400-1499/1425.Constrained%20Subsequence%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, hãy trả về tổng lớn nhất của một dãy con <strong>không rỗng</strong> của mảng sao cho với mọi hai số nguyên <strong>liên tiếp</strong> trong dãy con là <code>nums[i]</code> và <code>nums[j]</code>, trong đó <code>i &lt; j</code>, điều kiện <code>j - i &lt;= k</code> được thỏa mãn.</p>

<p>Một <em>dãy con</em> của một mảng được tạo thành bằng cách xóa đi một số phần tử (có thể bằng không) khỏi mảng, đồng thời giữ nguyên thứ tự ban đầu của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,2,-10,5,20], k = 2
<strong>Đầu ra:</strong> 37
<b>Giải thích:</b> Dãy con là [10, 2, 5, 20].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,-2,-3], k = 1
<strong>Đầu ra:</strong> -1
<b>Giải thích:</b> Dãy con phải không rỗng, vì vậy ta chọn số lớn nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,-2,-10,-5,20], k = 2
<strong>Đầu ra:</strong> 23
<b>Giải thích:</b> Dãy con là [10, -2, -5, 20].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming + Monotonic Queue

<!-- thinking:start -->

> **Tư duy**
>
> Hiệu giữa các chỉ số liền kề trong dãy con không vượt quá $k$. Vì $n\le 10^5$, việc duyệt $[i-k,i-1]$ với mọi $i$ sẽ có độ phức tạp $O(nk)$.
>
> $f[i]=\textit{nums}[i]+\max(0,\max_{i-k\le j<i}f[j])$ là bài toán tìm giá trị lớn nhất trong một sliding window trên $f$. Một deque giảm dần chứa các chỉ số giúp mỗi bước chuyển có độ phức tạp khấu hao $O(1)$.
>
> Các giá trị có thể âm, nên ta có thể bắt đầu một dãy con mới tại $i$. Đáp án là giá trị lớn nhất của $f[i]$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là tổng lớn nhất của dãy con kết thúc tại <code>nums[i]</code> và thỏa mãn các điều kiện. Ban đầu, $f[i] = 0$, và đáp án là $\max_{0 \leq i \lt n} f(i)$.

Ta nhận thấy bài toán yêu cầu duy trì giá trị lớn nhất của một sliding window, đây là một trường hợp ứng dụng điển hình của monotonic queue. Ta có thể sử dụng monotonic queue để tối ưu bước chuyển của dynamic programming.

Ta duy trì một monotonic queue $q$ giảm dần từ đầu đến cuối, lưu các chỉ số $i$. Ban đầu, ta thêm sentinel $0$ vào queue.

Ta duyệt $i$ từ $0$ đến $n - 1$. Với mỗi $i$, ta thực hiện các thao tác sau:

- Nếu phần tử đầu $q[0]$ thỏa mãn $i - q[0] > k$, điều đó có nghĩa là phần tử đầu không còn nằm trong sliding window, nên ta cần xóa phần tử đầu khỏi queue;
- Sau đó, ta tính $f[i] = \max(0, f[q[0]]) + \textit{nums}[i]$, nghĩa là cộng $\textit{nums}[i]$ vào tổng dãy con lớn nhất trong sliding window;
- Tiếp theo, ta cập nhật đáp án $\textit{ans} = \max(\textit{ans}, f[i])$;
- Cuối cùng, ta thêm $i$ vào cuối queue và duy trì tính đơn điệu của queue. Nếu $f[q[\textit{back}]] \leq f[i]$, ta xóa phần tử cuối cho đến khi queue rỗng hoặc $f[q[\textit{back}]] > f[i]$.

Đáp án cuối cùng là $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def constrainedSubsetSum(self, nums: List[int], k: int) -> int:
        q = deque([0])
        n = len(nums)
        f = [0] * n
        ans = -inf
        for i, x in enumerate(nums):
            while i - q[0] > k:
                q.popleft()
            f[i] = max(0, f[q[0]]) + x
            ans = max(ans, f[i])
            while q and f[q[-1]] <= f[i]:
                q.pop()
            q.append(i)
        return ans
```

#### Java

```java
class Solution {
    public int constrainedSubsetSum(int[] nums, int k) {
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        int n = nums.length;
        int[] f = new int[n];
        int ans = -(1 << 30);
        for (int i = 0; i < n; ++i) {
            while (i - q.peekFirst() > k) {
                q.pollFirst();
            }
            f[i] = Math.max(0, f[q.peekFirst()]) + nums[i];
            ans = Math.max(ans, f[i]);
            while (!q.isEmpty() && f[q.peekLast()] <= f[i]) {
                q.pollLast();
            }
            q.offerLast(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        deque<int> q = {0};
        int n = nums.size();
        int f[n];
        f[0] = 0;
        int ans = INT_MIN;
        for (int i = 0; i < n; ++i) {
            while (i - q.front() > k) {
                q.pop_front();
            }
            f[i] = max(0, f[q.front()]) + nums[i];
            ans = max(ans, f[i]);
            while (!q.empty() && f[q.back()] <= f[i]) {
                q.pop_back();
            }
            q.push_back(i);
        }
        return ans;
    }
};
```

#### Go

```go
func constrainedSubsetSum(nums []int, k int) int {
	q := Deque{}
	q.PushFront(0)
	n := len(nums)
	f := make([]int, n)
	ans := nums[0]
	for i, x := range nums {
		for i-q.Front() > k {
			q.PopFront()
		}
		f[i] = max(0, f[q.Front()]) + x
		ans = max(ans, f[i])
		for !q.Empty() && f[q.Back()] <= f[i] {
			q.PopBack()
		}
		q.PushBack(i)
	}
	return ans
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
function constrainedSubsetSum(nums: number[], k: number): number {
    const q = new Deque<number>();
    const n = nums.length;
    q.pushBack(0);
    let ans = nums[0];
    const f: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        while (i - q.frontValue()! > k) {
            q.popFront();
        }
        f[i] = Math.max(0, f[q.frontValue()!]!) + nums[i];
        ans = Math.max(ans, f[i]);
        while (!q.isEmpty() && f[q.backValue()!]! <= f[i]) {
            q.popBack();
        }
        q.pushBack(i);
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
