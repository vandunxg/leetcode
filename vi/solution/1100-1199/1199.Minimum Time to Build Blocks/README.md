---
comments: true
difficulty: Hard
rating: 2250
source: Biweekly Contest 9 Q4
tags:
    - Greedy
    - Array
    - Math
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1199. Minimum Time to Build Blocks 🔒](https://leetcode.com/problems/minimum-time-to-build-blocks)

[中文文档](/solution/1100-1199/1199.Minimum%20Time%20to%20Build%20Blocks/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách các block, trong đó <code>blocks[i] = t</code> nghĩa là block thứ <code>i</code> cần <code>t</code> đơn vị thời gian để hoàn thành. Mỗi block chỉ có thể do đúng một worker xây.</p>

<p>Một worker có thể tách thành hai worker (tổng số worker tăng thêm một) hoặc xây một block rồi nghỉ. Cả hai lựa chọn đều tốn thời gian.</p>

<p>Chi phí thời gian để tách một worker thành hai worker là số nguyên <code>split</code>. Lưu ý, nếu hai worker tách cùng lúc thì chúng tách song song, nên thời gian vẫn chỉ là <code>split</code>.</p>

<p>Hãy trả về thời gian tối thiểu cần để xây tất cả block.</p>

<p>Ban đầu chỉ có <strong>một</strong> worker.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> blocks = [1], split = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>Ta dùng một worker để xây một block trong một đơn vị thời gian.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> blocks = [1,2], split = 5
<strong>Đầu ra:</strong> 7
<strong>Giải thích: </strong>Ta tách worker thành hai worker trong 5 đơn vị thời gian, rồi giao mỗi worker xây một block; tổng thời gian là 5 + max(1, 2) = 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> blocks = [1,2,3], split = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Tách một worker thành hai, rồi giao worker thứ nhất xây block cuối và tách worker thứ hai thành hai.
Sau đó, dùng hai worker chưa được giao việc để xây hai block đầu tiên.
Tổng thời gian là 1 + max(3, 1 + max(1, 2)) = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= blocks.length &lt;= 1000</code></li>
	<li><code>1 &lt;= blocks[i] &lt;= 10^5</code></li>
	<li><code>1 &lt;= split &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Worker có thể tách với chi phí $split$, sau đó làm việc song song. Việc xét xuôi mọi phương án tách khá phức tạp. Thay vào đó, xét ngược: gộp hai block thành một block có thời gian $split+\max(t_i,t_j)$, tương ứng với một lần tách rồi xây song song. Mỗi lần gộp hai thời gian nhỏ nhất còn lại để các block tốn nhiều thời gian phải chịu ít lần tách hơn; lặp lại bằng min-heap cho đến khi chỉ còn một thời gian.

<!-- thinking:end -->

Trước tiên, xét trường hợp chỉ có một block. Khi đó không cần tách worker, chỉ cần để worker xây block trực tiếp. Thời gian cần là $block[0]$.

Nếu có hai block, cần tách worker thành hai rồi để mỗi worker xây một block riêng. Thời gian cần là $split + \max(block[0], block[1])$.

Nếu có hơn hai block, ở mỗi bước ta phải cân nhắc cần tách bao nhiêu worker. Cách suy nghĩ xuôi khiến việc này khó xử lý.

Ta có thể suy nghĩ ngược: thay vì tách worker, hãy gộp các block. Chọn hai block bất kỳ $i$, $j$ để gộp. Thời gian xây block mới là $split + \max(block[i], block[j])$.

Để block cần nhiều thời gian tham gia vào quá trình gộp ít lần nhất có thể, mỗi lần ta tham lam chọn hai block có thời gian nhỏ nhất để gộp. Vì vậy, duy trì min-heap, mỗi lần lấy ra hai block nhỏ nhất để gộp, cho đến khi chỉ còn một block. Thời gian xây block cuối cùng còn lại chính là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số block.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minBuildTime(self, blocks: List[int], split: int) -> int:
        heapify(blocks)
        while len(blocks) > 1:
            heappop(blocks)
            heappush(blocks, heappop(blocks) + split)
        return blocks[0]
```

#### Java

```java
class Solution {
    public int minBuildTime(int[] blocks, int split) {
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for (int x : blocks) {
            q.offer(x);
        }
        while (q.size() > 1) {
            q.poll();
            q.offer(q.poll() + split);
        }
        return q.poll();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minBuildTime(vector<int>& blocks, int split) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int v : blocks) pq.push(v);
        while (pq.size() > 1) {
            pq.pop();
            int x = pq.top();
            pq.pop();
            pq.push(x + split);
        }
        return pq.top();
    }
};
```

#### Go

```go
func minBuildTime(blocks []int, split int) int {
	q := hp{}
	for _, v := range blocks {
		heap.Push(&q, v)
	}
	for q.Len() > 1 {
		heap.Pop(&q)
		heap.Push(&q, heap.Pop(&q).(int)+split)
	}
	return q.IntSlice[0]
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

#### TypeScript

```ts
function minBuildTime(blocks: number[], split: number): number {
    const pq = new MinPriorityQueue<number>();
    for (const x of blocks) {
        pq.enqueue(x);
    }
    while (pq.size() > 1) {
        pq.dequeue();
        pq.enqueue(pq.dequeue() + split);
    }
    return pq.dequeue();
}
```

#### Rust

```rust
use std::cmp::Reverse;
use std::collections::BinaryHeap;

impl Solution {
    pub fn min_build_time(blocks: Vec<i32>, split: i32) -> i32 {
        let mut pq = BinaryHeap::new();

        for x in blocks {
            pq.push(Reverse(x));
        }

        while pq.len() > 1 {
            pq.pop();
            let new_element = pq.pop().unwrap().0 + split;
            pq.push(Reverse(new_element));
        }

        pq.pop().unwrap().0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
