---
comments: true
difficulty: Medium
rating: 1713
source: Weekly Contest 310 Q3
tags:
    - Greedy
    - Array
    - Two Pointers
    - Prefix Sum
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2406. Divide Intervals Into Minimum Number of Groups](https://leetcode.com/problems/divide-intervals-into-minimum-number-of-groups)

[中文文档](/solution/2400-2499/2406.Divide%20Intervals%20Into%20Minimum%20Number%20of%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>intervals</code>, trong đó <code>intervals[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> biểu diễn đoạn <strong>bao gồm cả hai đầu mút</strong> <code>[left<sub>i</sub>, right<sub>i</sub>]</code>.</p>

<p>Bạn phải chia các đoạn thành một hoặc nhiều <strong>nhóm</strong> sao cho mỗi đoạn thuộc <strong>chính xác</strong> một nhóm, và không có hai đoạn nào trong cùng một nhóm <strong>giao nhau</strong>.</p>

<p>Trả về <em>số lượng nhóm <strong>nhỏ nhất</strong> cần tạo</em>.</p>

<p>Hai đoạn <strong>giao nhau</strong> nếu chúng có ít nhất một số chung. Ví dụ, các đoạn <code>[1, 5]</code> và <code>[5, 8]</code> giao nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[5,10],[6,8],[1,5],[2,3],[1,10]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể chia các đoạn thành những nhóm sau:
- Nhóm 1: [1, 5], [6, 8].
- Nhóm 2: [2, 3], [5, 10].
- Nhóm 3: [1, 10].
Có thể chứng minh rằng không thể chia các đoạn thành ít hơn 3 nhóm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,3],[5,6],[8,10],[11,13]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Không có đoạn nào chồng lấn, nên ta có thể đặt tất cả chúng vào cùng một nhóm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 10<sup>5</sup></code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>1 &lt;= left<sub>i</sub> &lt;= right<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt qua các nhóm hiện có với mỗi đoạn sẽ có độ phức tạp bậc hai khi $n\le 10^5$. Các nhóm không thể chứa những đoạn giao nhau, nên đáp án là số đoạn chồng lấn đồng thời lớn nhất. Sau khi sắp xếp theo đầu mút trái, một đoạn được đưa vào một nhóm khi và chỉ khi đầu mút phải hiện tại của nhóm đó nằm nghiêm ngặt bên trái nó.
>
> Một min-heap lưu các đầu mút phải của nhóm sẽ quyết định việc tái sử dụng: lấy phần tử trên cùng ra khi nhóm đó có thể nhận đoạn mới, nếu không thì mở một nhóm mới. Kích thước heap là số nhóm nhỏ nhất.

<!-- thinking:end -->

Trước tiên, ta sắp xếp các đoạn theo đầu mút trái. Ta dùng một min heap để duy trì đầu mút phải của mỗi nhóm (phần tử trên cùng của heap là giá trị nhỏ nhất trong các đầu mút phải của mọi nhóm).

Tiếp theo, ta duyệt qua từng đoạn:

- Nếu đầu mút trái của đoạn hiện tại lớn hơn phần tử trên cùng của heap, điều đó có nghĩa là đoạn hiện tại có thể được thêm vào nhóm tương ứng với phần tử trên cùng của heap. Ta lấy trực tiếp phần tử trên cùng ra khỏi heap, sau đó đưa đầu mút phải của đoạn hiện tại vào heap.
- Ngược lại, hiện tại không có nhóm nào có thể chứa đoạn hiện tại, nên ta tạo một nhóm mới và đưa đầu mút phải của đoạn hiện tại vào heap.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng `intervals`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minGroups(self, intervals: List[List[int]]) -> int:
        q = []
        for left, right in sorted(intervals):
            if q and q[0] < left:
                heappop(q)
            heappush(q, right)
        return len(q)
```

#### Java

```java
class Solution {
    public int minGroups(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for (var e : intervals) {
            if (!q.isEmpty() && q.peek() < e[0]) {
                q.poll();
            }
            q.offer(e[1]);
        }
        return q.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minGroups(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        priority_queue<int, vector<int>, greater<int>> q;
        for (auto& e : intervals) {
            if (q.size() && q.top() < e[0]) {
                q.pop();
            }
            q.push(e[1]);
        }
        return q.size();
    }
};
```

#### Go

```go
func minGroups(intervals [][]int) int {
	sort.Slice(intervals, func(i, j int) bool { return intervals[i][0] < intervals[j][0] })
	q := hp{}
	for _, e := range intervals {
		if q.Len() > 0 && q.IntSlice[0] < e[0] {
			heap.Pop(&q)
		}
		heap.Push(&q, e[1])
	}
	return q.Len()
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
function minGroups(intervals: number[][]): number {
    intervals.sort((a, b) => a[0] - b[0]);
    const q = new PriorityQueue({ compare: (a, b) => a - b });
    for (const [left, right] of intervals) {
        if (!q.isEmpty() && q.front() < left) {
            q.dequeue();
        }
        q.enqueue(right);
    }
    return q.size();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
