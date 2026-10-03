---
comments: true
difficulty: Medium
rating: 1418
source: Weekly Contest 253 Q2
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1962. Remove Stones to Minimize the Total](https://leetcode.com/problems/remove-stones-to-minimize-the-total)

[中文文档](/solution/1900-1999/1962.Remove%20Stones%20to%20Minimize%20the%20Total/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>piles</code> được <strong>đánh chỉ số từ 0</strong>, trong đó <code>piles[i]</code> là số lượng viên đá trong đống thứ <code>i<sup>th</sup></code>, cùng với một số nguyên <code>k</code>. Bạn phải thực hiện phép toán sau <strong>đúng</strong> <code>k</code> lần:</p>

<ul>
	<li>Chọn một <code>piles[i]</code> bất kỳ và <strong>lấy đi</strong> <code>floor(piles[i] / 2)</code> viên đá khỏi đống đó.</li>
</ul>

<p><strong>Lưu ý</strong> rằng bạn có thể thực hiện phép toán trên <strong>cùng một</strong> đống nhiều lần.</p>

<p>Trả về <em><strong>tổng số viên đá nhỏ nhất</strong> có thể còn lại sau khi thực hiện </em><code>k</code><em> phép toán</em>.</p>

<p><code>floor(x)</code> là <strong>số nguyên lớn nhất</strong> <strong>nhỏ hơn</strong> hoặc <strong>bằng</strong> <code>x</code> (tức là làm tròn <code>x</code> xuống).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [5,4,9], k = 2
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>&nbsp;Các bước của một kịch bản có thể xảy ra:
- Thực hiện phép toán trên đống 2. Các đống sau đó là [5,4,<u>5</u>].
- Thực hiện phép toán trên đống 0. Các đống sau đó là [<u>3</u>,4,5].
Tổng số viên đá trong [3,4,5] là 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [4,3,6,7], k = 3
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>&nbsp;Các bước của một kịch bản có thể xảy ra:
- Thực hiện phép toán trên đống 2. Các đống sau đó là [4,3,<u>3</u>,7].
- Thực hiện phép toán trên đống 3. Các đống sau đó là [4,3,3,<u>4</u>].
- Thực hiện phép toán trên đống 0. Các đống sau đó là [<u>2</u>,3,3,4].
Tổng số viên đá trong [2,3,3,4] là 12.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= piles.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= piles[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phép toán chia đôi một đống, rồi làm tròn xuống. Để tối thiểu hóa số đá còn lại, ta luôn chọn đống lớn nhất hiện tại; một max-heap có thể duy trì đống này.
>
> Lưu các kích thước dưới dạng số đối, thay phần tử đầu bằng một nửa của nó $k$ lần, sau đó đổi dấu tổng các phần tử.

<!-- thinking:end -->

Theo mô tả bài toán, để tối thiểu hóa tổng số viên đá còn lại, ta cần lấy đi nhiều viên đá nhất có thể khỏi các đống. Vì vậy, ta luôn chọn đống có nhiều viên đá nhất để thực hiện phép toán.

Ta tạo một priority queue (max heap) $pq$ để lưu số viên đá trong mỗi đống. Ban đầu, ta thêm số viên đá của tất cả các đống vào priority queue.

Tiếp theo, ta thực hiện $k$ phép toán. Ở mỗi phép toán, ta lấy phần tử đầu $x$ của priority queue, chia đôi $x$, rồi thêm nó trở lại priority queue.

Sau khi thực hiện $k$ phép toán, tổng của tất cả các phần tử trong priority queue chính là đáp án.

Độ phức tạp thời gian là $O(n + k \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng `piles`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minStoneSum(self, piles: List[int], k: int) -> int:
        pq = [-x for x in piles]
        heapify(pq)
        for _ in range(k):
            heapreplace(pq, pq[0] // 2)
        return -sum(pq)
```

#### Java

```java
class Solution {
    public int minStoneSum(int[] piles, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        for (int x : piles) {
            pq.offer(x);
        }
        while (k-- > 0) {
            int x = pq.poll();
            pq.offer(x - x / 2);
        }
        int ans = 0;
        while (!pq.isEmpty()) {
            ans += pq.poll();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minStoneSum(vector<int>& piles, int k) {
        priority_queue<int> pq;
        for (int x : piles) {
            pq.push(x);
        }
        while (k--) {
            int x = pq.top();
            pq.pop();
            pq.push(x - x / 2);
        }
        int ans = 0;
        while (!pq.empty()) {
            ans += pq.top();
            pq.pop();
        }
        return ans;
    }
};
```

#### Go

```go
func minStoneSum(piles []int, k int) (ans int) {
	pq := &hp{piles}
	heap.Init(pq)
	for ; k > 0; k-- {
		x := pq.pop()
		pq.push(x - x/2)
	}
	for pq.Len() > 0 {
		ans += pq.pop()
	}
	return
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
function minStoneSum(piles: number[], k: number): number {
    const pq = new MaxPriorityQueue<number>();
    for (const x of piles) {
        pq.enqueue(x);
    }
    while (k--) {
        pq.enqueue((pq.dequeue() + 1) >> 1);
    }
    return pq.toArray().reduce((a, b) => a + b, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
