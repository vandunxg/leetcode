---
comments: true
difficulty: Medium
rating: 1419
source: Weekly Contest 413 Q2
tags:
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3275. K-th Nearest Obstacle Queries](https://leetcode.com/problems/k-th-nearest-obstacle-queries)

[中文文档](/solution/3200-3299/3275.K-th%20Nearest%20Obstacle%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Có một mặt phẳng 2D vô hạn.</p>

<p>Cho số nguyên dương <code>k</code>. Đồng thời, bạn được cho một mảng 2D <code>queries</code>, chứa các truy vấn sau:</p>

<ul>
	<li><code>queries[i] = [x, y]</code>: Xây dựng một chướng ngại vật tại tọa độ <code>(x, y)</code> trên mặt phẳng. Đảm bảo rằng tại thời điểm thực hiện truy vấn này <strong>không</strong> có chướng ngại vật nào ở tọa độ đó.</li>
</ul>

<p>Sau mỗi truy vấn, hãy tìm <strong>khoảng cách</strong> của chướng ngại vật <code>k<sup>th</sup></code> <strong>gần nhất</strong> tính từ gốc tọa độ.</p>

<p>Trả về một mảng số nguyên <code>results</code>, trong đó <code>results[i]</code> là chướng ngại vật gần nhất thứ <code>k<sup>th</sup></code> sau truy vấn <code>i</code>, hoặc <code>results[i] == -1</code> nếu có ít hơn <code>k</code> chướng ngại vật.</p>

<p><strong>Lưu ý</strong> rằng ban đầu <strong>không</strong> có chướng ngại vật nào.</p>

<p><strong>Khoảng cách</strong> từ chướng ngại vật tại tọa độ <code>(x, y)</code> đến gốc tọa độ được tính bằng <code>|x| + |y|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = [[1,2],[3,4],[2,3],[-3,0]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,7,5,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu có 0 chướng ngại vật.</li>
	<li>Sau <code>queries[0]</code>, có ít hơn 2 chướng ngại vật.</li>
	<li>Sau <code>queries[1]</code>, các chướng ngại vật có khoảng cách lần lượt là 3 và 7.</li>
	<li>Sau <code>queries[2]</code>, các chướng ngại vật có khoảng cách lần lượt là 3, 5 và 7.</li>
	<li>Sau <code>queries[3]</code>, các chướng ngại vật có khoảng cách lần lượt là 3, 3, 5 và 7.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = [[5,5],[4,4],[3,3]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[10,8,6]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau <code>queries[0]</code>, có một chướng ngại vật ở khoảng cách 10.</li>
	<li>Sau <code>queries[1]</code>, các chướng ngại vật có khoảng cách là 8 và 10.</li>
	<li>Sau <code>queries[2]</code>, các chướng ngại vật có khoảng cách là 6, 8 và 10.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queries.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li>Tất cả <code>queries[i]</code> đều phân biệt.</li>
	<li><code>-10<sup>9</sup> &lt;= queries[i][0], queries[i][1] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Max-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Các chướng ngại vật xuất hiện lần lượt; sau mỗi lần xuất hiện, ta cần tìm khoảng cách Manhattan gần nhất thứ $k$ hiện tại. Với $q\le 2\times 10^5$, việc sắp xếp lại mỗi lần là không thể. Ta chỉ cần giữ lại $k$ khoảng cách nhỏ nhất; phần tử lớn nhất trong số đó chính là khoảng cách gần nhất thứ $k$.
>
> Một max-heap lưu $k$ khoảng cách đó (dưới dạng số đối). Trước khi có đủ $k$ điểm, đáp án là $-1$; sau đó, phần tử trên cùng của heap là đáp án. Mỗi lần cập nhật có độ phức tạp $O(\log k)$.

<!-- thinking:end -->

Ta có thể sử dụng một priority queue (max-heap) để duy trì $k$ chướng ngại vật gần gốc tọa độ nhất.

Duyệt qua mảng $\textit{queries}$, với mỗi truy vấn, tính tổng các giá trị tuyệt đối của $x$ và $y$, sau đó thêm tổng này vào priority queue. Nếu kích thước của priority queue lớn hơn $k$, lấy phần tử trên cùng ra. Nếu kích thước hiện tại của priority queue bằng $k$, thêm phần tử trên cùng vào mảng kết quả; ngược lại, thêm $-1$ vào mảng kết quả.

Sau khi duyệt xong, trả về mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log k)$, còn độ phức tạp không gian là $O(k)$. Trong đó, $n$ là độ dài của mảng $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def resultsArray(self, queries: List[List[int]], k: int) -> List[int]:
        ans = []
        pq = []
        for i, (x, y) in enumerate(queries):
            heappush(pq, -(abs(x) + abs(y)))
            if i >= k:
                heappop(pq)
            ans.append(-pq[0] if i >= k - 1 else -1)
        return ans
```

#### Java

```java
class Solution {
    public int[] resultsArray(int[][] queries, int k) {
        int n = queries.length;
        int[] ans = new int[n];
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        for (int i = 0; i < n; ++i) {
            int x = Math.abs(queries[i][0]) + Math.abs(queries[i][1]);
            pq.offer(x);
            if (i >= k) {
                pq.poll();
            }
            ans[i] = i >= k - 1 ? pq.peek() : -1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> resultsArray(vector<vector<int>>& queries, int k) {
        vector<int> ans;
        priority_queue<int> pq;
        for (const auto& q : queries) {
            int x = abs(q[0]) + abs(q[1]);
            pq.push(x);
            if (pq.size() > k) {
                pq.pop();
            }
            ans.push_back(pq.size() == k ? pq.top() : -1);
        }
        return ans;
    }
};
```

#### Go

```go
func resultsArray(queries [][]int, k int) (ans []int) {
	pq := &hp{}
	for _, q := range queries {
		x := abs(q[0]) + abs(q[1])
		pq.push(x)
		if pq.Len() > k {
			pq.pop()
		}
		if pq.Len() == k {
			ans = append(ans, pq.IntSlice[0])
		} else {
			ans = append(ans, -1)
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
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
function resultsArray(queries: number[][], k: number): number[] {
    const pq = new MaxPriorityQueue<number>();
    const ans: number[] = [];
    for (const [x, y] of queries) {
        pq.enqueue(Math.abs(x) + Math.abs(y));
        if (pq.size() > k) {
            pq.dequeue();
        }
        ans.push(pq.size() === k ? pq.front() : -1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
