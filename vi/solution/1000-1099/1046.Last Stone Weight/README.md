---
comments: true
difficulty: Easy
rating: 1172
source: Weekly Contest 137 Q1
tags:
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight)

[中文文档](/solution/1000-1099/1046.Last%20Stone%20Weight/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>stones</code>, trong đó <code>stones[i]</code> là khối lượng của viên đá thứ <code>i</code>.</p>

<p>Ta chơi một trò chơi với các viên đá. Mỗi lượt, ta chọn <strong>hai viên đá nặng nhất</strong> và đập chúng vào nhau. Giả sử hai viên đó có khối lượng <code>x</code> và <code>y</code>, với <code>x &lt;= y</code>. Kết quả sau khi đập là:</p>

<ul>
	<li>Nếu <code>x == y</code>, cả hai viên đá đều bị phá hủy; còn</li>
	<li>Nếu <code>x != y</code>, viên đá có khối lượng <code>x</code> bị phá hủy, còn viên đá có khối lượng <code>y</code> có khối lượng mới là <code>y - x</code>.</li>
</ul>

<p>Khi trò chơi kết thúc, còn lại <strong>nhiều nhất một</strong> viên đá.</p>

<p>Hãy trả về <em>khối lượng của viên đá còn lại cuối cùng</em>. Nếu không còn viên nào, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [2,7,4,1,8,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 
Ta đập 7 và 8, được 1, nên mảng trở thành [2,4,1,1,1]; tiếp theo,
ta đập 2 và 4, được 2, nên mảng trở thành [2,1,1,1]; tiếp theo,
ta đập 2 và 1, được 1, nên mảng trở thành [1,1,1]; tiếp theo,
ta đập 1 và 1, được 0, nên mảng trở thành [1]. Đây là khối lượng của viên đá cuối cùng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [1]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stones.length &lt;= 30</code></li>
	<li><code>1 &lt;= stones[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt đập dùng hai viên đá nặng nhất. Tìm phần tử lớn nhất bằng cách quét lại mỗi lần sẽ lặp nhiều phép so sánh; heap lấy các phần tử đó trong thời gian logarit.
>
> Đổi dấu khối lượng sẽ biến min-heap thành max-heap. Pop $y\ge x$; nếu hai giá trị khác nhau, push $y-x$. Lặp lại đến khi còn ít hơn hai viên đá.
>
> Nếu heap rỗng, đáp án là $0$; nếu không, đáp án là giá trị đối của phần tử trên đỉnh.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        h = [-x for x in stones]
        heapify(h)
        while len(h) > 1:
            y, x = -heappop(h), -heappop(h)
            if x != y:
                heappush(h, x - y)
        return 0 if not h else -h[0]
```

#### Java

```java
class Solution {
    public int lastStoneWeight(int[] stones) {
        PriorityQueue<Integer> q = new PriorityQueue<>((a, b) -> b - a);
        for (int x : stones) {
            q.offer(x);
        }
        while (q.size() > 1) {
            int y = q.poll();
            int x = q.poll();
            if (x != y) {
                q.offer(y - x);
            }
        }
        return q.isEmpty() ? 0 : q.poll();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        priority_queue<int> pq;
        for (int x : stones) {
            pq.push(x);
        }
        while (pq.size() > 1) {
            int y = pq.top();
            pq.pop();
            int x = pq.top();
            pq.pop();
            if (x != y) {
                pq.push(y - x);
            }
        }
        return pq.empty() ? 0 : pq.top();
    }
};
```

#### Go

```go
func lastStoneWeight(stones []int) int {
	q := &hp{stones}
	heap.Init(q)
	for q.Len() > 1 {
		y, x := q.pop(), q.pop()
		if x != y {
			q.push(y - x)
		}
	}
	if q.Len() > 0 {
		return q.IntSlice[0]
	}
	return 0
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
func (h *hp) push(v int) { heap.Push(h, v) }
func (h *hp) pop() int   { return heap.Pop(h).(int) }
```

#### TypeScript

```ts
function lastStoneWeight(stones: number[]): number {
    const pq = new MaxPriorityQueue<number>();
    for (const x of stones) {
        pq.enqueue(x);
    }
    while (pq.size() > 1) {
        const y = pq.dequeue();
        const x = pq.dequeue();
        if (x !== y) {
            pq.enqueue(y - x);
        }
    }
    return pq.isEmpty() ? 0 : pq.dequeue();
}
```

#### JavaScript

```js
/**
 * @param {number[]} stones
 * @return {number}
 */
var lastStoneWeight = function (stones) {
    const pq = new MaxPriorityQueue();
    for (const x of stones) {
        pq.enqueue(x);
    }
    while (pq.size() > 1) {
        const y = pq.dequeue();
        const x = pq.dequeue();
        if (x != y) {
            pq.enqueue(y - x);
        }
    }
    return pq.isEmpty() ? 0 : pq.dequeue();
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
