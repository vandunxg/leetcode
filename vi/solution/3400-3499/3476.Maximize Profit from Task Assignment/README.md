---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3476. Maximize Profit from Task Assignment 🔒](https://leetcode.com/problems/maximize-profit-from-task-assignment)

[中文文档](/solution/3400-3499/3476.Maximize%20Profit%20from%20Task%20Assignment/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>workers</code>, trong đó <code>workers[i]</code> biểu thị mức kỹ năng của worker thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên 2D <code>tasks</code>, trong đó:</p>

<ul>
	<li><code>tasks[i][0]</code> biểu thị yêu cầu kỹ năng cần có để hoàn thành task.</li>
	<li><code>tasks[i][1]</code> biểu thị lợi nhuận nhận được khi hoàn thành task.</li>
</ul>

<p>Mỗi worker có thể hoàn thành <strong>nhiều nhất</strong> một task, và họ chỉ có thể nhận task nếu mức kỹ năng của họ <strong>bằng</strong> yêu cầu kỹ năng của task. Hôm nay có thêm một <strong>worker bổ sung</strong> tham gia, người này có thể nhận <em>bất kỳ</em> task nào, <strong>không phụ thuộc</strong> vào yêu cầu kỹ năng.</p>

<p>Hãy trả về <strong>tổng lợi nhuận lớn nhất</strong> có thể nhận được bằng cách phân công task tối ưu cho các worker.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">workers = [1,2,3,4,5], tasks = [[1,100],[2,400],[3,100],[3,400]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1000</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Worker 0 hoàn thành task 0.</li>
	<li>Worker 1 hoàn thành task 1.</li>
	<li>Worker 2 hoàn thành task 3.</li>
	<li>Worker bổ sung hoàn thành task 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">workers = [10,10000,100000000], tasks = [[1,100]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">100</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì không có worker nào khớp với yêu cầu kỹ năng, chỉ worker bổ sung mới có thể hoàn thành task 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">workers = [7], tasks = [[3,3],[3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Worker bổ sung hoàn thành task 1. Worker 0 không thể làm việc vì không có task nào có yêu cầu kỹ năng bằng 7.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= workers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= workers[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= tasks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>tasks[i].length == 2</code></li>
	<li><code>1 &lt;= tasks[i][0], tasks[i][1] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Priority Queue

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi worker có thể nhận một task có cùng kỹ năng, và ta có thể giao một task còn lại cho bất kỳ ai. Vì $n,m\le 10^5$, ta nên luôn chọn lợi nhuận lớn nhất hiện tại.
>
> Mỗi worker trước tiên lấy task tốt nhất của kỹ năng đó; sau đó slot bổ sung lấy task còn lại có lợi nhuận lớn nhất trên toàn cục. Cho worker xử lý trước để task tốt nhất của họ không bị giữ lại cho slot bổ sung.
>
> Một hash map lưu các lợi nhuận trong một danh sách đã sắp xếp theo từng kỹ năng. Worker lấy phần tử lớn nhất; cuối cùng duyệt các phần tử đầu của những task còn lại để cộng thêm giá trị lớn nhất trên toàn cục.

<!-- thinking:end -->

Vì mỗi task chỉ có thể được hoàn thành bởi worker có một kỹ năng cụ thể, ta có thể nhóm các task theo yêu cầu kỹ năng và lưu chúng trong một hash table $\textit{d}$, trong đó key là yêu cầu kỹ năng và value là một priority queue được sắp xếp theo lợi nhuận giảm dần.

Sau đó, ta duyệt qua các worker. Với mỗi worker, ta tìm priority queue tương ứng trong hash table $\textit{d}$ dựa trên yêu cầu kỹ năng của họ, lấy phần tử đầu tiên (tức là lợi nhuận lớn nhất mà worker có thể nhận được), rồi xóa phần tử đó khỏi priority queue. Nếu priority queue rỗng, ta xóa nó khỏi hash table.

Cuối cùng, ta cộng lợi nhuận lớn nhất trong các task còn lại vào kết quả.

Độ phức tạp thời gian là $O((n + m) \times \log m)$, và độ phức tạp không gian là $O(m)$. Trong đó $n$ và $m$ lần lượt là số lượng worker và task.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, workers: List[int], tasks: List[List[int]]) -> int:
        d = defaultdict(SortedList)
        for skill, profit in tasks:
            d[skill].add(profit)
        ans = 0
        for skill in workers:
            if not d[skill]:
                continue
            ans += d[skill].pop()
        mx = 0
        for ls in d.values():
            if ls:
                mx = max(mx, ls[-1])
        ans += mx
        return ans
```

#### Java

```java
class Solution {
    public long maxProfit(int[] workers, int[][] tasks) {
        Map<Integer, PriorityQueue<Integer>> d = new HashMap<>();
        for (var t : tasks) {
            int skill = t[0], profit = t[1];
            d.computeIfAbsent(skill, k -> new PriorityQueue<>((a, b) -> b - a)).offer(profit);
        }
        long ans = 0;
        for (int skill : workers) {
            if (d.containsKey(skill)) {
                var pq = d.get(skill);
                ans += pq.poll();
                if (pq.isEmpty()) {
                    d.remove(skill);
                }
            }
        }
        int mx = 0;
        for (var pq : d.values()) {
            mx = Math.max(mx, pq.peek());
        }
        ans += mx;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxProfit(vector<int>& workers, vector<vector<int>>& tasks) {
        unordered_map<int, priority_queue<int>> d;
        for (const auto& t : tasks) {
            d[t[0]].push(t[1]);
        }
        long long ans = 0;
        for (int skill : workers) {
            if (d.contains(skill)) {
                auto& pq = d[skill];
                ans += pq.top();
                pq.pop();
                if (pq.empty()) {
                    d.erase(skill);
                }
            }
        }
        int mx = 0;
        for (const auto& [_, pq] : d) {
            mx = max(mx, pq.top());
        }
        ans += mx;
        return ans;
    }
};
```

#### Go

```go
func maxProfit(workers []int, tasks [][]int) (ans int64) {
	d := make(map[int]*hp)
	for _, t := range tasks {
		skill, profit := t[0], t[1]
		if _, ok := d[skill]; !ok {
			d[skill] = &hp{}
		}
		d[skill].push(profit)
	}
	for _, skill := range workers {
		if _, ok := d[skill]; !ok {
			continue
		}
		ans += int64(d[skill].pop())
		if d[skill].Len() == 0 {
			delete(d, skill)
		}
	}
	mx := 0
	for _, pq := range d {
		for pq.Len() > 0 {
			mx = max(mx, pq.pop())
		}
	}
	ans += int64(mx)
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
function maxProfit(workers: number[], tasks: number[][]): number {
    const d = new Map();
    for (const [skill, profit] of tasks) {
        if (!d.has(skill)) {
            d.set(skill, new MaxPriorityQueue<number>());
        }
        d.get(skill).enqueue(profit);
    }
    let ans = 0;
    for (const skill of workers) {
        const pq = d.get(skill);
        if (pq) {
            ans += pq.dequeue();
            if (pq.size() === 0) {
                d.delete(skill);
            }
        }
    }
    let mx = 0;
    for (const pq of d.values()) {
        mx = Math.max(mx, pq.front());
    }
    ans += mx;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
