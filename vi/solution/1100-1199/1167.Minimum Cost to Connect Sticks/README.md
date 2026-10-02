---
comments: true
difficulty: Medium
rating: 1481
source: Biweekly Contest 7 Q3
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1167. Minimum Cost to Connect Sticks 🔒](https://leetcode.com/problems/minimum-cost-to-connect-sticks)

[中文文档](/solution/1100-1199/1167.Minimum%20Cost%20to%20Connect%20Sticks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một số thanh gỗ với độ dài là các số nguyên dương. Độ dài được cho trong mảng <code>sticks</code>, trong đó <code>sticks[i]</code> là độ dài của thanh thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn có thể nối hai thanh bất kỳ có độ dài <code>x</code> và <code>y</code> thành một thanh với chi phí <code>x + y</code>. Bạn phải nối tất cả các thanh cho đến khi chỉ còn một thanh.</p>

<p>Hãy trả về <em>chi phí nhỏ nhất để nối tất cả các thanh đã cho thành một thanh theo cách này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sticks = [2,4,3]
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong>&nbsp;Ban đầu, ta có sticks = [2,4,3].
1. Nối hai thanh 2 và 3 với chi phí 2 + 3 = 5. Khi đó sticks = [5,4].
2. Nối hai thanh 5 và 4 với chi phí 5 + 4 = 9. Khi đó sticks = [9].
Chỉ còn một thanh nên quá trình kết thúc. Tổng chi phí là 5 + 9 = 14.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sticks = [1,8,3,5]
<strong>Đầu ra:</strong> 30
<strong>Giải thích:</strong> Ban đầu, ta có sticks = [1,8,3,5].
1. Nối hai thanh 1 và 3 với chi phí 1 + 3 = 4. Khi đó sticks = [4,8,5].
2. Nối hai thanh 4 và 5 với chi phí 4 + 5 = 9. Khi đó sticks = [9,8].
3. Nối hai thanh 9 và 8 với chi phí 9 + 8 = 17. Khi đó sticks = [17].
Chỉ còn một thanh nên quá trình kết thúc. Tổng chi phí là 4 + 9 + 17 = 30.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sticks = [5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chỉ có một thanh nên không cần làm gì. Tổng chi phí là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code><span>1 &lt;= sticks.length &lt;= 10<sup>4</sup></span></code></li>
	<li><code><span>1 &lt;= sticks[i] &lt;= 10<sup>4</sup></span></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí nối hai thanh bằng tổng độ dài của chúng, và thanh mới tạo ra sẽ tiếp tục được nối, nên các thanh dài bị tính chi phí nhiều lần. Mỗi lần nối hai thanh ngắn nhất sẽ hạn chế việc các giá trị lớn xuất hiện lại nhiều lần. Min-heap lấy ra hai thanh, đưa tổng của chúng trở lại heap và cộng dồn chi phí cho đến khi còn một thanh.

<!-- thinking:end -->

Ta dùng greedy, mỗi lần chọn hai thanh ngắn nhất để nối nhằm đảm bảo tổng chi phí nhỏ nhất.

Vì vậy, ta dùng priority queue (min heap) để lưu độ dài các thanh hiện có. Mỗi lần, lấy hai thanh ra khỏi priority queue để nối, rồi đưa thanh mới vào lại queue, lặp lại cho đến khi chỉ còn một thanh.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `sticks`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def connectSticks(self, sticks: List[int]) -> int:
        heapify(sticks)
        ans = 0
        while len(sticks) > 1:
            z = heappop(sticks) + heappop(sticks)
            ans += z
            heappush(sticks, z)
        return ans
```

#### Java

```java
class Solution {
    public int connectSticks(int[] sticks) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int x : sticks) {
            pq.offer(x);
        }
        int ans = 0;
        while (pq.size() > 1) {
            int z = pq.poll() + pq.poll();
            ans += z;
            pq.offer(z);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int connectSticks(vector<int>& sticks) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (auto& x : sticks) {
            pq.push(x);
        }
        int ans = 0;
        while (pq.size() > 1) {
            int x = pq.top();
            pq.pop();
            int y = pq.top();
            pq.pop();
            int z = x + y;
            ans += z;
            pq.push(z);
        }
        return ans;
    }
};
```

#### Go

```go
func connectSticks(sticks []int) (ans int) {
	hp := &hp{sticks}
	heap.Init(hp)
	for hp.Len() > 1 {
		x, y := heap.Pop(hp).(int), heap.Pop(hp).(int)
		ans += x + y
		heap.Push(hp, x+y)
	}
	return
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

#### TypeScript

```ts
function connectSticks(sticks: number[]): number {
    const pq = new Heap(sticks);
    let ans = 0;
    while (pq.size() > 1) {
        const x = pq.pop();
        const y = pq.pop();
        ans += x + y;
        pq.push(x + y);
    }
    return ans;
}

type Compare<T> = (lhs: T, rhs: T) => number;

class Heap<T = number> {
    data: Array<T | null>;
    lt: (i: number, j: number) => boolean;
    constructor();
    constructor(data: T[]);
    constructor(compare: Compare<T>);
    constructor(data: T[], compare: Compare<T>);
    constructor(data: T[] | Compare<T>, compare?: (lhs: T, rhs: T) => number);
    constructor(
        data: T[] | Compare<T> = [],
        compare: Compare<T> = (lhs: T, rhs: T) => (lhs < rhs ? -1 : lhs > rhs ? 1 : 0),
    ) {
        if (typeof data === 'function') {
            compare = data;
            data = [];
        }
        this.data = [null, ...data];
        this.lt = (i, j) => compare(this.data[i]!, this.data[j]!) < 0;
        for (let i = this.size(); i > 0; i--) this.heapify(i);
    }

    size(): number {
        return this.data.length - 1;
    }

    push(v: T): void {
        this.data.push(v);
        let i = this.size();
        while (i >> 1 !== 0 && this.lt(i, i >> 1)) this.swap(i, (i >>= 1));
    }

    pop(): T {
        this.swap(1, this.size());
        const top = this.data.pop();
        this.heapify(1);
        return top!;
    }

    top(): T {
        return this.data[1]!;
    }
    heapify(i: number): void {
        while (true) {
            let min = i;
            const [l, r, n] = [i * 2, i * 2 + 1, this.data.length];
            if (l < n && this.lt(l, min)) min = l;
            if (r < n && this.lt(r, min)) min = r;
            if (min !== i) {
                this.swap(i, min);
                i = min;
            } else break;
        }
    }

    clear(): void {
        this.data = [null];
    }

    private swap(i: number, j: number): void {
        const d = this.data;
        [d[i], d[j]] = [d[j], d[i]];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
