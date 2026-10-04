---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Math
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3506. Find Time Required to Eliminate Bacterial Strains 🔒](https://leetcode.com/problems/find-time-required-to-eliminate-bacterial-strains)

[中文文档](/solution/3500-3599/3506.Find%20Time%20Required%20to%20Eliminate%20Bacterial%20Strains/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>timeReq</code> và một số nguyên <code>splitTime</code>.</p>

<p>Trong thế giới vi mô của cơ thể người, hệ miễn dịch phải đối mặt với một thử thách phi thường: chống lại một quần thể vi khuẩn sinh sôi nhanh chóng, đe dọa sự sống còn của cơ thể.</p>

<p>Ban đầu, chỉ có một <strong>bạch cầu</strong> (<strong>WBC</strong>) được triển khai để tiêu diệt vi khuẩn. Tuy nhiên, bạch cầu đơn độc nhanh chóng nhận ra rằng nó không thể theo kịp tốc độ phát triển của vi khuẩn.</p>

<p>Bạch cầu nghĩ ra một chiến lược thông minh để chống lại vi khuẩn:</p>

<ul>
    <li>Chủng vi khuẩn thứ <code>i<sup>th</sup></code> cần <code>timeReq[i]</code> đơn vị thời gian để bị tiêu diệt.</li>
    <li>Một bạch cầu chỉ có thể tiêu diệt <strong>một</strong> chủng vi khuẩn. Sau đó, bạch cầu sẽ cạn kiệt và không thể thực hiện nhiệm vụ nào khác.</li>
    <li>Một bạch cầu có thể tự tách thành hai bạch cầu, nhưng việc này cần <code>splitTime</code> đơn vị thời gian. Sau khi tách, hai bạch cầu có thể <strong>song song</strong> tiêu diệt vi khuẩn.</li>
    <li><em>Chỉ một</em> bạch cầu có thể làm việc trên một chủng vi khuẩn. Nhiều bạch cầu <strong>không thể</strong> cùng tấn công một chủng theo cách song song.</li>
</ul>

<p>Hãy xác định <strong>thời gian nhỏ nhất</strong> cần thiết để tiêu diệt tất cả các chủng vi khuẩn.</p>

<p><strong>Lưu ý</strong> rằng các chủng vi khuẩn có thể bị tiêu diệt theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">timeReq = [10,4,5], splitTime = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Quá trình tiêu diệt diễn ra như sau:</p>

<ul>
    <li>Ban đầu chỉ có một bạch cầu. Bạch cầu tách thành 2 bạch cầu sau 2 đơn vị thời gian.</li>
    <li>Một trong hai bạch cầu tiêu diệt chủng 0 tại thời điểm <code>t = 2 + 10 = 12.</code> Bạch cầu còn lại tiếp tục tách, mất thêm 2 đơn vị thời gian.</li>
    <li>2 bạch cầu mới tiêu diệt vi khuẩn tại các thời điểm <code>t = 2 + 2 + 4</code> và <code>t = 2 + 2 + 5</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">timeReq = [10,4], splitTime = 5</span></p>

<p><strong>Đầu ra:</strong>15</p>

<p><strong>Giải thích:</strong></p>

<p>Quá trình tiêu diệt diễn ra như sau:</p>

<ul>
    <li>Ban đầu chỉ có một bạch cầu. Bạch cầu tách thành 2 bạch cầu sau 5 đơn vị thời gian.</li>
    <li>2 bạch cầu mới tiêu diệt vi khuẩn tại các thời điểm <code>t = 5 + 10</code> và <code>t = 5 + 4</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= timeReq.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= timeReq[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= splitTime &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê thứ tự tách của các bạch cầu nhanh chóng trở nên phức tạp. Ta đảo ngược quá trình: gộp hai chủng với chi phí $\textit{splitTime} + \max(t_i, t_j)$.
>
> Các thời gian lớn nên tham gia càng ít lần gộp về sau càng tốt, vì vậy ta luôn gộp hai giá trị nhỏ nhất hiện tại — đúng theo cấu trúc của mã hóa Huffman. Min-heap cho phép lấy ra giá trị còn lại cuối cùng, chính là tổng thời gian.

<!-- thinking:end -->

Trước hết, xét trường hợp chỉ có một loại vi khuẩn. Khi đó không cần tách bạch cầu (WBC); nó có thể trực tiếp tiêu diệt vi khuẩn, và chi phí thời gian là $\textit{timeSeq}[0]$.

Nếu có hai loại vi khuẩn, WBC cần tách thành hai, mỗi WBC tiêu diệt một loại vi khuẩn. Chi phí thời gian là $\textit{splitTime} + \max(\textit{timeSeq}[0], \textit{timeSeq}[1])$.

Nếu có hơn hai loại vi khuẩn, ở mỗi bước ta cần xem xét việc tách các WBC thành nhiều tế bào, điều này khó xử lý bằng cách tiếp cận từ đầu đến cuối.

Thay vào đó, ta có thể áp dụng cách suy nghĩ ngược: thay vì tách các WBC, ta gộp các vi khuẩn lại. Ta chọn hai loại vi khuẩn bất kỳ $i$ và $j$ để gộp thành một loại vi khuẩn mới. Chi phí thời gian cho lần gộp này là $\textit{splitTime} + \max(\textit{timeSeq}[i], \textit{timeSeq}[j])$.

Để hạn chế việc các vi khuẩn có thời gian tiêu diệt dài tham gia vào quá trình gộp, ta có thể tham lam chọn hai vi khuẩn có thời gian tiêu diệt nhỏ nhất để gộp ở mỗi bước. Do đó, ta duy trì một min-heap, liên tục lấy ra hai vi khuẩn có thời gian tiêu diệt nhỏ nhất và gộp chúng cho đến khi chỉ còn lại một loại vi khuẩn. Thời gian tiêu diệt của loại vi khuẩn cuối cùng này là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng vi khuẩn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minEliminationTime(self, timeReq: List[int], splitTime: int) -> int:
        heapify(timeReq)
        while len(timeReq) > 1:
            heappop(timeReq)
            heappush(timeReq, heappop(timeReq) + splitTime)
        return timeReq[0]
```

#### Java

```java
class Solution {
    public long minEliminationTime(int[] timeReq, int splitTime) {
        PriorityQueue<Long> q = new PriorityQueue<>();
        for (int x : timeReq) {
            q.offer((long) x);
        }
        while (q.size() > 1) {
            q.poll();
            q.offer(q.poll() + splitTime);
        }
        return q.poll();
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minEliminationTime(vector<int>& timeReq, int splitTime) {
        using ll = long long;
        priority_queue<ll, vector<ll>, greater<ll>> pq;
        for (int v : timeReq) {
            pq.push(v);
        }
        while (pq.size() > 1) {
            pq.pop();
            ll x = pq.top();
            pq.pop();
            pq.push(x + splitTime);
        }
        return pq.top();
    }
};
```

#### Go

```go
func minEliminationTime(timeReq []int, splitTime int) int64 {
    pq := hp{}
    for _, v := range timeReq {
        heap.Push(&pq, v)
    }
    for pq.Len() > 1 {
        heap.Pop(&pq)
        heap.Push(&pq, heap.Pop(&pq).(int)+splitTime)
    }
    return int64(pq.IntSlice[0])
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
function minEliminationTime(timeReq: number[], splitTime: number): number {
    const pq = new MinPriorityQueue<number>();
    for (const b of timeReq) {
        pq.enqueue(b);
    }
    while (pq.size() > 1) {
        pq.dequeue();
        pq.enqueue(pq.dequeue() + splitTime);
    }
    return pq.dequeue();
}
```

#### Rust

```rust
use std::cmp::Reverse;
use std::collections::BinaryHeap;

impl Solution {
    pub fn min_elimination_time(time_req: Vec<i32>, split_time: i32) -> i64 {
        let mut pq = BinaryHeap::new();
        for x in time_req {
            pq.push(Reverse(x as i64));
        }
        while pq.len() > 1 {
            pq.pop();
            let merged = pq.pop().unwrap().0 + split_time as i64;
            pq.push(Reverse(merged));
        }
        pq.pop().unwrap().0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
