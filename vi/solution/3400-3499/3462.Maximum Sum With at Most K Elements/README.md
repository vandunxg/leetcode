---
comments: true
difficulty: Medium
rating: 1416
source: Weekly Contest 438 Q2
tags:
    - Greedy
    - Array
    - Matrix
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3462. Maximum Sum With at Most K Elements](https://leetcode.com/problems/maximum-sum-with-at-most-k-elements)

[中文文档](/solution/3400-3499/3462.Maximum%20Sum%20With%20at%20Most%20K%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p data-pm-slice="1 3 []">Cho một ma trận số nguyên 2D <code>grid</code> có kích thước <code>n x m</code>, một mảng số nguyên <code>limits</code> có độ dài <code>n</code> và một số nguyên <code>k</code>. Nhiệm vụ là tìm <strong>tổng lớn nhất</strong> của <strong>nhiều nhất</strong> <code>k</code> phần tử trong ma trận <code>grid</code> sao cho:</p>

<ul data-spread="false">
	<li>
		<p>Số phần tử được lấy từ hàng <code>i<sup>th</sup></code> của <code>grid</code> không vượt quá <code>limits[i]</code>.</p>
	</li>
</ul>

<p data-pm-slice="1 1 []">Trả về <strong>tổng lớn nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2],[3,4]], limits = [1,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Từ hàng thứ hai, ta có thể lấy nhiều nhất 2 phần tử. Các phần tử được lấy là 4 và 3.</li>
	<li>Tổng lớn nhất có thể đạt được của nhiều nhất 2 phần tử được chọn là <code>4 + 3 = 7</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[5,3,7],[8,2,6]], limits = [2,2], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">21</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Từ hàng đầu tiên, ta có thể lấy nhiều nhất 2 phần tử. Phần tử được lấy là 7.</li>
	<li>Từ hàng thứ hai, ta có thể lấy nhiều nhất 2 phần tử. Các phần tử được lấy là 8 và 6.</li>
	<li>Tổng lớn nhất có thể đạt được của nhiều nhất 3 phần tử được chọn là <code>7 + 8 + 6 = 21</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == limits.length</code></li>
	<li><code>m == grid[i].length</code></li>
	<li><code>1 &lt;= n, m &lt;= 500</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= limits[i] &lt;= m</code></li>
	<li><code>0 &lt;= k &lt;= min(n * m, sum(limits))</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng có thể đóng góp nhiều nhất $\textit{limits}[i]$ phần tử và tổng cộng ta lấy nhiều nhất $k$ phần tử. Vì vậy, cần chọn các ô lớn nhất thỏa mãn điều kiện.
>
> Sắp xếp từng hàng, giữ lại $\textit{limit}$ giá trị lớn nhất của hàng đó, sau đó chọn $k$ giá trị lớn nhất trong các ứng viên này.
>
> Một min-heap có kích thước $k$ nhận các ứng viên của từng hàng theo thứ tự từ lớn đến nhỏ và loại bỏ phần tử nhỏ nhất khi bị vượt quá kích thước. Tổng các phần tử trong heap chính là đáp án.

<!-- thinking:end -->

Ta có thể dùng một priority queue (min-heap) $\textit{pq}$ để duy trì $k$ phần tử lớn nhất.

Duyệt qua từng hàng, sắp xếp các phần tử trong hàng, sau đó lấy $\textit{limit}$ phần tử lớn nhất của mỗi hàng và thêm chúng vào $\textit{pq}$. Nếu kích thước của $\textit{pq}$ vượt quá $k$, ta lấy phần tử trên cùng của heap ra.

Cuối cùng, tính tổng các phần tử trong $\textit{pq}$.

Độ phức tạp thời gian là $O(n \times m \times (\log m + \log k))$, còn độ phức tạp không gian là $O(k)$. Trong đó, $n$ và $m$ lần lượt là số hàng và số cột của ma trận $\textit{grid}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, grid: List[List[int]], limits: List[int], k: int) -> int:
        pq = []
        for nums, limit in zip(grid, limits):
            nums.sort()
            for _ in range(limit):
                heappush(pq, nums.pop())
                if len(pq) > k:
                    heappop(pq)
        return sum(pq)
```

#### Java

```java
class Solution {
    public long maxSum(int[][] grid, int[] limits, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        int n = grid.length;
        for (int i = 0; i < n; ++i) {
            int[] nums = grid[i];
            int limit = limits[i];
            Arrays.sort(nums);
            for (int j = 0; j < limit; ++j) {
                pq.offer(nums[nums.length - j - 1]);
                if (pq.size() > k) {
                    pq.poll();
                }
            }
        }
        long ans = 0;
        for (int x : pq) {
            ans += x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxSum(vector<vector<int>>& grid, vector<int>& limits, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        int n = grid.size();

        for (int i = 0; i < n; ++i) {
            vector<int> nums = grid[i];
            int limit = limits[i];
            ranges::sort(nums);

            for (int j = 0; j < limit; ++j) {
                pq.push(nums[nums.size() - j - 1]);
                if (pq.size() > k) {
                    pq.pop();
                }
            }
        }

        long long ans = 0;
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
type MinHeap []int

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func maxSum(grid [][]int, limits []int, k int) int64 {
	pq := &MinHeap{}
	heap.Init(pq)
	n := len(grid)

	for i := 0; i < n; i++ {
		nums := make([]int, len(grid[i]))
		copy(nums, grid[i])
		limit := limits[i]
		sort.Ints(nums)

		for j := 0; j < limit; j++ {
			heap.Push(pq, nums[len(nums)-j-1])
			if pq.Len() > k {
				heap.Pop(pq)
			}
		}
	}

	var ans int64 = 0
	for pq.Len() > 0 {
		ans += int64(heap.Pop(pq).(int))
	}

	return ans
}
```

#### TypeScript

```ts
function maxSum(grid: number[][], limits: number[], k: number): number {
    const pq = new MinPriorityQueue<number>();
    const n = grid.length;
    for (let i = 0; i < n; i++) {
        const nums = grid[i];
        const limit = limits[i];
        nums.sort((a, b) => a - b);
        for (let j = 0; j < limit; j++) {
            pq.enqueue(nums[nums.length - j - 1]);
            if (pq.size() > k) {
                pq.dequeue();
            }
        }
    }
    let ans = 0;
    while (!pq.isEmpty()) {
        ans += pq.dequeue();
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
