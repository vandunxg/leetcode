---
comments: true
difficulty: Medium
rating: 1979
source: Weekly Contest 243 Q3
tags:
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1882. Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers)

[中文文档](/solution/1800-1899/1882.Process%20Tasks%20Using%20Servers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>servers</code> và <code>tasks</code> được đánh chỉ số từ <strong>0</strong>, lần lượt có độ dài <code>n</code>​​​​​​ và <code>m</code>​​​​​​. <code>servers[i]</code> là <strong>trọng số</strong> của server thứ <code>i<sup>​​​​​​th</sup></code>​​​​, còn <code>tasks[j]</code> là <strong>thời gian cần thiết</strong> để xử lý task thứ <code>j<sup>​​​​​​th</sup></code> <strong>tính bằng giây</strong>.</p>

<p>Các task được phân công cho server thông qua một <strong>task queue</strong>. Ban đầu, tất cả server đều rảnh và queue <strong>rỗng</strong>.</p>

<p>Ở giây <code>j</code>, task thứ <code>j<sup>th</sup></code> được <strong>đưa vào</strong> queue (task thứ <code>0<sup>th</sup></code> được đưa vào ở giây <code>0</code>). Khi còn server rảnh và queue chưa rỗng, task ở đầu queue sẽ được phân công cho server rảnh có <strong>trọng số nhỏ nhất</strong>; nếu hòa, chọn server có <strong>chỉ số nhỏ nhất</strong>.</p>

<p>Nếu không còn server rảnh và queue chưa rỗng, ta chờ đến khi một server rảnh rồi lập tức phân công task tiếp theo. Nếu nhiều server rảnh cùng lúc, các task trong queue sẽ được phân công <strong>theo thứ tự đưa vào</strong>, vẫn tuân theo thứ tự ưu tiên về trọng số và chỉ số ở trên.</p>

<p>Một server được phân công task <code>j</code> ở giây <code>t</code> sẽ rảnh lại ở giây <code>t + tasks[j]</code>.</p>

<p>Xây dựng một mảng <code>ans</code>​​​​ có độ dài <code>m</code>, trong đó <code>ans[j]</code> là <strong>chỉ số</strong> của server được phân công task thứ <code>j<sup>​​​​​​th</sup></code>.</p>

<p>Trả về <em>mảng </em><code>ans</code>​​​​.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> servers = [3,3,2], tasks = [1,2,3,2,1,2]
<strong>Đầu ra:</strong> [2,2,0,2,1,2]
<strong>Giải thích: </strong>Các sự kiện theo thứ tự thời gian như sau:
- Ở giây 0, task 0 được thêm vào và xử lý bằng server 2 đến giây 1.
- Ở giây 1, server 2 trở nên rảnh. Task 1 được thêm vào và xử lý bằng server 2 đến giây 3.
- Ở giây 2, task 2 được thêm vào và xử lý bằng server 0 đến giây 5.
- Ở giây 3, server 2 trở nên rảnh. Task 3 được thêm vào và xử lý bằng server 2 đến giây 5.
- Ở giây 4, task 4 được thêm vào và xử lý bằng server 1 đến giây 5.
- Ở giây 5, tất cả server đều rảnh. Task 5 được thêm vào và xử lý bằng server 2 đến giây 7.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> servers = [5,1,4,3,2], tasks = [2,1,2,4,5,2,1]
<strong>Đầu ra:</strong> [1,4,1,4,1,3,2]
<strong>Giải thích: </strong>Các sự kiện theo thứ tự thời gian như sau:
- Ở giây 0, task 0 được thêm vào và xử lý bằng server 1 đến giây 2.
- Ở giây 1, task 1 được thêm vào và xử lý bằng server 4 đến giây 2.
- Ở giây 2, server 1 và 4 trở nên rảnh. Task 2 được thêm vào và xử lý bằng server 1 đến giây 4.
- Ở giây 3, task 3 được thêm vào và xử lý bằng server 4 đến giây 7.
- Ở giây 4, server 1 trở nên rảnh. Task 4 được thêm vào và xử lý bằng server 1 đến giây 9.
- Ở giây 5, task 5 được thêm vào và xử lý bằng server 3 đến giây 7.
- Ở giây 6, task 6 được thêm vào và xử lý bằng server 2 đến giây 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>servers.length == n</code></li>
	<li><code>tasks.length == m</code></li>
	<li><code>1 &lt;= n, m &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= servers[i], tasks[j] &lt;= 2 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàng đợi ưu tiên (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Các task đến vào các thời điểm $0,1,2,\ldots$. Ta chọn server rảnh có trọng số nhỏ nhất, sau đó là chỉ số nhỏ nhất; nếu không có server rảnh thì chờ server rảnh sớm nhất. Duyệt tất cả server cho từng task là quá chậm.
>
> Một heap idle lưu $(\textit{weight},\textit{index})$; một heap busy lưu $(\textit{free time},\textit{weight},\textit{index})$. Với task $j$, đưa các server đã hoàn thành trở lại idle; nếu không có server idle thì lấy server busy rảnh sớm nhất và nối task mới vào nó.

<!-- thinking:end -->

Ta dùng một min-heap $\textit{idle}$ để quản lý tất cả server đang rảnh, trong đó mỗi phần tử là một tuple $(x, i)$ biểu diễn server thứ $i$ có trọng số $x$. Ta dùng một min-heap khác $\textit{busy}$ để quản lý tất cả server đang bận, trong đó mỗi phần tử là một tuple $(w, s, i)$ biểu diễn server thứ $i$ sẽ rảnh ở thời điểm $w$ và có trọng số $s$. Ban đầu, ta thêm tất cả server vào $\textit{idle}$.

Tiếp theo, ta duyệt qua tất cả task. Với task thứ $j$, trước tiên ta lấy tất cả server trong $\textit{busy}$ sẽ rảnh ở thời điểm $j$ hoặc sớm hơn và thêm chúng vào $\textit{idle}$. Sau đó, ta lấy server có trọng số nhỏ nhất từ $\textit{idle}$, thêm nó vào $\textit{busy}$ và phân công cho task thứ $j$. Nếu $\textit{idle}$ rỗng, ta lấy server có thời điểm rảnh sớm nhất từ $\textit{busy}$, thêm nó trở lại $\textit{busy}$ và phân công cho task thứ $j$.

Sau khi duyệt qua tất cả task, ta thu được mảng đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O((n + m) \log n)$, trong đó $n$ là số server và $m$ là số task. Độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $m$ lần lượt là số server và số task.

Các bài tương tự:

- [2402. Meeting Rooms III](https://github.com/doocs/leetcode/blob/main/solution/2400-2499/2402.Meeting%20Rooms%20III/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def assignTasks(self, servers: List[int], tasks: List[int]) -> List[int]:
        idle = [(x, i) for i, x in enumerate(servers)]
        heapify(idle)
        busy = []
        ans = []
        for j, t in enumerate(tasks):
            while busy and busy[0][0] <= j:
                _, s, i = heappop(busy)
                heappush(idle, (s, i))
            if idle:
                s, i = heappop(idle)
                heappush(busy, (j + t, s, i))
            else:
                w, s, i = heappop(busy)
                heappush(busy, (w + t, s, i))
            ans.append(i)
        return ans
```

#### Java

```java
class Solution {
    public int[] assignTasks(int[] servers, int[] tasks) {
        int n = servers.length;
        PriorityQueue<int[]> idle = new PriorityQueue<>((a, b) -> {
            if (a[0] != b[0]) {
                return a[0] - b[0];
            }
            return a[1] - b[1];
        });
        PriorityQueue<int[]> busy = new PriorityQueue<>((a, b) -> {
            if (a[0] != b[0]) {
                return a[0] - b[0];
            }
            if (a[1] != b[1]) {
                return a[1] - b[1];
            }
            return a[2] - b[2];
        });
        for (int i = 0; i < n; i++) {
            idle.offer(new int[] {servers[i], i});
        }
        int m = tasks.length;
        int[] ans = new int[m];
        for (int j = 0; j < m; ++j) {
            int t = tasks[j];
            while (!busy.isEmpty() && busy.peek()[0] <= j) {
                int[] p = busy.poll();
                idle.offer(new int[] {p[1], p[2]});
            }
            if (!idle.isEmpty()) {
                int i = idle.poll()[1];
                ans[j] = i;
                busy.offer(new int[] {j + t, servers[i], i});
            } else {
                int[] p = busy.poll();
                int i = p[2];
                ans[j] = i;
                busy.offer(new int[] {p[0] + t, p[1], i});
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> assignTasks(vector<int>& servers, vector<int>& tasks) {
        using pii = pair<int, int>;
        using arr3 = array<int, 3>;
        priority_queue<pii, vector<pii>, greater<pii>> idle;
        priority_queue<arr3, vector<arr3>, greater<arr3>> busy;
        for (int i = 0; i < servers.size(); ++i) {
            idle.push({servers[i], i});
        }
        int m = tasks.size();
        vector<int> ans(m);
        for (int j = 0; j < m; ++j) {
            int t = tasks[j];
            while (!busy.empty() && busy.top()[0] <= j) {
                auto [_, s, i] = busy.top();
                busy.pop();
                idle.push({s, i});
            }

            if (!idle.empty()) {
                auto [s, i] = idle.top();
                idle.pop();
                ans[j] = i;
                busy.push({j + t, s, i});
            } else {
                auto [w, s, i] = busy.top();
                busy.pop();
                ans[j] = i;
                busy.push({w + t, s, i});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func assignTasks(servers []int, tasks []int) (ans []int) {
	idle := hp{}
	busy := hp2{}
	for i, x := range servers {
		heap.Push(&idle, pair{x, i})
	}
	for j, t := range tasks {
		for len(busy) > 0 && busy[0].w <= j {
			p := heap.Pop(&busy).(tuple)
			heap.Push(&idle, pair{p.s, p.i})
		}
		if idle.Len() > 0 {
			p := heap.Pop(&idle).(pair)
			ans = append(ans, p.i)
			heap.Push(&busy, tuple{j + t, p.s, p.i})
		} else {
			p := heap.Pop(&busy).(tuple)
			ans = append(ans, p.i)
			heap.Push(&busy, tuple{p.w + t, p.s, p.i})
		}
	}
	return
}

type pair struct {
	s int
	i int
}

type hp []pair

func (h hp) Len() int { return len(h) }
func (h hp) Less(i, j int) bool {
	a, b := h[i], h[j]
	return a.s < b.s || a.s == b.s && a.i < b.i
}
func (h hp) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)   { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }

type tuple struct {
	w int
	s int
	i int
}

type hp2 []tuple

func (h hp2) Len() int { return len(h) }
func (h hp2) Less(i, j int) bool {
	a, b := h[i], h[j]
	return a.w < b.w || a.w == b.w && (a.s < b.s || a.s == b.s && a.i < b.i)
}
func (h hp2) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp2) Push(v any)   { *h = append(*h, v.(tuple)) }
func (h *hp2) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function assignTasks(servers: number[], tasks: number[]): number[] {
    const idle = new PriorityQueue({
        compare: (a, b) => (a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]),
    });
    const busy = new PriorityQueue({
        compare: (a, b) =>
            a[0] === b[0] ? (a[1] === b[1] ? a[2] - b[2] : a[1] - b[1]) : a[0] - b[0],
    });
    for (let i = 0; i < servers.length; ++i) {
        idle.enqueue([servers[i], i]);
    }
    const ans: number[] = [];
    for (let j = 0; j < tasks.length; ++j) {
        const t = tasks[j];
        while (busy.size() > 0 && busy.front()![0] <= j) {
            const [_, s, i] = busy.dequeue()!;
            idle.enqueue([s, i]);
        }
        if (idle.size() > 0) {
            const [s, i] = idle.dequeue()!;
            busy.enqueue([j + t, s, i]);
            ans.push(i);
        } else {
            const [w, s, i] = busy.dequeue()!;
            busy.enqueue([w + t, s, i]);
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
